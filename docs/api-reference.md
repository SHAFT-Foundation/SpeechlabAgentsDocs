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

### Get Projects

Retrieve a list of projects with optional filtering and pagination:

```python
import requests

url = "https://translate-api.speechlab.ai/v1/projects"

# Query parameters
params = {
    "sortBy": "owner:asc,updatedAt:desc",  # Sort order
    "limit": 10,                           # Number of results per page
    "page": 1,                             # Page number
    "thirdPartyIDs": "ryan0227-1",        # Filter by third party ID
    "expand": "true"                       # Include expanded project details
}

headers = {
    'Authorization': 'Bearer <Your Bearer Token>'
}

response = requests.request("GET", url, headers=headers, params=params)
print(response.text)
```

The response includes:
- List of projects with their details including content, transcriptions, and translations
- Pagination information (page, limit, totalPages, totalResults)
- Department and owner filters
- Available transcription and translation language variants

Key query parameters:
- `sortBy`: Sort results by specified fields (e.g., "owner:asc,updatedAt:desc")
- `limit`: Number of results per page
- `page`: Page number for pagination
- `thirdPartyIDs`: Filter projects by third party ID
- `expand`: Include expanded project details when true

Example Response:
```json
{
    "projects": [
        {
            "_id": "66c620baaafc68002ee2ea1e",
            "name": "Test Video.mov",
            "owner": {
                "email": "ryan+credits@speechlab.ai",
                "firstName": "crediuts",
                "lastName": "ryan"
            },
            "content": {
                "fileUuid": "07d56e4e-ddae-4497-b94c-61658a1d8554",
                "language": "en",
                "contentDuration": 16.62,
                "media": {
                    "thumbnail": "base64_encoded_image_data",
                    "operationType": "INPUT",
                    "uri": "s3://speechlab-data-dev/original/07d56e4e-ddae-4497-b94c-61658a1d8554.mov",
                    "uuid": "07d56e4e-ddae-4497-b94c-61658a1d8554",
                    "category": "video",
                    "format": "mov",
                    "contentType": "video/quicktime"
                }
            },
            "transcription": {
                "speakers": ["Speaker 1"],
                "language": "en",
                "status": "COMPLETE",
                "medias": [
                    "66c621394f97baa7dec9c6e1",
                    "66c6213b4f97baa7dec9c709",
                    "66c6213baafc68002ee2ebe5"
                ],
                "transcriptionText": "..."
            },
            "translations": [
                {
                    "language": "es_la",
                    "status": "COMPLETE",
                    "translationText": "...",
                    "medias": [
                        "66c50c444f97baa7deb4edaf",
                        "66c50c474f97baa7deb4ee47",
                        "66c50d904f97baa7deb50a6b"
                    ],
                    "dub": [
                        {
                            "voiceMatchingMode": "source",
                            "language": "es_la",
                            "status": "COMPLETE",
                            "medias": [
                                "66c50c7b4f97baa7deb4f329",
                                "66c50c7c4f97baa7deb4f34d",
                                "66c50c7c4f97baa7deb4f357",
                                "66c50c7d4f97baa7deb4f3bd",
                                "66c50c7e4f97baa7deb4f415"
                            ]
                        }
                    ]
                }
            ],
            "projectType": "account",
            "createdAt": "2024-08-21T17:15:38.129Z",
            "updatedAt": "2024-08-21T17:15:38.129Z"
        }
    ],
    "page": 1,
    "limit": 10,
    "totalPages": 14,
    "totalResults": 135
}
```

