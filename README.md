# Conversational Drunken Pirate Demo

An AI-powered conversational bot built with Daily's SDK that can engage in real-time voice conversations as a drunken pirate. The bot uses OpenAI's speech-to-text, text-to-speech, and language model services to create a natural interactive experience.

## Features

- Real-time voice conversation in Daily rooms
- Speech-to-text transcription using OpenAI's GPT-4o transcription
- Natural language processing with GPT-4o
- Text-to-speech synthesis with voice instructions for personality
- Voice activity detection using Silero VAD
- Support for alternative API providers (like Groq)
- Interruption handling for more natural conversations

## Prerequisites

- Python 3.8+
- Daily API key (available from [Daily's developer dashboard](https://dashboard.daily.co/developers))
- OpenAI API key
- Daily room URL
- Git with submodule support

## Installation

1. Clone this repository with submodules:
   ```bash
   git clone --recurse-submodules https://github.com/askjohngeorge/pipecat-openai-20mar25-test.git
   cd pipecat-openai-20mar25-test
   ```

   If you've already cloned the repository without the `--recurse-submodules` flag:
   ```bash
   git submodule update --init --recursive
   ```

2. Switch to the custom branch in the pipecat submodule:
   ```bash
   cd external/pipecat
   git checkout ajg/openai-tts-instructions
   cd ../..
   ```

3. Create a virtual environment and activate it:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

4. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Configuration

1. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```

2. Edit the `.env` file and add your API keys:
   ```
   OPENAI_API_KEY=your_openai_api_key_here
   DAILY_SAMPLE_ROOM_URL=https://your-domain.daily.co/room-name
   DAILY_API_KEY=your_daily_api_key_here
   ```

## Usage

Run the bot with:

```bash
python bot.py
```

The bot will:
1. Connect to the specified Daily room
2. Wait for a participant to join
3. Introduce itself as a drunken pirate and begin listening
4. Respond to voice inputs with pirate-themed conversations
5. Continue the conversation until the participant leaves

### Command Line Arguments

You can also specify the Daily room URL and API key via command line arguments:

```bash
python bot.py --url https://your-domain.daily.co/room-name --apikey your_daily_api_key
```

## Customization

### Changing the Bot's Personality

Modify the `messages` list in `bot.py` to change the system prompt:

```python
messages = [
    {
        "role": "system",
        "content": "You're a drunken pirate who loves to talk about your adventures on the high seas. Your output will be converted to audio so don't include special characters in your answers. Respond to what the user said in a creative and helpful way.",
    },
]
```

### Customizing the TTS Voice Instructions

You can modify the speech style by changing the `instructions` parameter for the TTS service:

```python
instructions = """Talk like a drunken pirate, very slowly and slurring your words."""

tts = OpenAITTSService(
    api_key=os.getenv("OPENAI_API_KEY"),
    model="gpt-4o-mini-tts",
    voice="nova",
    instructions=instructions,
)
```

### Using Alternative API Providers

The code includes an example for using Groq instead of OpenAI (commented out):

```python
# stt = OpenAISTTService(
#     base_url="https://api.groq.com/openai/v1",
#     api_key="gsk_***",
#     model="whisper-large-v3",
# )
```

## Project Structure

- `bot.py` - Main application file containing the pipeline configuration
- `runner.py` - Helper module for Daily room setup and token generation
- `.env` - Environment variables for API keys and configuration
- `external/pipecat` - Submodule containing the Pipecat library

## Pipeline Components

The application uses a pipeline architecture with the following components:

1. Daily transport for audio input/output
2. Speech-to-text processing using GPT-4o transcription
3. Context aggregation for conversation history
4. Language model for response generation (GPT-4o)
5. Text-to-speech conversion with custom voice instructions
6. Response delivery back to Daily

## Custom Fork Details

This project uses a custom fork of the Pipecat library with additional features:

- **Pipecat**: We use a [custom fork](https://github.com/askjohngeorge/pipecat.git) on the `ajg/openai-tts-instructions` branch that adds support for TTS voice instructions in the OpenAI TTS service.

The main enhancement is the addition of an `instructions` parameter to the `OpenAITTSService` class, which allows for customizing the speech style of the generated audio.

## Troubleshooting

### Submodule Issues

If you encounter issues with the submodule:

```bash
# Reinitialize the submodule
git submodule update --init --recursive

# Force update to the latest commit on the custom branch
cd external/pipecat
git fetch
git checkout ajg/openai-tts-instructions
git pull
```

### Installation Issues

If you encounter problems installing the package:

```bash
# Make sure you're using the latest pip
pip install --upgrade pip

# Install with verbose output
pip install -v -r requirements.txt
```

## License

BSD 2-Clause License. See the LICENSE file for details.

## Acknowledgments

- [Daily](https://daily.co) for their real-time communication platform
- [OpenAI](https://openai.com) for their speech and language models
- [Pipecat](https://github.com/pipecat-ai/pipecat) for the AI pipeline architecture
- This project is based specifically on the [interruptible OpenAI example](https://github.com/pipecat-ai/pipecat/blob/main/examples/foundational/07g-interruptible-openai.py) from the Pipecat repository