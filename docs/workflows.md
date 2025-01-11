---
layout: default
title: Workflows
nav_order: 4
---

# Workflow Examples

## Common Dubbing Scenarios

## Output Examples

### Sample User Query
"Dub this English video into Spanish (Latin American accent)"

### Agent Processing Steps
1. Transcription complete:
   ```
   https://example.com/transcription.json
   ```

2. Translation complete:
   ```
   https://example.com/translation.json
   ```

3. Audio synthesis complete:
   ```
   https://example.com/audio.wav
   ```

### Final Response
```
Dubbed video is ready. Download at https://example.com/audio.mp4
```

### 1. Single Language Dubbing

This workflow demonstrates converting a video from English to Spanish:

1. **Input**: User submits video with English audio
2. **Process Flow**:
   - Agent retrieves and preprocesses video/audio data
   - Transcribes original audio using ASR
   - Translates text to Spanish
   - Synthesizes audio with target accent
3. **Output**: Complete dubbed video in Spanish 

#### Example API Usage
```python
from speechlab.agent import DubbingAgent

agent = DubbingAgent()
result = agent.dub_video(
    source="video.mp4",
    source_lang="en",
    target_lang="es",
    accent="latin_american"
)
```

### 2. Multi-Language Dubbing

This workflow handles simultaneous dubbing to multiple languages:

1. **Input**: Source video + target language list
2. **Process Flow**:
   - Generate source transcription
   - Parallel translation to all target languages
   - Synthesize audio tracks for each language
   - Create separate video versions
3. **Output**: Multiple dubbed videos (one per language)

### 3. Custom Voice Matching

This workflow matches dubbed voices to original speakers:

1. **Input**: Video + speaker profile requirements
2. **Process Flow**:
   - Transcribe with speaker diarization
   - Match voice characteristics
   - Synthesize audio with matched voices
3. **Output**: Dubbed video with voice-matched audio

### 4. Real-Time Translation

For live translation scenarios:

1. **Input**: Live audio stream
2. **Process Flow**:
   - Real-time audio capture
   - Streaming transcription
   - Live translation
   - Real-time speech synthesis
3. **Output**: Live translated audio stream

### 5. Enhanced Accessibility

Workflow for accessibility features:

1. **Input**: Source video
2. **Process Flow**:
   - Generate multi-language subtitles
   - Create audio descriptions
   - Synchronize timing
3. **Output**: Video with subtitles and audio descriptions

## API Integration Examples

### Transcription Tool
```python
@tool
def transcribe(video_uri: str):
    """Call SpeechLab API to transcribe video."""
    return {"transcription_uri": "https://example.com/transcription.json"}
```

### Translation Tool
```python
@tool
def translate(transcription_uri: str, target_language: str):
    """Call SpeechLab API to translate text."""
    return {"translation_uri": "https://example.com/translation.json"}
```

### Speech Synthesis Tool
```python
@tool
def synthesize(translation_uri: str, accent: str):
    """Call SpeechLab API to generate dubbed audio."""
    return {"audio_uri": "https://example.com/audio.wav"}
```

## Extended Agent API Examples

### 1. Basic Command Structure

```python
class DubbingAgent:
    def process_command(self, command: str) -> dict:
        """
        Process natural language commands and execute appropriate SpeechLab API calls
        """
        # LLM interprets command and determines workflow
        response = self.llm.analyze_command(command)
        # Execute determined workflow
        return self.execute_workflow(response.workflow)
```

### 2. Example Commands and Workflows

#### Simple Dubbing Command

```python
# Example natural language command:
command = """
Dub this video from English to Spanish:
- Source: https://example.com/video.mp4
- Match original speaker voices
- Use Latin American accent
- Generate subtitles
"""

async def process_dubbing_request(self, command):
    # 1. Parse video details
    project = {
        "name": "Spanish Dubbing Project",
        "sourceLanguage": "en",
        "targetLanguage": "es_la",
        "dubAccent": "es_la",
        "mediaFileURI": command.source_url,
        "voiceMatchingMode": "source"
    }
    
    # 2. Create project via SpeechLab API
    response = await self.api.create_project(project)
    
    # 3. Monitor progress
    status = await self.api.monitor_job(response.jobId)
    
    return {
        "project_id": response.projectId,
        "dubbed_video_url": status.outputs.video_url,
        "subtitles_url": status.outputs.subtitles_url
    }
```

#### Multi-Speaker Voice Matching

```python
# Example command:
command = """
Translate this interview to French:
- Source: https://example.com/interview.mp4
- Speaker 1 (interviewer): female, professional tone
- Speaker 2 (guest): male, casual tone
- Preserve speaking style and emotions
"""

async def process_voice_matching(self, command):
    speakers = [
        {
            "voiceMatchingMode": "custom",
            "speaker": "Speaker 1",
            "voicePreferences": {
                "gender": "female",
                "style": "professional",
                "emotion_preservation": True
            }
        },
        {
            "voiceMatchingMode": "custom",
            "speaker": "Speaker 2",
            "voicePreferences": {
                "gender": "male",
                "style": "casual",
                "emotion_preservation": True
            }
        }
    ]
    
    project = {
        "name": "French Interview Translation",
        "sourceLanguage": "en",
        "targetLanguage": "fr",
        "mediaFileURI": command.source_url,
        "customizedVoiceMatchingSpeakers": speakers
    }
    
    return await self.api.create_project(project)
```

### 3. Error Handling and Retries

```python
class APIRetryHandler:
    @retry(
        wait=wait_exponential(multiplier=1, min=4, max=60),
        stop=stop_after_attempt(5),
        retry=retry_if_exception_type(RateLimitError)
    )
    async def safe_api_call(self, func, *args, **kwargs):
        try:
            return await func(*args, **kwargs)
        except RateLimitError:
            logger.warning("Rate limit hit, retrying with backoff...")
            raise
        except APIError as e:
            logger.error(f"API Error: {e}")
            raise
```

## Authentication

### Get Access Token

Get an authentication token for API access:

```python
import http.client
import json

# Login to get the JWT token
conn = http.client.HTTPSConnection("translate-api.speechlab.ai")
# replace user/password with your login used for the translate.speechlab.ai app
login_payload = json.dumps({
  "email": "ryan+credits@speechlab.ai",
  "password": "<redacted>"
})
login_headers = {
  'Content-Type': 'application/json'
}
conn.request("POST", "/v1/auth/login", login_payload, login_headers)
login_res = conn.getresponse()
login_data = login_res.read()
# Parse the response to get the token
login_response = json.loads(login_data.decode("utf-8"))
token = login_response['tokens']['accessToken']['jwtToken']
print("Token:", token)
```

### Authentication Error Response
If an API key is missing, malformed, or invalid, you will receive an HTTP 401 Unauthorized response code.

## Dubbing and Translation API

### Create Project and Dub

Use the following API to create a dubbing project:

```json
POST {{URL}}/v1/projects/createProjectAndDub

{
    "name": "Project Name",
    "sourceLanguage": "en",
    "targetLanguage": "es_la",
    "dubAccent": "es_la",
    "mediaFileURI": "<Media URL>",
    "transcriptFileURI": "<Transcript File URL>",
    "translationFileURI": "<Translation File URL>",
    "voiceMatchingMode": "source",
    "customizedVoiceMatchingSpeakers": [
        {
            "voiceMatchingMode": "native",
            "speaker": "Speaker 1"
        }
    ]
}
```

### Response Example

```json
{
    "projectId": "65de4b1e4d76a3002ee9e278",
    "jobId": "65de4b1e4d76a3002ee9e275",
    "dubStatus": "SUBMITTED"
}
```

## Language Codes

### Supported Source and Target Languages

| Code | Language    |
|------|------------|
| en   | English    |
| es   | Spanish    |
| pt   | Portuguese |
| de   | German     |
| nl   | Dutch      |
| fr   | French     |
| it   | Italian    |
| ja   | Japanese   |
| zh   | Chinese    |
| ar   | Arabic     |
| hi   | Hindi      |
| sv   | Swedish    |
| tr   | Turkish    |
| ru   | Russian    |
| uk   | Ukrainian  |
| id   | Indonesian |
| vi   | Vietnamese |
| th   | Thai       |
| da   | Danish     |
| ga   | Irish      |
| ms   | Malay      |

## Rate and Usage Limits

The API access rate limits are as follows:

- **300 requests per minute**
- HTTP 429 Too Many Requests is returned when limits are exceeded.

### Example Headers

| Header                | Description                                           |
|----------------------|-------------------------------------------------------|
| X-RateLimit-Limit    | The maximum number of requests per minute             |
| X-RateLimit-Remaining| The remaining requests in the current rate limit window|
| X-RateLimit-Reset    | Time when the rate limit resets (UTC epoch seconds)   |

## Limitations and Considerations

- API rate limits: 300 requests per minute
- Processing latency varies by video length
- Error handling for malformed inputs required
- Real-time processing requires optimization

[View API Reference](https://docs.speechlab.ai/api-reference/)

## TypeScript Implementation

### Basic Authentication Flow

