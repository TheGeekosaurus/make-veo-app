# Gemini Veo - Make.com Custom App

A Make.com custom app for Google Gemini API video generation (Veo).

## Repository Structure

This repository mirrors the Make.com custom app structure so you can copy-paste each component into the Make.com Apps Editor or deploy via the VS Code Make Apps Editor extension.

```
src/
├── makecomapp.json                                      # App manifest (origins for VS Code deploy)
├── base.iml.json                                        # Base URL, auth headers, error handling
├── common.iml.json                                      # Common data (shared across modules)
├── groups.json                                          # Module group definitions
│
├── connections/
│   └── gemini-api/
│       ├── gemini-api.communication.iml.json            # Connection validation request
│       └── gemini-api.parameters.iml.json               # API Key parameter
│
├── modules/
│   ├── generate-video/
│   │   ├── generate-video.communication.iml.json        # POST to predictLongRunning
│   │   ├── generate-video.parameters.iml.json           # Model, prompt, aspect ratio, etc.
│   │   ├── generate-video.interface.iml.json            # Output: operationName
│   │   └── generate-video.samples.iml.json              # Sample output data
│   │
│   ├── get-video-status/
│   │   ├── get-video-status.communication.iml.json      # GET operation status
│   │   ├── get-video-status.parameters.iml.json         # Operation name input
│   │   ├── get-video-status.interface.iml.json          # Output: done, videoUri, state
│   │   └── get-video-status.samples.iml.json            # Sample output data
│   │
│   └── make-api-call/
│       ├── make-api-call.communication.iml.json         # Universal/arbitrary API call
│       ├── make-api-call.parameters.iml.json            # URL, method, headers, body
│       ├── make-api-call.interface.iml.json             # Dynamic output
│       └── make-api-call.samples.iml.json               # Sample output data
│
├── rpcs/
│   └── list-models/
│       ├── list-models.communication.iml.json           # GET /models for dropdown
│       └── list-models.parameters.iml.json              # No input params needed
│
├── webhooks/                                            # (empty - add if needed)
└── functions/                                           # (empty - custom IML functions)
```

## Components

| Component | Tab in Make Editor | File Suffix |
|---|---|---|
| Base | Base | `base.iml.json` |
| Common Data | Common Data | `common.iml.json` |
| Connection Communication | Communication | `.communication.iml.json` |
| Connection Parameters | Parameters | `.parameters.iml.json` |
| Module Communication | Communication | `.communication.iml.json` |
| Module Mappable Params | Mappable Parameters | `.parameters.iml.json` |
| Module Interface | Interface | `.interface.iml.json` |
| Module Samples | Samples | `.samples.iml.json` |
| RPC Communication | Communication | `.communication.iml.json` |
| RPC Parameters | Parameters | `.parameters.iml.json` |

## How to Use

### Copy-Paste into Make.com

1. Open your app in the [Make Apps Editor](https://www.make.com/en/apps)
2. Copy the contents of each `.iml.json` file into the corresponding tab
3. `base.iml.json` -> **Base** tab
4. Connection files -> **Connections** > create connection > paste into Communication and Parameters tabs
5. Module files -> **Modules** > create module > paste into Communication, Mappable Parameters, Interface, and Samples tabs
6. RPC files -> **RPCs** > create RPC > paste into Communication and Parameters tabs

### VS Code Deploy

1. Install the [Make Apps Editor extension](https://marketplace.visualstudio.com/items?itemName=Integromat.apps-sdk)
2. Update `makecomapp.json` with your app ID and API key path
3. Right-click `makecomapp.json` > "Deploy to Make (beta)"

## Modules

- **Generate Video** (Action) - Initiates async video generation via Veo
- **Get Video Status** (Action) - Polls the operation status and retrieves the video URI
- **Make an API Call** (Universal) - Arbitrary Gemini API call for advanced use cases

## Status

This is the empty infrastructure scaffold. The Gemini API details (request bodies, response mappings) need to be filled in based on the actual API specification.
