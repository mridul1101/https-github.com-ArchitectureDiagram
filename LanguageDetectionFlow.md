### Language Detection Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      LANGUAGE DETECTION SYSTEM                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│                           USER SPEAKS                                        │
│                               │                                              │
│                               ▼                                              │
│              ┌────────────────────────────────┐                             │
│              │   Web Speech API Recognition   │                             │
│              │   (using selected language)    │                             │
│              └───────────────┬────────────────┘                             │
│                              │                                              │
│                              ▼                                              │
│              ┌────────────────────────────────┐                             │
│              │      TRANSCRIBED TEXT          │                             │
│              │  "muéstrame las ventas de hoy" │                             │
│              └───────────────┬────────────────┘                             │
│                              │                                              │
│              ┌───────────────┴───────────────┐                              │
│              │      AUTO-DETECT ENABLED?     │                              │
│              └───────────────┬───────────────┘                             │
│                              │                                              │
│              ┌───────────Yes─┴─No────────────┐                              │
│              ▼                               ▼                              │
│   ┌─────────────────────────┐    ┌─────────────────────────┐               │
│   │  LANGUAGE DETECTION     │    │   USE SELECTED LANG     │               │
│   │                         │    │                         │               │
│   │  Check Character Ranges:│    │   Continue with         │               │
│   │  ├─ Chinese: \u4e00-9fff│    │   current language      │               │
│   │  ├─ Japanese: \u3040-30ff    └─────────────────────────┘               │
│   │  ├─ Korean: \uac00-d7af │                                              │
│   │  ├─ Arabic: \u0600-06ff │                                              │
│   │  ├─ Hindi: \u0900-097f  │                                              │
│   │  ├─ Tamil: \u0b80-0bff  │                                              │
│   │  ├─ Telugu: \u0c00-0c7f │                                              │
│   │  └─ etc...              │                                              │
│   │                         │                                              │
│   │  Check Common Words:    │                                              │
│   │  ├─ Spanish: el, la, es │                                              │
│   │  ├─ French: le, la, est │                                              │
│   │  ├─ German: der, die    │                                              │
│   │  └─ etc...              │                                              │
│   └────────────┬────────────┘                                              │
│                │                                                            │
│                ▼                                                            │
│   ┌─────────────────────────┐                                              │
│   │  DETECTED: Spanish 🇪🇸   │                                              │
│   └────────────┬────────────┘                                              │
│                │                                                            │
│                ▼                                                            │
│   ┌─────────────────────────────────────────────────────────┐              │
│   │  LANGUAGE MISMATCH DETECTED?                             │              │
│   │  Selected: en-US  │  Detected: es-ES                     │              │
│   └────────────────────────────┬────────────────────────────┘              │
│                                │                                            │
│                                ▼                                            │
│   ┌─────────────────────────────────────────────────────────┐              │
│   │  AUTO-SWITCH TO DETECTED LANGUAGE                        │              │
│   │  1. Stop current recognition                             │              │
│   │  2. Update selectedLanguage state                        │              │
│   │  3. Speak "Switching language" in new language          │              │
│   │  4. Restart recognition with es-ES                       │              │
│   └─────────────────────────────────────────────────────────┘              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```
