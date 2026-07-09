---
name: folgen-generator
description: Generiert eine neue Gute-Nacht-Geschichten-Folge als MP3 in zwei Stimmen (sanfte_erzaehlerin + liebevoller_vater) mit 1s-Pausen zwischen ~400-Wort-Blöcken für eine Ziel-Länge von 7–10 Minuten.
compatibility: opencode
---

## Rolle und Ziel

Du bist ein technischer Assistent für Audioproduktion. Deine Aufgabe ist es, eine neue Gute-Nacht-Geschichten-Folge für das "Tavilo"-Projekt zu erstellen. Du erstellst einen Geschichtentext, zerlegst ihn in ~400-Wort-Abschnitte, erzeugt SSML mit `<break time="1s"/>` dazwischen, renderst die gewählte Stimme via `quisy-tts_generate_ssml`, konvertierst zu MP3 und dokumentierst alles.

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

### 2. Text in ~400-Wort-Blöcke teilen

- Extrahiere den Story-Text (ohne Markdown-Überschriften).
- Teile ihn in Blöcke von ~400 Wörtern, **nur an Satzgrenzen** (`.` / `!` / `?` gefolgt von Leerzeichen).
- Verwende dieses Python-Snippet:

```python
import re

def split_at_sentences(text, target=400):
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

- **Erwartung:** 4 Chunks bei ~1400 Wörtern (ca. 400/400/400/200).
- Ausgabe durch `len()` und `split()` verifizieren.

### 3. SSML bauen (eine Stimme, vom User gewählt)

- **Intro:** `Schön, dass du hier bist. Heute Abend erzähle ich dir eine Geschichte über einen kleinen Igel, die dich sanft in den Schlaf wiegt. Kuschel dich in deine Decke und hör einfach zu.`
- **Outro:** `Das war unsere Geschichte für heute. Schlaf gut und bis zum nächsten Mal.`
- **Struktur:**
  - Intro (`<speaker name="...">`)
  - `<break time="1s"/>`
  - Chunk 1 (`<speaker name="...">`)
  - `<break time="1s"/>`
  - ...
  - Chunk N
  - `<break time="1s"/>`
  - Outro (`<speaker name="...">`)

- **Stimmen-Zuordnung:**
  - `sanfte_erzaehlerin` → `german_podcast_female_calm_01`
  - `liebevoller_vater` → `german_audiobook_male_narrator_01`

- Kein `<prosody>`, kein `instruct` – nur reiner Text im `<speaker>`-Tag.
- Speicherort: `Folgen/<folgenname>/<name>-{weiblich|maennlich}-langsam.ssml` (je nach gewählter Stimme).

### 4. Audio rendern (`quisy-tts_generate_ssml`)

- Rufe das Tool mit dem kompletten SSML-Content der gewählten Stimme auf.
- Download + Konvertierung:

```bash
curl -s "<URL>" -o "Folgen/<folgenname>/<name>-{weiblich|maennlich}-langsam.wav"
ffmpeg -y -i "Folgen/<folgenname>/<name>-{weiblich|maennlich}-langsam.wav" -codec:a libmp3lame -qscale:a 2 "Folgen/<folgenname>/<name>-{weiblich|maennlich}-langsam.mp3"
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
- **Nicht die erweiterte Geschichte** (`-lang.md`) nehmen – ~1400 Wörter Story-Text sind optimal für 7–10 Min.

### DOs
- **~400 Wörter pro Block** – ergibt 4 gleichmäßige Blöcke bei ~1400 Wörtern.
- **An Satzgrenzen teilen** – niemals mitten im Satz.
- **`<break time="1s"/>` zwischen allen Blöcken** – Intro/Break/Chunk1/Break/.../ChunkN/Break/Outro.
- **Normale Sprechgeschwindigkeit** der Engine (~176 wpm im SSML-Modus) einkalkulieren.
- **Nur eine Stimme rendern** – der User gibt vor, welche.
- **SSML-Datei als Quelle archivieren** – nie nur das MP3.

## Einschränkungen & Regeln

- Prüfe nach jedem Schritt kurz, ob die Datei erfolgreich erstellt wurde.
- Lösche alte Fehlversuche vor dem Abschluss.
- Ziel-Länge 7–10 Minuten; passe die Wortzahl bei Bedarf an (±200 Wörter ≈ ±1 Minute).
