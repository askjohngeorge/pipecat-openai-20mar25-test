# Daily AI Conversation Bot

An AI-powered conversational bot built with Daily's SDK that can engage in real-time voice conversations about dogs. The bot uses OpenAI's speech-to-text, text-to-speech, and language model services to create a natural interactive experience.

## Features

- Real-time voice conversation in Daily rooms
- Speech-to-text transcription using OpenAI's models
- Natural language processing with GPT-4o
- Text-to-speech synthesis for bot responses
- Voice activity detection using Silero VAD
- Support for alternative API providers (like Groq)
- Interruption handling for more natural conversations

## Prerequisites

- Python 3.8+
- Daily API key (available from [Daily's developer dashboard](https://dashboard.daily.co/developers))
- OpenAI API key
- Daily room URL

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/askjohngeorge/pipecat-openai-20mar25-test.git
   cd pipecat-openai-20mar25-test
   ```

2. Create a virtual environment and activate it:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install the required dependencies:
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
3. Introduce itself and begin listening
4. Respond to voice inputs with information about dogs
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
        "content": "You are very knowledgable about dogs. Your output will be converted to audio so don't include special characters in your answers. Respond to what the user said in a creative and helpful way.",
    },
]
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

## Pipeline Components

The application uses a pipeline architecture with the following components:

1. Daily transport for audio input/output
2. Speech-to-text processing (OpenAI)
3. Context aggregation for conversation history
4. Language model for response generation (OpenAI)
5. Text-to-speech conversion (OpenAI)
6. Response delivery back to Daily

## License

BSD 2-Clause License. See the LICENSE file for details.

## Acknowledgments

- [Daily](https://daily.co) for their real-time communication platform
- [OpenAI](https://openai.com) for their speech and language models
- [Pipecat](https://github.com/pipecat-ai/pipecat) for the AI pipeline architecture
- This project is based specifically on the [interruptible OpenAI example](https://github.com/pipecat-ai/pipecat/blob/main/examples/foundational/07g-interruptible-openai.py) from the Pipecat repository