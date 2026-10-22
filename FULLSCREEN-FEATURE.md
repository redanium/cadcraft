# Fullscreen Feature for CADCraft Web

## Overview
Fullscreen support has been added to the web version of CADCraft, allowing users to enter browser fullscreen mode for an immersive CAD experience.

## Implementation

### 1. Engine Command (`crates/engine/src/cmd/view.rs`)
- Added `fullscreen` command with three actions:
  - `enter`: Enter fullscreen mode
  - `exit`: Exit fullscreen mode  
  - `toggle`: Toggle fullscreen (default)
- Returns a control message that the web UI layer intercepts

### 2. UI Layer (`crates/ui-egui/src/lib.rs`)
- Added `handle_fullscreen()` function that:
  - Uses the Browser Fullscreen API via `web-sys`
  - Targets the `cadcraft_canvas` element
  - Returns success/failure status with user-friendly messages
- Intercepts fullscreen control messages in the `run()` method

### 3. Menu Integration (`crates/ui-egui/src/menus.rs`)
- Added `ui.fullscreen` command to UI_COMMANDS list
- Menu path: View > Fullscreen
- Keyboard shortcut: F11 (standard fullscreen shortcut)
- Routes to the engine's fullscreen command

### 4. Web Runner (`apps/cadcraft-web/src/main.rs`)
- Added fullscreen change event listener
- Logs fullscreen state changes for debugging
- Prepares for future fullscreen state synchronization

## Usage

### Via Menu
1. Click "View" menu
2. Select "Fullscreen"

### Via Keyboard
- Press **F11** to toggle fullscreen

### Programmatically (Control Channel/MCP)
```json
{"id": 1, "method": "engine.execute", "params": {"command": "fullscreen", "params": {"toggle": true}}}
```

```json
{"id": 2, "method": "engine.execute", "params": {"command": "fullscreen", "params": {"enter": true}}}
```

```json
{"id": 3, "method": "engine.execute", "params": {"command": "fullscreen", "params": {"exit": true}}}
```

## Browser Compatibility
Fullscreen API is supported in all modern browsers:
- Chrome/Edge: ✅
- Firefox: ✅
- Safari: ✅
- Opera: ✅

## Notes
- Fullscreen only works in the web version (WASM build)
- Native desktop builds return a message indicating fullscreen is web-only
- User interaction is required by browsers to enter fullscreen (satisfied by menu/keyboard invocation)
- Pressing Escape exits fullscreen (browser default behavior)

## Future Enhancements
- Add fullscreen indicator in status bar
- Sync fullscreen state with UI state
- Add fullscreen mode preference (true fullscreen vs browser fullscreen)
