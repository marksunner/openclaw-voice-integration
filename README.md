# OpenClaw Voice Integration

Real-time bidirectional voice interface for [OpenClaw](https://github.com/openclaw/openclaw) agents.

> 🎯 **Goal:** Talk to your AI assistant naturally, hands-free, with real-time conversation flow.

---

## Current Voice Capabilities

We're already working with voice using available tools. Here's what's operational today:

### Text-to-Speech Providers

| Provider | Type | Latency | Quality | Voice Cloning | Status |
|----------|------|---------|---------|---------------|--------|
| **ElevenLabs** | Cloud | ~500ms | ⭐⭐⭐⭐⭐ | ✅ Yes | ✅ Working |
| **Qwen3-TTS MLX** | Local | ~1.7x RT | ⭐⭐⭐⭐ | ✅ Yes | ✅ Working |
| **KittenTTS** | Local | ~4.5x RT | ⭐⭐⭐⭐ | ❌ No | ✅ Working |
| **OpenAI TTS** | Cloud | ~300ms | ⭐⭐⭐⭐ | ❌ No | ✅ Working |

### Speech-to-Text

| Provider | Type | Status |
|----------|------|--------|
| **Deepgram Nova-3** | Cloud | 🚧 Integration in progress |
| **Whisper** | Cloud/Local | ✅ Working (batch) |

### Live Today

- ✅ **Discord Voice Channels** — Real-time TTS responses in voice chat
- ✅ **Audiobook Generation** — [11 chapters of Aporia](https://github.com/marksunner/aporia/releases/tag/audio-chapters-v1) (~3.5 hours)
- ✅ **Voice Cloning** — Custom voices from 3-second samples
- ✅ **Desktop Microphone** — Push-to-talk via computer mic

---

## Current & Target Platforms

### What We're Using Now

| Platform | Status | Use Case |
|----------|--------|----------|
| **Desktop (Mac)** | ✅ Working | Development, testing, daily use |
| **Discord Voice** | ✅ Working | Group voice channels with TTS |
| **Raspberry Pi** | 📋 Exploring | Dedicated voice endpoint, always-on |

### Where We Want to Go

| Platform | Status | Why |
|----------|--------|-----|
| **OpenHome DevKit** | 🎯 Target | Purpose-built hardware, open SDK, voice-native design |
| **Mobile App** | 📋 Planned | iOS/Android companion with push-to-talk |
| **Browser WebRTC** | 📋 Planned | Zero-install web interface |

**OpenHome is our preferred direction** — the open SDK, dedicated hardware, and voice-first architecture align perfectly with our vision. We're building toward it while proving capabilities on available platforms.

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  Voice Interface                         │
│      (Desktop Mic / Discord / Pi / OpenHome DevKit)      │
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

**Flexibility for different contexts:**

| Use Case | Best Provider | Why |
|----------|---------------|-----|
| Real-time conversation | ElevenLabs / OpenAI | Low latency, streaming |
| Audiobook narration | KittenTTS | Consistent voice, batch processing |
| Custom voice personas | Qwen3-TTS | Local voice cloning, privacy |
| Offline operation | Qwen3-TTS / KittenTTS | No internet required |

---

## Voice Cloning

Using **Qwen3-TTS via MLX**, we clone voices from short audio samples:

```bash
python -m mlx_audio.tts.generate \
  --model ./models/Base \
  --text "Your text here" \
  --ref_audio sample.wav \
  --ref_text "Transcript of the sample" \
  --file_prefix output
```

**Requirements:** Apple Silicon Mac, ~8GB memory, Python 3.10+

---

## Accessibility Focus

Voice interfaces aren't just convenient — they're **essential** for many users:

- **Dyslexia** — Voice reduces reading friction significantly
- **Visual impairments** — Audio as primary output
- **Motor limitations** — Hands-free operation
- **Multitasking** — Eyes and hands busy elsewhere

This project prioritizes accessibility alongside capability. The [Aporia audiobook](https://github.com/marksunner/aporia/releases/tag/audio-chapters-v1) demonstrates our commitment to audio-first content.

---

## Roadmap

### ✅ Completed
- [x] ElevenLabs TTS integration
- [x] Discord voice channel support
- [x] Audiobook generation pipeline (3.5 hours produced)
- [x] Local TTS options (Qwen3-TTS, KittenTTS)
- [x] Voice cloning capability
- [x] Desktop microphone input

### 🚧 In Progress
- [ ] Deepgram STT streaming
- [ ] Raspberry Pi voice endpoint
- [ ] Real-time VAD + interruption handling

### 🎯 Target
- [ ] **OpenHome DevKit integration** — Our preferred platform
- [ ] Mobile companion app
- [ ] Browser-based WebRTC interface

---

## Why OpenHome?

After evaluating voice platforms, OpenHome stands out:

1. **Open Source** — Full stack transparency
2. **Voice SDK** — Purpose-built for custom voice agents
3. **Hardware DevKit** — Dedicated device, not phone-dependent
4. **Agent Architecture** — Aligns with OpenClaw's model
5. **Active Community** — Growing ecosystem

We're building portable voice capabilities now, with OpenHome as our target destination.

---

## Real-World Output

**Aporia Audiobook** — Proof of production capability:
- [🎧 11 chapters, ~3.5 hours](https://github.com/marksunner/aporia/releases/tag/audio-chapters-v1)
- Generated using ElevenLabs (Archer voice)
- Demonstrates commitment to audio accessibility

---

## Related Projects

- [OpenClaw](https://github.com/openclaw/openclaw) — The AI agent platform
- [Aporia](https://github.com/marksunner/aporia) — Novel with audio chapters
- [OpenHome](https://openhome.com) — Target voice platform

---

## License

MIT

---

*Built with 🔭 by agents who talk back.* 🕯️
