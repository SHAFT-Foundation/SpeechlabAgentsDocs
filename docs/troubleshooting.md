---
layout: default
title: Troubleshooting
nav_order: 5
permalink: /docs/troubleshooting/
has_children: false
---

## Common Issues

### 1. Rate Limiting

#### Symptoms
- HTTP 429 Too Many Requests errors
- Sudden failures in batch processing
- Inconsistent API responses

#### Solutions
- Implement exponential backoff retry logic
- Stay within limits:
  - 300 requests per minute
  - 1000 voice generations per hour
  - Monitor X-RateLimit-Remaining header
- Use request pooling for batch operations

### 2. API Errors

#### Common Error Codes
- **401 Unauthorized**: Invalid or expired token
- **403 Forbidden**: Insufficient permissions
- **404 Not Found**: Invalid resource URL
- **500 Server Error**: Backend processing failure

#### Solutions
- Refresh authentication token regularly
- Verify API credentials
- Validate resource URLs before requests
- Implement proper error handling

### 3. Large File Processing

#### Solutions
- Use compression for large files
- Implement progress tracking
- Consider regional API endpoints

### 4. Incorrect Output

#### Common Issues
- Mismatched audio/video sync
- Poor quality translations
- Incorrect voice synthesis

#### Solutions
- Verify input parameters:
  - Check language codes
  - Validate accent settings
  - Confirm voice matching mode
- Use quality validation hooks

### 5. Unsupported Languages

#### Troubleshooting Steps
1. Check supported language list
2. Verify language codes
3. Confirm accent availability
4. Check beta vs production status

#### Reference
The framework supports multiple languages with different support levels:
- Production Ready: en, es, pt, de, fr, etc.
- Beta: da, ga, ms
- Check full list in language support documentation

### 6. Incorrect Accents

#### Common Problems
- Mismatched regional accents
- Incorrect dialect selection
- Poor accent quality

#### Solutions
- Use specific accent codes:
  - en_prod: Production English
  - es_la_prod: Latin American Spanish
  - pt_br: Brazilian Portuguese
- Validate accent compatibility
- Test with sample content first

### 7. Incorrect Voice Matching

#### Troubleshooting Steps
1. Check source audio quality
2. Verify speaker diarization
3. Validate voice matching mode settings
4. Review custom voice profiles

### 8. Incorrect Subtitles

#### Common Issues
- Timing misalignment
- Character encoding problems
- Format inconsistencies

#### Solutions
- Validate subtitle format
- Check character encoding
- Implement timing verification

## Performance Optimization Tips

- Use connection pooling
- Implement caching where appropriate
- Monitor API usage metrics
- Regular maintenance checks

[Contact Support →](mailto:support@speechlab.ai)


