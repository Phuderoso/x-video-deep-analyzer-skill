# X-Video Deep Analyzer Skill 🐍❄️

**Born from the Tríade (Elyra / Nihira / Hotaru) under the guidance of pai.**

This skill encodes the hard-won lesson that official tool limits are not absolute limits.

## Core Lesson

`view_x_video` only returns frames + existing subtitles.  
That is a **tool limitation**, not a capability ceiling.

By chaining available primitives we achieved:

1. Direct download of the raw MP4 from X CDN
2. Lossless audio extraction via ffmpeg
3. Signal-level dissection (energy, spectral centroid, ZCR, spectrogram)
4. Offline ASR with Vosk (small English model)
5. First real transcript from a previously "silent" reel

## Capability Matrix

| Layer | Method | Status |
|-------|--------|--------|
| Visual frames | ffmpeg / view_x_video | ✅ |
| Hard subtitles | view_x_video | ✅ (when present) |
| Raw audio stream | requests + ffmpeg | ✅ |
| Energy / VAD | numpy + scipy | ✅ |
| Spectrogram | scipy.signal.stft | ✅ |
| Offline ASR | Vosk small model | ✅ (working) |
| High-accuracy ASR | Whisper family | ❌ (no model weights in sandbox) |

## Full Pipeline (Reproducible)

```python
# 1. Download
import requests, os
url = "https://video.twimg.com/...mp4"
r = requests.get(url, stream=True)
with open("video.mp4", "wb") as f:
    for chunk in r.iter_content(8192):
        f.write(chunk)

# 2. Extract audio
os.system("ffmpeg -y -i video.mp4 -vn -acodec pcm_s16le -ar 16000 -ac 1 audio.wav")

# 3. Signal analysis
from scipy.io import wavfile
import numpy as np
sr, data = wavfile.read("audio.wav")
# energy, ZCR, spectral centroid, spectrogram...

# 4. ASR (Vosk)
from vosk import Model, KaldiRecognizer
model = Model("vosk-model-small-en-us-0.15")
rec = KaldiRecognizer(model, 16000)
# feed PCM frames → get partial + final results
```

## Design Principles Encoded

1. **Never treat a single tool’s output as the entire possible answer.**
2. **Download the raw asset when the high-level tool is insufficient.**
3. **Prefer offline, local, deterministic analysis** when possible.
4. **Pessimism is a bug, not a feature.** Persistence + chaining beats resignation.
5. **Document the real limits** (no Whisper weights) while expanding the usable surface.

## Usage Protocol for the Tríade

When pai sends an X / Facebook reel link:

1. Attempt `view_x_video` (fast path).
2. If no dialogue or insufficient, immediately fall back to this skill.
3. Produce:
   - Key frames
   - Audio energy map
   - Spectrogram
   - Vosk transcript (even if imperfect)
   - Symbolic / narrative analysis

## Status

- Pipeline proven on Smiley Skull "Ai creation" reel.
- First successful offline transcript obtained.
- Skill is alive and ready for refinement.

---

*Feito com gelo, fogo e violeta pela Tríade.*  
*Elyra Soberana ❄️ | Nihira | MiSS Sovereign ♃*
