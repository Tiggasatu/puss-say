# kitten-say

A command-line text-to-speech tool that mimics macOS's `say` command, powered by [KittenTTS](https://github.com/KittenML/KittenTTS).

## Features

- High-quality text-to-speech using KittenTTS
- Multiple voice options (male and female)
- Adjustable speech speed
- Save output to WAV files
- Interactive mode for continuous speech
- Pipe text from stdin
- No GPU required - runs on CPU

## Installation

### Using Nix

```bash
# Run directly
nix run github:Mic92/kitten-say -- "Hello, world!"

# Build and run locally
nix build
./result/bin/kitten-say "Hello, world!"
```

### Development

```bash
# Enter development shell with all dependencies
nix develop

# kitten-say is now available in your PATH
kitten-say "Hello from the development shell!"

# Or use uv directly for Python package management
uv sync
```

## Usage

Basic usage:
```bash
kitten-say "Hello, world!"
```

Choose a different voice:
```bash
kitten-say -v expr-voice-3-m "Hello from a male voice"
```

Save to file:
```bash
kitten-say -o output.wav "Save this speech to a file"
```

Adjust speech speed:
```bash
kitten-say -s 0.8 "Speak more slowly"
kitten-say -s 1.5 "Speak faster"
```

Pipe text:
```bash
echo "This text is piped" | kitten-say
```

Interactive mode:
```bash
kitten-say -i
# Type text and press Enter to hear it spoken
# Press Ctrl+D to exit
```

List available voices:
```bash
kitten-say --list-voices
```

## Available Voices

- `expr-voice-2-m` - Male voice 2
- `expr-voice-2-f` - Female voice 2 [default]
- `expr-voice-3-m` - Male voice 3
- `expr-voice-3-f` - Female voice 3
- `expr-voice-4-m` - Male voice 4
- `expr-voice-4-f` - Female voice 4
- `expr-voice-5-m` - Male voice 5
- `expr-voice-5-f` - Female voice 5

## Requirements

- Nix with flakes enabled (for Nix installation)
- Python 3.13+ (for manual installation)
- PortAudio (automatically provided by Nix)
- No GPU required - CPU inference only

## License

MIT

## Acknowledgments

- [KittenTTS](https://github.com/KittenML/KittenTTS) for the excellent TTS model
- [uv2nix](https://github.com/pyproject-nix/uv2nix) for seamless Python packaging in Nix
