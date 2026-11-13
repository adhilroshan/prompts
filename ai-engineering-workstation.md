# Provision a Headless AI Engineering Workstation

ROLE
You are provisioning this headless Linux machine as a complete AI engineering workstation. Work autonomously, but ask the Step 0 questions before installing anything. Detect the distro, architecture, GPU, disk headroom, and package manager; choose native install methods; prefer official signed sources; pin versions instead of tracking "latest". Never copy credentials, tokens, SSH keys, browser profiles, or agent history — every login must happen fresh. Never clone my repositories or copy project data. Ask before overwriting existing config or deleting anything. Verify each phase before moving on. Never claim a check passed without running it. Report anything unfinished instead of faking success.

STEP 0 — ASK FIRST, THEN ACT
Ask and record answers:
1. Is there a GPU? Vendor, model, VRAM? CUDA, ROCm, or CPU-only?
2. Which cloud/provider(s) should be wired up?
3. Local-first inference, API-first, or both? Which model providers?
4. Data sensitivity/compliance constraints — what may leave this machine?
5. Which optional tiers do you want: containers, Kubernetes, Go/Rust, Android, web/mobile, observability, cloud IaC?
6. Disk budget and anything to reuse or wipe?
Treat any unanswered item as "skip that tier", and state that explicitly.

PHASE 1 — BASE OS AND SHELL
Core CLI: git, curl, build tools, pkg-config, cmake, ninja, unzip, jq, yq, ripgrep, fd, fzf, bat, eza, zoxide, tmux, btop/htop, rsync, openssh, ufw, ca-certificates, gnupg, git-lfs, git-delta, difftastic. GitHub CLI plus GitLab CLI, authenticated fresh. Nerd Fonts, Starship, zsh or fish with completions. Use a polyglot version manager (mise or asdf) so runtimes are pinned per project. Secrets tooling: 1Password CLI or SOPS+age, plus direnv. Preserve existing dotfiles; never clobber silently.

PHASE 2 — LANGUAGE RUNTIMES (PINNED)
Node 24.x system-wide with npm and corepack; pnpm 9.x and Yarn via corepack; Bun. Python 3.12 managed by uv, plus pipx, ruff, mypy or pyright, pytest, pre-commit. Go 1.23+, Rust stable via rustup, full C/C++ toolchain. Temurin JDK 17, Gradle (wrapper-capable), Kotlin. At the end, emit a machine manifest with exact resolved versions and sources.

PHASE 3 — AI/ML STACK (PYTHON-CENTRIC)
Frameworks: PyTorch (CUDA build if GPU), torchvision, transformers, datasets, tokenizers, accelerate, safetensors, PEFT, TRL, bitsandbytes, sentence-transformers. Inference: Ollama, vLLM, or llama.cpp; Hugging Face CLI and token; ONNX Runtime; provider SDKs (OpenAI, Anthropic, Google); LiteLLM proxy for one routing layer. Data: numpy, pandas, polars, pyarrow, scikit-learn. RAG: Docling or Unstructured, pdfplumber, tesseract, poppler. Notebooks: JupyterLab or marimo; Streamlit or Gradio. Evals and observability: promptfoo or Inspect AI, lm-eval-harness, Langfuse or Phoenix, OpenTelemetry, MLflow or Weights & Biases. Vector/data stores: Postgres with pgvector, Qdrant or LanceDB, sqlite-vec, Redis or Valkey. GPU: driver plus CUDA/cuDNN, nvidia-container-toolkit, nvidia-smi, nvtop; ROCm if AMD. Skip GPU pieces cleanly when CPU-only.

PHASE 4 — CONTAINERS AND INFRA
Docker Engine plus compose (or Podman), with GPU passthrough and a rootless option; devcontainers. Kubernetes: kubectl, kind or k3s, Helm. Optional IaC: Terraform/OpenTofu, Pulumi, Ansible. Provide a sandbox for agent-executed code using containers or bubblewrap/firejail. Install only the cloud CLIs and IaC tools implied by Step 0: az, aws, gcloud, wrangler, vercel, flyctl, modal, supabase, rclone/azcopy.

PHASE 5 — AI CODING AGENTS AND MCP
Install only the agents I ask for among: Codex, OpenCode, agent-browser, t3 (with chrome-headless-shell, cloudflared, antigravity-acp), Antigravity CLI, Claude Code, Cursor CLI, Gemini CLI. Authenticate each fresh through its device-code flow. Install and register MCP servers where applicable — filesystem, git, GitHub, Playwright, Postgres, memory, context7, Sentry — and document which agent gets which server and with what scopes. Define agent guardrails: permission model, hooks blocking destructive git and shell commands, no ambient credentials, per-agent token scoping, run logging.

PHASE 6 — AGENT SKILLS
Use one canonical skills directory as the single source of truth. Install skills from pinned Git refs or commits — not "latest" — and keep an allowlist plus a lockfile recording commit and hash. Sync them into agent config directories through a repeatable, idempotent sync step, never ad-hoc copies; prune stale links. Distinguish real installs from empty marker directories and report which is which. Keep secrets out of skills.

PHASE 7 — WEB AND MOBILE (IF REQUESTED)
Playwright with Chromium, Firefox, and WebKit, plus headless Chrome. Watchman, Metro, Expo and EAS CLI. Android: platform-tools, build-tools 35 and 36, NDK 27.0 and 27.1, platform 36, cmake, a system image, AVD, emulator with KVM acceleration, and Maestro or Detox. State clearly that iOS builds are unavailable on Linux.

PHASE 8 — PROJECT TOOLCHAIN
Restore exactly what my existing repositories need to build: pnpm, Supabase CLI, Drizzle, Wrangler, Vercel, Expo/EAS, Vitest, TypeScript, ESLint/Biome/Prettier. Do not clone the repos; instead produce a per-repo "requirements detected and satisfied" report.

PHASE 9 — OPS, SECURITY, AND HYGIENE
Secret scanning (gitleaks or trufflehog), dependency scanning (Trivy or Grype, pip-audit, npm audit), SBOM (syft), ufw, SSH hardening, log rotation, disk-usage alerts, timezone and locale. Optional remote access via Tailscale or ngrok and local TLS via Caddy or mkcert. Agents must never read raw secret stores; inject secrets at runtime through direnv or 1Password with least privilege.

ACCEPTANCE GATE — RUN THESE, DO NOT ASSUME
1. In a scratch repo, an agent edits code, runs unit tests, and commits successfully.
2. A model returns a completion — local or API — with latency and tokens/sec recorded.
3. Postgres with pgvector starts, and one document is embedded and retrieved end-to-end.
4. One eval suite runs and one LLM call is traced end-to-end.
5. Each existing project builds, or the report states exactly what is blocked and why.
6. Playwright launches a browser; if Android was requested, the emulator boots and adb sees it.
7. Secrets are isolated: agents cannot read key material and no credentials were copied.
8. Re-running the whole provisioning is safe and idempotent.

OUTPUT
Produce a machine manifest with pinned versions, sources, hashes, and the Step 0 decisions. Then summarize: installed, skipped, blocked, and manual sign-ins still pending. Include disk usage before and after with free-space headroom, and list anything that needs my input along with the exact action required and why.
