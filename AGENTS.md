# AGENTS.md — localai

Bash wrapper for `llama-server` to serve GGUF models from HuggingFace.

## Commands
`./bin/localai <model> [options] [-- extra llama-server args]`

- `-d`: Start in background. PID: `$tmpdir/localai.pid`. Logs: `$tmpdir/localai-<TIMESTAMP>.log`.
- `-k`: Stop background instance via PID file.
- `-s`: Check status (outputs "running (pid N)" or "stopped").

## Configuration
- **Global**: `localai.d/localai.conf` (overrides `llama_server`, `host`, `port`, `tmpdir`).
- **Model-specific**: `localai.d/models/<name>.conf` (requires `user`, `model`, `quant`, and optionally `args[]`).

## Adding a Model
Create `localai.d/models/<name>.conf` with:
```bash
user="huggingface_user"
model="repo-name-GGUF"
quant="UD-Q4_K_XL"
args=(--temp 0.6 --top-p 0.95)
```

## Notes
- `-s` / `-k` only require the PID file, no model config needed.
- `HF_TOKEN` is automatically loaded from `~/.cache/huggingface/token` if present.
- `llama-server` must be installed (default: `/opt/llama.cpp/bin/llama-server`).
