# OGConsole

> An interactive debugging toolkit and canvas inspector for OmniGraffle Automation scripting.

---

## Overview

**OGConsole** is a plug-in for OmniGraffle that provides a structured canvas inventory and registers a set of reusable helper functions directly in the OmniGraffle Automation Console. It is designed to make interactive debugging and scripting exploration as fast and convenient as possible.

Once executed, OGConsole displays a full inventory of the current canvas - shapes, layers, IDs, properties — and registers shorthand commands that can be called directly from the console without parentheses.

---

## Repository Contents

| File | Description |
|---|---|
| `OG.Console.omnijs` | Main plug-in · Canvas inspector + console helper functions |
| `Fetch.IP.to.Canvas.omnijs` | Proof of Concept · Fetches public IP from `api.myip.com` and places it as a text shape on the current canvas |
| `OGConsole_Sandbox.graffle` | Sandbox template · Reference document for exploring OGConsole |

---

## Requirements

- OmniGraffle 7.4 or later (macOS)
- Omni Automation enabled

---

## Installation

1. Download the `.omnijs` file(s)
2. Double-click to install into OmniGraffle
3. The plug-in appears under **Automation → OGConsole**

---

## Usage

### OG.Console.omnijs

Open the OmniGraffle Automation Console (**Automation → Automation Console**), then run the plug-in via the Automation menu. OGConsole will:

1. Display a structured inventory of the current canvas
2. Register all helper functions for the active console session

### Available Console Commands

After running OGConsole, the following commands are available directly in the console:

| Command | Description |
|---|---|
| `h` | Show this help overview |
| `i` | Canvas inventory (short overview) |
| `ig` | Graphic inventory (full shape details) |
| `id` | Document inventory |
| `shapes` | List all shapes on current canvas |
| `shape(n)` | Show all properties of shape at index `n` |
| `shapeProps(n)` | Show all properties incl. prototype chain |
| `sel` | Show currently selected shapes |
| `findByName(str)` | Find shapes whose name contains `str` |
| `findByText(str)` | Find shapes whose text contains `str` |
| `json(n)` | Copy shape `n` as JSON to clipboard |
| `cnvs` | Reference to the current canvas |
| `c` | Clear the console |

> **Note:** Commands without parentheses (`h`, `i`, `ig`, `id`, `sel`, `shapes`, `cnvs`, `c`) are implemented as JavaScript property getters - no parentheses needed.

---

### Fetch.IP.to.Canvas.omnijs · Proof of Concept

This plug-in demonstrates a complete Omni Automation workflow:

1. Sends a `GET` request to `https://api.myip.com`
2. Parses the JSON response
3. Places the public IP address as a styled text shape on the current canvas
4. Copies the result to the clipboard
5. Shows an alert with the result
6. Writes metadata (method, endpoint, timestamp) to the shape's user data

```
API Endpoint:  https://api.myip.com
Response:      { "ip": "x.x.x.x", "country": "...", "cc": "..." }
```

---

## Concepts Demonstrated

- `URL.FetchRequest` - Omni Automation HTTP client
- `JSON.parse` - parsing API responses
- `cnvs.newShape()` - programmatic shape creation
- `shape.setUserData()` - writing key/value metadata to shapes
- `Pasteboard.general.string` - clipboard access
- `Object.defineProperty` with `get` - console getter pattern
- `globalThis` - registering functions for interactive console use

---

## Console Getter Pattern

OGConsole uses JavaScript property getters on `globalThis` to enable calling functions without parentheses in the console:

```javascript
Object.defineProperty(globalThis, 'i', {
    configurable: true,
    get: () => dumpCanvas(cnvs())
})
```

> **Important:** `configurable: true` is required to allow re-registration when the plug-in is executed multiple times in the same session.

---

## Author

**Horst Ritter**
[https://ritter.github.io](https://ritter.github.io)

---

## License

MIT
