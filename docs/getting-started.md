---
layout: default
title: Getting Started
nav_order: 2
permalink: /docs/getting-started/
has_children: false
---

# Getting Started

## Prerequisites

Before you begin working with the **Dubbing Agent Framework**, ensure you have the following:

### 1. API Access
- **SpeechLab API Account**: Register at [SpeechLab API Portal](https://translate.speechlab.ai).
- **Authentication Token**: Ensure you have valid API credentials.
- **Rate Limit Guidelines**:
  - 300 requests/minute per account
  - 1000 voice generations/hour
  - 10GB storage quota

### 2. Development Environment
- **Python**: Version 3.7 or higher
- **Package Manager**: pip
- **Required Python Packages**:
  - `langchain`
  - `requests`
  - `python-dotenv`
  - `openai`

---

## Installation

### 1. Install via pip
Run the following command to install the Dubbing Agent Framework:

```bash
pip install dubbing-agent
```

### 2. Install from Source
To install from the GitHub repository:

```bash
git clone https://github.com/speechlab/dubbing-agent
cd dubbing-agent
pip install -e .
```

---

## Configuration

### 1. Environment Variables
Set up the required environment variables:

```env
SPEECHLAB_API_KEY=your_api_key_here
OPENAI_API_KEY=your_openai_key_here
DUBBING_AGENT_DEBUG=True
```

### 2. Basic Initialization

Initialize the agent with your API key and preferred configuration:

```python
from dubbing_agent import DubbingAgent

agent = DubbingAgent(
    api_key="your_api_key",
    language="en",
    voice_id="female_1",
    quality="high"
)
```

---

## Basic Usage

### 1. Text-to-Speech Conversion

Convert a single sentence into speech:

```python
result = agent.synthesize("Hello, world!")
agent.save_audio(result, "output.mp3")
```

### 2. Batch Processing

Process multiple text inputs:

```python
texts = [
    "First sentence to dub.",
    "Second sentence to dub.",
    "Third sentence to dub."
]

results = agent.batch_synthesize(texts)
agent.save_batch(results, "output_directory")
```

### 3. Custom Voice Parameters

Configure advanced voice settings:

```python
agent.configure_voice(
    speed=1.2,
    pitch=1.0,
    emphasis=1.3,
    pause_duration=0.8
)
```

---

## Common Use Cases

### 1. Video Dubbing

Dub an entire video file with a provided script:

```python
agent.dub_video(
    video_path="input.mp4",
    script_path="script.txt",
    output_path="dubbed_video.mp4",
    language="es"
)
```

### 2. Real-time Translation and Dubbing

Stream real-time translation and dubbing:

```python
agent.stream_translate_dub(
    input_stream,
    source_lang="en",
    target_lang="fr",
    output_stream
)
```

---

## Error Handling

Implement error handling for smoother workflows:

```python
from dubbing_agent.exceptions import DubbingError

try:
    result = agent.synthesize("Test text")
except DubbingError as e:
    print(f"Error during dubbing: {e}")
except RateLimitError as e:
    print(f"Rate limit exceeded: {e}")
```

---

## Best Practices

### 1. Resource Management
- Close connections after use.
- Monitor API usage to avoid exceeding limits.
- Cache frequently used audio segments for efficiency.

### 2. Quality Optimization
- Provide high-quality input text and scripts.
- Adjust voice parameters for natural results.
- Test small samples before scaling up batch processes.

### 3. Performance Tips
- Use parallel processing for batch operations.
- Stream large files to reduce memory usage.
- Enable caching for frequently used operations.

---

## Troubleshooting

### Common Issues and Solutions

1. **API Connection Problems**:
   - Verify your API key and credentials.
   - Check network connectivity.
   - Ensure proper endpoint configuration.

2. **Audio Quality Issues**:
   - Ensure input text formatting is correct.
   - Adjust voice parameters for better results.
   - Validate output format settings.

3. **Performance Bottlenecks**:
   - Monitor system resources during execution.
   - Use batch processing for multiple requests.
   - Enable debug logging to identify slow operations.

---

## Next Steps

- Explore the [API Reference](/docs/api-reference).
- Learn [Advanced Topics](/docs/advanced).
- Join the [Community Forum](https://community.speechlab.ai).
- Browse [Sample Projects](/docs/samples).

## Support

For additional help:

- **Email**: [support@speechlab.ai](mailto:support@speechlab.ai)
- **Documentation**: [docs.speechlab.ai](https://docs.speechlab.ai)
- **GitHub Issues**: [Report a Bug](https://github.com/speechlab/dubbing-agent/issues)

---

*Last updated: 2024-03-21*

