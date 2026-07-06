# Tavilo – Teststimmen für Schlafgeschichten

_Status: erstellt am 06.07.2026 · 5 weibliche + 5 männliche Stimmen_

Diese Stimmen wurden für das Tavilo-Schlafgeschichten-Projekt als Auswahl
generiert (Zielgruppe: Kinder 3–7 Jahre, Tonalität: warm / ruhig /
vertrauensvoll). Jede Stimme liegt in einem eigenen Unterordner mit:

- `voice-description.md` – Steckbrief (mit TTS-Instruktion auf Englisch)
- `voice-example.md` – Beispieltext (Deutsch)
- `voice-example.wav` – Beispielaudio (WAV, Quellformat)
- `voice-example.mp3` – Beispielaudio (MP3, 192 kbps)

## Weibliche Stimmen

| # | Ordner              | Voice ID        | Charakter                                                  |
| - | ------------------- | --------------- | --------------------------------------------------------- |
| 1 | `sanfte_mutter`     | `54397d59da91`  | Sehr sanft, mütterlich, nah – wie eine Mutter am Bett       |
| 2 | `ruhige_erzaehlerin`| `e20a57e14600`  | Ruhig, klar, gleichmäßig – erfahrene Geschichtenerzählerin  |
| 3 | `junge_frau`        | `0778e996492f`  | Jung, freundlich, träumerisch – wie eine große Schwester    |
| 4 | `warme_grossmutter` | `9fe2ad3cc01e`  | Warm, voll, tief – liebevolle Großmutter                    |
| 5 | `leise_fluesterin`  | `a835735e63dd`  | Sehr leise, flüsternd, hypnotisch – für das Ausstiegsritual |

## Männliche Stimmen

| # | Ordner              | Voice ID        | Charakter                                                  |
| - | ------------------- | --------------- | --------------------------------------------------------- |
| 1 | `sanfter_vater`     | `aff061ee54c2`  | Sanft, tief, beschützend – wie ein Vater am Bett            |
| 2 | `ruhiger_erzaehler` | `7efe7708a500`  | Ruhig, warm, klar – erfahrener Hörbuch-Sprecher             |
| 3 | `junger_mann`       | `7181961b9f12`  | Jung, freundlich, mit leisem Staunen – wie ein großer Bruder|
| 4 | `warmer_grossvater` | `229344980096`  | Warm, voll, tief – liebevoller Großvater                    |
| 5 | `leiser_fluesterer` | `3d79cca97b2a`  | Sehr leise, flüsternd, tief – für das Ausstiegsritual       |

## Empfehlung für die Auswahl

- Für das **Einstiegs-/Ausstiegsritual** eignen sich besonders die
  sehr nahen Stimmen (`sanfte_mutter`, `sanfter_vater`, `leise_fluesterin`,
  `leiser_fluesterer`).
- Für den **Erzählteil** der Geschichte eignen sich die klareren, etwas
  distanzierteren Stimmen (`ruhige_erzaehlerin`, `ruhiger_erzaehler`,
  `junge_frau`, `junger_mann`).
- Für eine **sehr warme, traditionelle Tonalität** bieten sich die
  großmütterlichen Stimmen (`warme_grossmutter`, `warmer_grossvater`) an.

## Hinweise

- Die `voice_id` ist die ID im QuisyTTS-System. Mit ihr lassen sich
  weitere Folgen mit derselben Stimme generieren
  (siehe Skill `voice-maker`).
- Die TTS-Instruktion in jeder `voice-description.md` ist absichtlich
  auf Englisch formuliert, da die TTS-Engine so am präzisesten steuerbar
  ist. Der Beispieltext ist auf Deutsch (Zielsprache der Geschichten).
- Audio-Spec (Loudness, Format) ist noch offen – siehe
  `/docs/01-projekt-briefing.md` §8.2.
