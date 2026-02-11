---
name: video-to-context
description: >
  Use when the user shares a screen recording, demo video, or bug reproduction video
  and you need to understand what happens in it. Extracts speech transcript (via Whisper GPU)
  and key frames (via ffmpeg) into a markdown document you can read directly.
  Symptoms: user mentions video, screen recording, .mov/.mp4 file, or asks you to watch/analyze a recording.
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
---

# Video-to-Context: Extract full context from videos

Turn screen recordings into readable context (transcript + key frames) so you can understand what happened without watching the video.

## When to use

- User shares a screen recording from a Jira card, Slack, or bug report
- You need to understand both what's shown on screen AND what's being said
- Video is in `.mov`, `.mp4`, `.webm`, or similar format

## Requirements

- **ffmpeg** (frame extraction)
- **whisper-ctranslate2** with CUDA (speech-to-text) -- see setup below
- **NVIDIA GPU** with CUDA drivers (CPU fallback possible but slow)
- **uv** package manager

## Setup (one-time)

Run this to install the transcription stack:

```bash
#!/bin/bash
set -euo pipefail

VENV="$HOME/.local/share/video-tools"

# Create isolated venv
uv venv "$VENV" --python 3.12

# Install whisper + CUDA libs
VIRTUAL_ENV="$VENV" uv pip install whisper-ctranslate2 nvidia-cublas-cu12 nvidia-cudnn-cu12

# Create wrapper that sets CUDA library paths
mkdir -p "$HOME/.local/bin"
cat > "$HOME/.local/bin/whisper-gpu" << 'SCRIPT'
#!/bin/bash
VENV="$HOME/.local/share/video-tools"
export LD_LIBRARY_PATH="$VENV/lib/python3.12/site-packages/nvidia/cublas/lib:$VENV/lib/python3.12/site-packages/nvidia/cudnn/lib:${LD_LIBRARY_PATH:-}"
exec "$VENV/bin/whisper-ctranslate2" "$@"
SCRIPT
chmod +x "$HOME/.local/bin/whisper-gpu"

# Verify
whisper-gpu --help &>/dev/null && echo "Ready." || echo "Setup failed."
```

Ensure `~/.local/bin` is in PATH, or create a symlink:
```bash
sudo ln -sf ~/.local/bin/whisper-gpu /usr/local/bin/whisper-gpu
```

## Extraction process

### Step 1: Transcribe audio

```bash
whisper-gpu "$VIDEO" \
  --model large-v3 \
  --language en \
  --device cuda \
  --compute_type int8_float16 \
  --output_format all \
  --output_dir "$OUTDIR" \
  --word_timestamps True \
  --vad_filter True \
  --batched True \
  --batch_size 8
```

This produces `.txt` (plain transcript), `.srt` (timestamped), and `.json` (word-level timestamps).

For non-English videos, change `--language` or remove it for auto-detection.

### Step 2: Extract scene-change frames

```bash
ffmpeg -y -i "$VIDEO" \
  -vf "select='gt(scene,0.3)',scale=1280:-1,drawtext=text='%{pts\:hms}':fontsize=24:fontcolor=white:box=1:boxcolor=black@0.7:x=w-tw-10:y=h-th-10" \
  -vsync vfr -q:v 3 \
  "$OUTDIR/frames/scene_%04d.jpg"
```

The `scene>0.3` threshold catches major visual changes (page navigations, modal opens, screen switches). Timestamps are burned into each frame's bottom-right corner.

### Step 3: Extract interval frames (every 5s)

```bash
ffmpeg -y -i "$VIDEO" \
  -vf "fps=1/5,scale=1280:-1,drawtext=text='%{pts\:hms}':fontsize=24:fontcolor=white:box=1:boxcolor=black@0.7:x=w-tw-10:y=h-th-10" \
  -q:v 3 \
  "$OUTDIR/frames/interval_%04d.jpg"
```

Interval frames fill gaps where no scene change was detected but something relevant might still be happening.

### Step 4: Read the output

1. Read the `.txt` transcript first for the full narrative
2. Read specific frames (`.jpg`) when you need to see what was on screen at a given moment
3. Cross-reference `.srt` timestamps with frame timestamps to match speech to visuals

## Gotcha: macOS filenames with Unicode spaces

macOS screen recordings use U+202F (NARROW NO-BREAK SPACE) instead of regular spaces in filenames. If your file isn't found, try:

```bash
# Rename to regular spaces
find "$DIR" -maxdepth 1 -name "Screen*Recording*" -exec bash -c '
  NEW=$(echo "$1" | sed "s/\xe2\x80\xaf/ /g")
  [ "$1" != "$NEW" ] && mv "$1" "$NEW"
' _ {} \;
```

Or add a glob fallback in your script:

```bash
if [ ! -f "$VIDEO" ]; then
  RESOLVED=$(find "$(dirname "$VIDEO")" -maxdepth 1 \
    -name "$(basename "$VIDEO" | sed 's/ /*/g')" -print -quit 2>/dev/null)
  [ -n "$RESOLVED" ] && VIDEO="$RESOLVED"
fi
```

## Output structure

```
video_context/
  context.md          # Transcript + frame list (read this first)
  recording.txt       # Plain transcript
  recording.srt       # Timestamped transcript
  recording.json      # Word-level timestamps
  frames/
    scene_0001.jpg    # Scene-change frames (timestamps burned in)
    scene_0002.jpg
    interval_0001.jpg # Every-5s frames (timestamps burned in)
    interval_0002.jpg
```

## Quick reference

| What | Command/Flag | Notes |
|------|-------------|-------|
| Whisper model | `large-v3` | Best accuracy. Use `medium` for speed. |
| Quantization | `int8_float16` | Good for 8GB VRAM GPUs (e.g. RTX 4060) |
| Scene threshold | `gt(scene,0.3)` | Lower = more frames. 0.3 is a good default. |
| Frame interval | `fps=1/5` | Every 5 seconds. Change denominator as needed. |
| CPU fallback | `--device cpu` | Works but 10-20x slower than CUDA |
| Output formats | `--output_format all` | Gets txt, srt, json, vtt, tsv at once |
| VAD filter | `--vad_filter True` | Skips silence, improves accuracy |
| Batched mode | `--batched True` | Faster on GPU, uses more VRAM |
