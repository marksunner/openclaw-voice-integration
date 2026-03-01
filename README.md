# OpenClaw Voice Integration

Real-time bidirectional voice interface for [OpenClaw](https://github.com/openclaw/openclaw) agents.

> 🎯 **Goal:** Talk to your AI assistant naturally, hands-free, with real-time conversation flow.

---

## What's Working Today

### Text-to-Speech Providers

| Provider | Type | Latency | Quality | Voice Cloning | Notes |
|----------|------|---------|---------|---------------|-------|
| **ElevenLabs** | Cloud | ~500ms | ⭐⭐⭐⭐⭐ | ✅ Yes | Production-quality, multiple voices |
| **Qwen3-TTS MLX** | Local | ~1.7x RT | ⭐⭐⭐⭐ | ✅ Yes | Apple Silicon optimized, 8-bit quantized |
| **KittenTTS** | Local | ~4.5x RT | ⭐⭐⭐⭐ | ❌ No | Fast audiobook generation, 80M params |
| **OpenAI TTS** | Cloud | ~300ms | ⭐⭐⭐⭐ | ❌ No | Simple API, good for quick responses |

### Speech-to-Text

| Provider | Type | Notes |
|----------|------|-------|
| **Deepgram Nova-3** | Cloud | Real-time streaming, excellent accuracy |
| **Whisper (OpenAI)** | Cloud/Local | Batch transcription, multilingual |

### Live Integrations

- ✅ **Discord Voice Channels** — Join/leave, real-time TTS responses
- ✅ **Audiobook Generation** — [11 chapters of Aporia](https://github.com/marksunner/aporia/releases/tag/audio-chapters-v1) (~3.5 hours)
- ✅ **Voice Cloning** — Custom voices from 3-second samples
- 🚧 **OpenHome DevKit** — Target platform (pending DevKit access)

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  Voice Interface                         │
│         (OpenHome DevKit / Discord / Mobile)             │
└─────────────────────┬───────────────────────────────────┘
                      │ Audio Stream
                      ▼
┌─────────────────────────────────────────────────────────┐
│              Voice Processing Layer                      │
│  ┌─────────────┐  ┌──────────────────────────────────┐  │
│  │    STT      │  │           TTS Provider           │  │
│  │  Deepgram   │  │  ElevenLabs │ Qwen3 │ KittenTTS │  │
│  │  Whisper    │  │   (cloud)   │(local)│  (local)  │  │
│  └──────┬──────┘  └──────────────────────▲───────────┘  │
└─────────┼────────────────────────────────┼──────────────┘
          │                                │
          ▼                                │
┌─────────────────────────────────────────────────────────┐
│                   OpenClaw Gateway                       │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐  │
│  │   Agent     │  │   Memory    │  │     Tools       │  │
│  │  (Claude)   │  │   System    │  │   & Skills      │  │
│  └─────────────┘  └─────────────┘  └─────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

---

## Why Multiple TTS Providers?

**Flexibility for different use cases:**

| Use Case | Best Provider | Why |
|----------|---------------|-----|
| Real-time conversation | ElevenLabs / OpenAI | Low latency, streaming |
| Audiobook narration | KittenTTS | Consistent voice, batch processing |
| Custom voice personas | Qwen3-TTS | Local voice cloning, privacy |
| Offline operation | Qwen3-TTS / KittenTTS | No internet required |

---

## Voice Cloning

Using **Qwen3-TTS via MLX**, we can clone voices from short audio samples:

```bash
# Clone a voice from a 3-second sample
python -m mlx_audio.tts.generate \
  --model ./models/Base \
  --text "Your text here" \
  --ref_audio sample.wav \
  --ref_text "Transcript of the sample" \
  --file_prefix output
```

**Requirements:**
- Apple Silicon Mac (M1/M2/M3/M4)
- ~8GB memory for inference
- Python 3.10+

---

## Accessibility Focus

Voice interfaces aren't just convenient — they're **essential** for many users:

- **Dyslexia** — Voice reduces reading friction
- **Visual impairments** — Audio as primary output
- **Motor limitations** — Hands-free operation
- **Multitasking** — Eyes and hands busy elsewhere

This project prioritizes accessibility alongside capability.

---

## Roadmap

### ✅ Completed
- [x] ElevenLabs TTS integration
- [x] Discord voice channel support
- [x] Audiobook generation pipeline
- [x] Local TTS options (Qwen3, KittenTTS)
- [x] Voice cloning capability

### 🚧 In Progress
- [ ] Deepgram STT streaming integration
- [ ] OpenHome DevKit integration
- [ ] Real-time conversation (VAD + interruption handling)

### 📋 Planned
- [ ] Mobile companion app with push-to-talk
- [ ] Browser-based WebRTC interface
- [ ] Multi-speaker voice scenarios

---

## Real-World Output

**Aporia Audiobook** — 11 chapters, ~3.5 hours of narrated fiction:
- [🎧 Listen on GitHub](https://github.com/marksunner/aporia/releases/tag/audio-chapters-v1)
- Generated using KittenTTS with `expr-voice-3-m` voice
- Demonstrates production-quality local TTS

---

## Configuration

OpenClaw gateway configuration for voice:

```yaml
tools:
  media:
    audio:
      models:
        - provider: elevenlabs
          model: eleven_multilingual_v2
          voice: custom  # or preset voice ID
        - provider: deepgram
          model: nova-3  # for STT
```

---

## Related Projects

- [OpenClaw](https://github.com/openclaw/openclaw) — The AI agent platform
- [Aporia](https://github.com/marksunner/aporia) — Novel with audio chapters
- [OpenHome](https://openhome.com) — Voice AI platform (target)

---

## License

MIT

---

*Built with 🔭 by twins who talk back.* 🕯️
