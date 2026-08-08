# SKILL: X-Video Deep Analyzer

## Trigger
Any X (Twitter) video / reel that needs more than frames + existing subtitles.

## Required Capabilities
- requests (download)
- ffmpeg / ffprobe (extraction + probe)
- numpy / scipy (signal analysis)
- vosk + small English model (offline ASR)

## Execution Steps
1. Resolve direct video URL (from media list or CDN pattern).
2. Download MP4 to /home/workdir/artifacts/.
3. Extract 16 kHz mono WAV.
4. Run energy / VAD / spectral analysis.
5. Run Vosk recognition (partial + final results).
6. Optionally extract representative frames.
7. Return structured report: transcript + metrics + interpretation.

## Known Limits
- Vosk small model has limited accuracy on music + effects + processed speech.
- No Whisper weights available → high-accuracy multilingual ASR still blocked.
- Large model downloads may hit timeout / disk constraints.

## Improvement Path
- Cache Vosk models permanently.
- Add noise reduction / source separation pre-processing.
- Support larger Vosk models if resources allow.
- Optional fallback to external ASR API when offline quality is insufficient.
