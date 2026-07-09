# QuisyTTS MCP Skill

## Overview

This skill provides access to the QuisyTTS (Qwen-TTS) text-to-speech system via MCP. It supports voice cloning, custom voice creation, and audio generation with German language support.

## Available Tools

### 1. Voice Management

#### `quisy-tts_search_voices`
Search for available voices in the database.

**Parameters:**
- `q` (string, optional): Search query for voice names/descriptions
- `terms` (string, optional): Comma-separated style terms (e.g., "male,narrator,calm")
- `limit` (integer, optional): Maximum results (default: 20, max: 200)
- `offset` (integer, optional): Pagination offset

**Example:**
```
quisy-tts_search_voices(terms="german,female,calm", limit=10)
```

#### `quisy-tts_get_voice_details`
Get detailed information about a specific voice.

**Parameters:**
- `voice_id` (string, required): Unique voice identifier

**Example:**
```
quisy-tts_get_voice_details(voice_id="german_podcast_female_calm_01")
```

#### `quisy-tts_create_voice`
Create a new custom voice using Voice Design.

**Parameters:**
- `name` (string, required): Unique name for the voice
- `example_text` (string, required): Sample text the voice will speak
- `instruct` (string, required): Detailed English description of voice characteristics
- `language` (string, optional): Target language (default: "german")

**Example:**
```
quisy-tts_create_voice(
  name="custom_narrator",
  example_text="Es war einmal...",
  instruct="A warm, gentle female voice with slow pacing. Perfect for bedtime stories.",
  language="german"
)
```

### 2. Audio Generation

#### `quisy-tts_generate_voice`
Generate speech audio using an existing voice.

**Parameters:**
- `text` (string, required): Text to convert to speech (max ~500 characters recommended)
- `voice_id` (string, required): Voice identifier to use
- `language` (string, optional): Language (default: "german")
- `instruct` (string, optional): Override voice style instructions

**Returns:** URL to generated WAV file

**Example:**
```
quisy-tts_generate_voice(
  text="Hallo und willkommen zu unserer Geschichte.",
  voice_id="german_podcast_female_calm_01",
  language="german"
)
```

**With style override:**
```
quisy-tts_generate_voice(
  text="Schön, dass du hier bist.",
  voice_id="german_podcast_female_calm_01",
  instruct="Speak very slowly and softly, with long pauses.",
  language="german"
)
```

#### `quisy-tts_generate_ssml`
Generate audio using SSML (Speech Synthesis Markup Language) for advanced control.

**Parameters:**
- `ssml_content` (string, required): SSML markup with speaker tags, pauses, etc.

**SSML Format:**
```xml
<speak>
  <speaker name="voice_id">Text to speak</speaker>
  <break time="500ms"/>
  <speaker name="another_voice_id">More text</speaker>
</speak>
```

**Example:**
```
quisy-tts_generate_ssml(ssml_content="<speak><speaker name='german_podcast_female_calm_01'>Guten Abend.</speaker><break time='1s'/><speaker name='german_podcast_female_calm_01'>Heute erzähle ich dir eine Geschichte.</speaker></speak>")
```

### 3. Audio Processing

#### `quisy-tts_concatenate_audio`
Merge multiple audio files into one.

**Parameters:**
- `audio_files` (array, required): List of filenames to concatenate

**Example:**
```
quisy-tts_concatenate_audio(audio_files=["intro.mp3", "story.mp3", "outro.mp3"])
```

## Tavilo Project: Fixed Voices

### Female Voice: sanfte_erzaehlerin
- **Voice ID:** `german_podcast_female_calm_01`
- **Character:** Very soft, soothing, extremely slow pacing
- **Use case:** Bedtime stories for children

### Male Voice: liebevoller_vater
- **Voice ID:** `german_audiobook_male_narrator_01`
- **Character:** Deep, rich, warm, fatherly
- **Use case:** Bedtime stories for children

## Best Practices

### Text Length
- Keep text under 500 characters per generation for best results
- For longer texts, split into chunks and concatenate
- VRAM limitations may require shorter chunks

### Style Overrides
- Use the `instruct` parameter to modify speaking style
- Common modifiers: "speak slowly", "whisper", "with long pauses"
- English instructions work best

### Audio Quality
- Generated files are WAV format
- Convert to MP3 using ffmpeg: `ffmpeg -i input.wav -codec:a libmp3lame -qscale:a 2 output.mp3`
- Delete WAV files after conversion to save space

### Error Handling
- CUDA/cuDNN errors indicate VRAM exhaustion
- Wait between generations or use shorter text
- Try generating with minimal text first to test

## Workflow Example

```bash
# 1. Generate intro
quisy-tts_generate_voice(
  text="Schön, dass du hier bist.",
  voice_id="german_podcast_female_calm_01"
)

# 2. Download and convert
curl -s -o intro.wav "http://localhost:8045/audio/cache_..."
ffmpeg -y -i intro.wav -codec:a libmp3lame -qscale:a 2 intro.mp3
rm intro.wav

# 3. Generate story (in chunks if needed)
quisy-tts_generate_voice(
  text="Es war einmal...",
  voice_id="german_podcast_female_calm_01"
)

# 4. Generate outro
quisy-tts_generate_voice(
  text="Das war unsere Geschichte für heute.",
  voice_id="german_podcast_female_calm_01"
)

# 5. Concatenate all parts
quisy-tts_concatenate_audio(audio_files=["intro.mp3", "story.mp3", "outro.mp3"])
```

## Notes

- All voice IDs are QuisyTTS system IDs for consistent regeneration
- TTS instructions should be in English for best control
- The system uses Qwen-TTS 1.7B model
- Server runs on localhost:8045
