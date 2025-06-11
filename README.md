# Offline YouTube Viewer

A self-hosted web application for managing and viewing offline YouTube videos downloaded via Downie, featuring metadata processing, search/filtering, and seamless IINA integration.

## Features

- 🎬 **Offline Video Management** - Browse and play locally downloaded YouTube videos
- 🔍 **Smart Search & Filtering** - Search by title/author, filter by watch status
- 🖼️ **Thumbnail Generation** - Automatic thumbnail creation for local videos
- ▶️ **IINA Integration** - One-click video playback with progress tracking
- 📊 **Watch Progress** - Visual progress bars and watch status tracking
- 🔗 **YouTube Integration** - Mark videos as watched on YouTube for algorithm tracking
- 📱 **Responsive Design** - Clean, modern interface with dark mode support
- 🗂️ **Channel Management** - Automatic channel URL resolution and caching

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
  git clone https://github.com/yourusername/yt.git
  cd yt
  ./yt
  ```

The app will automatically install required Ruby gems and start the web server at `http://localhost:7778`.

## Usage

### Downloading Videos

1. Use Downie to download YouTube videos to `~/Downloads/YouTube/`
2. Ensure Downie is configured to save JSON metadata files
3. The app will automatically process metadata and generate thumbnails

### Video Management

- **Play Videos**: Click the play button overlay on thumbnails
- **Mark as Watched**: Click the ✓ button (also opens YouTube at video end)
- **Delete Videos**: Click the trash icon (opens YouTube at video end and deletes files)
- **Search**: Use the search bar to filter by title or channel name
- **Filter**: Sort by download date or upload date, filter by watch status

### Channel Management

Visit `/channels` to manage cached channel URLs. The app automatically tries to resolve YouTube channel URLs and falls back to search URLs when needed.

## Configuration

The app uses these default paths:
- **Download Directory**: `~/Downloads/YouTube/`
- **Cache File**: `~/Downloads/YouTube/channel_cache.json`
- **Server**: `http://0.0.0.0:7778`

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

- `GET /` - Main video grid interface
- `GET /channels` - Channel management interface
- `POST /mark-watched` - Mark video as watched
- `DELETE /video` - Delete video and associated files
- `GET /thumbnails/:file` - Serve thumbnail images

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run `bin/ci` to ensure all checks pass
5. Submit a pull request

## License

This project is open source. Feel free to use, modify, and distribute as needed.

## Acknowledgments

- Built for use with [Downie](https://software.charliemonroe.net/downie/) by Charlie Monroe
- Designed for [IINA](https://iina.io/) video player integration
- Uses [Tailwind CSS](https://tailwindcss.com/) for styling
