# Production User Input Guide for Video Synthesis Pipeline

## 🎬 Overview

The Production Video Synthesis Pipeline provides multiple ways for users to provide their content requirements, photos, and voice samples. This guide covers all available input methods for the **production system** with full API and WebSocket support.

## 🚀 Quick Start (Production System)

### Prerequisites
First, start the production system:

```bash
# Start full production system (API + Frontend)
python start_pipeline.py

# Or start just the API server
python frontend_api_websocket.py
```

### Interactive Input Collection (Recommended)
The easiest way to get started with the production system:

```bash
# Check servers and start interactive input
python start_interactive_input.py

# Or directly use production input collector
python production_user_input.py
```

This will:
1. **Check API Health** - Verify production server is running
2. **Create Session** - Get a unique session ID via API
3. **Content Description** - What you want to create
4. **File Upload** - Upload face image and voice via API endpoints
5. **Preferences** - Audience, tone, style settings
6. **Start Generation** - Begin video processing with real-time monitoring

## 📋 What Users Need to Provide

### 1. Content Information
- **Title**: Short, descriptive title for the video
- **Topic**: Main subject matter to cover
- **Description**: Detailed explanation (optional but helpful)

### 2. Media Files
- **Face Image**: Clear photo of the person (required)
  - Formats: JPG, PNG, BMP, TIFF
  - Validated by production security system
  - Uploaded via `/upload/face` API endpoint
  
- **Voice Sample**: Audio for voice cloning (optional)
  - Formats: WAV, MP3, M4A, FLAC, OGG
  - Validated by production security system
  - Uploaded via `/upload/audio` API endpoint

### 3. Preferences (Optional)
- **Audience**: general, students, professionals, experts
- **Tone**: professional, friendly, motivational, casual
- **Emotion**: confident, inspired, curious, excited, calm
- **Content Type**: Short-Form Video Reel, Educational Tutorial, Presentation, Explainer Video
- **Background Animation**: Yes/No

## 🔧 Input Methods

### Method 1: Interactive Production CLI (Recommended)
```bash
python start_interactive_input.py
```

**Features:**
- Automatic server health checking
- Production API integration
- Real-time WebSocket updates
- Secure file upload and validation
- Session management
- Progress monitoring

**Best for:** New users, guided experience, production quality

### Method 2: Direct API Usage (Advanced)
For developers and automation:

#### Step 1: Create Session
```bash
curl -X POST http://localhost:5002/session/create \
  -H "Content-Type: application/json" \
  -d '{"user_id": "my_user"}'
```

#### Step 2: Upload Face Image
```bash
curl -X POST http://localhost:5002/upload/face \
  -F "file=@/path/to/face.jpg" \
  -F "session_id=your_session_id"
```

#### Step 3: Upload Voice (Optional)
```bash
curl -X POST http://localhost:5002/upload/audio \
  -F "file=@/path/to/voice.wav" \
  -F "session_id=your_session_id"
```

#### Step 4: Start Processing
```bash
curl -X POST http://localhost:5002/process/start \
  -H "Content-Type: application/json" \
  -d '{
    "session_id": "your_session_id",
    "config": {
      "title": "Machine Learning Basics",
      "topic": "Introduction to ML concepts",
      "audience": "students",
      "tone": "friendly"
    }
  }'
```

**Best for:** Automation, batch processing, custom integrations

### Method 3: Web Interface
```bash
# Start full system
python start_pipeline.py

# Then open browser to:
# http://localhost:8080
```

**Features:**
- Full React-based UI
- Drag & drop file uploads
- Real-time progress visualization
- Session management
- Download interface

**Best for:** Non-technical users, visual interface

### Method 4: Legacy Simple Pipeline
For compatibility with older workflows:

```bash
python simple_pipeline.py --session-file user_assets/session_12345/user_inputs.json
```

**Note:** This uses the basic pipeline, not the production API system.

## 🌐 Production System Architecture

### API Endpoints
- `POST /session/create` - Create new session
- `POST /upload/face` - Upload face image
- `POST /upload/audio` - Upload audio file
- `POST /process/start` - Start video generation
- `GET /session/{id}/status` - Check progress
- `GET /outputs/{session_id}` - List generated files
- `GET /download/{session_id}/{filename}` - Download files

### WebSocket Events
- Real-time progress updates
- File upload status
- Processing notifications
- Error reporting

### Security Features
- Comprehensive file validation
- Virus scanning
- Session isolation
- Resource management
- Automatic cleanup

## 📁 File Organization (Production)

### Session Management
The production system uses isolated sessions:

```
NEW/sessions/
└── {session_id}/
    ├── uploads/
    │   ├── face_image.jpg
    │   └── voice_sample.wav
    ├── outputs/
    └── metadata.json
```

### Session Data Structure
```json
{
  "session_id": "uuid-session-id",
  "user_id": "user_identifier",
  "state": "processing",
  "created_at": "2024-01-14T10:30:00Z",
  "files": {
    "face_image": "face_image.jpg",
    "voice_sample": "voice_sample.wav"
  },
  "config": {
    "title": "Machine Learning Basics",
    "topic": "Introduction to ML",
    "audience": "students",
    "tone": "friendly"
  }
}
```

## ✅ File Validation (Production)

### Enhanced Security
- **Format Validation**: Strict MIME type checking
- **Content Scanning**: Deep file analysis
- **Size Limits**: Configurable file size restrictions
- **Virus Scanning**: Optional virus detection
- **Metadata Sanitization**: Clean file metadata

### Error Handling
- Detailed error messages
- Retry mechanisms
- Graceful degradation
- Comprehensive logging

## 🚨 Error Handling & Troubleshooting

### Common Issues

#### API Server Not Running
```
❌ Cannot connect to API server
💡 Make sure to start the server with: python start_pipeline.py
```

#### Session Expired
```
❌ Session not found. Please start a new session.
```
**Solution**: Sessions expire after inactivity. Create a new session.

#### File Upload Failed
```
❌ Upload failed: Invalid file type
```
**Solution**: Check file format and size requirements.

#### WebSocket Connection Failed
```
⚠️ WebSocket setup failed: Connection refused
```
**Solution**: Real-time updates unavailable, but API functionality continues.

## 📊 Monitoring & Progress

### Real-time Monitoring
- WebSocket progress updates
- Processing stage information
- Resource usage statistics
- Error notifications

### Status Checking
```bash
# Check session status
curl http://localhost:5002/session/{session_id}/status

# List outputs
curl http://localhost:5002/outputs/{session_id}
```

### Web Interface
- Visual progress bars
- Stage-by-stage updates
- Download management
- Error reporting

## 🎯 Best Practices

### For Face Images
- Use clear, well-lit photos
- Face should be centered and visible
- Avoid sunglasses or face coverings
- Recommended size: 512x512 or larger

### For Voice Samples
- Record in quiet environment
- Speak clearly and naturally
- 10-30 seconds is usually sufficient
- Multiple sentences work better than single words

### For Content
- Be specific about your topic
- Provide context in description
- Choose appropriate audience and tone
- Consider if animation would enhance content

### For Production Usage
- Always check server health first
- Use meaningful session IDs
- Monitor progress via WebSocket or API
- Clean up sessions when complete

## 🔄 Complete Workflow Example

```bash
# 1. Start production system
python start_pipeline.py

# 2. Use interactive input
python start_interactive_input.py

# 3. Follow prompts:
#    - Enter video title and topic
#    - Upload face image (validates automatically)
#    - Upload voice sample (optional)
#    - Select preferences
#    - Confirm and start generation

# 4. Monitor progress:
#    - Real-time updates in terminal
#    - Web interface at http://localhost:8080
#    - API status endpoint

# 5. Download results:
#    - Via web interface
#    - Via API download endpoints
```

## 🆘 Getting Help

### Health Checks
```bash
# Check API health
curl http://localhost:5002/health

# Check system capabilities
curl http://localhost:5002/capabilities
```

### Logs
- API logs: `logs/pipeline/`
- Error logs: `logs/errors/`
- System logs: `logs/system/`

### Debug Mode
For detailed logging during troubleshooting:
```bash
python production_user_input.py --debug
```

The production user input system provides enterprise-grade file handling, session management, and real-time progress monitoring while maintaining the user-friendly interactive experience.