# Tavilo – Stimmen für Schlafgeschichten

_Stand: 09.07.2026 · 1 weibliche + 1 männliche Stimme (final)_

## Ausgewählte Stimmen

| Rolle | Ordner | Voice ID | Charakter |
|---|---|---|---|
| Weiblich | `sanfte_erzaehlerin` | `german_podcast_female_calm_01` | Sehr sanft, langsam, warm, natürlich |
| Männlich | `liebevoller_vater` | `german_audiobook_male_narrator_01` | Tief, warm, väterlich, authentisch |

## Fester Intro/Outro-Text

**Intro:** „Schön, dass du hier bist. Heute Abend erzähle ich dir eine Geschichte über [Thema], die dich sanft in den Schlaf wiegt. Kuschel dich in deine Decke und hör einfach zu."

**Outro:** „Das war unsere Geschichte für heute. Schlaf gut und bis zum nächsten Mal."

## Ordnerstruktur

```
voices/
├── sanfte_erzaehlerin/
│   ├── voice-description.md
│   ├── voice-example.md
│   ├── voice-example.mp3
│   └── voice-sheet.pdf
├── liebevoller_vater/
│   ├── voice-description.md
│   ├── voice-example.md
│   ├── voice-example.mp3
│   └── voice-sheet.pdf
├── intro-outro/
│   ├── intro-text.md
│   ├── outro-text.md
│   ├── intro-sanfte_erzaehlerin.mp3
│   ├── intro-liebevoller_vater.mp3
│   ├── outro-sanfte_erzaehlerin.mp3
│   └── outro-liebevoller_vater.mp3
└── README.md
```

## Hinweise

- Intro, Outro und Klangmotiv bleiben bei jeder Geschichte exakt gleich (akustische Wiedererkennung für Kinder).
- Die Voice-IDs sind QuisyTTS-IDs für konsistente Nachgenerierung.
- TTS-Instruktionen sind auf Englisch für beste Steuerbarkeit.
