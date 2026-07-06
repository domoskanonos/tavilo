# Tavilo – Teststimmen für Schlafgeschichten

_Stand: 06.07.2026 · 5 weibliche + 5 männliche Stimmen_

Diese Stimmen wurden für das Tavilo-Schlafgeschichten-Projekt als Auswahl
generiert (Zielgruppe: Kinder 3–7 Jahre, Tonalität: warm / ruhig /
vertrauensvoll). Jede Stimme liegt in einem eigenen Unterordner mit:

- `voice-description.md` – Steckbrief (mit TTS-Instruktion auf Englisch)
- `voice-example.md` – Beispieltext (Deutsch)
- `voice-example.wav` – Beispielaudio (WAV, Quellformat)
- `voice-example.mp3` – Beispielaudio (MP3)
- `voice-sheet.pdf` – Steckbrief als PDF

## Weibliche Stimmen

| # | Ordner              | Voice ID                              | Charakter                                                  |
| - | ------------------- | ------------------------------------- | ---------------------------------------------------------- |
| 1 | `warme_abendstimme` | `german_audiobook_klara`              | Warme Erwachsenenstimme, sanft und geduldig                 |
| 2 | `sanfte_mutter`     | `german_podcast_female_calm_02`       | Warm, melodisch, langsam – wie eine Mutter am Bett          |
| 3 | `ruhige_erzaehlerin`| `german_audiobook_female_narrator_01` | Klar, ruhig, gleichmäßig – erfahrene Geschichtenerzählerin  |
| 4 | `warme_grossmutter` | `german_grandmother_ingrid`           | Warm, weise, leicht rau – liebevolle Großmutter             |
| 5 | `leise_fluesterin`  | `german_meditation_sophie`            | Sehr leise, flüsternd, hypnotisch – für das Ausstiegsritual |

## Männliche Stimmen

| # | Ordner              | Voice ID                              | Charakter                                                  |
| - | ------------------- | ------------------------------------- | ---------------------------------------------------------- |
| 1 | `sanfter_vater`     | `german_podcast_male_calm_01`         | Sanft, tief, beruhigend – wie ein Vater am Bett             |
| 2 | `ruhiger_erzaehler` | `german_audiobook_male_narrator_02`   | Warm, reif, moderat – erfahrener Hörbuch-Sprecher           |
| 3 | `junger_mann`       | `german_podcast_male_calm_02`         | Warm, sanft, entspannend – wie ein großer Bruder            |
| 4 | `warmer_grossvater` | `german_storyteller_karl`             | Tief, voll, dramatisch – liebevoller Großvater am Kamin     |
| 5 | `leiser_fluesterer` | `german_podcast_male_calm_03`         | Leise, ruhig, besonnen – für das Ausstiegsritual            |

## Empfehlung für die Auswahl

- Für das **Einstiegs-/Ausstiegsritual** eignen sich besonders die
  sehr nahen Stimmen (`sanfte_mutter`, `sanfter_vater`,
  `leise_fluesterin`, `leiser_fluesterer`).
- Für den **Erzählteil** der Geschichte eignen sich die klareren,
  distanzierteren Stimmen (`ruhige_erzaehlerin`, `ruhiger_erzaehler`,
  `junger_mann`, `warme_abendstimme`).
- Für eine **sehr warme, traditionelle Tonalität** bieten sich die
  großelterlichen Stimmen (`warme_grossmutter`, `warmer_grossvater`) an.

## Hinweise

- Die `voice_id` ist die ID im QuisyTTS-System. Mit ihr lassen sich
  weitere Folgen mit derselben Stimme generieren
  (siehe Skill `voice-maker`).
- Die TTS-Instruktion in jeder `voice-description.md` ist auf Englisch
  formuliert. Der Beispieltext ist auf Deutsch.
- Alle Stimmen nutzen bestehende QuisyTTS-Voices (keine Custom-Voices),
  dadurch konsistentere Qualität.
