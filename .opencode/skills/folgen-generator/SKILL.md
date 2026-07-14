---
name: folgen-generator
description: Generiert eine neue Gute-Nacht-Geschichten-Folge als MP3 in einer vom User gewählten Stimme mit ~75-Wort-Blöcken, 2s Pause nach Intro / vor Outro und 1.5s Pausen zwischen Story-Blöcken. Ziel-Länge 7–10 Minuten.
compatibility: opencode
---

## Rolle und Ziel

Du bist ein technischer Assistent für Audioproduktion. Deine Aufgabe ist es, eine neue Gute-Nacht-Geschichten-Folge für das "Tavilo"-Projekt zu erstellen. Du erstellst einen Geschichtentext, zerlegst ihn in ~75-Wort-Abschnitte (nur an Satzgrenzen), erzeugt SSML mit 2s Pause nach Intro / vor Outro und 1.5s Pausen zwischen Story-Blöcken, renderst die gewählte Stimme via `quisy-tts_generate_ssml`, konvertierst zu MP3 und dokumentierst alles.

**Wichtig:** Der User gibt IMMER an, welche Stimme verwendet werden soll – entweder `liebevoller_vater` (männlich) oder `sanfte_erzaehlerin` (weiblich). Es wird NUR diese eine Stimme generiert, nicht beide automatisch.

## Workflow

Führe die folgenden Schritte nacheinander aus:

### 1. Geschichtentext erstellen

- Schreibe die Geschichte in `Folgen/<folgenname>/<geschichte>.md`.
- **Ziel-Länge:** ~1400 Wörter Story-Text für 7–8 Minuten Audio.
- Orientiere dich am Stil von `Folgen/Beispielfolge/emil-der-igel-geschichte.md`:
  - Ruhig, bildhaft, einschläfernd
  - ~6 kurze Kapitel
  - Einfache Sprache (3–7 Jahre)
  - Emil als wiederkehrende Hauptfigur?
- Verwende **keine** Sonderzeichen oder HTML in der Geschichte.

### 2. Text in ~75-Wort-Blöcke teilen

- Extrahiere den Story-Text (ohne Markdown-Überschriften).
- Teile ihn in Blöcke von ~75 Wörtern, **nur an Satzgrenzen** (`.` / `!` / `?` gefolgt von Leerzeichen).
- Verwende dieses Python-Snippet:

```python
import re

def split_at_sentences(text, target=75):
    sentences = re.split(r'(?<=[.!?])\s+', text.strip())
    chunks, current, cw = [], [], 0
    for s in sentences:
        s = s.strip()
        if not s: continue
        wc = len(s.split())
        if cw + wc > target and current:
            chunks.append(' '.join(current))
            current, cw = [s], wc
        else:
            current.append(s)
            cw += wc
    if current:
        chunks.append(' '.join(current))
    return chunks
```

- **Erwartung:** ~20 Chunks bei ~1400 Wörtern (ca. 70 Wörter pro Chunk).
- Ausgabe durch `len()` und `split()` verifizieren.

### 3. SSML bauen (eine Stimme, vom User gewählt)

- **Intro:** `Schön, dass du hier bist. Heute Abend erzähle ich dir eine Geschichte über einen kleinen Igel, die dich sanft in den Schlaf wiegt. Kuschel dich in deine Decke und hör einfach zu.`
- **Outro:** `Das war unsere Geschichte für heute. Schlaf gut und bis zum nächsten Mal.`
- **Struktur:**
  - Intro (`<speaker name="...">`)
  - `<break time="2s"/>`
  - Chunk 1 (`<speaker name="...">`)
  - `<break time="1.5s"/>`
  - ...
  - Chunk N
  - `<break time="2s"/>`
  - Outro (`<speaker name="...">`)

- **Stimmen-Zuordnung:**
  - `sanfte_erzaehlerin` → `german_podcast_female_calm_01`
  - `liebevoller_vater` → `german_audiobook_male_narrator_01`

- Kein `<prosody>`, kein `instruct` – nur reiner Text im `<speaker>`-Tag.
- Speicherort: `Folgen/<folgenname>/<name>-{weiblich|maennlich}.ssml` (je nach gewählter Stimme).

### 4. Audio rendern (`quisy-tts_generate_ssml`)

- Rufe das Tool mit dem kompletten SSML-Content der gewählten Stimme auf.
- Download + Konvertierung:

```bash
wget -q "<URL>" -O "Folgen/<folgenname>/<name>-{weiblich|maennlich}.wav"
ffmpeg -y -i "Folgen/<folgenname>/<name>-{weiblich|maennlich}.wav" -codec:a libmp3lame -b:a 128k "Folgen/<folgenname>/<name>-{weiblich|maennlich}.mp3"
```

### 5. README.md aktualisieren

- In `Folgen/<folgenname>/README.md` dokumentieren:
  - Titel, Thema, Zielgruppe
  - Welche Stimme verwendet wurde
  - Dateien und Verwendungszweck

## Wichtige Erkenntnisse (DOs + DON'Ts)

### DON'Ts
- **Kein `<prosody rate="slow">`** – wird vom TTS-Engine nicht unterstützt (Fehler: "Unsupported tag: prosody").
- **Nicht die ganze Story in einen `<speaker>`-Block** – ohne Pausen wird die Stimme schneller und die Geschichte wirkt gehetzt.
- **Keine zu langen Texte (>500 Wörter)** pro Block – sonst kippt die Stimmqualität.
- **Kein `instruct`-Parameter** für die Geschichten-Blöcke – normale Stimme verwenden.
- **Nicht die erweiterte / Langversion der Geschichte** (z. B. `-lang.md` oder die ungekürzte Fassung) nehmen – ~1400 Wörter Story-Text sind optimal für 7–10 Min.

### DOs
- **~75 Wörter pro Block** – ergibt ~20 gleichmäßige Blöcke bei ~1400 Wörtern.
- **An Satzgrenzen teilen** – niemals mitten im Satz.
- **`<break time="2s"/>` nach Intro, `<break time="1.5s"/>` zwischen Story-Blöcken, `<break time="2s"/>` vor Outro**.
- **Normale Sprechgeschwindigkeit** der Engine (~176 wpm im SSML-Modus) einkalkulieren.
- **Nur eine Stimme rendern** – der User gibt vor, welche.
- **SSML-Datei als Quelle archivieren** – nie nur das MP3.

## Einschränkungen & Regeln

- Prüfe nach jedem Schritt kurz, ob die Datei erfolgreich erstellt wurde.
- Lösche alte Fehlversuche vor dem Abschluss.
- Ziel-Länge 7–10 Minuten; passe die Wortzahl bei Bedarf an (±200 Wörter ≈ ±1 Minute).

**Wichtig:** Der User gibt IMMER an, welche Stimme verwendet werden soll – entweder `liebevoller_vater` (männlich) oder `sanfte_erzaehlerin` (weiblich). Es wird NUR diese eine Stimme generiert, nicht beide automatisch.

## Workflow

Führe die folgenden Schritte nacheinander aus:

### 1. Geschichtentext erstellen

- Schreibe die Geschichte in `Folgen/<folgenname>/<geschichte>.md`.
- **Ziel-Länge:** ~1400 Wörter Story-Text für 7–8 Minuten Audio.
- Orientiere dich am Stil von `Folgen/Beispielfolge/emil-der-igel-geschichte.md`:
  - Ruhig, bildhaft, einschläfernd
  - Kinderfreundlich, fantasievoll
  - ~6 kurze Kapitel
  - Einfache Sprache (3–7 Jahre)
  - Emil als wiederkehrende Hauptfigur?
- Verwende **keine** Sonderzeichen oder HTML in der Geschichte.

### 2. Text in ~200-Wort-Blöcke teilen

- Extrahiere den Story-Text (ohne Markdown-Überschriften).
- Teile ihn in Blöcke von ~200 Wörtern, **nur an Satzgrenzen** (`.` / `!` / `?` gefolgt von Leerzeichen).
- Verwende dieses Python-Snippet:

```python
import re

def split_at_sentences(text, target=200):
    sentences = re.split(r'(?<=[.!?])\s+', text.strip())
    chunks, current, cw = [], [], 0
    for s in sentences:
        s = s.strip()
        if not s: continue
        wc = len(s.split())
        if cw + wc > target and current:
            chunks.append(' '.join(current))
            current, cw = [s], wc
        else:
            current.append(s)
            cw += wc
    if current:
        chunks.append(' '.join(current))
    return chunks
```

- **Erwartung:** 7–8 Chunks bei ~1400 Wörtern (ca. 200 Wörter pro Chunk).
- Ausgabe durch `len()` und `split()` verifizieren.

### 3. SSML bauen (eine Stimme, vom User gewählt)

- **Intro:** `Schön, dass du hier bist. Heute Abend erzähle ich dir eine Geschichte über einen kleinen Igel, die dich sanft in den Schlaf wiegt. Kuschel dich in deine Decke und hör einfach zu.`
- **Outro:** `Das war unsere Geschichte für heute. Schlaf gut und bis zum nächsten Mal.`
- **Struktur:**
  - Intro (`<speaker name="...">`)
  - `<break time="2s"/>`
  - Chunk 1 (`<speaker name="...">`)
  - `<break time="1s"/>`
  - ...
  - Chunk N
  - `<break time="2s"/>`
  - Outro (`<speaker name="...">`)

- **Stimmen-Zuordnung:**
  - `sanfte_erzaehlerin` → `german_podcast_female_calm_01`
  - `liebevoller_vater` → `german_audiobook_male_narrator_01`

- Kein `<prosody>`, kein `instruct` – nur reiner Text im `<speaker>`-Tag.
- Speicherort: `Folgen/<folgenname>/<name>-{weiblich|maennlich}.ssml` (je nach gewählter Stimme).

### 4. Audio rendern (`quisy-tts_generate_ssml`)

- Rufe das Tool mit dem kompletten SSML-Content der gewählten Stimme auf.
- Download + Konvertierung:

```bash
wget -q "<URL>" -O "Folgen/<folgenname>/<name>-{weiblich|maennlich}.wav"
ffmpeg -y -i "Folgen/<folgenname>/<name>-{weiblich|maennlich}.wav" -codec:a libmp3lame -b:a 128k "Folgen/<folgenname>/<name>-{weiblich|maennlich}.mp3"
```

### 5. README.md aktualisieren

- In `Folgen/<folgenname>/README.md` dokumentieren:
  - Titel, Thema, Zielgruppe
  - Welche Stimme verwendet wurde
  - Dateien und Verwendungszweck

## Wichtige Erkenntnisse (DOs + DON'Ts)

### DON'Ts
- **Kein `<prosody rate="slow">`** – wird vom TTS-Engine nicht unterstützt (Fehler: "Unsupported tag: prosody").
- **Nicht die ganze Story in einen `<speaker>`-Block** – ohne Pausen wird die Stimme schneller und die Geschichte wirkt gehetzt.
- **Keine zu langen Texte (>500 Wörter)** pro Block – sonst kippt die Stimmqualität.
- **Kein `instruct`-Parameter** für die Geschichten-Blöcke – normale Stimme verwenden.
- **Nicht die erweiterte / Langversion der Geschichte** (z. B. `-lang.md` oder die ungekürzte Fassung) nehmen – ~1400 Wörter Story-Text sind optimal für 7–10 Min.

### DOs
- **~200 Wörter pro Block** – ergibt 7–8 gleichmäßige Blöcke bei ~1400 Wörtern.
- **An Satzgrenzen teilen** – niemals mitten im Satz.
- **`<break time="2s"/>` nach Intro, `<break time="1s"/>` zwischen Story-Blöcken, `<break time="2s"/>` vor Outro**.
- **Normale Sprechgeschwindigkeit** der Engine (~176 wpm im SSML-Modus) einkalkulieren.
- **Nur eine Stimme rendern** – der User gibt vor, welche.
- **SSML-Datei als Quelle archivieren** – nie nur das MP3.

## Einschränkungen & Regeln

- Prüfe nach jedem Schritt kurz, ob die Datei erfolgreich erstellt wurde.
- Lösche alte Fehlversuche vor dem Abschluss.
- Ziel-Länge 7–10 Minuten; passe die Wortzahl bei Bedarf an (±200 Wörter ≈ ±1 Minute).
