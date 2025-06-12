# Offline YouTube Viewer

A self-hosted web application for managing and viewing offline YouTube videos downloaded via Downie, featuring metadata processing, search/filtering, and seamless IINA integration.

## Features

- 🎬 **Offline Video Management** - Browse and play locally downloaded YouTube videos
- 🔍 **Smart Search & Filtering** - Real-time search by title/author, filter by watch status
- 🖼️ **Interactive Thumbnails** - Hover overlays with play icons and automatic thumbnail generation
- ▶️ **IINA Integration** - Seamless video playback with bidirectional progress sync
- 📊 **Progress Tracking** - Visual progress bars with real-time sync between web and IINA
- 🔗 **YouTube Integration** - Mark videos as watched, sync timestamps, maintain algorithm tracking
- 📱 **Responsive Design** - Clean, modern Netflix-style interface with full dark mode support
- 🗂️ **Channel Management** - Automatic channel URL resolution and intelligent caching
- 🎭 **Theater Mode** - Immersive fullscreen viewing with persistent preferences
- 📝 **Rich Descriptions** - Collapsible video descriptions with proper formatting

## Screenshots

The app provides a Netflix-style grid interface for browsing your offline video collection with thumbnails, progress indicators, and interactive controls.

## Requirements

- **Ruby** (any recent version)
- **[IINA](https://iina.io/)** - Required for video playback
- **[Downie](https://software.charliemonroe.net/downie/)** - For downloading YouTube videos with metadata
- **ffprobe** (optional) - For improved video duration detection

## Installation

1. **Install IINA**:
  ```bash
  brew install --cask iina
  ```

2. **Clone and run**:
  ```bash
  git clone https://github.com/ericboehs/yt.git
  cd yt
  ./yt
  ```

The app will automatically install required Ruby gems and start the web server at `http://localhost:7777` (or the port specified by the `PORT` environment variable).

## Usage

### Running as a Service (macOS)

To run the application automatically on startup using macOS Launch Agents:

1. **Create a launch agent plist file** at `~/Library/LaunchAgents/com.ericboehs.yt.plist`:
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
  <plist version="1.0">
    <dict>
      <key>EnvironmentVariables</key>
      <dict>
        <key>PATH</key>
        <string>/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin</string>
        <key>PORT</key>
        <string>7777</string>
        <key>RACK_ENV</key>
        <string>production</string>
      </dict>
      <key>KeepAlive</key>
      <true/>
      <key>Label</key>
      <string>com.ericboehs.yt</string>
      <key>ProgramArguments</key>
      <array>
        <string>ruby</string>
        <string>/path/to/your/yt/script</string>
      </array>
      <key>RunAtLoad</key>
      <true/>
      <key>StandardErrorPath</key>
      <string>/Users/yourname/Library/Logs/com.ericboehs.yt/stdout.log</string>
      <key>StandardOutPath</key>
      <string>/Users/yourname/Library/Logs/com.ericboehs.yt/stdout.log</string>
      <key>WorkingDirectory</key>
      <string>/path/to/your/yt/directory</string>
    </dict>
  </plist>
  ```

2. **Create the log directory**:
  ```bash
  mkdir -p ~/Library/Logs/com.ericboehs.yt
  ```

3. **Load the service**:
  ```bash
  launchctl load -w ~/Library/LaunchAgents/com.ericboehs.yt.plist
  ```

4. **Manage the service**:
  ```bash
  # Restart the service
  launchctl unload ~/Library/LaunchAgents/com.ericboehs.yt.plist
  launchctl load -w ~/Library/LaunchAgents/com.ericboehs.yt.plist
   
  # View logs
  tail -f ~/Library/Logs/com.ericboehs.yt/stdout.log
   
  # Stop the service
  launchctl unload ~/Library/LaunchAgents/com.ericboehs.yt.plist
  ```

**Important notes:**
- Update the paths to match your system (replace `/path/to/your/` and `yourname` with actual paths)
- Ensure your `PATH` includes the directory where Ruby is installed
- The service will automatically restart if it crashes (`KeepAlive`)
- Logs are written to the specified log file for debugging

### Downloading Videos

1. Use Downie to download YouTube videos to `~/Downloads/YouTube/`
2. Ensure Downie is configured to save JSON metadata files
3. The app will automatically process metadata and generate thumbnails

### Video Management

- **Play Videos**: Click thumbnails or use hover overlay play button to start videos
- **Theater Mode**: Toggle fullscreen viewing with T key or theater button
- **Progress Sync**: Seamless progress tracking between web player and IINA
- **Mark as Watched**: Click the ✓ button (also opens YouTube at video end)
- **Delete Videos**: Click the trash icon (opens YouTube at video end and deletes files)
- **Search**: Real-time search from any page, with YouTube fallback suggestions
- **Filter**: Sort by download/upload date, filter by watch status (unwatched/partial/watched)

### Channel Management

Visit `/channels` to manage cached channel URLs. The app automatically tries to resolve YouTube channel URLs and falls back to search URLs when needed.

## Configuration

The app uses these default paths:
- **Download Directory**: `~/Downloads/YouTube/`
- **Cache File**: `~/Downloads/YouTube/channel_cache.json`
- **Server**: `http://0.0.0.0:7777` (configurable via `PORT` env var)

## Development

### Running Tests

```bash
bin/yt-test
```

### Linting and Quality Checks

```bash
bin/ci
```

This runs:
- editorconfig-checker
- rubocop (code style)
- test suite with coverage
- coverage reporting

### Pre-commit Hooks

The repository includes pre-commit hooks that automatically run the CI pipeline:

```bash
# Hooks are automatically installed when you run the app
# They run bin/ci before each commit
```

## Architecture

- **Backend**: Ruby/Sinatra web application
- **Frontend**: ERB templates with Tailwind CSS
- **Video Processing**: Integration with IINA for playback, ffprobe for metadata
- **File Management**: JSON metadata processing with automatic updates
- **Caching**: Channel URL resolution with persistent caching

## API Endpoints

- `GET /` - Main video grid interface with search and filtering
- `GET /video` - Individual video player view with theater mode
- `GET /channels` - Channel management interface
- `POST /sync-progress` - Bidirectional progress sync between web player and IINA
- `POST /mark-watched` - Mark video as watched (with YouTube timestamp)
- `DELETE /video` - Delete video and associated files
- `GET /thumbnails/:file` - Serve thumbnail images
- `GET /video-file/:file` - Serve video files for web playback

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run `bin/ci` to ensure all checks pass
5. Submit a pull request

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT). Feel free to use, modify, and distribute as needed.

## Acknowledgments

- Built for use with [Downie](https://software.charliemonroe.net/downie/) by Charlie Monroe
- Designed for [IINA](https://iina.io/) video player integration
- Uses [Tailwind CSS](https://tailwindcss.com/) for styling
