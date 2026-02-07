# Gemini Veo - Make.com Custom App

A Make.com custom app for Google Gemini API: text generation, image generation, image editing, and video generation (Veo).

## Repository Structure

This repository mirrors the Make.com custom app local development structure. Each module directory contains separate **files** for every "tab" in the Make Apps Editor (no sub-folders - each block is a flat file within the module directory).

```
src/
├── makecomapp.json                                          # App manifest (origins for VS Code deploy)
├── base.iml.json                                            # Base URL, auth headers, error handling
├── common.iml.json                                          # Common data (shared across modules)
├── groups.json                                              # Module group definitions
│
├── connections/
│   └── gemini-api/
│       ├── gemini-api.communication.iml.json                # Connection validation request
│       └── gemini-api.parameters.iml.json                   # API Key parameter
│
├── modules/
│   ├── generate-content/                                    # [Text Generation] group
│   │   ├── generate-content.communication.iml.json          # POST models/{model}:generateContent
│   │   ├── generate-content.mappable-parameters.iml.json    # Model, prompt, generationConfig
│   │   ├── generate-content.static-parameters.iml.json      # (empty)
│   │   ├── generate-content.interface.iml.json              # Output: text, tokens, finishReason
│   │   └── generate-content.samples.iml.json                # Sample output
│   │
│   ├── generate-image/                                      # [Image Generation] group
│   │   ├── generate-image.communication.iml.json            # POST with responseModalities: IMAGE
│   │   ├── generate-image.mappable-parameters.iml.json      # Model, prompt
│   │   ├── generate-image.static-parameters.iml.json        # (empty)
│   │   ├── generate-image.interface.iml.json                # Output: imageData, imageMimeType
│   │   └── generate-image.samples.iml.json                  # Sample output
│   │
│   ├── edit-image/                                          # [Image Generation] group
│   │   ├── edit-image.communication.iml.json                # POST with inline_data + text prompt
│   │   ├── edit-image.mappable-parameters.iml.json          # Model, prompt, imageMimeType, imageData
│   │   ├── edit-image.static-parameters.iml.json            # (empty)
│   │   ├── edit-image.interface.iml.json                    # Output: edited imageData
│   │   └── edit-image.samples.iml.json                      # Sample output
│   │
│   ├── generate-video/                                      # [Video Generation] group
│   │   ├── generate-video.communication.iml.json            # POST to predictLongRunning (async)
│   │   ├── generate-video.mappable-parameters.iml.json      # Model, prompt, aspect ratio, etc.
│   │   ├── generate-video.static-parameters.iml.json        # (empty)
│   │   ├── generate-video.interface.iml.json                # Output: operationName
│   │   └── generate-video.samples.iml.json                  # Sample output
│   │
│   ├── get-video-status/                                    # [Video Generation] group
│   │   ├── get-video-status.communication.iml.json          # GET operation status
│   │   ├── get-video-status.mappable-parameters.iml.json    # Operation name input
│   │   ├── get-video-status.static-parameters.iml.json      # (empty)
│   │   ├── get-video-status.interface.iml.json              # Output: done, videoUri, state
│   │   └── get-video-status.samples.iml.json                # Sample output
│   │
│   └── make-api-call/                                       # [Other] group
│       ├── make-api-call.communication.iml.json             # Universal/arbitrary API call
│       ├── make-api-call.mappable-parameters.iml.json       # URL, method, headers, body
│       ├── make-api-call.static-parameters.iml.json         # (empty)
│       ├── make-api-call.interface.iml.json                 # Dynamic output
│       └── make-api-call.samples.iml.json                   # Sample output
│
├── rpcs/
│   └── list-models/
│       ├── list-models.communication.iml.json               # GET /models for dropdown
│       └── list-models.parameters.iml.json                  # No input params needed
│
├── webhooks/                                                # (empty - add if needed)
└── functions/                                               # (empty - custom IML functions)
```

## File Naming Convention

Each file maps to a specific **tab** in the Make.com Apps Editor:

| File Suffix | Make Editor Tab | Description |
|---|---|---|
| `.communication.iml.json` | Communication | API request URL, method, body, response mapping |
| `.mappable-parameters.iml.json` | Mappable Parameters | User-facing input fields (mapped in scenario) |
| `.static-parameters.iml.json` | Static Parameters | Module config that doesn't change per execution |
| `.interface.iml.json` | Interface | Output bundle structure (available to next modules) |
| `.samples.iml.json` | Samples | Example output data for the scenario designer |
| `.parameters.iml.json` | Parameters | Connection/RPC input fields |

## How to Use

### Copy-Paste into Make.com Apps Editor

1. Open your app in the [Make Apps Editor](https://www.make.com/en/apps)
2. For each component, copy the JSON from the corresponding file into the matching tab:
   - **Base** tab <- `base.iml.json`
   - **Connections** > new connection > Communication <- `gemini-api.communication.iml.json`
   - **Connections** > Parameters <- `gemini-api.parameters.iml.json`
   - **Modules** > new module > Communication <- `<module>.communication.iml.json`
   - **Modules** > Mappable Parameters <- `<module>.mappable-parameters.iml.json`
   - **Modules** > Static Parameters <- `<module>.static-parameters.iml.json`
   - **Modules** > Interface <- `<module>.interface.iml.json`
   - **Modules** > Samples <- `<module>.samples.iml.json`
   - **RPCs** > new RPC > Communication <- `list-models.communication.iml.json`

### VS Code Deploy (Make Apps Editor Extension)

1. Install the [Make Apps Editor extension](https://marketplace.visualstudio.com/items?itemName=Integromat.apps-sdk)
2. Update `makecomapp.json` with your app ID and API key path
3. Right-click `makecomapp.json` > "Deploy to Make (beta)"

## Modules

| Module | Type | Group | API Endpoint |
|---|---|---|---|
| Generate Content | Action | Text Generation | `POST /models/{model}:generateContent` |
| Generate Image | Action | Image Generation | `POST /models/{model}:generateContent` (with responseModalities) |
| Edit Image | Action | Image Generation | `POST /models/{model}:generateContent` (with inline_data) |
| Generate Video | Action | Video Generation | `POST /models/{model}:predictLongRunning` |
| Get Video Status | Action | Video Generation | `GET /{operationName}` |
| Make an API Call | Universal | Other | User-defined |
