---
layout: default
title: API Reference
nav_order: 4
permalink: /docs/api-reference/
has_children: false
---

# API Reference

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

---

## Dubbing and Translation API

### Create Project and Dub

Use the following API to create a dubbing project:

```json
POST {{URL}}/v1/projects/createProjectAndDub

Body:
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

---

## Language Codes

### Supported Source and Target Languages

| Code | Language         |
|------|-----------------|
| en   | English         |
| es   | Spanish         |
| pt   | Portuguese      |
| de   | German          |
| nl   | Dutch           |
| fr   | French          |
| it   | Italian         |
| ja   | Japanese        |
| zh   | Chinese         |
| ar   | Arabic          |
| hi   | Hindi           |
| sv   | Swedish         |
| tr   | Turkish         |
| ru   | Russian         |
| uk   | Ukrainian       |
| id   | Indonesian      |
| vi   | Vietnamese      |
| th   | Thai            |
| da   | Danish          |
| ga   | Irish           |
| ms   | Malay           |

### Accents

| Code       | Accent             |
|------------|--------------------|
| en_prod    | English (Production) |
| es_la_prod | Spanish Latin      |
| pt_br      | Portuguese (Brazil) |

---

## Rate and Usage Limits

The API access rate limits are as follows:

- **300 requests per minute**
- HTTP 429 Too Many Requests is returned when limits are exceeded.

### Example Headers

| Header               | Description                                          |
|----------------------|------------------------------------------------------|
| X-RateLimit-Limit    | The maximum number of requests per minute.           |
| X-RateLimit-Remaining| The remaining requests in the current rate limit window. |
| X-RateLimit-Reset    | Time when the rate limit resets (UTC epoch seconds).  |

---

## Example Python Code

```python
import requests
import json

url = "https://translate-api.speechlab.ai/v1/projects/createProjectAndDub"

payload = json.dumps({
  "name": "Project Example",
  "sourceLanguage": "en",
  "targetLanguage": "es_la",
  "dubAccent": "es_la",
  "mediaFileURI": "<Media URL>",
  "transcriptFileURI": "<Transcript URL>",
  "translationFileURI": "<Translation URL>",
  "voiceMatchingMode": "source",
  "customizedVoiceMatchingSpeakers": [
    {
      "voiceMatchingMode": "native",
      "speaker": "Speaker 1"
    }
  ]
})
headers = {
  'Content-Type': 'application/json',
  'Authorization': 'Bearer <Your Bearer Token>'
}

response = requests.request("POST", url, headers=headers, data=payload)
print(response.text)
```

