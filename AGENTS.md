# Repository Guidelines

## Project Structure & Module Organization
- `chatter.py`: Gradio UI entrypoint; CLI flags `--host/--port/--share/--public`.
- `chatterbox/src/chatterbox/`: Core logic (`tts.py`, `vc.py`, `utils.py`) and model code under `models/`.
- `data/`: Sample sentences and sets for quick smoke tests.
- `output/`: Generated audio plus `.settings.json/.csv` metadata per run; keep large binaries out of commits.
- `temp/`: Intermediate chunk audio; safe to clear after runs. `settings.json` stores last-used UI state (treat as local).

## Build, Test, and Development Commands
- Current dev baseline: Python 3.11.14.
- Create env (Python 3.11): `python3 -m venv .venv && source .venv/bin/activate`.
- Install deps: `pip install --force-reinstall -r requirements.txt` (fall back to `requirements.base.with.versions.txt` or `requirements_frozen.txt` if needed).
- Run UI locally: `python chatter.py --host 0.0.0.0 --port 7860` (add `--share` for a Gradio share link).
- GPU/CUDA: Torch bundles CUDA libs; ensure FFmpeg is on PATH for audio I/O.

## Coding Style & Naming Conventions
- Follow PEP 8; 4-space indents; snake_case for functions/vars, CapWords for classes, lowercase module filenames.
- Prefer small, composable helpers in `utils.py`; keep side effects explicit (pass paths/context rather than using globals when possible).
- Add concise docstrings and type hints for new public functions; reuse existing logging patterns instead of new frameworks.

## Testing Guidelines
- No automated tests yet; run a manual smoke test before proposing changes:
  1) Launch UI (`python chatter.py --port 7860`).
  2) Use `data/sentences-test.json` or a short text input; generate once.
  3) Confirm audio lands in `output/` with matching `.settings.*` artifacts and without errors in the console.
- For VC changes, also run a small conversion with two short clips and verify the output sample rate matches the model.

## Commit & Pull Request Guidelines
- Commit messages are short, imperative, and lowercase (e.g., `patch whisper model`); group related edits together.
- PRs should include: purpose and scope, setup/commands run, observed outputs (logs or sample file names), and any UI screenshots when UX changes.
- Link issues or todo items when available; call out GPU/FFmpeg requirements or new assets needed to reproduce results.

## Security & Configuration Tips
- Do not commit generated audio, `.settings.json`, or other user-specific artifacts; keep them local or add to `.gitignore` if new.
- Avoid hardcoding paths or credentials; prefer environment variables and document any new config keys.
