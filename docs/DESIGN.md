# Voice Integration Design Document

## Overview

This document outlines the design for integrating real-time voice capabilities with OpenClaw agents.

## Goals

1. **Natural Interaction** — Voice should feel like talking to a person, not a command system
2. **Low Latency** — Response time under 500ms for acknowledgments
3. **Context Aware** — Maintain conversation context across turns
4. **Privacy First** — Local processing where possible, clear about cloud usage

## Non-Goals (v1)

- Multi-speaker diarization
- Offline-only operation
- Custom wake words (rely on push-to-talk or hardware button)

## Architecture Decisions

### Speech-to-Text
**Decision:** OpenAI Whisper API for v1, with fallback to local whisper.cpp

**Rationale:**
- Whisper API has excellent accuracy
- Low latency (streaming support)
- Cost-effective for conversational use
- Local fallback for privacy-sensitive deployments

### Text-to-Speech
**Decision:** OpenAI TTS with local Piper fallback

**Rationale:**
- OpenAI TTS sounds natural
- Multiple voice options
- Piper provides offline capability

### Voice Activity Detection
**Decision:** Silero VAD (local, real-time)

**Rationale:**
- Runs locally (privacy)
- Low latency
- Accurate pause detection

## Data Flow

```
1. User speaks
2. VAD detects speech start
3. Audio captured and streamed to STT
4. Transcript sent to OpenClaw
5. Response generated (potentially streamed)
6. TTS converts response to audio
7. Audio played to user
8. VAD monitors for interruption
```

## Latency Budget

| Phase | Target | Notes |
|-------|--------|-------|
| VAD detection | <50ms | Local processing |
| STT transcription | <300ms | Streaming helps |
| OpenClaw processing | <1000ms | Depends on model |
| TTS generation | <200ms | Can stream |
| Audio playback | <50ms | Local |
| **Total** | **<1600ms** | For simple queries |

## Open Questions

1. How to handle tool use that takes time? (progressive feedback?)
2. Multi-turn vs. single-turn by default?
3. Personality consistency in voice?

## References

- [OpenHome Voice SDK Docs](https://docs.openhome.com)
- [Whisper API](https://platform.openai.com/docs/guides/speech-to-text)
- [Silero VAD](https://github.com/snakers4/silero-vad)
