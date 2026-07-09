# TASK: Create a Minimal Working Example for Local Audio Generation (ACE-Step 1.5)

## Context
We need a local, commercial-use safe (MIT/Apache 2.0) pipeline to generate a short 3-5 second intro for a children's bedtime audio play. The instrument must be a soft, calming glockenspiel or music box. We are using the **ACE-Step 1.5** model (or a direct Apache 2.0 equivalent available via Hugging Face `transformers`/`diffusers` if ACE-Step requires custom repo cloning).

## Your Capabilities
You have full access to the workspace source code and the system console/terminal.

## Objective
Step-by-step, set up the environment, write a minimal Python script to generate the audio, apply a clean post-processing fade-out, and verify the output.

## Execution Steps

### Step 1: Environment Setup (Console)
Create a local python virtual environment (if not already present) and install the required dependencies via console. Ensure GPU support (CUDA) is utilized if available.
Required packages usually include:
- `torch` (with CUDA support if applicable)
- `transformers`
- `diffusers`
- `accelerate`
- `pydub`
- `soundfile` / `scipy`

### Step 2: Write the Python Script (`generate_intro.py`)
Create a minimal, self-contained Python script that:
1. Loads the **ACE-Step 1.5** model weights from Hugging Face.
2. Uses the following prompt parameters:
   - **Text Prompt:** "Gentle glockenspiel melody, soft music box, lullaby vibe, bedtime story intro, high quality, ambient, slow tempo, 48kHz"
   - **Negative Prompt:** "drums, bass, fast rhythm, vocals, noise, distortion"
3. Limits the generation duration target to exactly 4 seconds (or the closest step allowed by the model).
4. Saves the raw output as a WAV file.

### Step 3: Add Post-Processing (Fade-Out)
Integrate `pydub` into the script to ensure the audio doesn't cut off abruptly:
- Load the generated WAV.
- Trim/Ensure it is exactly 4 seconds long.
- Apply a smooth, linear **1.5-second fade-out** at the end.
- Export the final file as `bedtime_intro_commercial.wav`.

### Step 4: Execution & Validation (Console)
1. Run the script via the console.
2. Confirm that `bedtime_intro_commercial.wav` exists in the workspace.
3. Output the exact duration and properties of the final file to the console.

Please start by checking the terminal environment and creating the file structure.