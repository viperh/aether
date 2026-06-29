# Aether

A desktop system monitoring and management tool built with Go and React. Aether combines real-time system metrics, an integrated terminal, a file explorer, and a network map into a single frameless application with a cyberpunk-inspired UI.

![Wails](https://img.shields.io/badge/Wails-v2-blue)
![Go](https://img.shields.io/badge/Go-1.23-00ADD8)
![React](https://img.shields.io/badge/React-18-61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-4.6-3178C6)

## Features

- **System Monitoring** — Live CPU (per-core), memory, and disk usage with animated gauges
- **Integrated Terminal** — Multi-tab PowerShell terminal powered by ConPTY and xterm.js
- **File Explorer** — Browse the filesystem with breadcrumb navigation and drive selection
- **Network Map** — Interactive world map showing active outbound connections with GeoIP lookup
- **Frameless Window** — Custom title bar with live clock, hostname, OS info, and uptime

## Tech Stack

| Layer    | Technology                                    |
| -------- | --------------------------------------------- |
| Backend  | Go 1.23, Wails v2                             |
| Frontend | React 18, TypeScript, Vite                    |
| Styling  | Tailwind CSS (custom theme with glow effects) |
| Charts   | Recharts                                      |
| Terminal | xterm.js with WebGL rendering                 |
| Maps     | react-simple-maps                             |
| Icons    | Lucide React                                  |

## Prerequisites

- [Go](https://go.dev/dl/) 1.23+
- [Node.js](https://nodejs.org/) 18+
- [Wails CLI](https://wails.io/docs/gettingstarted/installation)

```bash
go install github.com/wailsapp/wails/v2/cmd/wails@latest
```

## Getting Started

Clone the repository and install frontend dependencies:

```bash
git clone https://github.com/qViperH/aether.git
cd aether
cd frontend && npm install && cd ..
```

### Development

```bash
wails dev
```

This starts the Go backend and Vite dev server with hot reload.

### Production Build

```bash
wails build
```

The compiled binary is output to `build/bin/`.

## Project Structure

```
aether/
├── main.go                     # App entry point & service wiring
├── app.go                      # Lifecycle hooks (startup, domReady, shutdown)
├── internal/
│   ├── sysmon/                 # CPU, memory, disk monitoring
│   ├── terminal/               # ConPTY shell session management
│   ├── network/                # Connection tracking & GeoIP lookup
│   └── fileexplorer/           # Directory listing & file operations
├── frontend/
│   └── src/
│       ├── components/
│       │   ├── layout/         # TopBar
│       │   ├── stats/          # CPU, Memory, Disk gauges
│       │   ├── terminal/       # Multi-tab terminal
│       │   ├── file-explorer/  # File browser
│       │   └── network/        # World map & connection list
│       └── hooks/              # useSystemStats, useNetworkConnections, useTerminal
└── wails.json                  # Wails project config
```

## Architecture

aether follows a service-oriented architecture where each backend module runs as an independent Go service bound to the frontend via Wails RPC.

```
┌─────────────────────────────────────┐
│           TopBar                    │
├────────────┬────────────┬───────────┤
│   File     │  Terminal  │   Stats   │
│  Explorer  │   Panel    │   Panel   │
├────────────┴────────────┴───────────┤
│     Network Map + Connections       │
└─────────────────────────────────────┘
```

Real-time updates are pushed from Go to the frontend via Wails events (`sysmon:stats`, `terminal:output:{id}`, `network:connections`). Each service manages its own goroutine lifecycle with context cancellation and mutex-guarded state.

## Author

**qViperH** — [qviperh.dev@proton.me](mailto:qviperh.dev@proton.me)

## License

MIT
