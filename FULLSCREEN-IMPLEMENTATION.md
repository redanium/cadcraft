# Fullscreen Feature for CADCraft Web

## Overview
Fullscreen support has been added to the web version of CADCraft, allowing users to enter browser fullscreen mode for an immersive CAD experience.

## Implementation

### 1. Control Channel Handler (`crates/ui-egui/src/control.rs`)
- Added `ui.fullscreen` control method that handles fullscreen API calls
- Supports three actions: `enter`, `exit`, and `toggle` (default)
- Uses Browser Fullscreen API via `web-sys` crate
- Compiles only for WASM targets with conditional compilation
- Returns success/failure status with user-friendly messages

### 2. Menu Integration (`crates/ui-egui/src/menus.rs`)
- Added `ui.fullscreen` command to UI_COMMANDS list
- Menu path: View > Fullscreen
- Keyboard shortcut: Ctrl+Shift+F (avoids conflict with F11 OTRACK)
- Routes to the control channel handler

### 3. Web Runner (`apps/cadcraft-web/src/main.rs`)
- Added fullscreen change event listener
- Logs fullscreen state changes for debugging
- Prepares for future fullscreen state synchronization

### 4. Dependencies (`crates/ui-egui/Cargo.toml`)
- Added `wasm-bindgen` and `web-sys` as conditional WASM dependencies
- `web-sys` features: Window, Document, Element, HtmlCanvasElement

## Usage

### Via Menu
1. Click "View" menu
2. Select "Fullscreen"

### Via Keyboard
- Press **Ctrl+Shift+F** to toggle fullscreen

### Programmatically (Control Channel/MCP)
```json
{"id": 1, "method": "ui.fullscreen", "params": {"action": "toggle"}}
```

```json
{"id": 2, "method": "ui.fullscreen", "params": {"action": "enter"}}
```

```json
{"id": 3, "method": "ui.fullscreen", "params": {"action": "exit"}}
```

## Browser Compatibility
Fullscreen API is supported in all modern browsers:
- Chrome/Edge: ✅
- Firefox: ✅
- Safari: ✅
- Opera: ✅

## Notes
- Fullscreen only works in the web version (WASM build)
- Native desktop builds return an error: "Fullscreen is only available in the web version."
- User interaction is required by browsers to enter fullscreen (satisfied by menu/keyboard invocation)
- Pressing Escape exits fullscreen (browser default behavior)
- The fullscreen state is tracked via the `fullscreenchange` event listener

## Future Enhancements
- Add fullscreen indicator in status bar
- Sync fullscreen state with UI state
- Add fullscreen mode preference (true fullscreen vs browser fullscreen)
- Add visual feedback when entering/exiting fullscreen

## Testing
To test the fullscreen feature:
1. Build and serve the web app: `trunk serve --port 9000`
2. Open http://localhost:9000 in a browser
3. Press F11 or navigate to View > Fullscreen
4. Verify the browser enters fullscreen mode
5. Press Escape to exit fullscreen
