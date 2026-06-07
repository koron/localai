# AGENTS.md — localai

This is a bash wrapper around `llama-server` (llama.cpp) that serves GGUF models from HuggingFace.

## Commands

```bash
./bin/localai Qwen3.5-9B            # start serving (foreground)
./bin/localai -d Qwen3.5-9B         # start serving (background, saves PID to /tmp/localai.pid)
./bin/localai -k Qwen3.5-9B         # stop background instance by PID file
./bin/localai -s Qwen3.5-9B         # check if running (prints "running (pid N)" or "stopped")
./bin/localai Qwen3.5-9B -- ...     # extra args appended to llama-server
```

## Layout

- `bin/localai` — entrypoint script
- `localai.d/localai.conf` — global defaults (llama-server path, host, port, tmpdir)
- `localai.d/models/<model>.conf` — per-model config: `user`, `model`, `quant`, `args[]`

## Adding a model

Create `localai.d/models/<name>.conf` with:

```bash
user="huggingface_user"
model="repo-name-GGUF"
quant="UD-Q4_K_XL"
args=(--temp 0.6 --top-p 0.95)
```

The script resolves `hf "${user}/${model}:${quant}"` via llama-server.

## Notes

- `-s` / `-k` need no model config — they only read the PID file
- `-d` prevents double-start (errors if PID file has a live process)
- `-d` writes logs to `/tmp/localai-<YYYYMMDD_HHMMSS>.log`, survives logout via `disown` + stdio detach
- `HF_TOKEN` auto-loaded from `~/.cache/huggingface/token` if present
- `llama-server` must be installed separately (default `/opt/llama.cpp/bin/llama-server`, overridable in `localai.d/localai.conf`)
