# codex-devcontainer
A basic devcontainer that starts with minimal requirements for Python development with OpenAI Codex CLI support.

## Python tooling

- All Python-focused images now install [uv](https://github.com/astral-sh/uv) alongside CPython for faster, deterministic dependency management.
- Prefer `uv pip install --system -r requirements.txt` when working with requirements files or `uv pip install <package>` for ad-hoc packages instead of the legacy `pip install` invocations.
- Classic `pip` binaries remain available for compatibility, but new workflows and examples should favour `uv` going forward.

## Dockerfile variants

- `.devcontainer/Dockerfile.ubuntu-py313`
- `.devcontainer/Dockerfile.ubuntu-py314`
- `.devcontainer/Dockerfile.alpine-py313`
- `.devcontainer/Dockerfile.alpine-py314`
- `.devcontainer/Dockerfile.slim-py314`

Root-as-default-user options mirror the above setups:

- `.devcontainer/Dockerfile.ubuntu-py313-root`
- `.devcontainer/Dockerfile.ubuntu-py314-root`
- `.devcontainer/Dockerfile.alpine-py313-root`
- `.devcontainer/Dockerfile.alpine-py314-root`
- `.devcontainer/Dockerfile.slim-py314-root`

UX/UI focused variants:

- `.devcontainer/Dockerfile.ubuntu-nextjs` — Ubuntu 24.04, Node.js 22 LTS, Corepack (pnpm/yarn), and Storybook/linting tools for rich Next.js prototyping.
- `.devcontainer/Dockerfile.ubuntu-nextjs-root` — Same as above but leaves `root` as the working user.
- `.devcontainer/Dockerfile.ubuntu-go` — Ubuntu 24.04, Go 1.22.5, Node.js 22, Air live reloader, and Wails desktop UI prerequisites.
- `.devcontainer/Dockerfile.ubuntu-go-root` — Ubuntu-based Go setup with root as the default user.
- `.devcontainer/Dockerfile.slim-nextjs` — Node 22 on Debian bookworm-slim with the minimal desktop libs required for Next.js UI tooling.
- `.devcontainer/Dockerfile.slim-nextjs-root` — Slim Next.js image that keeps the root user for CI-style workflows.
- `.devcontainer/Dockerfile.slim-go` — Debian bookworm-slim with Go 1.22.5, Node.js 22, Air, and Wails dependencies trimmed to the essentials.
- `.devcontainer/Dockerfile.slim-go-root` — Slim Go image using root as the default user.
