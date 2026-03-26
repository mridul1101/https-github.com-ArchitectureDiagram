### VoiceVisualization Component

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     VoiceVisualization Component                             │
│                /src/components/VoiceVisualization.js                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  PROPS                                                                       │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │ listening         : boolean   - Is voice active?                    │    │
│  │ interimTranscript : string    - Real-time speech text              │    │
│  │ audioLevel        : number    - Mic volume (0-100)                  │    │
│  │ voiceEnabled      : boolean   - TTS toggle state                    │    │
│  │ setVoiceEnabled   : function  - Toggle TTS callback                 │    │
│  │ continuousMode    : boolean   - Continuous mode state               │    │
│  │ toggleContinuousMode: function - Toggle continuous callback         │    │
│  │ queryHistory      : array     - Past queries                        │    │
│  │ onHistorySelect   : function  - History item click callback         │    │
│  │ showHelp          : boolean   - Help panel visibility               │    │
│  │ toggleHelp        : function  - Toggle help callback                │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  LAYOUT                                                                      │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │  ┌──────────────────────────────────────────────────────────────┐ │    │
│  │  │                   WAVEFORM DISPLAY                            │ │    │
│  │  │     ▓▓ ▓▓▓ ▓▓▓▓▓ ▓▓▓ ▓▓ ▓▓▓▓ ▓▓▓ ▓▓▓▓▓ ▓▓ ▓▓▓ ▓▓          │ │    │
│  │  │                  (animated bars)                              │ │    │
│  │  └──────────────────────────────────────────────────────────────┘ │    │
│  │                                                                    │    │
│  │  ┌──────────────────────────────────────────────────────────────┐ │    │
│  │  │  INTERIM TEXT BOX (when interimTranscript exists)            │ │    │
│  │  │  🎤 Hearing: "your spoken words appear here..."              │ │    │
│  │  └──────────────────────────────────────────────────────────────┘ │    │
│  │                                                                    │    │
│  │  ┌──────────────────────────────────────────────────────────────┐ │    │
│  │  │  CONTROLS ROW                                                 │ │    │
│  │  │  [☑ Voice Feedback] [☐ Continuous Mode] [📜 History ▼] [❓]  │ │    │
│  │  └──────────────────────────────────────────────────────────────┘ │    │
│  │                                                                    │    │
│  │  ┌──────────────────────────────────────────────────────────────┐ │    │
│  │  │  HISTORY DROPDOWN (when expanded)                            │ │    │
│  │  │  • Previous query 1                                          │ │    │
│  │  │  • Previous query 2                                          │ │    │
│  │  │  • Previous query 3                                          │ │    │
│  │  └──────────────────────────────────────────────────────────────┘ │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---
