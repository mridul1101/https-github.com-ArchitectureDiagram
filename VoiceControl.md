# Voice Control Architecture Diagram


```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE                          │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │          SalesDashboard / MultiTableDashboard            │  │
│  │                                                          │  │
│  │  ┌───────────────────────────────────────────────────┐  │  │
│  │  │         VoiceVisualization Component             │  │  │
│  │  │                                                   │  │  │
│  │  │  ┌────────────┐  ┌──────────────────────────┐   │  │  │
│  │  │  │ Waveform   │  │ Voice Controls           │   │  │  │
│  │  │  │ (20 bars)  │  │ ☑ Voice Feedback         │   │  │  │
│  │  │  │            │  │ ☑ Continuous Mode        │   │  │  │
│  │  │  └────────────┘  └──────────────────────────┘   │  │  │
│  │  │                                                   │  │  │
│  │  │  ┌─────────────────────────────────────────┐    │  │  │
│  │  │  │ Interim Transcript Box                  │    │  │  │
│  │  │  │ "Hearing: Show me sales for..."         │    │  │  │
│  │  │  └─────────────────────────────────────────┘    │  │  │
│  │  │                                                   │  │  │
│  │  │  ┌─────────────────────────────────────────┐    │  │  │
│  │  │  │ Query History Dropdown                  │    │  │  │
│  │  │  │ • Previous query 1                      │    │  │  │
│  │  │  │ • Previous query 2                      │    │  │  │
│  │  │  └─────────────────────────────────────────┘    │  │  │
│  │  └───────────────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              ↓
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                      VOICE CONTROL HOOK                         │
│                   (useVoiceControl.js)                          │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │  Speech      │  │   Web Audio  │  │  Speech Synthesis    │  │
│  │  Recognition │  │   API        │  │  (Text-to-Speech)    │  │
│  │              │  │              │  │                      │  │
│  │  • Start     │  │  • Analyzer  │  │  • speakText()       │  │
│  │  • Stop      │  │  • FFT       │  │  • Voice selection   │  │
│  │  • Events    │  │  • Frequency │  │  • Rate/Pitch/Vol    │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
          ↓                    ↓                      ↓
          ↓                    ↓                      ↓
┌─────────────────────────────────────────────────────────────────┐
│                      BROWSER WEB APIs                           │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ Web Speech   │  │ MediaDevices │  │  SpeechSynthesis     │  │
│  │ API          │  │ API          │  │  API                 │  │
│  │              │  │              │  │                      │  │
│  │ Microphone   │  │ Audio Stream │  │  Audio Output        │  │
│  │ Input →      │  │ → Frequency  │  │  ← Spoken Text       │  │
│  │ Text         │  │    Analysis   │  │                      │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

## Data Flow Diagram

```
┌─────────┐
│  USER   │  Speaks: "Show me sales for North region"
└────┬────┘
     │
     ↓
┌────────────────────┐
│  Microphone Input  │
└────────┬───────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  Web Speech API (SpeechRecognition)            │
│  • Captures audio                              │
│  • Converts to text                            │
│  • Sends interim results                       │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  useVoiceControl Hook                          │
│  • Manages state (listening, transcript)       │
│  • Triggers onQuery callback                   │
│  • Manages continuous mode                     │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  Dashboard Component                           │
│  • Receives text: "Show me sales for North..." │
│  • Calls sendQuery(text)                       │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  HTTP POST to Backend                          │
│  POST /nl-query-sales                          │
│  Body: { text: "Show me sales for North..." }  │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  Flask Backend (backendDebate.py)              │
│  • Receives natural language query             │
│  • Sends to Gemini AI                          │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  Gemini 2.5 Flash (AI Model)                   │
│  • Receives schema + query                     │
│  • Generates SQL: SELECT * FROM sales          │
│                    WHERE region = 'North'      │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  SQLite Database                               │
│  • Executes SQL query                          │
│  • Returns rows: [{id:1, product:'A',...}]     │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  Flask Response                                │
│  JSON: { query, sql, rows, count }             │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  Dashboard Component                           │
│  • Displays results in table/chart             │
│  • Adds to query history                       │
│  • Triggers voice feedback                     │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌────────────────────────────────────────────────┐
│  Speech Synthesis (Text-to-Speech)             │
│  speakText("Found 12 results")                 │
└────────┬───────────────────────────────────────┘
         │
         ↓
┌─────────┐
│  USER   │  Hears: "Found 12 results"
└─────────┘  Sees: Data table with 12 rows
```
