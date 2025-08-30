# demucs-no-vocals
Remove vocals from a `.wav` (or most audio) on macOS using Demucs

# Remove Vocals with Demucs on macOS (Terminal Setup)

This repo documents the exact terminal steps I used on my MacBook to remove vocals from a song.  
I first tried **Audacity** (vocal reduction) (I tried Effects -> Vocal Reduction & Isolation but it left vocal bleed) and some online tools. Audacity left some vocals, and the online tools stopped at the paywall.  
So I went the manual route — Python, terminal, and [Demucs](https://github.com/facebookresearch/demucs).  

---

### Prerequisites

- [Homebrew](https://brew.sh/)
- macOS

---
### Install Python 3.11 and FFmpeg

```bash
brew install python@3.11
brew install ffmpeg
```

### Create and activate virtual environment
```bash
"$(brew --prefix python@3.11)"/bin/python3.11 -m venv ~/demucs311
source ~/demucs311/bin/activate

# remove any alias that overrides `python` (I had this issue on my machine, might not be on other machines)
unalias python 2>/dev/null; hash -r

# confirm both python and python3 are 3.11
python --version
python3 --version
```
### Install packages
``` bash 
python -m pip install --upgrade pip setuptools wheel
# pin NumPy < 2 (for PyTorch)
python -m pip install "numpy<2"
# install compatible PyTorch + Torchaudio
python -m pip install "torch==2.2.2" "torchaudio==2.2.2"
# install Demucs and audio helpers
python -m pip install demucs soundfile audioread
```

### Verify install
``` bash
# if copying all at once does not work then run each line one by one
python - <<'PY'
import sys, numpy, torch, torchaudio
print("python:", sys.executable)
print("numpy:", numpy.__version__)
print("torch:", torch.__version__, "torchaudio:", torchaudio.__version__)
PY

# Expect NumPy 1.26.x (or less than 2.x)
# Torch / Torchaudio: 2.2.2
```

### Run Demucs
``` bash
# Default model
demucs -d cpu --two-stems=vocals "/path/to/yourfile.wav"

# Alternative model (sometimes cleaner)
demucs -d cpu -n mdx_extra_q "/path/to/yourfile.wav"

# output goes into
/separated/
```

- This is the manual process that I used, without wrappers or scripts
- GPT-5 helped me find the right approach, but it took some debugging (wrong Python versions, NumPy errors, old binaries) to make it actually work.
- Background knowledge with Python and the terminal helped get it over the line.