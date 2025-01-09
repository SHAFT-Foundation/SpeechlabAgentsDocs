---
layout: default
title: Troubleshooting
nav_order: 7
---

# Troubleshooting Guide

## Common Issues

### Authentication Problems

#### Issue: 401 Unauthorized
- **Cause**: Invalid or expired token
- **Solution**: Refresh your authentication token
- **Example**:  ```python
  await client.refresh_token()  ```

### Rate Limiting

#### Issue: 429 Too Many Requests
- **Cause**: Exceeded 300 requests/minute
- **Solution**: Implement exponential backoff
- **Example**:  ```python
  @retry(wait=wait_exponential())
  async def safe_request():
      # Your code here  ```

### Media Processing

#### Issue: Invalid Media Format
- **Supported Formats**: MP4, MOV, WAV
- **Max File Size**: 2GB
- **Resolution**: Up to 4K

### Quality Issues

#### Poor Voice Matching
1. Check source audio quality
2. Verify speaker labels
3. Use native voice matching mode

## Performance Optimization

### Batch Processing
- Group similar requests
- Use concurrent processing
- Monitor memory usage

### Network Issues
- Implement request timeouts
- Use connection pooling
- Handle retries properly

[Contact Support →](mailto:support@example.com) 