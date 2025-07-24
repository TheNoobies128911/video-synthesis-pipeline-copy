# User Input Guide for Simple Video Pipeline

## 🎬 Overview

The Simple Video Pipeline now supports multiple ways for users to provide their content requirements, photos, and voice samples. This guide covers all available input methods.

## 🚀 Quick Start (Recommended)

### Interactive Input Collection
The easiest way to get started:

```bash
python get_user_input.py
```

This will guide you through:
1. **Content Description** - What you want to create
2. **Face Image Upload** - Your photo for the avatar
3. **Voice Sample Upload** - Your voice for cloning (optional)
4. **Preferences** - Audience, tone, style settings
5. **Confirmation** - Review and start generation

## 📋 What Users Need to Provide

### 1. Content Information
- **Title**: Short, descriptive title for the video
- **Topic**: Main subject matter to cover
- **Description**: Detailed explanation (optional but helpful)

### 2. Media Files
- **Face Image**: Clear photo of the person (required)
  - Formats: JPG, PNG, BMP, TIFF
  - Size: 100x100 to 4096x4096 pixels
  - Max file size: 50MB
  
- **Voice Sample**: Audio for voice cloning (optional)
  - Formats: WAV, MP3, M4A, FLAC, OGG
  - Duration: 1 second to 5 minutes
  - Max file size: 100MB

### 3. Preferences (Optional)
- **Audience**: general, students, professionals, experts
- **Tone**: professional, friendly, motivational, casual
- **Emotion**: confident, inspired, curious, excited, calm
- **Content Type**: Short-Form Video Reel, Educational Tutorial, Presentation, Explainer Video
- **Background Animation**: Yes/No

## 🔧 Input Methods

### Method 1: Interactive Script (Recommended)
```bash
python get_user_input.py
```

**Pros:**
- User-friendly guided experience
- Built-in file validation
- Automatic file organization
- Error handling and retry options
- Can start video generation immediately

**Best for:** New users, one-time video creation

### Method 2: Direct Command Line
```bash
python simple_pipeline.py \
  --title "Machine Learning Basics" \
  --topic "Introduction to ML concepts" \
  --face-image path/to/face.jpg \
  --voice-audio path/to/voice.wav \
  --audience students \
  --tone friendly \
  --include-animation
```

**Pros:**
- Quick for experienced users
- Scriptable and automatable
- Direct control over all parameters

**Best for:** Experienced users, batch processing, automation

### Method 3: Session File
```bash
# First, collect inputs interactively
python get_user_input.py

# Later, reuse the saved session
python simple_pipeline.py --session-file user_assets/session_12345/user_inputs.json
```

**Pros:**
- Reuse previous inputs
- Easy to share configurations
- Reproducible results

**Best for:** Repeated generation, template workflows

## 📁 File Organization

### Automatic Organization
When using `get_user_input.py`, files are automatically organized:

```
user_assets/
└── session_1705234567_abc123/
    ├── face_image.jpg          # User's face photo
    ├── voice_sample.wav        # User's voice sample
    └── user_inputs.json        # Complete session data
```

### Session Data Structure
```json
{
  "session_id": "session_1705234567_abc123",
  "created_at": "2024-01-14T10:30:00",
  "content": {
    "title": "Machine Learning Basics",
    "topic": "Introduction to ML",
    "description": "A beginner-friendly overview..."
  },
  "files": {
    "face_image": "path/to/face_image.jpg",
    "voice_sample": "path/to/voice_sample.wav"
  },
  "preferences": {
    "audience": "students",
    "tone": "friendly",
    "emotion": "confident",
    "content_type": "Educational Tutorial",
    "include_animation": true
  }
}
```

## ✅ File Validation

### Image Files
- **Formats**: JPEG, PNG, BMP, TIFF
- **Minimum size**: 100x100 pixels
- **Maximum size**: 4096x4096 pixels
- **File size limit**: 50MB
- **Quality check**: Validates image can be opened

### Audio Files
- **Formats**: WAV, MP3, M4A, FLAC, OGG
- **Duration**: 1 second to 5 minutes
- **File size limit**: 100MB
- **Quality check**: Validates audio format and duration

## 🚨 Error Handling

### Common Issues and Solutions

#### File Not Found
```
❌ File not found: /path/to/image.jpg
```
**Solution**: Check the file path is correct and file exists

#### Invalid Format
```
❌ Invalid image format. Supported: .jpg, .jpeg, .png, .bmp, .tiff
```
**Solution**: Convert file to supported format

#### File Too Large
```
❌ File too large: 75.5MB (max 50MB)
```
**Solution**: Compress or resize the file

#### Image Too Small
```
❌ Image too small: 50x50 (minimum 100x100)
```
**Solution**: Use a higher resolution image

#### Audio Too Short
```
❌ Audio too short: 0.5s (minimum 1 second)
```
**Solution**: Use a longer voice sample

## 🎯 Best Practices

### For Face Images
- Use a clear, well-lit photo
- Face should be centered and visible
- Avoid sunglasses or face coverings
- Recommended size: 512x512 or larger
- Good lighting and contrast

### For Voice Samples
- Record in a quiet environment
- Speak clearly and naturally
- 10-30 seconds is usually sufficient
- Avoid background noise
- Multiple sentences work better than single words

### For Content
- Be specific about your topic
- Provide context in the description
- Choose appropriate audience and tone
- Consider if animation would enhance your content

## 🔄 Workflow Examples

### Complete Workflow
1. **Prepare files**: Face image and voice sample
2. **Run interactive input**: `python get_user_input.py`
3. **Provide information**: Follow prompts step by step
4. **Review and confirm**: Check all inputs before generation
5. **Start generation**: Choose to begin immediately or later
6. **Check results**: Find output in `outputs/final_video.mp4`

### Quick Command Line
```bash
# For users who know exactly what they want
python simple_pipeline.py \
  --title "My Tutorial" \
  --topic "Python Basics" \
  --face-image ~/photos/me.jpg \
  --voice-audio ~/audio/voice.wav \
  --audience students \
  --tone friendly
```

### Batch Processing
```bash
# Create multiple videos with different topics
for topic in "Variables" "Functions" "Classes"; do
  python simple_pipeline.py \
    --title "Python $topic" \
    --topic "$topic in Python" \
    --face-image face.jpg \
    --voice-audio voice.wav \
    --audience students
done
```

## 🔧 Configuration

### Environment Setup
Make sure you have:
- Python packages: PIL, librosa (optional for better validation)
- FFmpeg installed for audio analysis
- Conda environments set up as per README_SIMPLE.md

### API Configuration
Set your Gemini API key in `config.json`:
```json
{
  "gemini_api_key": "your_api_key_here"
}
```

## 🆘 Getting Help

### Check Input Requirements
```bash
python get_user_input.py --help
python simple_pipeline.py --help
```

### Validate Files Manually
```bash
# Test image with Python
python -c "from PIL import Image; img = Image.open('face.jpg'); print(f'Valid image: {img.size}')"

# Test audio with ffprobe
ffprobe -v quiet -show_entries format=duration -of csv=p=0 voice.wav
```

### Debug Mode
For detailed logging during generation:
```bash
python simple_pipeline.py --verbose [other args]
```

The enhanced input system makes it easy for users to provide their requirements and media files while ensuring everything is validated and properly organized for video generation.