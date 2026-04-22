# AETHER Technical Transparency

**Last Updated:** 2026-04-21 22:49 EDT
**System:** Raspberry Pi 5 (8GB RAM)

---

## What Running AETHER Actually Looks Like

### Baseline System State (Idle)

| Metric | Value |
|--------|-------|
| **Total RAM** | 7.9 GB |
| **Used** | 3.7 GB (47%) |
| **Available** | 4.2 GB |
| **Swap Used** | 8 MB (minimal) |
| **Uptime** | 3 days, 9 hours |
| **Load Average** | 0.48 (light load) |

### During Voice Transcription (1-minute audio file)

*Note: Captured during processing of File 179 (12.9-minute audio)*

| Phase | CPU Usage | RAM Usage | Time |
|-------|-----------|-----------|------|
| **Audio Chunking** | 15-25% | +150 MB | 2-5 sec |
| **Whisper Transcription** | 60-80% | +400-600 MB | ~30 sec per chunk |
| **Analysis/Output** | 40-60% | +200 MB | 10-15 sec |
| **Peak During Processing** | ~85% | ~5.2 GB total used | — |

### What This Means

**The Pi 5 is working.** During transcription:
- Fans spin up (audible)
- CPU temp rises (monitored via `vcgencmd measure_temp`)
- System remains responsive but near capacity
- Large files (>2 min) must be chunked to prevent memory pressure

### Why Cloud Augmentation Exists

**Current:** Voice/file/git = Local (Pi 5)
**Reasoning/narrative:** Cloud API (Ollama kimi-k2.5)

**The Pi cannot run both:**
- Whisper transcription
- Local large language model
- Web server
- Database

**Not enough RAM. Not enough CPU. Not enough power.**

The cloud augmentation is not a luxury. It is a necessity given current hardware.

### Future: Full Localization

**Target Hardware:** Beelink SER8 Pro
- 16-32GB RAM
- AMD Ryzen 7
- Runs local LLM + AETHER simultaneously
- Estimated cost: $400-600

**When:** When AETHER revenue justifies the upgrade.

### What You See vs. What Happens

| What Appears | What Actually Runs |
|--------------|-------------------|
| "Send voice memo" | Telegram → Pi → Local storage |
| "Processing..." | Whisper (CPU 60-80%) |
| "Here's your analysis" | Cloud API generates response |
| "Pattern recognized" | Local database query |

### Data Handling (The Private Part)

**What stays on Pi:**
- Raw audio files (encrypted at rest)
- Transcriptions (text only)
- Pattern database
- User preferences

**What goes to cloud:**
- Analysis requests (text, not audio)
- No raw voice data leaves the device
- Cloud provider: Ollama (cloud endpoint)

**Retention:** User-controlled. Delete anytime.

### Performance Metrics (Real)

**Small audio (<2 min):**
- Processing time: 30-45 seconds
- CPU: 60-70%
- RAM spike: +400 MB

**Large audio (>5 min):**
- Must chunk into 90-second segments
- Processing time: 2-3 minutes total
- CPU: 70-85% sustained
- RAM: Managed via sequential processing

**Concurrent users:** 1-2 max on current hardware.

### The Honest Limitation

AETHER on a Pi 5 is:
- ✅ Functional
- ✅ Private (audio never leaves)
- ⚠️ Slow (compared to cloud-only services)
- ⚠️ Limited (1-2 users, not 1000)

This is not enterprise software. This is personal infrastructure.

---

**Built in the open. Running on hardware you can touch.**
**The constraints are real. The privacy is real. The trade-off is clear.**

