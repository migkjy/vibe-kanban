# Vibe Kanban - Development Guide

## Quick Start (Windows)

### Dedicated Ports
- **Frontend**: 5555 (avoid 3000 - commonly used by other projects)
- **Backend**: Auto-assigned (saved to temp file)

### Start Development Server

**Step 1: Start Backend** (with CORS enabled)
```bash
cd "E:\(claude).programming\vibe-kanban"
VK_ALLOWED_ORIGINS="http://localhost:5555" RUST_LOG=debug cargo run --bin server
```

**Step 2: Get Backend Port**
```bash
cat "C:\Users\migkj\AppData\Local\Temp\vibe-kanban\vibe-kanban.port"
```

**Step 3: Start Frontend** (replace XXXXX with backend port)
```bash
cd "E:\(claude).programming\vibe-kanban\frontend"
BACKEND_PORT=XXXXX npx vite --port 5555 --host
```

### One-liner (Bash)
```bash
# Start backend in background, then frontend
cd "E:\(claude).programming\vibe-kanban" && \
VK_ALLOWED_ORIGINS="http://localhost:5555" RUST_LOG=debug cargo run --bin server &
sleep 5 && \
BACKEND_PORT=$(cat "C:\Users\migkj\AppData\Local\Temp\vibe-kanban\vibe-kanban.port") && \
cd frontend && BACKEND_PORT=$BACKEND_PORT npx vite --port 5555 --host
```

## Critical Configuration

### Environment Variables (Required)
| Variable | Purpose | Example |
|----------|---------|---------|
| `VK_ALLOWED_ORIGINS` | CORS whitelist for frontend | `http://localhost:5555` |
| `BACKEND_PORT` | Backend port for Vite proxy | Auto-assigned port number |
| `RUST_LOG` | Rust logging level | `debug` or `info` |

### Common Issues

#### "Connection failed" or "Forbidden"
- **Cause**: `VK_ALLOWED_ORIGINS` not set or mismatched with frontend URL
- **Fix**: Restart backend with correct `VK_ALLOWED_ORIGINS`

#### "Authentication Failed - Forbidden"
- **Cause**: CORS blocking OAuth callback
- **Fix**: Ensure `VK_ALLOWED_ORIGINS` matches exactly (including port)

#### Port already in use
- Check: `netstat -ano | findstr ":5555"`
- Kill: `taskkill //PID <PID> //F`

## Architecture

- **Backend**: Rust (Axum) - Auto-assigns port, writes to temp file
- **Frontend**: React + Vite - Proxies `/api` to backend
- **Auth**: GitHub OAuth via vibekanban.com

## Stopping Servers

```bash
# Find and kill processes
tasklist | findstr "node cargo"
taskkill //PID <PID> //F
```
