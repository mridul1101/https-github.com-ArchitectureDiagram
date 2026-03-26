### useVoiceControl Hook Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        useVoiceControl Hook                                  │
│                   /src/hooks/useVoiceControl.js                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │                          STATE                                      │    │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐               │    │
│  │  │ listening    │ │ interimText  │ │ voiceEnabled │               │    │
│  │  │  (boolean)   │ │  (string)    │ │  (boolean)   │               │    │
│  │  └──────────────┘ └──────────────┘ └──────────────┘               │    │
│  │  ┌──────────────┐ ┌──────────────┐                                 │    │
│  │  │ continuousMode│ │ audioLevel  │                                 │    │
│  │  │  (boolean)   │ │  (0-100)     │                                 │    │
│  │  └──────────────┘ └──────────────┘                                 │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │                          REFS                                       │    │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐               │    │
│  │  │ recognition  │ │ audioContext │ │  analyser    │               │    │
│  │  │    Ref       │ │    Ref       │ │    Ref       │               │    │
│  │  └──────────────┘ └──────────────┘ └──────────────┘               │    │
│  │  ┌──────────────┐                                                   │    │
│  │  │ animation    │                                                   │    │
│  │  │    Ref       │                                                   │    │
│  │  └──────────────┘                                                   │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │                        FUNCTIONS                                    │    │
│  │                                                                      │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │  startListening()                                            │   │    │
│  │  │  • Creates SpeechRecognition instance                        │   │    │
│  │  │  • Sets language, interimResults, continuous mode            │   │    │
│  │  │  • Attaches event handlers (onresult, onend, onerror)       │   │    │
│  │  │  • Starts audio visualization                                │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  │                                                                      │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │  stopListening()                                             │   │    │
│  │  │  • Stops recognition                                         │   │    │
│  │  │  • Stops audio visualization                                 │   │    │
│  │  │  • Resets states                                             │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  │                                                                      │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │  startAudioVisualization()                                   │   │    │
│  │  │  • Gets user media (microphone)                              │   │    │
│  │  │  • Creates AudioContext + AnalyserNode                       │   │    │
│  │  │  • Connects microphone to analyser                           │   │    │
│  │  │  • Starts visualization loop                                 │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  │                                                                      │    │
│  │  ┌─────────────────────────────────────────────────────────────┐   │    │
│  │  │  speakText(text)                                             │   │    │
│  │  │  • Creates SpeechSynthesisUtterance                          │   │    │
│  │  │  • Sets rate, pitch, volume                                  │   │    │
│  │  │  • Selects appropriate voice                                 │   │    │
│  │  │  • Speaks the text                                           │   │    │
│  │  └─────────────────────────────────────────────────────────────┘   │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────┐    │
│  │                     RETURNED API                                    │    │
│  │  {                                                                   │    │
│  │    listening,           // Boolean: currently recording?            │    │
│  │    interimTranscript,   // String: real-time speech text           │    │
│  │    voiceEnabled,        // Boolean: TTS enabled?                    │    │
│  │    setVoiceEnabled,     // Function: toggle TTS                     │    │
│  │    continuousMode,      // Boolean: continuous listening?           │    │
│  │    toggleContinuousMode,// Function: toggle continuous              │    │
│  │    audioLevel,          // Number: 0-100 mic volume                 │    │
│  │    startListening,      // Function: begin recording                │    │
│  │    stopListening,       // Function: stop recording                 │    │
│  │    speakText            // Function: text-to-speech                 │    │
│  │  }                                                                   │    │
│  └────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```
