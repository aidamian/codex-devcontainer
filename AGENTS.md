# Repository Status
- Primary focus: devcontainer definitions for OpenAI Codex CLI workflows across Ubuntu, Alpine, and slim Python bases.
- Python images ship with CPython 3.13/3.14, Node.js 20, global `uv`, and Git preinstalled; Next.js and Go variants include Python tooling (pip/uv-ready) for auxiliary scripts.
- All images include Git and `openssh-client`, satisfying Codex CLI prerequisites for fetching repositories, managing branches, and pushing over SSH.
- `post-create.sh` runs once to install `@openai/codex` globally via npm.

# Operator Guidance
- Use `uv pip install --system -r requirements.txt` (or equivalent) instead of `pip install`; `uv` is available globally in every Python-oriented image.
- For non-Python images that still depend on Python tooling (Next.js/Go variants), `uv` can be added the same way if a project requires it.
- Devcontainers default to `/workspaces` owned by the `vscode` user (or `root` in the `*-root` images); adjust ownership only if custom mounts demand it.

# Open Items
- Monitor upstream Python 3.14 release candidates for final version tags and refresh the Dockerfiles when the stable build lands.
- Evaluate adding pre-built `uv` caches for common templates once baseline usage patterns emerge.
