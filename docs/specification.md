---
target_repository: NoEasyMark
target_path: docs/specification.md
file_name: specification.md
document_kind: project-specification
authoritative: true
audience: implementing-agent, maintainer, reviewer
related_instruction_file: .github/instructions/pdf-conversion.instructions.md
repository_name_style: "Use `NoEasyMark` for the GitHub repository name and public project styling. Use lowercase `noeasymark` only for implementation identifiers such as the PyPI distribution, Python import package, CLI executable, schema/config field names, and filesystem directories intentionally named after those identifiers."
repository_description: "NoEasyMark is an agent-driven PDF-to-Markdown conversion system with OCR, evidence tracking, quality gates, corpus fixtures, and reviewable deliverables."
repository_visibility: Public
jumpstart_your_project_with_copilot: ""
manual_setup_steps:
  - description: "Create the GitHub repository `franklesniak/NoEasyMark` from the `franklesniak/copilot-repo-template` template using GitHub's `Use this template` flow in the web interface. In the `Use this template` form, set `Repository name` to `NoEasyMark`, set `Description` to the `repository_description` frontmatter value, set `Visibility` to `Public`, and leave the `Jumpstart your project with Copilot` field blank. Do not start from `git clone` of the template repository, because the resulting clone's `origin` would still point at `franklesniak/copilot-repo-template` and the new repository's history would carry every upstream development commit. Leave `Include all branches` unchecked unless the operator has an explicit reason to preserve non-default upstream branches."
    rationale: "The implementing agent works inside a clone of the target repository. The initial GitHub-side repository creation is an operator action, and the target repository must not be a literal clone whose `origin` still points at `franklesniak/copilot-repo-template`."
  - description: "Copy this specification into the new repository at `docs/specification.md` before the implementing agent begins Phase 0."
    rationale: "The implementing agent reads the authoritative spec from inside the target repository so source-of-truth links, agent instructions, markdown link validation, and future template-sync decisions are self-contained in `franklesniak/NoEasyMark`."
  - description: "Confirm repository visibility is Public in Settings → General → Danger Zone."
    rationale: "Required visibility for this project."
  - description: "Confirm the upstream template repository is publicly readable at `https://github.com/franklesniak/copilot-repo-template` before final public handoff. The preferred remediation when the upstream is private is to flip it to public so all NoEasyMark-facing references resolve unchanged. If the operator chooses to keep the upstream private, do not change that repository from the NoEasyMark clone; the implementing agent applies this exact fallback rule: replace every public-facing template hyperlink in `docs/specification.md`, `.github/copilot-instructions.md`, `README.md`, `TEMPLATE_UPDATE_PROCEDURE.md`, `.cursor/rules/repository-instructions.mdc`, `.hermes.md`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, and any other human-facing documentation, setup guide, or handoff file with a non-hyperlinked identifier in the exact form `the upstream template repository franklesniak/copilot-repo-template` (no Markdown link); keep machine-readable template-sync metadata (including `.template-sync/marker.yml`'s full HTTPS `source_repo` URL) intact; and add an operator action to `_TODO_AFTER-BUILD-MANUAL-STEPS.md` to either make the template public or accept the private-provenance posture indefinitely. The fallback rule is mechanical so a public NoEasyMark reader sees no broken hyperlinks regardless of upstream visibility."
    rationale: "NoEasyMark is public. Public readers should not be sent to unactionable links, while the downstream template-sync marker still needs the canonical upstream URL for future maintainer use. Naming the exact replacement form lets the agent apply the fallback deterministically without operator input."
  - description: "Enable Discussions in Settings → General → Features."
    rationale: "Discussions surface is referenced from CONTRIBUTING.md and the issue templates."
  - description: "Enable Issues in Settings → General → Features (default on, confirm)."
    rationale: "Issue templates and routine project issue tracking require Issues to be enabled."
  - description: "Enable private vulnerability reporting in Settings → Security → Code security and analysis → Private vulnerability reporting."
    rationale: "SECURITY.md directs reporters to use this channel rather than email."
  - description: "Enable Dependabot alerts and Dependabot security updates in Settings → Security → Code security and analysis."
    rationale: "Backstops the inherited `dependabot.yml` configuration."
  - description: "Enable secret scanning and push protection in Settings → Security → Code security and analysis."
    rationale: "Prevents accidental credential commits during scaffolding."
  - description: "Create the `triage` issue label (color `#d4c5f9`, description `Needs triage`) in Issues → Labels."
    rationale: "Referenced by the inherited issue templates as the initial state for new issues."
  - description: "Create the build-time issue and PR labels `schema-design-review`, `cross-platform-regression`, and `cross-platform-check` in Issues → Labels. Operator-chosen colors are fine; if creating labels non-interactively, use defaults: `schema-design-review` `#5319e7`, `cross-platform-regression` `#d4c5f9`, `cross-platform-check` `#c2e0c6`."
    rationale: "Referenced by build-time PR templates and label automation; fixed fallback colors let an agent create labels without pausing."
  - description: "Set the default branch to `main` if not already (Settings → Branches)."
    rationale: "Branch automation and CI workflows assume `main`."
  - description: "Confirm GitHub Actions has read+write workflow permissions (Settings → Actions → General → Workflow permissions)."
    rationale: "Phase 0 retains `auto-fix-precommit.yml` by default. That workflow pushes auto-fix commits to its scoped Copilot coding-agent branches and cannot do so without write permission. The setting does not grant push rights to `main`; branch protection still applies."
  - description: "Confirm Git LFS is available for `franklesniak/NoEasyMark`: the repository/account has LFS support and quota, and the repository is not marked as a template repository while it contains LFS-managed files. Verify this by committing and pushing a tiny LFS-tracked test file on a throwaway branch, confirming the push uploads an LFS object, then deleting the throwaway branch and test file. If GitHub rejects the push as LFS-disabled or over quota, correct the repository/account setting or billing/quota condition before uploading the corpus."
    rationale: "Committed PDFs must use Git LFS per the spec; pushing an LFS-tracked file to a repo whose server-side LFS support is disabled fails."
  - description: "After the implementing agent customizes issue templates, manually verify that the `triage` label is auto-applied to new issues; adjust the issue-template labels if not."
    rationale: "Confirms the issue-template label automation is wired through after the template's commented `triage` labels are activated for NoEasyMark."
manual_devcontainer_setup:
  rationale: |
    The implementing agent runs end-to-end inside a devcontainer so it can use its
    highest-trust execution mode (`claude --dangerously-skip-permissions`,
    `codex --dangerously-bypass-approvals-and-sandbox`, or the equivalent flag for whichever agent CLI the operator
    chooses) without touching the host. The devcontainer's filesystem and process
    tree are isolated from the host except for the mounted workspace directory; the
    agent's "skip permissions" flag only relaxes its in-container sandbox, not
    host-side access. This bootstrap devcontainer is the development environment
    until the implementing agent's Phase 0 work commits the canonical
    `.devcontainer/cpu/devcontainer.json` and `.devcontainer/cuda/devcontainer.json`
    variants defined in the **Phase 0** section of the spec body. After Phase 0
    lands, the bootstrap file is removed and the spec-defined variants become the
    long-term development environment.

    The devcontainer is the recommended substrate for the implementing agent's
    initial autonomous run because it isolates "skip permissions" execution from
    the host filesystem. The completed `noeasymark` runtime itself runs host-native
    on supported Linux distributions, supported macOS versions, and supported
    Windows versions per the spec body's **Cross-platform Toolchain Installation
    (Host-Native)** section; operators who do not want to use a devcontainer for
    conversion work may install the toolchain directly on the host using the
    per-OS commands documented there. The devcontainer path below is one valid
    setup for the initial development run; an operator who is comfortable
    installing the toolchain natively (and accepts the broader host-side surface
    that an unsandboxed coding-agent CLI implies) may skip the devcontainer setup
    entirely.
  recommended_sequence: |
    (a) Complete the GitHub-side manual setup steps above. (b) Install the host-side
    devcontainer prerequisites listed below. (c) Clone `franklesniak/NoEasyMark` to
    the host. (d) Copy this specification into the clone at `docs/specification.md`.
    (e) Drop the bootstrap devcontainer file (either `cpu_contents` or
    `cuda_contents` below) into `.devcontainer/devcontainer.json` in the clone.
    (f) Open the clone in VS Code and reopen in the devcontainer. (g) Inside the
    container, authenticate the chosen agent CLI (GitHub access for build-time PR
    work is through the agent's GitHub MCP connector, not `gh`). (h) Optionally complete
    the corpus upload from inside the container per `initial_corpus_upload` below.
    (i) Confirm the agent CLI exposes the expected permission-relaxation flag.
    (j) Launch the agent in autonomous mode against `docs/specification.md`. The
    agent pauses partway through Phase 0 at `SCHEMA_DESIGN_REVIEW`; run the normal
    review loop on the milestone PR, then either submit a GitHub PR review whose
    state is `APPROVED` on the current PR head or merge that recorded milestone PR,
    and re-launch the agent to resume. Marking a draft PR "ready for review" is
    only a stage change and does not satisfy the gate. (k) Once the agent
    has committed `.devcontainer/cpu/devcontainer.json` and
    `.devcontainer/cuda/devcontainer.json`, delete the bootstrap file and rebuild
    against the cpu variant (or cuda if you need GPU).
  host_prerequisites:
    - summary: "Install Docker Desktop (Windows or macOS) or Docker Engine (Linux)."
      windows_command: "winget install -e --id Docker.DockerDesktop"
      macos_command: "brew install --cask docker"
      linux_reference: "https://docs.docker.com/engine/install/"
      verify: "Open a fresh terminal and run `docker version`. Both Client and Server sections must print version information. On Windows, confirm the WSL2 backend is enabled in Docker Desktop > Settings > General."
    - summary: "Install Visual Studio Code."
      windows_command: "winget install -e --id Microsoft.VisualStudioCode"
      macos_command: "brew install --cask visual-studio-code"
      linux_reference: "https://code.visualstudio.com/Download"
      verify: "Run `code --version`. It should print a semantic version, a commit hash, and an architecture string."
    - summary: "Install the VS Code Dev Containers extension."
      command: "code --install-extension ms-vscode-remote.remote-containers"
      verify: "Run `code --list-extensions`. The output should contain `ms-vscode-remote.remote-containers`."
    - summary: "(CUDA bootstrap only) Enable GPU pass-through to containers."
      linux_reference: "Install the NVIDIA Container Toolkit per https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html"
      windows_reference: "Install the latest NVIDIA driver supporting WSL2 (https://www.nvidia.com/Download/index.aspx), then in Docker Desktop > Settings > Resources confirm the WSL2 backend is engaged. GPU pass-through is automatic once both are in place."
      verify: "Run `docker run --rm --gpus all nvidia/cuda:12.4.0-base-ubuntu22.04 nvidia-smi`. The output must include the host GPU details. Skip this prerequisite and use the CPU bootstrap if you do not intend to exercise GPU engines during early development."
  bootstrap_devcontainer:
    location: ".devcontainer/devcontainer.json (top-level; not committed)"
    rationale: |
      VS Code's Dev Containers extension uses `.devcontainer/devcontainer.json`
      automatically when no variant subdirectory exists. Phase 0 of the implementing
      agent's work removes this file and commits the spec-defined
      `.devcontainer/cpu/devcontainer.json` and `.devcontainer/cuda/devcontainer.json`
      variants per the spec body. The bootstrap file is short-lived and is therefore
      not committed.
    choose_one: |
      Pick `cpu_contents` if you have no NVIDIA GPU or do not intend to exercise GPU
      engines during early development. Pick `cuda_contents` if you want the
      bootstrap to mirror the spec body's CUDA variant. You can rebuild the
      container against the other choice later by editing
      `.devcontainer/devcontainer.json` and running "Dev Containers: Rebuild
      Container" in VS Code.
    cpu_contents: |
      {
        "name": "noeasymark-bootstrap-cpu",
        "image": "mcr.microsoft.com/devcontainers/base:ubuntu-24.04",
        "features": {
          "ghcr.io/devcontainers/features/python:1": { "version": "3.13" },
          "ghcr.io/devcontainers/features/node:1": { "version": "lts" },
          "ghcr.io/devcontainers/features/git-lfs:1": {},
          "ghcr.io/devcontainers/features/common-utils:2": {}
        },
        "postCreateCommand": "sudo apt-get update && sudo apt-get install -y --no-install-recommends qpdf mupdf-tools ghostscript tesseract-ocr poppler-utils pandoc build-essential && curl -LsSf https://astral.sh/uv/install.sh | sh && npm install -g @anthropic-ai/claude-code @openai/codex && echo 'export PATH=\"$HOME/.local/bin:$PATH\"' >> ~/.bashrc",
        "remoteUser": "vscode",
        "customizations": {
          "vscode": {
            "extensions": [
              "ms-python.python",
              "ms-python.vscode-pylance",
              "tamasfe.even-better-toml",
              "redhat.vscode-yaml",
              "DavidAnson.vscode-markdownlint",
              "GitHub.vscode-pull-request-github"
            ]
          }
        }
      }
    cuda_contents: |
      {
        "name": "noeasymark-bootstrap-cuda",
        "image": "nvidia/cuda:13.2.1-devel-ubuntu24.04",
        "runArgs": ["--gpus", "all"],
        "features": {
          "ghcr.io/devcontainers/features/python:1": { "version": "3.13" },
          "ghcr.io/devcontainers/features/node:1": { "version": "lts" },
          "ghcr.io/devcontainers/features/git-lfs:1": {},
          "ghcr.io/devcontainers/features/common-utils:2": {}
        },
        "postCreateCommand": "sudo apt-get update && sudo apt-get install -y --no-install-recommends qpdf mupdf-tools ghostscript tesseract-ocr poppler-utils pandoc build-essential && curl -LsSf https://astral.sh/uv/install.sh | sh && npm install -g @anthropic-ai/claude-code @openai/codex && echo 'export PATH=\"$HOME/.local/bin:$PATH\"' >> ~/.bashrc",
        "remoteUser": "vscode",
        "customizations": {
          "vscode": {
            "extensions": [
              "ms-python.python",
              "ms-python.vscode-pylance",
              "tamasfe.even-better-toml",
              "redhat.vscode-yaml",
              "DavidAnson.vscode-markdownlint",
              "GitHub.vscode-pull-request-github"
            ]
          }
        }
      }
    cuda_image_tag_fallback: |
      The CUDA bootstrap names `nvidia/cuda:13.2.1-devel-ubuntu24.04` as an example
      of what the CUDA devel Ubuntu 24.04 stream looks like at the time this
      specification was authored. Before using it, verify the tag with
      `docker manifest inspect nvidia/cuda:13.2.1-devel-ubuntu24.04` or Docker
      Hub's tag listing. NVIDIA periodically retires older tags, so this literal
      may not be live when you run this procedure. If the manifest check fails or
      https://hub.docker.com/r/nvidia/cuda/tags does not return this tag, walk the
      tag list and pick the greatest stable semantic-versioned current
      `*-devel-ubuntu24.04` tag, then record the substitution in your bootstrap
      setup notes. Phase 0 of the implementing agent runs the same discovery
      procedure against the OCI Distribution registry and pins the canonical
      `.devcontainer/cuda/devcontainer.json` to whatever it finds, regardless of
      what this literal says.
  steps:
    - step: 1
      summary: "Confirm the host prerequisites above are installed and verified."
    - step: 2
      summary: "Clone the NoEasyMark repository to your host if you have not already."
      command_powershell: "git clone https://github.com/franklesniak/NoEasyMark.git; Set-Location NoEasyMark"
      command_bash: "git clone https://github.com/franklesniak/NoEasyMark.git && cd NoEasyMark"
      after: "A `NoEasyMark/` directory now exists, and your working directory is inside it."
    - step: 3
      summary: "Copy this specification into the repository at `docs/specification.md`."
      command_powershell: |
        New-Item -ItemType Directory -Path docs -Force | Out-Null
        Copy-Item -Path '..\research-misc\2026\pdf-conversion\pdf-conversion-02b-spec-modified-for-rename.md' -Destination 'docs\specification.md'
      command_bash: |
        mkdir -p docs
        cp ../research-misc/2026/pdf-conversion/pdf-conversion-02b-spec-modified-for-rename.md docs/specification.md
      detail: "Adjust the source path to match where this specification lives on your host. The agent reads `docs/specification.md` as the authoritative spec, so the file must be present before the agent starts."
    - step: 4
      summary: "Create the bootstrap devcontainer file."
      command_powershell: "New-Item -ItemType Directory -Path .devcontainer -Force | Out-Null"
      command_bash: "mkdir -p .devcontainer"
      detail: "Save either `bootstrap_devcontainer.cpu_contents` or `bootstrap_devcontainer.cuda_contents` from this frontmatter section to `.devcontainer/devcontainer.json` exactly as shown. Do not commit this file; Phase 0 will replace it."
    - step: 5
      summary: "Open the repository in the devcontainer."
      detail: "Run `code .` from the NoEasyMark clone. When VS Code opens, press Ctrl+Shift+P (Cmd+Shift+P on macOS), select `Dev Containers: Reopen in Container`, and wait for the build to complete. First-time build typically takes 5-15 minutes; the postCreateCommand installs Python 3.13, uv, image-processing tools, Node, and both agent CLIs (`@anthropic-ai/claude-code` and `@openai/codex`)."
    - step: 6
      summary: "Inside the container terminal, verify the toolchain installed correctly."
      command: "python3 --version && uv --version && git-lfs --version && claude --version && codex --version && tesseract --version && qpdf --version && pandoc --version"
      verify: "Every command should print a version string. If `claude` or `codex` is missing, re-run `npm install -g @anthropic-ai/claude-code @openai/codex` inside the container; if `npm install -g` fails with EACCES, prepend `sudo`."
    - step: 7
      summary: "Confirm the agent's GitHub access (no `gh` needed)."
      detail: "NoEasyMark does not use the `gh` CLI. The implementing agent reaches GitHub for its build-time PR work (phase PRs, the `SCHEMA_DESIGN_REVIEW` gate) through its GitHub MCP connector, configured in the agent platform/settings rather than inside the container. Operator-side GitHub setup (creating the repo, enabling settings, creating labels) is performed in the web interface. The conversion runtime itself performs no GitHub actions."
    - step: 8
      summary: "Authenticate the agent CLI you intend to use."
      claude_code_command: "claude auth login"
      codex_command: "codex login"
      detail: "Follow the OAuth prompt; the browser-based exchange completes through devcontainer port forwarding back to your host. The session credential is stored under `~/.claude/` or `~/.codex/` inside the container only."
    - step: 9
      summary: "(Optional but recommended) Complete the initial corpus upload from inside the container."
      detail: "Follow the `initial_corpus_upload` section of this frontmatter. Git LFS and Git are already installed inside the container; because NoEasyMark does not configure `gh`, make sure Git itself is authenticated for the closing `git push` (a personal access token through the Git credential helper, an SSH remote with a mounted key, or your platform's credential manager) before running the upload — a fresh HTTPS clone in the container has no stored credentials. The corpus-upload procedure adds the binary-asset LFS rules to `.gitattributes` itself in step 5, so it does not depend on any Phase 0 milestone; the implementing agent later treats the existing LFS rules as semantically equivalent and does not duplicate them."
    - step: 10
      summary: "Verify the agent CLI exposes the permission-relaxation flag you intend to use."
      claude_code_command: "claude --help 2>&1 | grep -i dangerously"
      codex_command: "codex --help 2>&1 | grep -i 'dangerously\\|bypass'"
      detail: "Confirm the expected flag name appears in the CLI's own help output before launching the autonomous run. If the installed CLI release has renamed the flag, copy the current equivalent from `--help` and use it in the next step instead of the literal flag shown below. Catching a renamed flag at this point prevents a wasted autonomous run."
    - step: 11
      summary: "Launch the implementing agent in autonomous mode."
      claude_code_invocation: |
        # In the container terminal:
        claude --dangerously-skip-permissions
        # Inside the Claude Code REPL, type:
        /goal Implement the NoEasyMark project per docs/specification.md, working from Phase 0 forward. Stop only at named blocking gates or unrecoverable errors per the spec. Phase 0 itself contains the SCHEMA_DESIGN_REVIEW gate: when you reach it, exit cleanly so the operator can run the review loop and then either submit an APPROVED GitHub PR review on the current milestone-PR head or merge the recorded milestone PR before you resume the remaining Phase 0 work and the later phases that consume load-bearing schemas.
      codex_invocation: |
        # In the container terminal:
        codex exec --dangerously-bypass-approvals-and-sandbox "Implement the NoEasyMark project per docs/specification.md, working from Phase 0 forward. Stop only at named blocking gates or unrecoverable errors per the spec. Phase 0 itself contains the SCHEMA_DESIGN_REVIEW gate: when you reach it, exit cleanly so the operator can run the review loop and then either submit an APPROVED GitHub PR review on the current milestone-PR head or merge the recorded milestone PR before you resume the remaining Phase 0 work and the later phases that consume load-bearing schemas."
      detail: "Both invocations relax the agent's permission prompts within the devcontainer only; the host filesystem and other host processes remain isolated. If step 10 reported a renamed flag, substitute the current flag name from the CLI's help output."
    - step: 12
      summary: "Handle the SCHEMA_DESIGN_REVIEW gate when the agent pauses mid-Phase-0."
      detail: "Partway through Phase 0, the agent opens the milestone schema PR as ready for review (not draft), writes `.implementation-state/gates/schema-design-review.json` with `status: pending` plus the PR URL, and returns control to the operator. Headless runtimes that can set a process status use the development-time gate exit code defined in the spec body; interactive runtimes stop work and leave the session at the prompt or exit normally. The agent does not loop or poll while the gate is open. In GitHub, open the milestone PR, run the normal review loop (including Claude/Copilot/human review comments and follow-up commits pushed to the PR branch), then do one of two gate-satisfying actions: submit a GitHub PR review whose state is `APPROVED` after the latest PR-head commit, or merge the recorded milestone PR into its expected base branch. A `CHANGES_REQUESTED` review keeps the gate open; the agent will address it on the same PR branch when relaunched. Marking a draft PR `Ready for review` only requests/permits review and does not approve the gate. Once the approval review or merge is recorded, re-run the same launch command from step 11. On resumption, the agent reads the gate state file, queries the PR through its GitHub MCP connector, fetches the remote branch or merged base, fast-forwards its local checkout, verifies the local `HEAD` equals the current PR head SHA for an open PR (or the fetched expected base-branch tip for a merged PR), updates the gate file, and continues with the remaining Phase 0 work and subsequent phases. Conversion-time gates do not fire during implementation. Stop the agent at any time with Ctrl+C inside the container terminal; resume by re-running the launch command. The agent reads its own state from the work queue and resumes deterministically."
    - step: 13
      summary: "Switch to the canonical devcontainer variant once the agent has committed it."
      command_powershell: |
        # Run from your HOST shell, not inside the container, after running `git pull` and
        # confirming `.devcontainer/cpu/devcontainer.json` and `.devcontainer/cuda/devcontainer.json`
        # exist in your local clone:
        Remove-Item .devcontainer\devcontainer.json
      command_bash: |
        # Run from your HOST shell, not inside the container, after running `git pull` and
        # confirming `.devcontainer/cpu/devcontainer.json` and `.devcontainer/cuda/devcontainer.json`
        # exist in your local clone:
        rm .devcontainer/devcontainer.json
      detail: "The implementing agent commits `.devcontainer/cpu/devcontainer.json` and `.devcontainer/cuda/devcontainer.json` to its current phase branch (typically `implementation/phase-00-template-adoption` or its successor) as part of Phase 0 devcontainer adoption. Once those two files appear in your local clone after a `git pull`, the bootstrap file is no longer needed and you can remove it. With the top-level bootstrap file removed, VS Code's `Dev Containers: Reopen in Container` shows both variants as explicit choices. Pick `cpu` for further development unless an active tool profile requires GPU engines. You do not need to wait for the Phase 0 schema PR or any other Phase 0 PR to merge to `main` before doing this; the local presence of the two committed variant files is what matters."
  notes:
    - "The bootstrap devcontainer file is not committed. Phase 0 commits the canonical cpu/ and cuda/ variants per the spec body."
    - "`claude --dangerously-skip-permissions` and `codex --dangerously-bypass-approvals-and-sandbox` (or the equivalent flag in your installed agent CLI version) only loosen permissions within the container. The host filesystem and other host processes remain isolated; the only host-visible surface is the mounted workspace folder. If the installed agent CLI has renamed these flags, prefer the documented current help output from `claude --help` or `codex --help` over the examples here."
    - "The container can reach the public internet for API calls (Anthropic, OpenAI, GitHub, package registries). Restrict outbound access through your host firewall if your network policy requires it."
    - "If you use the CUDA bootstrap and GPU pass-through fails, `noeasymark doctor --gpu` (which the agent runs during Phase 0) reports the failure with an actionable message and the agent continues with CPU-only engines until the active tool profile requires a GPU engine."
    - "The npm-installed agent CLIs update independently of the bootstrap image. Re-run `npm install -g @anthropic-ai/claude-code @openai/codex` inside the container periodically to pick up new agent releases without rebuilding the container."
    - "If the bootstrap image's apt packages or NVIDIA CUDA tag have shifted by the time you run this procedure, accept the freshest equivalents and record the substitution; Phase 0 re-verifies image tags against the spec body's defaults."
initial_corpus_upload:
  rationale: |
    The ComEd Blue Book 2025, ComEd DER Interconnection Guidelines, and ComEd Service and Meter
    Requirements 2025 serve as both pilot corpus and gold-set source material for the first
    NoEasyMark runs. They live in the `corpus/` directory of this repository so they are
    version-controlled, auditable, and available to the implementing agent without requiring an
    operator-supplied local path. Because committed PDFs must use Git LFS per the spec, the
    upload procedure begins with Git LFS setup.
  recommended_sequence: |
    The recommended sequence is to (a) complete the manual GitHub setup steps listed above,
    (b) install Git LFS and add the LFS rules to `.gitattributes` yourself per step 5 below
    (the idempotent command skips the append when the PDF LFS rule is already present, and
    the implementing agent will detect existing rules during Phase 0 and treat them as
    semantically equivalent rather than duplicate-append), (c) perform the rest of the corpus
    upload steps below. Adding the LFS rules manually up front decouples the corpus upload
    from any Phase 0 milestone or schema-design pause, so the corpus is ready as soon as the
    agent starts conversion work.
  prerequisites:
    - summary: "Install Git LFS on the local machine (one-time, per machine)."
      windows_command: "winget install -e --id GitHub.GitLFS"
      alternative: "Or download the installer from https://git-lfs.com and run it."
      verify: "Open a fresh terminal and run `git lfs version`. It should print a version string such as `git-lfs/3.5.1 (GitHub; windows amd64; go 1.21.5)`. If the command is not found, restart the terminal (the installer modifies PATH) and try again."
    - summary: "Enable Git LFS in your global Git config (one-time, per machine)."
      command: "git lfs install"
      verify: "Should print `Git LFS initialized.`. Safe to run more than once."
    - summary: "Confirm pre-commit is available for the corpus-upload commits."
      command: "pre-commit --version"
      alternative: "If the command is missing, install it using the template repository's documented contributor setup, then open a fresh terminal and re-run `pre-commit --version`."
      verify: "The command should print the installed pre-commit version. Every corpus-upload commit below runs `pre-commit run --all-files` before committing, and any auto-fixes are included in the same commit."
    - summary: "Confirm repository-side Git LFS support before the first corpus push."
      command: "After the `.gitattributes` LFS rules exist, create a throwaway branch, add a tiny file matching an LFS-tracked pattern, commit it, push it, and confirm the push prints LFS upload progress. Then delete the throwaway branch and test file."
      alternative: "If you cannot perform a test push yet because the repository has not been created or permissions are not in place, defer corpus upload until after the repository exists and this test has passed."
      verify: "The test push should upload an LFS object and the committed file should appear as a Git LFS pointer in Git history. Do not proceed with the corpus upload if GitHub reports LFS disabled, LFS unsupported because the repository is still marked as a template repository, or quota exhaustion."
  corpus_directory_layout: |
    Each corpus entry is uploaded as a per-document directory containing the source PDF
    plus an optional `reference/` subdirectory that mirrors the operator-curated reference
    materials from the source repository (hand-converted Markdown and extracted images).
    The reference materials are not auto-imported as gold-set entries — the spec keeps the
    operator-reviewed gold-set gate at `PILOT_GLOSSARY_REVIEW` — but having the reference
    materials sitting next to the source PDF makes pre-filling the gold-set forms much
    faster than cross-referencing a separate repository. The target structure is:

      corpus/
        <output-folder-name>/
          source.pdf                       (renamed from the source repository's PDF)
          reference/                       (optional; copied verbatim from source repo)
            README.md
            markdown/
              *.md
            images/
              *.jpeg | *.png
  upload_targets:
    - source_repository: "franklesniak/lesniak-residence-construction-plan"
      source_directory: "references/codes-and-standards/comed-blue-book-2025/"
      source_pdf_filename: "original/ComEd_Bluebook_2026_0105.pdf"
      noeasymark_target_directory: "corpus/comed-blue-book-2025/"
      noeasymark_source_pdf_target: "corpus/comed-blue-book-2025/source.pdf"
      noeasymark_reference_target: "corpus/comed-blue-book-2025/reference/"
      noeasymark_document_title_at_init: "ComEd Blue Book 2025"
      title_provenance_note: |
        The 1st edition of the ComEd Blue Book is dated July 2025; the source
        repository's README confirms this. The `_2026_0105` segment of the
        PDF filename is a ComEd-side revision/download token and is not part
        of the document's edition or title. The canonical title at init is
        therefore "ComEd Blue Book 2025" and the canonical folder name is
        `comed-blue-book-2025`.
    - source_repository: "franklesniak/lesniak-residence-construction-plan"
      source_directory: "references/codes-and-standards/comed-der-interconnection-guidelines/"
      source_pdf_filename: "original/DER_Interconnection_Guidelines_for_Customers_2024-v3-fixed.pdf"
      noeasymark_target_directory: "corpus/comed-der-interconnection-guidelines/"
      noeasymark_source_pdf_target: "corpus/comed-der-interconnection-guidelines/source.pdf"
      noeasymark_reference_target: "corpus/comed-der-interconnection-guidelines/reference/"
      noeasymark_document_title_at_init: "ComEd DER Interconnection Guidelines"
    - source_repository: "franklesniak/lesniak-residence-construction-plan"
      source_directory: "references/codes-and-standards/comed-service-and-meter-requirements-2025/"
      source_pdf_filename: "original/service_and_meter_requirements.pdf"
      noeasymark_target_directory: "corpus/comed-service-and-meter-requirements-2025/"
      noeasymark_source_pdf_target: "corpus/comed-service-and-meter-requirements-2025/source.pdf"
      noeasymark_reference_target: "corpus/comed-service-and-meter-requirements-2025/reference/"
      noeasymark_document_title_at_init: "ComEd Service and Meter Requirements 2025"
  steps:
    - step: 1
      summary: "Confirm Git LFS prerequisites are done."
      detail: "If you skipped the prerequisites above, do them now. `git lfs version` must print a version and `git lfs install` must have been run at least once on this machine."
    - step: 2
      summary: "Clone the new NoEasyMark repository locally (after creating it from the copilot-repo-template on GitHub)."
      command_powershell: "git clone https://github.com/franklesniak/NoEasyMark.git"
      command_bash: "git clone https://github.com/franklesniak/NoEasyMark.git"
      after: "A `NoEasyMark/` directory now exists in your current working directory."
    - step: 3
      summary: "Change into the cloned directory."
      command_powershell: "Set-Location NoEasyMark"
      command_bash: "cd NoEasyMark"
    - step: 4
      summary: "Initialize Git LFS for this clone (one-time, per clone)."
      command_powershell: "git lfs install --local"
      command_bash: "git lfs install --local"
      verify: "Should print `Git LFS initialized.`."
    - step: 5
      summary: "Add any missing binary-asset LFS rules to `.gitattributes` (each rule is added only when absent so a pre-existing `*.pdf` rule does not suppress the image rules), run pre-commit, then commit and push only when a rule was actually added, before staging any corpus files. This operational copy matches the canonical Phase 0 `.gitattributes` binary-asset LFS block, covering PDFs and the image formats Phase 7 generates plus those commonly present in reference materials. When the implementing agent later runs Phase 0, it detects these existing rules as semantically equivalent and does not duplicate them."
      command_powershell: |
        $lfsRules = @(
          '*.pdf filter=lfs diff=lfs merge=lfs -text'
          '*.png filter=lfs diff=lfs merge=lfs -text'
          '*.jpg filter=lfs diff=lfs merge=lfs -text'
          '*.jpeg filter=lfs diff=lfs merge=lfs -text'
          '*.gif filter=lfs diff=lfs merge=lfs -text'
          '*.tiff filter=lfs diff=lfs merge=lfs -text'
          '*.tif filter=lfs diff=lfs merge=lfs -text'
          '*.webp filter=lfs diff=lfs merge=lfs -text'
          '*.bmp filter=lfs diff=lfs merge=lfs -text'
        )
        if (-not (Test-Path -Path .gitattributes -PathType Leaf)) {
          New-Item -ItemType File -Path .gitattributes | Out-Null
        }
        $existing = @(Get-Content -Path .gitattributes)
        foreach ($rule in $lfsRules) {
          if ($existing -notcontains $rule) {
            Add-Content -Path .gitattributes -Value $rule
          }
        }
        git add .gitattributes
        pre-commit run --all-files
        if ($LASTEXITCODE -ne 0) {
          git add -u
          pre-commit run --all-files
          if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
        }
        git add -u
        git diff --cached --quiet
        if ($LASTEXITCODE -ne 0) {
          git commit -m 'Track binary assets with Git LFS'
          if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
          git push
        } else {
          Write-Host 'Binary-asset LFS rules already present; nothing to commit.'
        }
      command_bash: |
        set -e
        touch .gitattributes
        for rule in \
          '*.pdf filter=lfs diff=lfs merge=lfs -text' \
          '*.png filter=lfs diff=lfs merge=lfs -text' \
          '*.jpg filter=lfs diff=lfs merge=lfs -text' \
          '*.jpeg filter=lfs diff=lfs merge=lfs -text' \
          '*.gif filter=lfs diff=lfs merge=lfs -text' \
          '*.tiff filter=lfs diff=lfs merge=lfs -text' \
          '*.tif filter=lfs diff=lfs merge=lfs -text' \
          '*.webp filter=lfs diff=lfs merge=lfs -text' \
          '*.bmp filter=lfs diff=lfs merge=lfs -text'; do
          grep -qxF "$rule" .gitattributes || printf '%s\n' "$rule" >> .gitattributes
        done
        git add .gitattributes
        pre-commit run --all-files || {
          git add -u
          pre-commit run --all-files
        }
        git add -u
        if git diff --cached --quiet; then
          echo 'Binary-asset LFS rules already present; nothing to commit.'
        else
          git commit -m 'Track binary assets with Git LFS'
          git push
        fi
    - step: 6
      summary: "Materialize the source PDFs in your local lesniak-residence-construction-plan clone."
      detail: "Git LFS stores small pointer files in the repository and downloads the actual binary blobs on demand. If you do not have the real PDF bytes on disk yet, run `git lfs pull` inside your local lesniak-residence-construction-plan clone. Each ComEd PDF should grow from ~132 bytes (pointer) to its real size (several MB). Adjust the source-repository path if your clone is not a sibling of the NoEasyMark clone; both commands leave the current shell in the NoEasyMark clone."
      command_powershell: "Push-Location ..\\lesniak-residence-construction-plan; git lfs pull; Pop-Location"
      command_bash: "(cd ../lesniak-residence-construction-plan && git lfs pull)"
    - step: 7
      summary: "Create the corpus directory tree in the NoEasyMark repo (one parent and three per-document directories, each with an optional reference-materials subdirectory)."
      command_powershell: |
        New-Item -ItemType Directory -Path corpus\comed-blue-book-2025 -Force | Out-Null
        New-Item -ItemType Directory -Path corpus\comed-blue-book-2025\reference -Force | Out-Null
        New-Item -ItemType Directory -Path corpus\comed-der-interconnection-guidelines -Force | Out-Null
        New-Item -ItemType Directory -Path corpus\comed-der-interconnection-guidelines\reference -Force | Out-Null
        New-Item -ItemType Directory -Path corpus\comed-service-and-meter-requirements-2025 -Force | Out-Null
        New-Item -ItemType Directory -Path corpus\comed-service-and-meter-requirements-2025\reference -Force | Out-Null
      command_bash: |
        mkdir -p corpus/comed-blue-book-2025/reference
        mkdir -p corpus/comed-der-interconnection-guidelines/reference
        mkdir -p corpus/comed-service-and-meter-requirements-2025/reference
    - step: 8
      summary: "Copy each ComEd PDF into its per-document directory as `source.pdf`. Adjust the source path prefix to match where your lesniak-residence-construction-plan clone lives on disk."
      command_powershell: |
        Copy-Item -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-blue-book-2025\original\ComEd_Bluebook_2026_0105.pdf' -Destination 'corpus\comed-blue-book-2025\source.pdf'
        Copy-Item -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-der-interconnection-guidelines\original\DER_Interconnection_Guidelines_for_Customers_2024-v3-fixed.pdf' -Destination 'corpus\comed-der-interconnection-guidelines\source.pdf'
        Copy-Item -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-service-and-meter-requirements-2025\original\service_and_meter_requirements.pdf' -Destination 'corpus\comed-service-and-meter-requirements-2025\source.pdf'
      command_bash: |
        cp ../lesniak-residence-construction-plan/references/codes-and-standards/comed-blue-book-2025/original/ComEd_Bluebook_2026_0105.pdf corpus/comed-blue-book-2025/source.pdf
        cp ../lesniak-residence-construction-plan/references/codes-and-standards/comed-der-interconnection-guidelines/original/DER_Interconnection_Guidelines_for_Customers_2024-v3-fixed.pdf corpus/comed-der-interconnection-guidelines/source.pdf
        cp ../lesniak-residence-construction-plan/references/codes-and-standards/comed-service-and-meter-requirements-2025/original/service_and_meter_requirements.pdf corpus/comed-service-and-meter-requirements-2025/source.pdf
      alternative: "If you do not have a local clone of lesniak-residence-construction-plan, download each PDF directly from ComEd's public site or use the source repository paths in upload_targets above: open the listed `source_repository`, navigate to the listed `source_directory`, select the listed `source_pdf_filename`, choose 'Download raw file' in the GitHub UI, and save into the appropriate `corpus/<output-folder-name>/source.pdf` location."
    - step: 9
      summary: "Copy each document's reference materials (the operator-curated hand-converted Markdown and extracted images) from the source repository into the per-document `reference/` subdirectory. This step is optional but strongly recommended: the reference materials make gold-set seeding at `PILOT_GLOSSARY_REVIEW` much faster than cross-referencing a separate repository."
      command_powershell: |
        Copy-Item -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-blue-book-2025\README.md' -Destination 'corpus\comed-blue-book-2025\reference\README.md' -Force
        Copy-Item -Recurse -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-blue-book-2025\markdown'  -Destination 'corpus\comed-blue-book-2025\reference\markdown' -Force
        Copy-Item -Recurse -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-blue-book-2025\images'    -Destination 'corpus\comed-blue-book-2025\reference\images' -Force
        Copy-Item -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-der-interconnection-guidelines\README.md' -Destination 'corpus\comed-der-interconnection-guidelines\reference\README.md' -Force
        Copy-Item -Recurse -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-der-interconnection-guidelines\markdown'  -Destination 'corpus\comed-der-interconnection-guidelines\reference\markdown' -Force
        Copy-Item -Recurse -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-der-interconnection-guidelines\images'    -Destination 'corpus\comed-der-interconnection-guidelines\reference\images' -Force
        Copy-Item -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-service-and-meter-requirements-2025\README.md' -Destination 'corpus\comed-service-and-meter-requirements-2025\reference\README.md' -Force
        Copy-Item -Recurse -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-service-and-meter-requirements-2025\markdown'  -Destination 'corpus\comed-service-and-meter-requirements-2025\reference\markdown' -Force
        Copy-Item -Recurse -Path '..\lesniak-residence-construction-plan\references\codes-and-standards\comed-service-and-meter-requirements-2025\images'    -Destination 'corpus\comed-service-and-meter-requirements-2025\reference\images' -Force
      command_bash: |
        mkdir -p corpus/comed-blue-book-2025/reference corpus/comed-der-interconnection-guidelines/reference corpus/comed-service-and-meter-requirements-2025/reference
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-blue-book-2025/README.md corpus/comed-blue-book-2025/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-blue-book-2025/markdown corpus/comed-blue-book-2025/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-blue-book-2025/images corpus/comed-blue-book-2025/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-der-interconnection-guidelines/README.md corpus/comed-der-interconnection-guidelines/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-der-interconnection-guidelines/markdown corpus/comed-der-interconnection-guidelines/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-der-interconnection-guidelines/images corpus/comed-der-interconnection-guidelines/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-service-and-meter-requirements-2025/README.md corpus/comed-service-and-meter-requirements-2025/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-service-and-meter-requirements-2025/markdown corpus/comed-service-and-meter-requirements-2025/reference/
        cp -r ../lesniak-residence-construction-plan/references/codes-and-standards/comed-service-and-meter-requirements-2025/images corpus/comed-service-and-meter-requirements-2025/reference/
      alternative: "Skip this step if you do not have hand-curated reference materials for these documents. Gold-set construction at `PILOT_GLOSSARY_REVIEW` will fall back to the spec's standard review-packet workflow."
    - step: 10
      summary: "Stage the new corpus files."
      command_powershell: "git add corpus/"
      command_bash: "git add corpus/"
    - step: 11
      summary: "Verify Git LFS is tracking every binary asset (PDFs and images); this is the authoritative LFS-tracking check."
      command_powershell: "git lfs ls-files"
      command_bash: "git lfs ls-files"
      verify: "Output should list all three `corpus/<output-folder-name>/source.pdf` entries and every `corpus/<output-folder-name>/reference/images/*.{png,jpg,jpeg,gif,tiff,tif,webp,bmp}` entry that was copied in step 9, each shown with an LFS object hash prefix. Reference Markdown files (`*.md`) and the reference `README.md` are not tracked by LFS — they are text and appear as normal Git blobs. If a PDF or image is missing from the output, the .gitattributes rules were not active when you ran `git add` — re-check step 5 and re-run from step 10 after fixing the rules."
    - step: 12
      summary: "Sanity-check staged PDF and image diff sizes after the authoritative `git lfs ls-files` check."
      command_powershell: "git diff --cached --stat"
      command_bash: "git diff --cached --stat"
      verify: "This is a size sanity check, not the authoritative LFS-tracking check. Each `corpus/<output-folder-name>/source.pdf` entry and each `corpus/<output-folder-name>/reference/images/*` entry should show a tiny diff (a few lines of LFS pointer text), not a multi-megabyte binary diff. Reference Markdown files appear as ordinary diffs sized appropriately for their content. If you see a multi-megabyte diff on any binary asset, stop, run `git reset corpus/`, and re-check step 5."
    - step: 13
      summary: "Run pre-commit, commit, and push."
      command_powershell: |
        pre-commit run --all-files
        if ($LASTEXITCODE -ne 0) {
          git add -u
          git add corpus/
          pre-commit run --all-files
          if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
        }
        git add -u
        git add corpus/
        git commit -m 'Add initial gold-set corpus (ComEd documents and reference materials)'
        if ($LASTEXITCODE -ne 0) { exit $LASTEXITCODE }
        git push
      command_bash: |
        set -e
        pre-commit run --all-files || {
          git add -u
          git add corpus/
          pre-commit run --all-files
        }
        git add -u
        git add corpus/
        git commit -m 'Add initial gold-set corpus (ComEd documents and reference materials)'
        git push
      verify: "The push should display LFS upload progress for each `source.pdf` file. If LFS uploads do not appear, the rule did not apply — see step 11."
  notes:
    - "After the corpus is committed, the implementing agent (or you) runs `noeasymark init` once per document, providing the per-document `source.pdf` path, document title from upload_targets, and an explicit output-folder-name override matching the corpus directory. For example: `noeasymark init --source corpus/comed-blue-book-2025/source.pdf --title \"ComEd Blue Book 2025\" --output-folder-name comed-blue-book-2025`. The override is required for the initial corpus so the reviewable `<output_root>/<output-folder-name>/` workspace, folder-scoped approvals, and adjacent `corpus/<output-folder-name>/reference/` helper materials share the same stable document handle even if a title-derived slug would collide or change after a transliteration-library update."
    - "If a `corpus/<output-folder-name>/reference/` directory exists for the document, the implementing agent surfaces its contents at `PILOT_GLOSSARY_REVIEW` so the operator can use them as pre-fill material when authoring gold-set entries. The reference materials are not auto-imported as gold-set entries; the operator-reviewed gold-set gate is preserved."
    - "The agent writes a canonical copy of each source PDF into the deliverable at `<output_root>/<output-folder-name>/original/source.pdf` (the deliverable's `.gitattributes` tracks it via Git LFS, and it is page-split into `source.part-NNN.pdf` plus a reassembly manifest when it exceeds `lfs_max_file_bytes`). The runtime does not commit the deliverable; the operator commits it if and where they choose. The original `corpus/` files remain in place as the operator-curated input set and are not modified or deleted by the agent."
    - "If a future ComEd document update arrives (for example a 2026 Blue Book replacing the 2025 edition), add the new file under a new canonical directory name (for example `corpus/comed-blue-book-2026/source.pdf`) rather than overwriting the existing one, so prior runs remain reproducible."
---

<!-- markdownlint-disable MD013 -->

# NoEasyMark: Agent-Driven PDF to Markdown Conversion

**Status:** Active
**Owner:** Frank Lesniak
**Last Updated:** 2026-06-06
**Scope:** NoEasyMark repository scaffolding, runtime architecture, content handling, CI, conversion pipeline, and development handoff rules.
**Related:** `.github/instructions/pdf-conversion.instructions.md`, the Phase 0 agent instruction inventory, and the template-sync marker.

This document also carries YAML frontmatter with operator setup, bootstrap devcontainer, and initial corpus-upload instructions. When a Markdown renderer hides frontmatter, use the raw/source view for those pre-implementation instructions; the body below remains the rendered implementation contract.

## Premise

A coding agent executes the pipeline end-to-end. It writes code, runs CLIs, produces reviewable local deliverables and reports, and resumes work across sessions. Human involvement is limited to named gates. The shipped conversion runtime does not open pull requests, post to GitHub, or commit its output; it writes a self-contained, commit-ready deliverable folder that the operator reviews and commits at their discretion. (Pull requests and CI responses do appear during the separate task of *building* NoEasyMark from this specification — see the **Implementation milestoning** row in **Closed Handoff Decisions** — but they are not part of the conversion runtime.)

The agent owns the pipeline and orchestration. The original PDF, rendered page images, manifests, schemas, tests, prompts, judge backend configurations, redacted tool-profile snapshots, and adjudication records own the truth.

Three distinct agent roles are in play and should not be conflated:

- The **implementing agent** is the coding agent the operator uses to build NoEasyMark from this specification. Any capable coding agent, such as OpenAI Codex, Claude Code, or another coding agent, may implement the spec. The spec is the contract and is implementer-agnostic.
- A **supervising coding agent** is an operator-run coding agent outside the shipped `noeasymark` runtime that may help execute local review workflows, respond to review packets, or diagnose quality-check failures when the operator asks it to. It is separate from the CLI runtime and from judge backends.
- A **judge backend** is whatever the resulting `noeasymark` tool invokes at runtime to adjudicate disputed spans. The set of supported judge backends is defined in **Judge Backends** and is operator-configurable.

Reference links in this document are non-normative and may drift as vendors change their tools. The implementation must consult current vendor documentation when building backend adapters, and `noeasymark doctor --backends` plus pilot evidence are the operational authority for what a configured backend can actually do. During Phase 0, every shipped backend or cloud/document-service engine YAML that depends on a vendor model id, alias, endpoint host, API-version value, capability claim, billing/credit assumption, or CLI flag records a structured `change_notes.vendor_doc_freshness[]` entry. Each entry names the YAML field checked, observed value, status (`verified`, `updated`, or `verification_deferred`), implementing-agent identity, and either an official documentation URL with UTC access date or an installed-tool evidence record (`command`, observed tool version when available, and SHA-256 hash of the captured output) for facts verified from CLI help. If the authoritative source is unreachable or inconclusive, the YAML stays disabled or keeps the conservative placeholder for that field, the freshness entry records `status: verification_deferred`, and `_TODO_AFTER-BUILD-MANUAL-STEPS.md` records `verification_deferred` with the exact value that needs operator confirmation.

## Naming

The public tool name and canonical GitHub repository name are **NoEasyMark**. The GitHub repository SHOULD be created and displayed as `franklesniak/NoEasyMark`; lowercase `noeasymark` is reserved for implementation identifiers: the intended PyPI distribution name, Python import package, CLI executable, schema/config field names, and filesystem directories intentionally named after those identifiers. The implementing agent records the implementation identifiers in local Python packaging metadata: `[project].name = "noeasymark"`, import root `src/noeasymark/`, and a `[project.scripts]` console-script command named `noeasymark` that points to the Typer CLI entry point. This metadata preserves import/CLI consistency during implementation; it does not reserve the PyPI namespace or publish a distribution. Publishing to PyPI is not part of this implementation specification unless a later release task explicitly re-checks PyPI name availability and name-retention constraints, then adds a publication phase, trusted publishing configuration, release signing policy, and maintainer approval gate.

The name frames the project as hard-case PDF-to-Markdown conversion: complex source PDFs are no easy mark, and generated Markdown must be backed by source-page proof rather than trusted at face value.

## What the Agent Owns vs. What It Must Not Do

| Agent owns | Agent must not do |
| --- | --- |
| Scaffold the repo, write CLIs, run pipelines | Make undocumented or non-replayable judgment calls |
| Inspect outline and propose chapter and chunk boundaries | Treat cleaned or OCR-derived PDFs as canonical source truth |
| Run engines, collect candidates, invoke configured judge backends | Skip pages silently when an engine fails |
| Produce per-chapter reviewable local outputs and review reports | Invent text to bridge disagreement |
| Detect and flag disputed spans for review | Auto-approve its own glossary |
| Run build-time repository validation before implementation commits | Bypass `pre-commit`, CI, or other build-time repository validation |
| Track spend, runtime, progress, backend calls, and retry signatures | Bypass runtime schema validation, run-state validation, or progress gates |
| Resume from schema-validated on-disk state | Make undisclosed network calls or write durable data outside the spec-declared repo, run-state, folder-state, artifact/cache, output, or operator-approved archive locations; ephemeral OS temp directories are allowed only for per-call scratch that is provenance-recorded and cleaned |
| Enforce local/perimeter/internet/external-archive content handling choices | Treat operator rights, licenses, or source-document terms as resolved by the tool |

## Closed Handoff Decisions

| Decision | Value |
| --- | --- |
| Implementation baseline | Python `>=3.13,<3.15` for the core package, matching the upstream template's active bugfix-version policy; Typer CLI; Pydantic v2 schemas; `pyproject.toml` at repository root. Environment and dependency management uses `uv` with a committed `uv.lock` for reproducibility. NoEasyMark rewires the template's pip-oriented Python workflow during Phase 0 so core CI runs `uv lock --check`, `uv sync --locked --dev` for the base project plus the PEP 735 `dev` dependency group, and test/type/lint commands through `uv run`. The `dev` dependency set is declared under top-level `[dependency-groups]`, not under `[project.optional-dependencies]`, and `uv sync --locked --dev` is treated as equivalent to `uv sync --locked --group dev` for this repository. Heavy conversion engines and GPU/VLM stacks live in named published optional extras under `[project.optional-dependencies]` and are not installed by core CI through `--all-extras`; optional-stack CI jobs may install a narrowly named extra such as `--extra ocr`, `--extra judge-litellm`, or `--extra engines-smoke` only when the job is intentionally validating that stack. Runtime compatibility for optional stacks is declared and checked by `noeasymark doctor`; the core package and core CI do not fail only because an optional engine is unavailable on a supported Python version. The repository is initialized from the `franklesniak/copilot-repo-template` template so that pre-commit, markdownlint, remark link validation, GitHub Actions, mypy, ruff/black, and agent instruction files (`.github/copilot-instructions.md`, `.github/instructions/`, `.cursor/rules/repository-instructions.mdc`, `.hermes.md`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`) are inherited rather than re-invented. |
| License | MIT, inherited from `franklesniak/copilot-repo-template`. The implementing agent updates `LICENSE` copyright holder and year and ensures `pyproject.toml` uses the PEP 639 form `license = "MIT"` (an SPDX license expression) plus `license-files = ["LICENSE"]`, and does **not** add a `License :: OSI Approved :: MIT License` Trove classifier (PEP 639 deprecates license classifiers when an SPDX expression is present; the live template already follows this form). The repository-license decision is separate from dependency-license obligations: the later **Pinned Dependency Policy** and install-profile rows require dependency/license-footprint review, including the initial PyMuPDF renderer dependency whose current [PyMuPDF package metadata](https://pypi.org/project/PyMuPDF/) describes AGPL/commercial licensing. The agent does not change the repository license without an explicit pre-Phase-0 maintainer instruction. Re-licensing later requires every contributor's consent and is out of scope for the implementing agent. |
| Upstream template visibility | The intended upstream state is that `franklesniak/copilot-repo-template` is public, so NoEasyMark can link to it freely in `.template-sync/marker.yml` (`source_repo:`, used by sync tooling), in human-facing surfaces (`docs/specification.md`, `README.md`, `TEMPLATE_UPDATE_PROCEDURE.md`, `.github/copilot-instructions.md`, `.cursor/rules/repository-instructions.mdc`, `.hermes.md`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, and other human-facing documentation, setup guides, or handoff files), and in this specification's **Reference Notes**. Phase 0 verifies the current upstream visibility through the implementing agent's GitHub MCP connector. If the upstream is public, no annotation is required. If the upstream is private, the agent does not change another repository's visibility; it preserves machine-readable template-sync provenance, replaces each public-facing human hyperlink to the template with the exact non-linked phrase `the upstream template repository franklesniak/copilot-repo-template`, and records an operator action in `_TODO_AFTER-BUILD-MANUAL-STEPS.md` to either make the upstream public or accept private upstream provenance before final public handoff. |
| Repository layout | Flat-root layout consistent with the upstream template: code under `src/noeasymark/`, tests under `tests/`, schemas under `schemas/`, configuration under `config/`. Operator-supplied source PDFs and adjacent operator-curated reference materials live under `corpus/<output-folder-name>/`, committed via Git LFS for the PDFs (see **Initial corpus upload** in the document frontmatter); the `corpus/` tree is the persistent operator-input set and is not mutated by the runtime once committed. Per-run mutable state lives under the gitignored `.noeasymark-state/<run_id>/`. Folder-scoped private state that can be shared by successive runs of the same document lives under `.noeasymark-state/_folders/<output-folder-name>/`. Bulky per-run scratch lives under the gitignored `.noeasymark-artifacts/<run_id>/`. The shared folder-scoped response cache lives under `.noeasymark-artifacts/_shared/<output-folder-name>/responses/` unless a run explicitly selects run-scoped caching. Reviewable deliverables live under `<output_root>/<output-folder-name>/` (default `output_root: ./output/`); the runtime writes them commit-ready but never commits them. Development-time gate-record artifacts live under the gitignored `.implementation-state/`. |
| Corpus input vs produced deliverable | `corpus/<output-folder-name>/` is the operator-curated *input* set (source PDFs and reference materials). Committing a source PDF (or any reference material) into `corpus/<output-folder-name>/` is itself the operator's exercise of their rights, license terms, and source-document terms; it is the consent record for that file. The conversion *deliverable* the runtime produces — final Markdown, final images, the original-PDF reference copy, evidence sidecars, manifests, reports — is written under `<output_root>/<output-folder-name>/` (with `output_root` defaulting to `./output/`) on local disk only. The runtime never commits the deliverable and there is no repository-storage approval gate. Whether, where, and how the operator commits the produced deliverable is entirely the operator's own action and responsibility, governed by the operator's rights, licenses, and source-document terms exactly as the corpus input is. `noeasymark doctor` reports the resolved input (`corpus/`) and output (`output_root`) locations so a reviewer can see which files predate a run and which the run produced. |
| Markdown dialect | GitHub Flavored Markdown. Display math uses fenced `$$ … $$`, inline math uses `$ … $`. HTML comments are used for review markers. The `markdownlint` and `remark` configurations from the template repository are the lint authority. |
| CI tooling | GitHub Actions, inherited from the template (`precommit-ci.yml`, `python-ci.yml`, `data-ci.yml`, `markdownlint.yml`, `check-placeholders.yml`, `auto-fix-precommit.yml`, plus optional language-stack workflows while retained). NoEasyMark adds tool-specific jobs only when the template's coverage is insufficient. Non-GitHub CI platforms are an operator extension and are out of scope for the initial spec. |
| Run identity | `run_id` follows one of two canonical grammars. The base grammar is `<output-folder-name>-<UTC yyyymmdd-HHMMSS>-<6-char random>`, where the random segment is six lowercase hexadecimal characters drawn from `[0-9a-f]` via a cryptographic RNG so it matches the existing source-fingerprint alphabet. The extended grammar adds a trailing 3-digit zero-padded UTC milliseconds-of-second segment: `<output-folder-name>-<UTC yyyymmdd-HHMMSS>-<6-char random>-<UTC mmm>` where `mmm` is `\d{3}` (the millisecond portion of the same UTC timestamp). Every `run_id` parser in the runtime — including `--run-id` validation, `--reuse <id>` resolution, work-queue lookup, and `noeasymark unlock <run_id>` argument parsing — accepts either grammar. `noeasymark init` writes a pointer file at `.noeasymark-state/active_run` that resolves the default run for subsequent commands. Every command also accepts `--run-id <id>`. `init --fresh` creates a new run even when an existing one for the same folder is present; `init --reuse <id>` attaches to an existing run by repointing the `active_run` pointer at that id, displaying the existing run's carried-forward folder-scope approvals and content-handling choices for operator visibility, and *never* modifying the existing run's `run_config.json`, `approvals_record`, or any folder-scope state. `--reuse` is a pure pointer operation: the runtime loads the reused run's existing `run_config.json`, derives the authoritative `output_folder_name` from that file, and rejects any simultaneously supplied source path, imported init bundle, document title, output-folder override, pilot-page override, approval flag, or other init input whose value is absent from or conflicts with the reused run. A conflicting `--reuse` invocation exits with usage/configuration exit code 2, prints the reused run's actual `output_folder_name`, source hash, document title, and run-state path, and leaves `.noeasymark-state/active_run` unchanged. The display is emitted to stdout in the normal rich table form, emitted as JSON when `--json` is present, and appended to the command log as a non-state-changing event; no extra state file is written merely because approvals were displayed. `--reuse` is intentionally non-prompting and idempotent so that resuming a run cannot silently alter its configuration. Operators who need to change a field in an existing run edit the relevant `run_config.json` field directly (the next mutating subcommand re-validates the file against schema) or start a new run with `--fresh`. Multiple runs coexist on disk. If the generated base-grammar `run_id` collides with an existing on-disk run (for example, two `init --fresh` calls in the same second), the runtime regenerates the random suffix and retries up to five times under the base grammar; if the run is still colliding, the runtime promotes the `run_id` to the extended grammar by appending the current UTC millisecond suffix to guarantee uniqueness. If even the extended-grammar form collides (rare; requires three or more `init --fresh` calls within the same millisecond), the runtime exits with usage-error exit code 1 and prints the colliding ids so the operator can retry. An existing `run_id` is never overwritten. |
| Configuration model | Three schema-validated configuration/state records with explicit ownership: repo-level `pipeline.toml` at repository root holds repository-scope defaults and limits; per-run `.noeasymark-state/<run_id>/run_config.json` holds operator inputs, content-handling approvals, allowlists, ceilings, local deliverable placement, and pre-pilot backend preferences; `.noeasymark-state/<run_id>/tool_profile.json` holds active backend bindings, model parameters, prompts, quality thresholds, gold-set status, artifact strategy, render-profile alias, and behavior-affecting runtime profile. A field may appear in more than one record only as an explicitly named snapshot or override, such as `tool_profile_hash` (frozen content hash of the snapshot bound to a specific call) or `run_config.<limit>_override`; otherwise ownership is exclusive. Built-in defaults apply only when the owning record omits an optional field. |
| Output folder name | Auto-generated by Unicode-NFKD normalization, transliteration to ASCII via the version-pinned `anyascii` package, lowercase, replacement of any run of non-`[a-z0-9]` characters with a single `-`, leading/trailing hyphen trim, hard cap of 80 characters, empty-result fallback to `untitled`, and rejection of OS-reserved names (`con`, `prn`, `aux`, `nul`, `com1`...`com9`, `lpt1`...`lpt9`), reserved names with extensions, and names ending in a dot or space. Reserved-name validation applies after slug normalization and before collision suffixing; it rejects a final path segment when the full segment or its stem before the first dot casefolds to a reserved name. The `anyascii` package version is pinned in `pyproject.toml` so the transliteration result is reproducible across machines; an upstream version bump is treated as a tool-profile change. Collision checks are case-insensitive. On collision with an existing folder, append `-<6-char source fingerprint>` when the source PDF SHA-256 is available; otherwise append `-<6-char title hash>`, then a deterministic numeric suffix if needed. The 6-char title hash is the first 6 lowercase hexadecimal characters of the SHA-256 of the operator-supplied document title after NFC normalization, computed identically to the source fingerprint so the two paths share encoding. Folder names never change silently once a folder exists. On `init`, the runtime computes the worst-case asset path length (repository root + `<output_root>/<output-folder-name>/images/ch<NN>-page-<PPPP>-<kind>-<II>-<64-char hash>.png`) and emits a `PATH_LENGTH_WARNING` when the result exceeds 240 characters on any platform; on Windows the warning additionally directs the operator to enable long-path support or supply a shorter override. The warning is informational; the run continues. |
| Render profile | Named alias (default `default-300dpi`) declared in `config/render_profiles/<name>.yaml`; the runtime computes `render_profile_hash_full` as the SHA-256 of the canonical-JSON (RFC 8785) of the profile fields and computes `render_profile_id` as that digest's 12-character lowercase hexadecimal prefix. Profile fields are a closed set with explicit value domains so two implementations produce byte-identical canonical JSON: `dpi` (positive integer), `color_space` (closed enum `srgb`, `gray`, or `cmyk`; default `srgb`), `bit_depth` (closed enum integer `8` or `16`; default `8`), `image_format` (closed enum `png`, `jpeg`, or `webp`; default `png`), `image_compression` (closed enum of strings — for `png` one of `zlib-level-1` through `zlib-level-9` (default `zlib-level-6`); for `jpeg` one of `jpeg-quality-1` through `jpeg-quality-100`; for `webp` one of `webp-quality-1` through `webp-quality-100`), `mediabox_application` (closed enum `honor` or `ignore`; default `honor`), `cropbox_application` (closed enum `honor` or `ignore`; default `honor`), `rotation_application` (closed enum `apply` or `ignore`; default `apply`), `renderer_id` (closed enum; initial supported value is the literal `pymupdf`), and `renderer_version` (string from `pymupdf.__version__` when `renderer_id` is `pymupdf`, e.g., `1.24.5`). The `render_profile` schema enforces these enums plus cross-field compatibility: `color_space: cmyk` is valid only with `image_format: jpeg`, because PNG and WebP profiles are RGB/gray output surfaces in this spec. The short id is recorded in every crop, evidence sidecar, image asset, report, and filename-oriented reference; the full hash is used in cache identity and provenance. Any change to a profile alias is treated as a tool-profile change and triggers the mini-pilot rule. |
| Approval persistence | Identity-class approvals (external artifact archive identity) persist at folder scope. The canonical full record lives internally at `.noeasymark-state/_folders/<output-folder-name>/approvals.json` and validates against the `approvals_record` schema; this internal record is the single source of truth for folder-scope approvals. There is no repository-storage approval, because the runtime never commits its output. Content-handling approvals (perimeter, internet, restricted-diversity acknowledgment, unknown-cost acknowledgment) live in per-run `run_config.json`. `noeasymark init` displays carried-forward folder-scope approvals from the internal record and requires explicit re-confirmation. |
| GitHub automation transport (runtime) | **None.** The shipped `noeasymark` conversion runtime performs no GitHub interactions whatsoever: it does not create PRs, post issues/comments, read PR or check-run status, manage labels, request reviewers, verify repository LFS readiness, call the GitHub REST/GraphQL APIs, embed PyGithub, or invoke `gh`. The runtime is GitHub-agnostic and credential-free; it produces a commit-ready local deliverable folder (see **Final Output Shape**) that the operator commits at their discretion. Consequently `noeasymark doctor` performs no `gh`/GitHub checks, and no exit code is reserved for missing GitHub automation. The only GitHub usage in this project is the *build-time* workflow of the implementing agent (see **Implementation milestoning** and **Development-time GitHub transport**), performed through the implementing platform's GitHub MCP connector, never through `gh` and never through shipped `noeasymark` code. |
| Engine activation policy | Each shipped engine YAML defaults to `enabled: false`. At `doctor --quick` time, every engine in the Phase 6 "Initial engine set" whose declared dependencies are present and runnable on the host is reported as dependency-eligible; this report does not flip the YAML's `enabled` field and does not make the engine part of an active conversion. Pilot selection records the actual runtime activation roster in `tool_profile.json` after validation. Engines outside the Initial engine set require explicit operator enablement in the YAML before doctor will smoke-call them. Cloud-class engines (`network_class: perimeter` or `internet`) additionally require the corresponding content-handling approval and allowlist entry before activation. The complete activation matrix (which engines an unmodified first pilot can pick up automatically and which require operator action first) is the **Shipped Engine Activation Matrix** in **Phase 6**. |
| Starter quality thresholds | The spec ships a starter `quality_thresholds` profile that operators may amend or waive at `PILOT_GLOSSARY_REVIEW` but is not silently accepted. The starter values for blocking and advisory-warning thresholds are: character error rate against gold blocking `<= 0.02` / warning `<= 0.01`; word error rate against gold blocking `<= 0.05` / warning `<= 0.03`; heading accuracy blocking `>= 0.95` / warning `>= 0.97`; figure-caption pairing accuracy (when visual pages exist) blocking `>= 0.95` / warning `>= 0.97`; table handling accuracy (when table pages exist) blocking `>= 0.90` / warning `>= 0.95`; equation/symbol handling accuracy (when relevant pages exist) blocking `>= 0.90` / warning `>= 0.95`; multi-column reading-order accuracy blocking `>= 0.95` / warning `>= 0.97`; review marker density blocking `<= 0.05` markers per accepted Markdown block / warning `<= 0.02`. Cost-per-page and runtime-per-page are tracked but have no default blocking threshold; the operator sets them at the gate if desired. |
| Initial gold-set corpus | The initial gold-set construction draws from publicly-published ComEd source documents the operator has identified as suitable: the ComEd Blue Book 2025 ([curated GitHub path](https://github.com/franklesniak/lesniak-residence-construction-plan/tree/main/references/codes-and-standards/comed-blue-book-2025), [official ComEd documents guide](https://www.comed.com/my-account/my-service/construction-remodeling/documents-guides); a direct public PDF URL was not confirmed at spec-authoring time), the ComEd DER Interconnection Guidelines ([curated GitHub path](https://github.com/franklesniak/lesniak-residence-construction-plan/tree/main/references/codes-and-standards/comed-der-interconnection-guidelines), [official ComEd PDF](https://www.comed.com/cdn/assets/v3/assets/blt3ebb3fed6084be2a/blt2bdb5921266bf5c4/67742fbd63a97db2f50004f7/DER_Interconnection_Guidelines_for_Customers_2024-v3.pdf?branch=prod_alias)), and the ComEd Service and Meter Requirements 2025 ([curated GitHub path](https://github.com/franklesniak/lesniak-residence-construction-plan/tree/main/references/codes-and-standards/comed-service-and-meter-requirements-2025), [official ComEd PDF](https://www.comed.com/SiteCollectionDocuments/service_and_meter_requirements.pdf)). The curated GitHub paths are the implementation copy locations; the official ComEd links preserve publisher provenance. These documents serve as both pilot corpus and gold-set source material. The operator confirms at `init` time that the active local processing acknowledgment, perimeter/internet approvals, and external archive choices are consistent with each document's republication terms before any conversion run begins. |
| Budget ceiling | No hard spend ceiling by default; spend ledger always on; optional hard ceiling available. The dollar ceiling is enforced only for calls whose `cost_tracking.mode` is `native`, `estimated_by_model`, or `estimated_by_page`. Subscription, credit-pool, runtime-only, and acknowledged unknown-cost backends accept optional or mandatory `invocation_ceiling` and `runtime_ceiling_seconds` as described in **Subscription Credit Accounting**. |
| Wall-clock ceiling | No hard global wall-clock ceiling by default; runtime ledger always on; mandatory per-call timeouts and no-progress halt. |
| No-progress halt | Independent named halt categories: `NO_PROGRESS_HALT_CALLS` (paid or agent-driven calls without progress), `NO_PROGRESS_HALT_TIME` (active processing without state advancement), `NO_PROGRESS_HALT_UNITS` (consecutive scheduled units without advancement), `NO_PROGRESS_HALT_CHUNK` (per-chunk repeated-attempt cap), `NO_PROGRESS_HALT_SIGNATURE` (repeated identical failure signature). Whichever fires first halts the run. Thresholds live in `pipeline.toml`. |
| Judge backends | Two backend families supported: `LiteLLMBackend` for HTTP/API providers supported by LiteLLM, and `CLIAgentBackend` for CLI agents that satisfy the documented subprocess contract. Shipped backend YAML files are examples until explicitly enabled or bound in the active tool profile; `doctor --backends` smoke-calls only enabled or bound backends unless the operator passes an all-configurations flag. |
| Judge routing | Abstract judge roles bind to a backend, model, and parameters in `.noeasymark-state/<run_id>/tool_profile.json`. Pilot evidence may change bindings or enable optional roles. No specific provider, agent, or model is required by default. |
| Run orchestrator scope | `noeasymark run` advances exactly one next undone unit per invocation by default, so every invocation produces a reviewable diff. `--once` is the explicit default; `--until <gate>` runs until the named gate is hit; `--continuous` loops until the next blocking gate, completion, or anti-spin halt. |
| Work queue and resumability | The orchestrator owns a schema-validated `.noeasymark-state/<run_id>/work_queue.json` DAG. Each unit records `unit_id`, `unit_type`, chapter/chunk/page scope, dependencies, input hashes, output paths, status, attempt counters, last failure signature, and blocking gate when applicable. `noeasymark run` chooses the next runnable unit from this DAG rather than inferring progress from filenames alone. |
| Engine family classification | Engines that delegate to a different network-class service (for example MarkItDown configured to call Azure Document Intelligence) are installed as separate engine YAML files that declare the delegated service in `delegates_to`, inherit the delegated service's `network_class`, and use the delegated service's family for diversity counting. Delegation chains are capped at one hop: an engine that sets `delegates_to` may not target another engine that itself sets `delegates_to`, and an engine may not delegate to itself. `noeasymark doctor` rejects deeper chains and self-delegation at configuration validation time. |
| Diversity under restricted network modes | The default diversity rule requires successful candidates from at least two engine families. Operators in local-processing-only mode or any restricted-network mode that cannot meet the rule must record an explicit `restricted_diversity_acknowledgment` in `run_config.json`; accepted spans under the relaxed rule carry a `RESTRICTED_DIVERSITY` review flag and appear in the chapter report. |
| Chunk overlap merge | Earlier chunk owns shared pages by default. The chapter-QA step compares overlap text and emits a warning when the candidates materially diverge so the operator can override on a per-chapter basis. |
| Schema-validation failure scope | The signature `schema_validation_failure:<response_schema_id>:<response_schema_version>` retires the offending backend for the current unit on the third strike; if the same backend is retired at the unit level a second time within the run, it is promoted to `JUDGE_BACKEND_BLOCKED` for the run. |
| Topic-description placement | The optional topic description lives in the reusable middle block of every paid prompt. Its content is hashed into `topic_description_hash`; the hash is part of the cache key so a topic change does not poison prior caches. |
| Tool-profile mini-pilot scope | The mini-pilot scope after a tool-profile change is the intersection of the change-affected pages with the existing pilot set, capped at 20 pages. If the intersection is empty, the mini-pilot falls back to a fresh stratified sample of equivalent size. |
| Allowlist defaults | "Optional allowlist" means optional to populate, not optional to declare. Operators must explicitly choose either `all` (allow every component in the approved class) or a structured allowlist. `noeasymark doctor` refuses an approval that omits both. |
| Endpoint identifiers | Each endpoint identifier is a discriminated-union record with `matcher_type` ∈ {`hostname`, `hostname_suffix`, `tenant_id`, `deployment_name`, `container_name`, `endpoint_name`, `env_url_host`} and a typed `value` field. `env_url_host` resolves at runtime by reading the named environment variable, parsing its value as a URL, and matching against the URL's host component. Wildcards are only allowed where the matcher type explicitly supports them. |
| Failure classification | Each backend or engine YAML records an ordered list of match rules evaluated against a `normalized_failure_event` record. The `normalized_failure_event` schema declares: `event_id` (deterministic from `unit_id` + `attempt_index` + `occurred_at`), `occurred_at` (UTC ISO 8601 with millisecond precision), `unit_id`, `attempt_index`, `event_type` (`engine_failure` or `backend_failure`), `component_id`, `match_type` (the matcher that fired), `exit_code` (nullable integer), `signal` (nullable string), `http_status` (nullable integer), `provider_error_code` (nullable string), `limit_subreason` (nullable enum: `configured_ceiling_exceeded` or `provider_quota_or_credit_exhausted`), `exception_class` (nullable string), `model_id` (nullable string), `response_schema_id` (nullable string), `response_schema_version` (nullable string), `output_contract_failure` (nullable string), `output_contract_path` (nullable repository-relative or artifact-relative path), `stderr_excerpt` and `stdout_excerpt` (each truncated to `failure_excerpt_max_chars`, default 2048), `signature_stem` (the raw substring matched by the rule's `pattern`), `signature` (the resolved `signature_template`), and `category` (`retryable`, `correctable`, or `fatal`). Supported `match_type` values include `exit_code`, `signal`, `timeout`, `http_status`, `provider_error_code`, `exception_class`, `output_contract_failure`, `stderr_regex`, `stdout_regex`, and `json_path`. Each rule has a `pattern`, a `category`, and a `signature_template` that drives anti-spin signature normalization. The match-rule list is evaluated first-match-wins, so operators put narrower rules before broader catch-alls. `signature_template` uses Python `str.format`-style named placeholders that resolve against the fields of the `normalized_failure_event` record (for example `provider_error:{provider_error_code}` or `cli_exit:{exit_code}:{signature_stem}`); the resolved string is the anti-spin signature counted against per-engine, per-backend, and per-chunk caps. |
| Versioning conventions | Prompts, response schemas, judge-backend YAML, engine YAML, and `cache_key` use monotonically increasing zero-padded integer versions (`v0001`, `v0002`). The `noeasymark` Python package itself uses semantic versioning. |
| Network classification | Every backend, source-content-receiving engine, and artifact archive declares a `network_class` value of `device`, `perimeter`, or `internet`. Operator content-handling choices approve each class independently. If unsure whether a component is truly `device` or `perimeter`, classify it as `internet`. |
| Vendor caching | Use vendor-native prompt caching where available; structure prompts as stable prefix, optional reusable middle blocks, and variable suffix to maximize valid cache hits. |
| Vendor cache metrics | Backend configuration declares `vendor_cache_metrics: native`, `inferred`, or `unavailable`; reports state which mode was used. |
| Response caching | Two-layer cache entry: minimal `cache_key` for lookup using only output-affecting fields, plus rich `provenance` bundle for audit. Cached responses are replayable only while every `cache_key` component is unchanged. The response schema id, response schema version, and backend-emitted schema hash are cache-key components, not provenance-only fields. Provenance may expand without invalidating the cache. Every component hash is computed independently (see **Cache Key Canonicalization**) and the final `cache_key` is the SHA-256 hex digest of the canonical-JSON object that maps each component name to its component hex digest. |
| LiteLLM Router | Pipeline owns retries and fallback routing by default. LiteLLM Router retries/fallbacks are allowed only when explicitly enabled in backend YAML and recorded in call provenance. |
| Doctor smoke calls | `noeasymark doctor --backends` uses synthetic, non-source-derived prompts and fixtures only. It verifies schema output, auth, endpoint identity, allowlist matching, timeout behavior, and transcript/persistence declarations without sending source PDF content, candidate text, crops, or document metadata. |
| Artifact storage | The deliverable is written to `output_root` (default `./output/`) as a commit-ready folder; the runtime never commits it. Interim artifacts stay under the gitignored `.noeasymark-artifacts/`. The deliverable carries a `.gitattributes` with Git LFS rules and its `original/` PDF is page-split when it exceeds `lfs_max_file_bytes` (default 2 GiB) so a later operator commit succeeds. Bulky run artifacts may optionally be archived after pilot. |
| Artifact disk budget | Disk usage is tracked alongside spend and runtime. `pipeline.toml` defines warning defaults for artifact growth and minimum free space; `run_config.json` may set per-run hard ceilings for artifact bytes and rendered-page bytes. The pipeline checks disk budget before full-page rendering, OCR copies, engine batches, and archive staging. |
| External artifact archives | Persistent storage of source-derived artifacts outside the device requires explicit external artifact archive approval in addition to any perimeter or internet network-class approval. |
| Branching (runtime) | **None.** The conversion runtime creates no branches and opens no pull requests; it writes the deliverable to `output_root` and stops. `run_config.json` carries no `integration_branch` or `chapter_branch_prefix` fields, and the `branch_record` schema is not part of this specification. (Build-time branches for *constructing* `noeasymark` — `implementation/...` and the phase branches — are described under **Implementation milestoning** and are unrelated to conversion runs.) |
| Run initialization | A schema-valid run configuration created by `noeasymark init` is required before preprocessing, pilot execution, or full-run orchestration. |
| Manual review | Block affected chapter; continue independent chapters; final blocks unresolved normal/high items unless explicitly waived. |
| Diagram text | Preserve diagrams as tight crops; do not transcribe interior diagram text by default. |
| CI failures (build-time only) | During *construction* of `noeasymark`, deterministic pre-commit/formatter failures are auto-fixed (up to 2 attempts) and nontrivial failures go through the coding-agent review workflow first. The conversion runtime has no CI or PR loop; local chapter-QA failures are surfaced in the local review report and the run halts for operator attention. |
| Large chapters | Force intra-chapter processing chunks at hard maximum 25 pages unless explicitly overridden. |
| Multi-column pages | Detect layout regions before conversion; process multi-column pages region-by-region; fail or flag unresolved reading-order risk. |
| Evidence geometry | Use one canonical page, coordinate, render-profile, crop-reference, and evidence-sidecar model across all engines and final Markdown. |
| Consensus acceptance | Accept spans without judge review only when independent engine families agree after deterministic low-risk normalization, the agreement is backed by materially independent source lineage, and no high-risk exception applies. Agreement that traces only to the same poor embedded text layer is not independent evidence. |
| MarkItDown | Include as pilot candidate/utility engine, not default high-fidelity converter. |
| Devcontainer | Use CUDA-enabled devcontainer if practical; auto-select gitignored artifact storage; no dangerous permission-bypass settings by default in committed devcontainer files. Operator-invoked agent launch flags inside the short-lived bootstrap devcontainer remain governed by the frontmatter bootstrap instructions and are not baked into the shared devcontainer defaults. |
| Backend failures | Use exponential backoff with jitter; pause resumably on repeated or correctable judge-backend failures instead of failing destructively. |
| Schema-validation failures | Treated as normalized backend failure signatures and counted by the existing anti-spin mechanism. |
| Topic context | Accept an optional one-sentence topic description to seed glossary and judge context. |
| Pilot page selection | Operator-known pilot pages are optional. The tool auto-selects a stratified pilot set and allows operator review/edit before pilot execution. |
| Pilot gold set and thresholds | The pilot must produce or import a human-approved gold set for a representative subset before gold-based quality metrics can pass. Full-run scale-up requires approved metric targets, default floor checks, and explicit waiver records for any unavailable or failed gold metric. |
| Tracking surface | The runtime maintains a local `reports/status-<run_id>.md` file under the deliverable folder (there is no GitHub issue). Paths in it are relative to the deliverable root. |
| Markdown assembly | Apply conservative, evidence-recorded normalization for running headers, footers, page numbers, line wraps, soft hyphens, Unicode normalization, lists, footnotes, sidebars, and page artifacts; risky transformations go to judge or manual review. |
| Tool-profile changes | Post-pilot changes to judge bindings, backend parameters, prompts, engine set, artifact strategy, or thresholds require `doctor` validation, audit recording, cache-key review, and a targeted mini-pilot when output behavior can change. |
| Rights and content handling | The workflow permits local conversion subject to the operator's rights, licenses, and source-document terms. The separately recorded approvals are local processing acknowledgment, perimeter processing approval, internet processing approval, and external artifact archive approval. The runtime writes durable local state, artifacts, caches, reports, and deliverables only to the spec-declared local paths (`.noeasymark-state/`, `.noeasymark-artifacts/`, and `output_root`) and writes persistent external artifacts only when external artifact archive approval, matching network-class approval, and allowlist checks all pass. It performs no repository or remote-discussion actions, so there is no repository-storage, remote issue/comment source-content exposure, or absolute-path disclosure approval; the operator's own act of committing the produced deliverable is their exercise of rights. |
| Implementation milestoning | The implementing agent runs in `/goal`-style autonomous mode on a development integration branch named `implementation/noeasymark`, distinct from conversion-run branches. Phase branches are named `implementation/phase-<NN>-<short-name>` and target `implementation/noeasymark`; the final development PR targets `main` from `implementation/noeasymark` after the phase PRs are merged or after the maintainer explicitly asks to use the last cumulative phase branch as the final source. The explicit fallback signal is a maintainer-authored top-level PR comment on the latest cumulative phase PR with the exact command line `/noeasymark final-source <branch-name>`; the agent verifies the commenter has write permission, copies the decision into `.implementation-state/final-source.json`, and uses that named branch as the final PR source. The agent may create the next phase branch from the previous phase branch tip so work can continue without waiting for human merge. Phase 0 sub-phase branches (`implementation/phase-00a-template-adoption`, `implementation/phase-00b-schemas`, `implementation/phase-00c-post-schema-cli`, and any additional Phase 0 sub-phase branches the agent chooses) stack on each other: `00a`'s PR targets `implementation/noeasymark`; `00b`'s PR targets `00a`'s branch; `00c`'s PR targets `00b`'s branch; subsequent Phase 0 PRs continue the chain. When a parent PR merges, the agent rebases each dependent branch onto the new parent tip in a single mechanical step, pushes the rebased branch with `--force-with-lease` to update the dependent PR, and lets CI re-run. The rebase action is bounded to "rebase only" — the agent never edits commit content during the rebase, and any rebase conflict halts the agent with `STACKED_REBASE_CONFLICT` so the maintainer can resolve manually. PR-review `APPROVED` is the gate signal for `SCHEMA_DESIGN_REVIEW`; the milestone PR (`00b`) does not have to merge before `00c` opens — `00c` opens against `00b`'s branch while `00b`'s schema review is pending. The agent is authorized to self-merge its own phase PRs into the development integration branch `implementation/noeasymark` once (a) every required CI check on the PR is green, (b) the PR touches no protected files except those whose edits have a matching `protected_file_decisions` entry in `.template-sync/marker.yml` written in the same PR, (c) no open GitHub review whose state is `CHANGES_REQUESTED` exists on the PR, and (d) the `SCHEMA_DESIGN_REVIEW` gate (when applicable to the PR's scope) is `approved`. The agent never merges into `main`; the final PR from `implementation/noeasymark` to `main` is always merged by the maintainer. The maintainer may also choose to merge any phase PR manually instead of letting the agent self-merge; this is a normal, expected workflow and does not require any spec change. When phase PRs are stacked, every later PR body names its parent phase branch, lists which earlier phase PRs are included in its diff, and states that the diff will shrink after earlier PRs merge; this cumulative stacking is intentional and is not a reason for the agent to halt. **Phase 0 is explicitly authorized to produce multiple PRs.** The schema-set milestone PR mid-Phase-0 plus the post-`SCHEMA_DESIGN_REVIEW` Phase-0 wrap-up are the planned-for case, and additional Phase-0 PRs are permitted whenever the agent reaches a natural reviewable boundary inside Phase 0. Sub-phase phase branches use the convention `implementation/phase-<NN><lowercase-letter>-<short-name>` (for example `implementation/phase-00a-template-adoption`, `implementation/phase-00b-schemas`, `implementation/phase-00c-post-schema-cli`). Other phases remain single-PR unless a future spec edit explicitly opens them up. Each spec phase remains the natural validation checkpoint. Within a phase the agent writes the implementation, writes comprehensive tests for everything it adds (unit tests for schemas and pure functions, integration tests for CLI subcommands, fixture-based tests for engine and backend adapters, end-to-end smoke tests against synthetic fixtures), runs full pre-commit and the available local CI-equivalent validation commands before pushing, then verifies required hosted CI after the PR opens, and only stops when (a) it hits a named blocking gate the spec requires human input at, (b) a test failure cannot be diagnosed and fixed within the spec's anti-spin limits, (c) it discovers an ambiguity not resolved by the spec, or (d) it has completed the spec end-to-end. **End-to-end definition.** "Completed the spec end-to-end" means: (1) every subcommand listed under **CLI Surface** is implemented and its `--help` validates; (2) every schema named in the Phase 0 schema list validates against fixtures (valid and invalid trees) committed under `schemas/examples/`; (3) every shipped backend, engine, render-profile, and artifact-archive YAML validates against its schema; (4) the retained template-inherited workflows and the NoEasyMark-added workflows are green; (5) the agent has run a **deferred-content init smoke** with the exact four-invocation script below, including the non-interactive approval audit flags `--approval-decider`, `--approval-decider-role`, and `--approval-rationale`, and confirmed that the resulting `run_config.json`, the folder-scope `approvals.json` snapshot, and `work_queue.json` all schema-validate and that `noeasymark status` reports the next undone unit as `preprocess`; and (6) `_TODO_AFTER-BUILD-MANUAL-STEPS.md` has been written as the final committed artifact of the development handoff PR. The deterministic definition-of-done smoke always runs the four-invocation script below against the committed synthetic one-page fixture `tests/fixtures/smoke/synthetic-source.pdf` (authored solely for testing; not source-derived), because that fixture is byte-reproducible and always present. Additionally, when the operator has committed a corpus PDF, the agent also runs a corpus-backed init smoke against the smallest committed corpus document (`corpus/comed-der-interconnection-guidelines/source.pdf` by default) as a non-blocking realism check; if no corpus PDF is present, that additional check is recorded as pending in `_TODO_AFTER-BUILD-MANUAL-STEPS.md`. The synthetic fixture has a stable shape so reviewers know why it is useful for later preprocessing and rendering tests: page size US Letter (612 × 792 PDF user-space points) portrait, no rotation, embedded text layer marked `quality_label: good` by preprocessing, one heading line "Smoke Test Document" rendered at 18 pt Helvetica-Bold near the top of the page, one body paragraph rendered at 11 pt Helvetica with this exact text — "This is a synthetic one-page fixture authored for NoEasyMark tests. It is shaped for later preprocessing, page-render, source-manifest, and outline-detection tests, while the deferred-content init smoke itself uses it only for initialization metadata and hashes. The fixture's text and visual content are intentionally not derived from any third-party document." — and one labeled rectangle drawn as a filled 200 × 100 pt RGB blue (`#1f4e79`) rectangle below the paragraph, followed by the figure-caption line "Figure 1: Synthetic figure" rendered at 9 pt Helvetica-Oblique. The agent generates this fixture once at scaffolding time using ReportLab (declared in the `dev` dependency group with a compatible-release constraint `reportlab~=X.Y.Z` against the version available at Phase 0 adoption; the committed `uv.lock` pins the exact resolved patch version so the lockfile is the audit-authoritative record of which version produced the committed PDF) and post-processes the ReportLab output with `qpdf --deterministic-id --object-streams=generate --linearize <input> <output>` to strip the embedded creation/modification timestamps and stabilize the `/ID` trailer for byte-deterministic output across machines and ReportLab patch releases; qpdf is already a hard NoEasyMark host-native dependency (and is installed in both canonical devcontainer variants), so no additional dependency is introduced. The agent commits the resulting post-processed PDF under Git LFS along with the generator script. A NoEasyMark-owned pytest at `tests/test_synthetic_fixture_reproducibility.py` re-runs the generator + qpdf and asserts byte-identity against the committed file; this test is included in `cross-platform-check.yml` so that cross-OS regeneration is verified on `ubuntu-latest`, `macos-latest`, and `windows-latest`. `_TODO_AFTER-BUILD-MANUAL-STEPS.md` then records that the corpus-backed smoke remains pending until the operator uploads the corpus. The deferred-content init smoke reads only file metadata and hash inputs required for initialization, may copy or hash source bytes into private gitignored run artifacts needed to establish the canonical working copy, processes no source-derived page content, performs no rendering, OCR, text extraction, outline analysis, or page-manifest construction from a real corpus PDF, issues no judge backend calls, opens no PRs, and is the closing definition-of-done check — not a pilot run. The Phase 0 `SCHEMA_DESIGN_REVIEW` gate is a development-time blocking gate for later phases that consume any schema named in the Phase 0 schema list; while it is pending, the agent may continue only Phase 0 work that does not import a schema module and whose tests do not load a fixture under `schemas/examples/`. The pre-gate / post-gate partition for Phase 0 is enumerated in **Phase 0**. The gate is recorded as a PR review state plus the `.implementation-state/gates/schema-design-review.json` artifact described in Phase 0; the agent's resumption logic checks the state file first, then falls back to a GitHub MCP query for open PRs labeled `schema-design-review` when the state file is missing. The agent never bundles two phases into a single PR unless the spec explicitly says so, and never merges Phase 0 with another phase. |
| Development-time GitHub transport | In autonomous-mode development runs (the agent's own implementation work against `implementation/noeasymark` and its phase branches), the implementing agent performs every GitHub interaction — PR open, PR comment, PR review query, label set, status-check read, gate resumption query, phase-PR self-merge, and `SCHEMA_DESIGN_REVIEW` label discovery — through the implementing platform's **GitHub MCP connector**, never through `gh`. This row is an intentional NoEasyMark-specific override for autonomous-mode development runs: it narrows the inherited plugin-first/`gh`-fallback protocol for that mode and does not weaken inherited plugin-first behavior for ordinary interactive repository work. At Phase 0 start, and again before the first operation that would depend on a new remote capability, the agent writes a local `.implementation-state/github-connector-capabilities.json` inventory naming each required operation and whether the current connector exposes it. If a non-mutating query capability is missing, the agent records the exact unobservable item and falls back to the matching local state file only when the spec defines one; otherwise it halts with `MANUAL_SETUP_INCOMPLETE` and names the required manual verification. If a mutating capability is missing, the agent does not shell out to `gh` in autonomous mode: missing PR creation, PR comments, labels, or review queries halt with a manual maintainer action; missing phase-PR self-merge capability simply disables agent self-merge and the maintainer merges that phase PR manually after the checks and review conditions pass. Manual phase-PR merge is always an expected placement path, not a spec failure. The shipped `noeasymark` conversion runtime performs no GitHub interaction at all (see **GitHub automation transport (runtime)**), so there is no runtime `gh` capability surface for `doctor` to validate. The pre-existing carve-out in **GitHub automation transport** for implementing-agent platform connectors remains as an escape hatch for interactive maintainer-driven sessions (for example, a maintainer running Codex interactively and asking it to do platform-connector PR work outside the autonomous loop); it is not the default. `noeasymark doctor` does not refuse a development environment that lacks an alternate platform connector. |
| Default branch naming (runtime) | **Not applicable.** The conversion runtime creates no branches, so there are no `integration_branch` / `chapter_branch_prefix` defaults and no branch-collision checks. Deliverable destination is controlled solely by `output_root` (default `./output/`). |
| Diagram-text policy | Each chunk's `chunk_manifest` records `diagram_text_policy`, which defaults to `crop_only` (preserve diagrams as tight crops without transcribing interior text). An operator who wants interior diagram text transcribed for a chunk records `diagram_text_policy: transcribe` in that chunk's manifest before convert runs; the value participates in `evidence_content_hash` so a policy change invalidates the cache. The default is set at run-init time and may be flipped per chunk at chunk-planning time; later changes require explicit re-conversion. |
| Init bundle import | `noeasymark init --import <path>` accepts a TOML or JSON file whose top-level `schema_id` is `init_bundle`. The bundle carries only the operator-input fields enumerated under **Plain-Language Inputs** plus the content-handling approvals; it does not carry run-internal state, tool-profile bindings, or ledger data. To clone or reuse an existing run's state, use `--reuse <run_id>` instead. |
| Chapter review report | Each chapter's review surface is a local report the runtime writes into the deliverable at `<output_root>/<output-folder-name>/reports/chapter-NN-review.html` (rendered Markdown, embedded image crops, QA metrics, flags, links, and a reviewer checklist mirroring the chapter-QA gates), accompanied by the machine-readable `reports/chapter-NN.json`. There are no chapter pull requests and no `config/pr_templates/chapter.md`. The runtime renders the report with a strict built-in renderer (not Jinja2/Liquid/Mustache); the only recognized placeholder grammar is `{{ dotted.path }}` whose first segment is one of `chapter_report`, `chapter_manifest`, `run_config`, or `tool_profile_public`; filters, conditionals, loops, function calls, includes, environment lookup, and arbitrary expression evaluation are invalid, and missing values fail rendering unless declared in the renderer's `not_applicable` allowlist. The report body covers: a one-paragraph summary (source page range, chunk count, source-page coverage percentage, accepted-span count); a "QA Metrics" table (character/word error rate vs gold, heading accuracy, figure-caption pairing accuracy, table-handling accuracy, equation-handling accuracy, multi-column reading-order accuracy, review-marker density, per-backend cost); a "Flags" section (restricted-diversity span count and evidence matrix summary, overlap-divergence pages, manual-review items by severity, waivers in effect); a "Links" section (chapter report, evidence sidecar, chapter Markdown file); and a "Generated" footer (`run_id`, `tool_profile_hash_compact`, `glossary_version`, `render_profile_id`, timestamp). Values that are unavailable (for example vendor cache hit rate when `unavailable`) render as `not_applicable` so the report is always shape-stable. |
| Reconversion artifact supersession | When a tool-profile change or operator-requested re-conversion replaces a chapter's outputs, the prior artifacts (images, evidence sidecars, chapter manifest, chapter report) are moved into `.noeasymark-state/<run_id>/superseded/<UTC timestamp>/` rather than deleted in place, and the new chapter_manifest's `superseded_artifacts` list records the relocation. The implementing agent never deletes superseded artifacts as part of the re-conversion itself; the operator runs `noeasymark cache prune --superseded` to reclaim disk space when satisfied with the new output. |
| Agent-runs retention | Per-call CLI-agent durable diagnostic directories live under `.noeasymark-artifacts/<run_id>/agent-runs/<unit_id>/` and are retained until the owning chapter is published successfully, at which point directories whose owning unit completed without a failed attempt are deleted. Ephemeral subprocess working directories may still be created under the platform temp root for isolation, but any retained transcript, schema, prompt, copied-evidence manifest, stdout/stderr excerpt, or failure artifact is copied into the durable diagnostic directory before cleanup. Directories whose owning unit produced any `FAILED` attempt (engine or backend) are retained until `noeasymark cache prune` runs, so postmortem diagnostics survive chapter publication. When a tool-profile change resets anti-spin counters, existing failed-agent-run directories are reclassified as historical diagnostics by recording their owning `tool_profile_hash_full` and `normalized_failure_event_id`; they no longer contribute to the new profile's anti-spin counters but remain available for postmortem review until prune. The default retention is overridable via `agent_runs_retention` in `pipeline.toml` (`on_chapter_publish` is default; `on_run_complete`, `on_prune_only`, and `immediate_after_success` are the other supported values). |
| CLI agent auth verification | `noeasymark doctor --backends` verifies CLI-agent authentication by issuing a synthetic doctor smoke call (non-source-derived prompt + dummy schema) and treating the schema-valid response as proof of auth. The doctor never inspects credential file contents (`~/.codex/auth.json`, `~/.claude/auth.json`, or equivalents). Auth-failure signatures from the smoke call surface as actionable error messages naming the CLI's own re-auth command (`codex login`, `claude auth login`, or the agent's documented equivalent). |
| Stack pruning | PowerShell and Terraform support are removed during Phase 0 because NoEasyMark is Python-only. The removal follows the template's stack-pruning module boundaries and is the default; an operator who needs either stack must say so in the implementation task before Phase 0 begins. The implementing agent then records the retained stack in `.template-sync/marker.yml` during adoption. |
| Identity placeholders | The shipped repository name is `NoEasyMark`, the GitHub owner is `franklesniak`, and the canonical owner/repo pair is `franklesniak/NoEasyMark`. Package, distribution, and CLI names are all `noeasymark`. `SECURITY.md` directs reporters to GitHub's private vulnerability reporting flow (Security tab -> Report a vulnerability) rather than to any email address; no security-contact email is shipped. `CODE_OF_CONDUCT.md` directs reporters to contact the repository maintainers through the contact information published on their GitHub profile pages (`https://github.com/franklesniak` for the sole maintainer at adoption time); no code-of-conduct contact email is shipped. PR-template reviewer routing uses `.github/CODEOWNERS` and is auto-populated from the GitHub owner handle. |

The deferred-content init smoke is a bounded preflight check, not a full conversion run. Even when the synthetic fixture is present for later preprocessing tests, the smoke itself does not render pages, OCR, extract text, analyze outlines, or construct source-page manifests; it only verifies initialization state, private working-copy establishment, hashes, approvals, work-queue readiness, and the wired-up preflight surface. The smoke executes exactly four CLI invocations in order against the synthetic fixture and asserts the documented outcome of each:

1. `noeasymark init --source tests/fixtures/smoke/synthetic-source.pdf --title "Smoke Test Document" --output-folder-name smoke-test-document --local-processing acknowledge --perimeter-mode deny --internet-mode deny --external-artifact-archive decline --approval-decider noeasymark-smoke --approval-decider-role maintainer --approval-rationale "deferred-content init smoke; source-derived content stays local"` — asserts exit code 0; asserts a schema-valid `run_config.json` was written; asserts the active-run pointer file resolves to the new `run_id`; asserts the captured `source_pdf_sha256` matches the synthetic fixture's actual SHA-256; asserts the folder-scope `approvals.json` snapshot records the decline state; asserts the initial `work_queue.json` is schema-valid and its first runnable unit is `preprocess`.
2. `noeasymark status` — asserts exit code 0; asserts the printed next-undone-unit string contains `preprocess`; asserts the `doctor --quick` preflight that `status` invokes internally completes without exiting non-zero. (`status` transitively exercises `doctor --quick`.)
3. `noeasymark doctor` (the full doctor, without `--quick`, `--gpu`, or `--backends`) — asserts exit code 0; asserts every shipped backend YAML and every shipped engine YAML schema-validates; asserts every shipped render-profile YAML schema-validates; asserts every shipped artifact-archive YAML schema-validates; asserts the runtime performs no GitHub or `gh` checks of any kind (the runtime never contacts GitHub).
4. `noeasymark init --source tests/fixtures/smoke/synthetic-source.pdf --title "Smoke Test Document"` — invoked a second time, plain `init` against an already-active run for the same folder. Asserts exit code 1 (usage-error); asserts the printed message names the active `run_id` and the three explicit choices (`--reuse <run_id>`, `--fresh`, `--import <path>`); asserts the active-run pointer file is unchanged.

`doctor --backends` and `doctor --gpu` are intentionally excluded from the smoke: `doctor --backends` adds no incremental coverage in the deferred-content state because no backends are bound (the backend-YAML schema regressions it would otherwise catch are already caught by full `doctor` above); `doctor --gpu` depends on CUDA availability that differs across the three OS runners and would produce cross-OS noise. The smoke is intentionally fast (well under five seconds on a hosted runner) so it can run as part of every PR's CI.

## Default Constants and Thresholds

The defaults below are repeated throughout this specification; this table is the single source of truth for repository-scope `pipeline.toml` constants plus selected `run_config.json`, render-profile, and schema-wide defaults. Only rows whose Owner is `pipeline.toml` are materialized into `pipeline.toml`. Values are overridable via `pipeline.toml` (repository scope) or `run_config.json` (per-run scope) where the surrounding section says so. Values not listed here are defined inline in the section that owns them.

| Constant | Default | Owner | Notes |
| --- | --- | --- | --- |
| `processing_chunk_target_pages_min` | 10 | `pipeline.toml` | Phase 4 lower edge of the chunk-size target band. |
| `processing_chunk_target_pages_max` | 15 | `pipeline.toml` | Phase 4 upper edge of the chunk-size target band; the chunker aims at the upper edge and only falls back toward the lower edge when a natural section boundary inside the band would otherwise be split. |
| `processing_chunk_hard_max_pages` | 25 | `pipeline.toml` | Phase 4 hard maximum unless explicit per-chapter override. |
| `chunk_boundary_min_gap_points` | 24.0 | `pipeline.toml` | Phase 4 projection-profile vertical-whitespace gap threshold for natural chunk boundaries. |
| `pilot_sample_pages_min` | 30 | `pipeline.toml` | Phase 9 lower edge of the stratified pilot-scope band. |
| `pilot_sample_pages_max` | 50 | `pipeline.toml` | Phase 9 upper edge of the stratified pilot-scope band; the sampler aims at the upper edge and only drops toward the lower edge when stratification cells are exhausted. |
| `gold_set_min_pages` | 5 | `pipeline.toml` | Phase 9 gold-set floor. |
| `mini_pilot_max_pages` | 20 | `pipeline.toml` | Tool-profile mini-pilot cap. |
| `engine_same_signature_cap` | 3 | `pipeline.toml` | Engine retire threshold per chunk per signature. |
| `backend_same_signature_cap` | 3 | `pipeline.toml` | Backend retire-for-unit threshold per signature. |
| `chunk_full_attempt_cap` | 3 | `pipeline.toml` | `NO_PROGRESS_HALT_CHUNK` trigger. |
| `no_progress_call_window` | 20 paid/agent-driven calls | `pipeline.toml` | `NO_PROGRESS_HALT_CALLS` trigger. |
| `no_progress_time_minutes` | 90 | `pipeline.toml` | `NO_PROGRESS_HALT_TIME` trigger over active processing time. |
| `no_progress_unit_window` | 5 consecutive units | `pipeline.toml` | `NO_PROGRESS_HALT_UNITS` trigger. |
| `output_folder_name_max_chars` | 80 | `pipeline.toml` | After NFKD normalization. |
| `worst_case_path_warning_chars` | 240 | `pipeline.toml` | `PATH_LENGTH_WARNING` trigger. |
| `run_id_random_suffix_chars` | 6 | `pipeline.toml` | Plus collision-retry policy. |
| `output_folder_collision_suffix_chars` | 6 | `pipeline.toml` | Source-fingerprint or title-hash suffix length. |
| `overlap_divergence_threshold` | `0.95` | `pipeline.toml` | `OVERLAP_DIVERGENCE` warning trigger; after low-risk whitespace normalization, compare adjacent overlap text with `rapidfuzz.distance.Indel.normalized_similarity` and warn when similarity is below this value. The exact `rapidfuzz` function is pinned so version bumps that change other comparators do not silently alter this gate. |
| `overlap_divergence_profile_id` | `overlap_low_risk_v1` | `pipeline.toml` | Closed overlap-comparison normalization profile. The default profile NFC-normalizes, strips NoEasyMark review-marker comments, normalizes line breaks to spaces, collapses Unicode whitespace runs to one ASCII space, trims ends, preserves case, and preserves protected-zone text verbatim. |
| `force_ocr_glyph_anomaly_threshold` | 0.02 | `pipeline.toml` | Phase 3 threshold above which `ocr-force.pdf` is generated after `ocr-redo.pdf` still shows glyph anomalies. |
| `ocr_languages_default` | `["eng"]` | `pipeline.toml` | Repository-scope default Tesseract/OCRmyPDF language identifiers. Values are validated against `tesseract --list-langs`; multi-language OCR is represented as an ordered array and joined with `+` only when constructing `-l LANG[+LANG]` command arguments. |
| `failure_excerpt_max_chars` | 2048 | `pipeline.toml` | Maximum stored `stderr_excerpt` and `stdout_excerpt` length in `normalized_failure_event`. |
| `multi_column_x_jump_fraction` | 0.4 | `pipeline.toml` | Phase 5 reading-order heuristic for suspicious x-coordinate jumps as a fraction of page width. |
| `multi_column_y_band_points` | 6.0 | `pipeline.toml` | Phase 5 reading-order heuristic y-band height for same-line column jumps. |
| `multi_column_semantic_discontinuity_threshold` | 0.5 | `pipeline.toml` | Phase 5 `lexical_window_cosine_distance` threshold near column boundaries. |
| `multi_column_cosine_window_chars` | 50 | `pipeline.toml` | Phase 5 character-window size on each side of a candidate column boundary for `lexical_window_cosine_distance`. |
| `multi_column_cosine_ngram_size` | 3 | `pipeline.toml` | Phase 5 character-n-gram size for `lexical_window_cosine_distance`. |
| `multi_column_lexical_window_profile` | `ascii_alnum_v1` | `pipeline.toml` | Phase 5 lexical-window normalization profile. The default keeps the existing ASCII letters/digits behavior; opt-in script-aware profiles must declare offset and cache-key impact. |
| `full_vs_region_alignment_min` | 0.90 | `pipeline.toml` | Phase 5 minimum per-region normalized similarity between full-page extraction and region-by-region extraction before the comparison check flags review. |
| `layout_detector_audit_page_cap_per_chapter` | 25 | `pipeline.toml` | Maximum pages per chapter on which lower-priority layout detectors may run in audit mode after a primary detector already succeeded. |
| `layout_detector_audit_runtime_ceiling_seconds` | 300 | `pipeline.toml` | Per-chapter wall-clock ceiling for audit-mode layout detector work. |
| `layout_detector_audit_spend_ceiling_usd` | 0.00 | `pipeline.toml` | Per-chapter spend ceiling for audit-mode layout detector work; paid or remote audit detectors require an explicit run override. |
| `tesseract_region_crop_psm` | 6 | `pipeline.toml` | Default page segmentation mode for region-cropped Tesseract, treating each crop as a single uniform text block. |
| `tesseract_region_crop_oem` | 1 | `pipeline.toml` | Default Tesseract OCR engine mode for region crops, using the LSTM engine path. |
| `tesseract_region_crop_confidence_review_min` | 0.70 | `pipeline.toml` | Minimum normalized crop-level confidence before a region-cropped Tesseract span is accepted without a low-confidence review flag. |
| `engine_default_max_workers` | 1 | `pipeline.toml` | Conservative default worker cap per engine when an engine YAML does not provide a narrower value. |
| `engine_default_cpu_threads` | 4 | `pipeline.toml` | Conservative default CPU-thread cap per engine, also used when normalizing vendor thread settings. |
| `table_structure_confidence_min` | 0.80 | `pipeline.toml` | Minimum table-structure confidence for Markdown or sanitized-HTML table emission without routing the table to review. |
| `caption_pairing_max_vertical_gap_points` | 36.0 | `pipeline.toml` | Maximum vertical gap between a visual block and a candidate external caption before the caption pairing becomes uncertain. |
| `caption_pairing_min_confidence` | 0.80 | `pipeline.toml` | Minimum confidence for an automatic figure/table caption pairing. |
| `equation_symbol_coverage_min` | 0.95 | `pipeline.toml` | Minimum visible-symbol coverage for accepting a LaTeX or verbatim equation transcription without review. |
| `visual_stitching_max_page_gap` | 1 | `pipeline.toml` | Maximum source-page gap for automatic multi-page visual stitching candidates. |
| `visual_asset_crop_iou_min` | 0.98 | `pipeline.toml` | Minimum IoU between final asset geometry and source crop geometry for visual asset QA to pass. |
| `visual_asset_blank_border_warn_ratio` | 0.15 | `pipeline.toml` | Blank-border ratio above which visual asset QA warns that the crop may include excess whitespace or neighboring content. |
| `material_conflict_prose_similarity_min` | 0.98 | `pipeline.toml` | Minimum normalized similarity for an independent prose candidate not to block consensus acceptance as a material conflict. |
| `evidence_packet_text_token_budget_default` | 12000 | `pipeline.toml` | Default text-token budget for a judge evidence packet before any role/backend-specific context profile supplies a narrower value. The budget covers candidate spans, lineage summaries, glossary context, nearby context, and omitted-evidence manifests; schema instructions and stable prompt prefix are budgeted separately. |
| `evidence_packet_image_crop_budget_default` | 4 | `pipeline.toml` | Default maximum rendered crops attached to one judge evidence packet before role/backend-specific context or modality policy supplies a narrower value. Required disputed-span crops are counted first. |
| `judge_confidence_calibration_min_gold_decisions` | 20 | `pipeline.toml` | Minimum role/backend decisions against approved gold-set truth before the pilot may treat a numeric confidence calibration as statistically usable. Smaller samples still record observations, but thresholds remain conservative unless the reviewer explicitly approves an override. |
| `page_furniture_repetition_min_pages` | 3 | `pipeline.toml` | Minimum distinct pages on which a normalized candidate header/footer/page-number string must recur before deterministic page-furniture removal may consider it. |
| `page_furniture_repetition_min_fraction` | 0.60 | `pipeline.toml` | Minimum fraction of applicable pages in the chapter or repeated-section group on which a candidate page-furniture string must recur before automatic removal. |
| `page_furniture_vertical_band_fraction` | 0.08 | `pipeline.toml` | Top and bottom page-height band used for running-header/footer detection. Standalone page numbers may use the same bands or the outer margin band when geometry evidence shows they are not body text. |
| `page_furniture_geometry_tolerance_points` | 12.0 | `pipeline.toml` | Maximum median-position drift for repeated page-furniture candidates after rotation and page-size normalization. |
| `page_furniture_typography_similarity_min` | 0.90 | `pipeline.toml` | Minimum similarity between normalized font-size/style/weight features for repeated page-furniture candidates before automatic removal. |
| `list_classifier_indent_tolerance_points` | 12.0 | `pipeline.toml` | Maximum layout-indentation drift for treating adjacent lines as belonging to the same list level during Markdown assembly. |
| `table_classifier_column_tolerance_points` | 8.0 | `pipeline.toml` | Maximum x-position drift for treating aligned cell boundaries as one table column during Markdown assembly. |
| `soft_hyphen_join_unsplit_occurrence_min` | 2 | `pipeline.toml` | Minimum independent unsplit occurrences needed to prove a discretionary line-break hyphen join when glossary approval or direct engine agreement is absent. |
| `pilot_feature_stratum_min_pages` | 2 | `pipeline.toml` | Default minimum pages selected for each present pilot feature stratum before the sampler fills to the upper pilot size. |
| `pilot_rare_feature_stratum_min_pages` | 1 | `pipeline.toml` | Minimum pages selected for a present feature stratum that has fewer available candidates than `pilot_feature_stratum_min_pages`. |
| `runtime_outlier_mad_multiplier` | 3.0 | `pipeline.toml` | Starter operational-metric warning threshold for per-page runtime outliers relative to the pilot median absolute deviation. |
| `duplicate_content_window_chars` | 400 | `pipeline.toml` | Character-window size for adjacent-chunk duplicate-content QA after low-risk normalization and excluded-block filtering. |
| `duplicate_content_similarity_threshold` | 0.98 | `pipeline.toml` | Normalized similarity at or above which adjacent-chunk windows are treated as duplicate candidates after exact-hash precheck. |
| `reading_order_pairwise_warn_min` | 0.98 | `pipeline.toml` | Warning threshold for Phase 10 pairwise reading-order QA on multi-column-risk pages. Blocking threshold remains governed by the approved metric policy. |
| `candidate_chars_per_source_page_warn_min` | 200 | `pipeline.toml` | Starter smoke-check warning floor for accepted source-derived character count per nonblank, text-bearing source page when no pilot baseline is available. |
| `accepted_blocks_per_source_page_warn_min` | 1 | `pipeline.toml` | Starter smoke-check warning floor for accepted source-derived block count per nonblank, text-bearing source page when no pilot baseline is available. |
| `source_derived_block_ratio_warn_min` | 0.80 | `pipeline.toml` | Starter smoke-check warning floor for the ratio of accepted source-derived blocks to all non-generated final Markdown blocks in a chapter. |
| `candidate_density_pilot_deviation_warn_ratio` | 0.50 | `pipeline.toml` | Warn when a chapter's character or accepted-block density falls below this fraction of the matching pilot stratum baseline without an approved exception. |
| `outline_weight_pdf_outline` | 0.6 | `pipeline.toml` | Phase 3 outline-confidence weight for the PDF outline-entry signal. |
| `outline_weight_typography` | 0.3 | `pipeline.toml` | Phase 3 outline-confidence weight for the heading-typography agreement signal. |
| `outline_weight_toc` | 0.1 | `pipeline.toml` | Phase 3 outline-confidence weight for the parsed-TOC agreement signal. The three weights sum to 1.0 by construction; the agent rejects a `pipeline.toml` whose three outline weights do not sum to exactly 1.0 (subject to a `1e-6` tolerance for float-representation noise). |
| `text_layer_signal_sample_size` | 20 | `pipeline.toml` | Phase 3 sample size for `visual_text_alignment_score`. The sampling RNG is seeded deterministically from `source_pdf_sha256` and `source_page_index`. |
| `response_cache_size_cap` | 0 (no cap) | `pipeline.toml` | Bytes; `0` disables the cap. Positive values accept case-insensitive binary suffix parsing (`K`, `KB`, `KiB`, `M`, `MB`, `MiB`, `G`, `GB`, `GiB`, `T`, `TB`, `TiB`; powers of 1024). |
| `response_cache_eviction_policy` | `lru` | `pipeline.toml` | `lru`, `lfu`, or `none`. |
| `response_cache_scope` | `folder` | `run_config.json` | `folder` (shared across runs of the same document) or `run`. |
| `render_profile_id_truncation_chars` | 12 | `pipeline.toml` | SHA-256 prefix used as id. |
| `fuzzy_consensus_min_chars` | 40 | `pipeline.toml` | Phase 8 minimum normalized span length before `consensus_with_normalization` may accept a non-protected span by fuzzy similarity rather than routing it to judge/manual review. |
| `judge_low_confidence_review_threshold` | 0.85 | `pipeline.toml` | Phase 8 threshold below which a judge-accepted span is still wrapped in a searchable review marker unless a role-specific tool-profile threshold overrides it. |
| `default_render_dpi` | 300 | `config/render_profiles/default-300dpi.yaml` | Operators may install higher-DPI alias profiles. |
| `confidence_scale` | float `[0.0, 1.0]` | schema-wide | Producer records `confidence_method`. |
| `output_root` | `./output` | `pipeline.toml` (run override) | Root directory under which the runtime writes the deliverable folder `<output-folder-name>/`. Per-run override via `run_config.output_root` or `--output-root`. The runtime never commits the deliverable. |
| `lfs_max_file_bytes` | `2147483648` (2 GiB) | `pipeline.toml` (run override) | Per-file Git LFS size threshold deciding whether the original PDF is page-split for commit-readiness. Per-run override via `run_config.lfs_max_file_bytes`. Default matches GitHub Free/Pro (2 GiB); raise for Team (4 GiB), Enterprise Cloud (5 GiB), or other hosts/limits only after Phase 0 records the exact host, plan, and evidence source used to verify the byte limit. |
| `restricted_diversity_acknowledgment_default` | `false` | `run_config.json` | Default disallows the relaxed diversity rule. |
| `unknown_cost_acknowledgment_default` | `false` | `run_config.json` | Default disallows backends declared `cost_tracking.mode: unknown`. When granted, invocation and runtime ceilings are mandatory for those backends. The same mandatory-ceiling rule also applies to any backend declared `cost_tracking.mode: subscription_credit_pool`, independent of this acknowledgment. |
| `chapter_max_unresolved_review_markers` | 5 | `pipeline.toml` | Chapter-QA cap on review markers that are not high or normal severity (those classes already block independently). |
| `chunk_id_min_capacity` | 99 | `pipeline.toml` | Minimum per-chapter chunk-id capacity used when computing `chunk_id_width`; wider ids are used automatically when the initial planned chunk count exceeds this value. |
| `figure_crop_padding_points` | 8.0 | `pipeline.toml` | Default padding around figure, table, diagram, and equation crops, expressed in PDF user-space points. |
| `figure_crop_padding_pixels` | 24 | `pipeline.toml` | Default padding around the same crops, expressed in pixels at the active `render_profile_id`. The runtime resolves the larger of the two when the two values disagree after conversion. |
| `diagram_text_policy_default` | `crop_only` | `run_config.json` | Default per-chunk handling for interior diagram text. Supported values are `crop_only` and `transcribe`; operators may override this at init or per chunk. |
| `delegates_to_max_chain_length` | 1 | `pipeline.toml` | An engine may delegate to at most one other engine; deeper chains and self-delegation fail at `noeasymark doctor` time. |
| `outline_low_confidence_threshold` | 0.85 | `pipeline.toml` | Phase 3 floor for per-boundary outline-recovery confidence; below this value `outline_status` is `OUTLINE_LOW_CONFIDENCE` and `CHAPTER_MAP_REVIEW` gates the run. The conservative default matches the spec's heading-accuracy posture. |
| `text_layer_quality_glyph_good_max` | 0.02 | `pipeline.toml` | Phase 3 `glyph_anomaly_rate` ceiling that still maps to `quality_label: good`. Values >0.02 and <0.05 map to `mixed`; values >=0.05 map to `poor`. |
| `text_layer_quality_glyph_mixed_max` | 0.05 | `pipeline.toml` | Phase 3 `glyph_anomaly_rate` ceiling that still maps to `quality_label: mixed`; values at or above this map to `poor`. |
| `text_layer_quality_alignment_good_min` | 0.80 | `pipeline.toml` | Phase 3 `visual_text_alignment_score` floor for `quality_label: good`. Values in [0.50, 0.80) map to `mixed`; values <0.50 map to `poor`. Missing signal is treated as `good` and marks the label provisional. |
| `text_layer_quality_alignment_mixed_min` | 0.50 | `pipeline.toml` | Phase 3 `visual_text_alignment_score` floor for `quality_label: mixed`; values below this map to `poor`. |
| `text_layer_quality_conflict_good_max` | 0.10 | `pipeline.toml` | Phase 3 `ocr_conflict_score` ceiling for `quality_label: good`. Values >0.10 and <0.30 map to `mixed`; values >=0.30 map to `poor`. Missing signal is treated as `good` and marks the label provisional. |
| `text_layer_quality_conflict_mixed_max` | 0.30 | `pipeline.toml` | Phase 3 `ocr_conflict_score` ceiling for `quality_label: mixed`; values at or above this map to `poor`. |
| `artifact_bytes_warning_threshold` | 10 GiB | `pipeline.toml` | Soft warning when total per-run artifact disk usage crosses this size; produces a `status` warning before render or OCR batches. Distinct from the optional hard `artifact_bytes_ceiling` in `run_config.json`. |
| `rendered_page_bytes_warning_threshold` | 3 GiB | `pipeline.toml` | Soft warning when total rendered-page bytes cross this size. Distinct from the optional hard `rendered_page_bytes_ceiling` in `run_config.json`. |
| `minimum_free_space_warning_threshold` | 10 GiB | `pipeline.toml` | Soft warning when free disk space at the artifact root drops below this floor at a phase boundary. Distinct from the optional hard `minimum_free_space_bytes` in `run_config.json`. |
| `artifact_path_warning_chars` | 240 | `pipeline.toml` | Soft warning threshold for fully resolved artifact path length, including the artifact root. Mirrors the existing output-path warning posture for Windows compatibility. |
| `artifact_filename_component_max_chars` | 96 | `pipeline.toml` | Maximum safe stem length before deterministic truncation for generated artifact filenames. Extensions, ids, and collision suffixes are counted outside this stem. |
| `artifact_hash_shard_prefix_chars` | 4 | `pipeline.toml` | Number of lowercase SHA-256 prefix characters used for two-level artifact directory sharding (`aa/bb`). |
| `ocr_policy_default` | `conditional_redo_then_force` | `pipeline.toml` | Phase 3 OCR policy default per the prose. Supported values are `conditional_redo_then_force` (default; redo when `text_layer_quality` is `poor`/`mixed`/`unknown`, force when redo still leaves glyph anomalies above `force_ocr_glyph_anomaly_threshold`), `always_redo`, `always_force`, `always_redo_then_force`, and `none`. |
| `lock_backend` | `filelock` | `pipeline.toml` | Single-writer lock implementation. Supported values are `filelock` (default; exclusive-only OS advisory lock) and `portalocker` (when shared-reader semantics are needed). Bumping this is a Phase 0-class change and re-runs `doctor`. |

Placement notes for constants whose storage path or run override path is not obvious from the Owner cell:

| Constant(s) | Repository default path | Per-run override path |
| --- | --- | --- |
| `overlap_divergence_threshold` | `pipeline.toml` `[overlap_divergence].threshold` | `run_config.overlap_divergence.threshold` |
| `overlap_divergence_profile_id` | `pipeline.toml` `[overlap_divergence].profile_id` plus the closed `[overlap_divergence].normalization_profile` object | `run_config.overlap_divergence.profile_id` only when the referenced profile exists in the same closed profile registry |
| `multi_column_x_jump_fraction` | `pipeline.toml` `[multi_column_detection].x_jump_fraction` | `run_config.multi_column_detection.x_jump_fraction` |
| `multi_column_y_band_points` | `pipeline.toml` `[multi_column_detection].y_band_points` | `run_config.multi_column_detection.y_band_points` |
| `multi_column_semantic_discontinuity_threshold` | `pipeline.toml` `[multi_column_detection].semantic_discontinuity_threshold` | `run_config.multi_column_detection.semantic_discontinuity_threshold` |
| `multi_column_cosine_window_chars` | `pipeline.toml` `[multi_column_detection].cosine_window_chars` | `run_config.multi_column_detection.cosine_window_chars` |
| `multi_column_cosine_ngram_size` | `pipeline.toml` `[multi_column_detection].cosine_ngram_size` | `run_config.multi_column_detection.cosine_ngram_size` |
| `output_root` | `pipeline.toml` `output_root` | `run_config.output_root` or `--output-root` |
| `lfs_max_file_bytes` | `pipeline.toml` `lfs_max_file_bytes` | `run_config.lfs_max_file_bytes` |
| `response_cache_scope` | No repository default file; initialized directly in `run_config.response_cache_scope`. | `run_config.response_cache_scope` |
| `default_render_dpi` | `config/render_profiles/default-300dpi.yaml` `dpi` | No scalar run override; install or choose another render-profile alias through `run_config.render_profile_alias_override` before pilot or tool-profile approval after pilot. |

Crop-padding rationale: `figure_crop_padding_points` protects padding in PDF source-coordinate space, while `figure_crop_padding_pixels` provides a rendered-pixel lower bound for profiles where point-to-pixel conversion would otherwise be too small. The runtime still applies whichever configured value produces the larger absolute crop box.

Hash truncation lengths are intentionally role-specific rather than globally uniform. `output_folder_collision_suffix_chars` is 6 because it is part of a human-facing folder name and is used only after a detected collision; `render_profile_id_truncation_chars` is 12 because render profiles are referenced across many artifacts; `tool_profile_hash_compact`-style log fields use 16 because they summarize a larger behavioral surface. Any truncated hash field must name the truncation convention in the field name or schema description and must retain a sibling full digest when it is used for audit rather than filename collision avoidance.

## Identifier Conventions

The pipeline uses a small set of stable identifiers across schemas, command-line arguments, file paths, and audit records. Implementations follow these forms exactly so that the work queue, response cache, evidence sidecars, and operator tracking stay aligned.

- `output-folder-name` is derived per the rules in **Closed Handoff Decisions**. It is the user-visible handle for a document and the root of every reviewable path. `noeasymark init` writes a folder-name derivation record in folder state with the normalized title, transliteration library and version, truncation decision, collision probe result, `collision_suffix_kind` (`none`, `source_fingerprint`, `title_hash`, or `numeric`), suffix value when present, and source PDF hash when available. This makes a 6-character source-fingerprint suffix visually distinguishable from a 6-character title-hash suffix during review.
- `run_id` has a base grammar `<output-folder-name>-<UTC yyyymmdd-HHMMSS>-<6-char random>` and an extended grammar `<output-folder-name>-<UTC yyyymmdd-HHMMSS>-<6-char random>-<UTC mmm>` (3-digit zero-padded millisecond suffix), produced under the collision-retry rule described in **Closed Handoff Decisions**. Both grammars are valid `run_id` shapes that every command, schema, branch-formation rule, and parser must accept.
- `chapter_id = ch<NN>` where `<NN>` is the zero-padded chapter index. The width is computed once at chapter-manifest creation time as `max(2, number_of_digits(proposed_chapter_count))`, recorded as `chapter_id_width` in the chapter manifest, and never changes for the life of the output folder. A later re-run whose proposed chapter count would require a wider id stops with a usage-error exit so the operator can either rescope the proposed chapter map or perform a controlled rename of every existing chapter id rather than letting different chapters use different widths. The initial NoEasyMark release intentionally does not ship a `noeasymark rename-chapters` subcommand because cross-record id width promotion is a destructive, hard-to-validate operation; the documented remediations are (a) re-run `noeasymark chunk` with a manually-edited chapter manifest whose chapter count fits the existing width, or (b) `noeasymark init --fresh` against the same source with a different `--output-folder-name` and re-do conversion. A future release may add a `rename-chapters` subcommand if real conversion experience produces a recurring need, but it is not authorized by this specification.
- Chapter indexes are 1-based. Index 0 is reserved permanently for non-chapter front matter emitted as `markdown/00-front-matter.md`; no `chapter_manifest` may use `chapter_index: 0` or `chapter_id: ch00`.
- `chunk_id = <chapter_id>-chk<MM>`, for example `ch01-chk03`. `<MM>` width is computed at the first chunk-planning pass for the chapter as `max(2, number_of_digits(max(chunk_id_min_capacity, planned_chunk_count)))`, recorded as `chunk_id_width` in the chapter manifest alongside `chapter_id_width`, and is stable for the life of the output folder. `planned_chunk_count` is the actual number of chunks in the accepted initial chunk plan, after hard-maximum splits and manual chapter-map edits. A later re-run whose proposed chunk count would require a wider id stops with a usage-error exit so the operator can either rescope chunking or perform a controlled rename rather than letting different chunks use different widths. `chunk_id` is the operator-facing handle for a processing chunk and is the argument to `noeasymark convert`, `noeasymark figures`, and `noeasymark judge`.
- `unit_id = <unit_type>:<scope_id>` is the work-queue identifier consumed by the orchestrator. `<unit_type>` is one of `init`, `preprocess`, `chunk`, `segment`, `convert`, `figures`, `judge`, `glossary_build`, `glossary_apply`, `pilot`, `publish_chapter`, `finalize`, or `migrate`. `<scope_id>` matches the kind of unit: chunk-scoped units use the chunk_id; chapter-scoped units use the chapter_id; whole-run units use the run_id; the migration unit uses the schema id and version range. Each work-queue entry stores `unit_id` and may also store `chunk_id` or `chapter_id` for convenient lookup.
- Asset filenames use `images/<chapter_id>-page-<PPPP>-<kind>-<II>[-<hash>].png` where `<PPPP>` is the zero-padded `source_page_index` of the asset's primary page using width `max(4, number_of_digits(page_count))`, computed deterministically at preprocess time, and `<II>` is a zero-padded two-digit per-page index of the asset within that page. The optional collision suffix starts as the first 6 lowercase hexadecimal characters of the SHA-256 of the asset's primary `region_id` and is extended by two hexadecimal characters at a time until the otherwise-formed filename is unique within the chapter.
- Chapter Markdown filenames use `markdown/<chapter_id_digits>-<chapter-or-section>.md` where `<chapter_id_digits>` is the numeric portion of `chapter_id` (for example `01` for `ch01`, `001` for `ch001`) and `<chapter-or-section>` is the chapter slug. The reserved non-chapter front-matter file is the only committed Markdown file outside this chapter-derived pattern; it is always `markdown/00-front-matter.md`, has no `chapter_manifest`, and its evidence sidecar uses `chapter_id: null` rather than a synthetic `ch00`. The slug is derived from the outline node's title using the same NFKD normalization, `anyascii` transliteration, lowercase, hyphenation, OS-reserved-name guard, and empty-result fallback rules as `output-folder-name`, but is capped at 40 characters to leave headroom under the worst-case path warning threshold. Chapter slug collision checks are case-insensitive within the output folder. On collision, append `-<4-char chapter-title hash>`; if still colliding, append `-<2-digit numeric suffix>` after trimming the base slug further so the whole slug remains at or below 40 characters. The chapter-title hash is the first 4 lowercase hexadecimal characters of the SHA-256 of the outline node title after NFC normalization plus the 1-based `chapter_index`, encoded as canonical JSON. The slug and its collision derivation record are stored in the chapter manifest and never change after chapter publication. Operator-facing commands that take a chapter handle (such as `noeasymark publish-chapter`) accept the same `chapter_id` form used elsewhere in the spec (`ch01`, not the digits-only `01`); the digits-only form appears only inside committed filenames.
- **Deterministic-prefix identifier table.** Every project-prefixed deterministic id uses a table prefix that already includes its trailing hyphen; the emitted form is `<prefix><14-char lowercase hex>` as described in **Phase 0**. The fixed table below keeps schemas, filenames, and cross-record references aligned across modules. New schemas that need a deterministic id pick a previously-unused prefix and update this table in the same PR.

  | Schema and field | Prefix | Example |
  | --- | --- | --- |
  | `candidate_output.candidate_id` | `co-` | `co-3f7a1e9b2c4d80` |
  | `candidate_output.spans[].span_id` | `cs-` | `cs-3f7a1e9b2c4d80` |
  | `candidate_lineage.lineage_id` | `cl-` | `cl-3f7a1e9b2c4d80` |
  | `crop_record.crop_id` | `cr-` | `cr-3f7a1e9b2c4d80` |
  | `crop_record.crop_ref_id` (cross-record pointer to a `crop_record`) | `crf-` | `crf-3f7a1e9b2c4d80` |
  | `layout_region.region_id` | `rg-` | `rg-3f7a1e9b2c4d80` |
  | `extra_evidence.extra_evidence_id` | `xev-` | `xev-3f7a1e9b2c4d80` |
  | `adjudication_record.adjudication_id` | `ad-` | `ad-3f7a1e9b2c4d80` |
  | `triage_decision.triage_id` | `tr-` | `tr-3f7a1e9b2c4d80` |
  | `audit_decision.audit_id` | `au-` | `au-3f7a1e9b2c4d80` |
  | `review_marker.review_marker_id` | `rv-` | `rv-3f7a1e9b2c4d80` (machine-emitted markers use the deterministic `rv-<14-char>` form; a hand-authored review marker may instead use any grammar-valid `bare_token` id, which the parser accepts and `evidence_sidecar.review_marker_ids` references verbatim) |
  | `evidence_sidecar.block_entry.block_id` | `bk-` | `bk-3f7a1e9b2c4d80` |
  | `figure_record.figure_id` | `fg-` | `fg-3f7a1e9b2c4d80` |
  | `table_record.table_id` | `tb-` | `tb-3f7a1e9b2c4d80` |
  | `equation_record.equation_id` | `eq-` | `eq-3f7a1e9b2c4d80` |
  | `gate_record.gate_id` | `gt-` | `gt-3f7a1e9b2c4d80` |
  | `manual_review_input.manual_review_id` | `mr-` | `mr-3f7a1e9b2c4d80` |
  | `manual_review_waiver.waiver_id` (and all sibling waivers: `independent_auditor_waiver`, `mini_pilot_waiver`) | `wv-` | `wv-3f7a1e9b2c4d80` |
  | `pilot_plan.pilot_plan_id` | `pp-` | `pp-3f7a1e9b2c4d80` |
  | `gold_set.gold_set_id` | `gs-` | `gs-3f7a1e9b2c4d80` |
  | `gold_set.entries[].entry_id` | `gse-` | `gse-3f7a1e9b2c4d80` |
  | `glossary_entry.entry_id` | `ge-` | `ge-3f7a1e9b2c4d80` |
  | `response_cache_entry.entry_id` | `rc-` | `rc-3f7a1e9b2c4d80` |
  | `normalized_failure_event.event_id` | `fe-` | `fe-3f7a1e9b2c4d80` |
  | `work_queue_event.event_id` | `wqe-` | `wqe-3f7a1e9b2c4d80` |

  Deterministic-prefix ids use the schema-wide parent-context-plus-position preimage unless a row below names a producer-specific preimage:

  | Field | Hash preimage |
  | --- | --- |
  | `gate_record.gate_id` | Canonical JSON of `gate_name`, `scope`, `opened_at`, `opened_by_unit_id`, and the enclosing `run_id` or development-state gate scope. |
  | `response_cache_entry.entry_id` | Canonical JSON of `cache_key`, `response_cache_scope` (`folder` or `run`), `backend_id`, `model_id`, `response_schema_id`, and `response_schema_version`. |
  | `normalized_failure_event.event_id` | Canonical JSON of `unit_id`, `attempt_index`, and `occurred_at`, matching the failure-classification contract. |
  | `work_queue_event.event_id` | Canonical JSON of `unit_id`, `occurred_at`, `transition`, `actor`, and the event's zero-based ordinal in `work_queue_events.jsonl`. |
  | `review_marker.review_marker_id` | Machine-emitted markers use canonical JSON of `chapter_id`, `chunk_id`, `marker_source`, `severity`, the primary source span or block refs, and the marker emission index; hand-authored markers remain verbatim grammar-valid `bare_token` ids. |

  `work_unit.unit_id` does not use the deterministic-prefix form; it uses the `<unit_type>:<scope_id>` form defined earlier in this list. `run_id`, `chapter_id`, and `chunk_id` follow their own grammar described above.

## Plain-Language Inputs

The tool should ask for only simple operator inputs.

Required inputs:

- Source PDF path.
- Document title.
- Local processing acknowledgment: confirmation that the operator wants the workflow to process the PDF locally, subject to the operator's rights, licenses, and source-document terms.

Optional inputs:

- One-sentence topic description.
- Output folder name override.
- Optional hard spend ceiling (enforced only for backends with dollar-denominated cost estimates).
- Optional invocation ceilings or backend-runtime ceilings (`runtime_ceiling_seconds`) for subscription, credit-pool, or acknowledged unknown-cost backends.
- Optional hard wall-clock ceiling.
- Optional hard minimum-free-space floor.
- Known pilot pages (`source_page_index`, 1-based physical PDF page order; printed page labels are not accepted here). This is optional; if omitted, the tool auto-selects representative pilot pages.
- Seed glossary terms.
- Default diagram text policy (`crop_only` or `transcribe`).
- Render-profile alias override; omitted means use `config/render_profiles/default-300dpi.yaml`.
- Per-run OCR policy override; omitted means use `pipeline.toml`'s `ocr_policy_default`.
- Per-run OCR languages; omitted means use `pipeline.toml`'s `ocr_languages_default`.
- Perimeter processing approval: whether backends or engines declared as `network_class: perimeter` may receive source-derived content, paired with either `all` or a structured allowlist scoping which perimeter components are approved.
- Internet processing approval: whether backends or engines declared as `network_class: internet` may receive source-derived content, paired with either `all` or a structured allowlist scoping which internet components are approved.
- Output location (`output_root`, default `./output/`): the directory under which the commit-ready deliverable folder `<output_root>/<output-folder-name>/` is written. The runtime writes the folder but never commits it.
- External artifact archive approval: whether source-derived artifacts may be persistently stored outside the device in an approved artifact archive, paired with either `all` or a structured allowlist scoping which archive targets are approved.
- Maximum committed-file size (`lfs_max_file_bytes`, default 2 GiB; raise for hosts or plans with larger Git LFS per-file limits): when the original PDF exceeds this size, the deliverable's `original/` directory holds page-split parts (`source.part-NNN.pdf`) plus a `source.split-manifest.json` reassembly record instead of a single `source.pdf`, so a later operator commit stays within the host's limit.
- Restricted-diversity acknowledgment: whether the operator accepts a relaxed diversity rule when content-handling restrictions or local engine availability prevent assembling two engine families. Default is no.
- Unknown-cost acknowledgment: whether the operator accepts the use of any backend whose configuration declares `cost_tracking.mode: unknown`. Default is no. When granted, `invocation_ceiling` and `runtime_ceiling_seconds` become mandatory for those backends so that opaque-cost calls cannot run unbounded. The same per-run `invocation_ceiling` and `runtime_ceiling_seconds` requirement applies whenever any enabled or tool-profile-bound backend declares `cost_tracking.mode: subscription_credit_pool`, regardless of the unknown-cost acknowledgment, because subscription-pool consumption can run silently expensive when the vendor exposes no per-call credit signal.
- Approval audit identity, role, and rationale for approval or denial decisions that will be persisted. Interactive prompts collect these fields when they are missing; non-interactive invocations supply them through the shared approval audit flags or an imported init bundle.
- Optional pre-pilot backend preferences by role when the operator already knows a preferred backend. These are soft hints in `pre_pilot_backend_preferences`; pilot validation finalizes the active judge bindings in `tool_profile.json`.

Approval persistence:

- External artifact archive identity persists at folder scope in the private internal record `.noeasymark-state/_folders/<output-folder-name>/approvals.json`.
- The runtime also writes a redacted copy into the deliverable at `<output_root>/<output-folder-name>/.noeasymark/approvals.json` using the `approvals_record_public` schema. The public copy is derived output, not the canonical approval record.
- Perimeter, internet, restricted-diversity acknowledgment, and unknown-cost acknowledgment live in per-run `run_config.json`.
- `noeasymark init` displays the carried-forward folder-scope approvals and requires the operator to re-confirm them before they take effect for the new run.

Definitions:

- **Output folder name** means a short folder-safe name used in paths, for example `national-electric-code-handbook` or `comed-service-meter-2025`. The tool auto-generates it from the document title using the canonical algorithm in **Identifier Conventions**, including normalization, transliteration, reserved-name rejection, collision suffixing, and derivation records, unless the operator supplies an override. The tool never silently renames, merges, or reuses an existing output folder.
- Avoid the term `document slug` in user-facing prompts, reports, and setup instructions.
- **Run identity.** Base grammar is `<output-folder-name>-<UTC yyyymmdd-HHMMSS>-<6-char random>`; the extended-grammar form `<output-folder-name>-<UTC yyyymmdd-HHMMSS>-<6-char random>-<UTC mmm>` is produced only when the base form collides on disk and is recognized by every `run_id` parser in the runtime. `noeasymark init` writes a pointer file at `.noeasymark-state/active_run` that resolves the default run for later commands. Every command accepts `--run-id <id>` to override the pointer. `init --fresh` creates a new run when one already exists for the same folder; `init --reuse <id>` attaches to an existing run by repointing the pointer, displaying its carried-forward approvals advisorily, and never modifying the existing run's `run_config.json` or folder-scope approvals.

Source PDF path handling:

- The operator may supply a repository-relative path, an absolute local path, or a path outside the repository; the resolved schema `path_kind` is one of `repo_relative`, `absolute_in_repo`, `absolute_external`, or `relative_external`. `noeasymark init` computes the source PDF SHA-256 from the supplied file and copies the bytes into the run's canonical working original at `.noeasymark-artifacts/<run_id>/original/source.pdf` before any derived artifact is created. The original operator-supplied path is kept only in private internal state and never appears in the deliverable's public records.
- The same source bytes are also written into the deliverable at `<output_root>/<output-folder-name>/original/source.pdf` (Git LFS; page-split when over `lfs_max_file_bytes`). That deliverable copy is a reviewable copy of the canonical source bytes, not a replacement for the private run working copy. The runtime writes it but does not commit it.
- Public manifests identify the source by SHA-256 and by the deliverable-relative path. They do not expose an absolute local input path by default.
- A subsequent `noeasymark init --fresh` for the same document reuses `corpus/<output-folder-name>/source.pdf` when it exists rather than prompting for a new path; the operator-curated input set is the natural source of truth for repeated runs of the same document. If the operator wants to seed from a different file, supply `--source <path>` explicitly. The `corpus/<output-folder-name>/reference/` subdirectory remains read-only context for `PILOT_GLOSSARY_REVIEW`; the runtime never writes back into `corpus/`.

Example topic description:

```text
National Electrical Code Handbook; dense electrical systems code commentary, wiring methods, service equipment, grounding, bonding, tables, exceptions, and technical diagrams.
```

The topic description is context, not authority. The agent must still treat the rendered page image and source evidence as ground truth.

## Content Handling Modes

The tool records five separate choices. These choices are workflow guardrails, not legal advice.

| Choice | Meaning |
| --- | --- |
| Local processing acknowledgment | The operator confirms that the workflow may process the PDF on the local machine or devcontainer, subject to the operator's rights, licenses, and source-document terms. |
| Perimeter processing approval | The operator approves the use of components declared `network_class: perimeter`, such as an employer-internal Azure OpenAI tenant, an on-prem inference cluster, or any other backend/engine that is external to the host but inside a trusted network perimeter. The approval must explicitly choose either `all` or a structured allowlist. |
| Internet processing approval | The operator approves the use of components declared `network_class: internet`, such as public-internet endpoints, public API providers, or hosted Document AI services. The approval must explicitly choose either `all` or a structured allowlist. |
| External artifact archive approval | The operator confirms whether source-derived artifacts may be persistently stored outside the device in an approved artifact archive. The archive identity persists at folder scope; the network-class approval for the archive is recorded per run. |
| Restricted-diversity acknowledgment | The operator confirms whether the pipeline may accept the relaxed engine-diversity rule (two distinct engines from a single family) when content-handling restrictions or local engine availability prevent assembling two engine families. Per run, defaults to no; when granted, accepted spans carry the `RESTRICTED_DIVERSITY` review flag and are enumerated in the chapter report. |

Source-derived content includes the source PDF, normalized PDFs, OCR-derived PDFs, rendered page images, crops, extracted text, candidate Markdown, final Markdown, figure/table/equation assets, manual review files, prompts or model inputs that include source text or images, raw model responses, model logs containing source evidence, adjudication records with source excerpts, and reports or review surfaces that quote or render source content. Hashes, relative paths, aggregate metrics, retry signatures, and redacted tool-profile fields are not source-derived content unless they reveal source text or sensitive document metadata in context.

`network_class` values mean:

| Network Class | Meaning |
| --- | --- |
| `device` | Runs on the same host/devcontainer without sending source-derived content outside that boundary. Examples include local Tesseract, local PyMuPDF, or a same-device local model server. Loopback HTTP, Unix sockets, named pipes, or equivalent same-host IPC remain `device` only when traffic never leaves the host/devcontainer and any server-side persistence stays within the declared local state, artifact, or output paths. |
| `perimeter` | Sends source-derived content outside the host but inside a trusted private perimeter, such as a private VNet endpoint, on-prem inference cluster, or approved internal tenant boundary. |
| `internet` | Sends source-derived content to a public-internet or otherwise not-clearly-private endpoint. If unsure whether a component is truly `device` or `perimeter`, classify it as `internet`. |

If neither perimeter nor internet processing is approved, the pipeline runs in **local-processing-only mode**. Only components declared `network_class: device` are eligible. Cloud OCR, hosted Document AI, and remote judge backends are disabled. If no usable local judge backend is available for a configured role, unresolved disputed spans become `MANUAL_REVIEW`.

If only perimeter is approved, internet-class components are disabled and vice versa. The operator may approve both classes for maximum backend flexibility.

The pipeline always writes its source-derived deliverables to the local `output_root` (default `./output/`) as a commit-ready folder and never commits them. There is no repository mode and no repository-storage approval.

If external artifact archive approval is not granted, the pipeline may still use local gitignored artifact storage. It must not upload source-derived artifacts, caches, logs, crops, reports, review HTML, or model-call transcripts to cloud object storage, shared file storage, or any other persistent external archive. If external artifact archive approval is granted, the approved archive must also pass the relevant `network_class` approval and allowlist check. The archive identity and retention expectation are recorded in the folder-scope approvals record and archive configuration; `run_config.content_handling.external_artifact_archive` records the folder approval reference plus this run's mode and allowlist.

Provider-side transcript or cache retention disclosed by a backend or engine configuration is governed by that component's `network_class`, allowlist match, `transcript_persistence`, `session_persistence`, and vendor-cache declarations, not by the external artifact archive approval. By contrast, any NoEasyMark-controlled persistent upload or copy to an object store, shared filesystem, artifact archive, or other external storage target requires the external artifact archive approval chain above. `doctor --backends` and `doctor --artifacts` surface the two classes separately so an operator can approve a provider call without also approving NoEasyMark-managed artifact uploads.

The conversion runtime writes no remote discussion surfaces — no GitHub issues, PR comments, or review comments. Every review surface (the local tracking file, the chapter-review HTML, and manual-review files) is a local file under `output_root` and is part of the local deliverable, so it may contain source-derived content. Nothing is uploaded by the runtime; the operator controls any later distribution and remains responsible for their rights, licenses, and source-document terms when they do so.

### Allowlist Format

Each per-run content-handling approval record under `run_config.content_handling` (the `perimeter`, `internet`, and `external_artifact_archive` network-class records) is persisted as a structured record:

- `granted` (boolean, required): `true` when the operator approved the class, `false` when declined.
- `mode` (enum `all|allowlist`, required when `granted: true`, absent when `granted: false`):
  - `mode: all` approves every configured component in the relevant class. `noeasymark init` snapshots what `all` resolved to at decision time, and `noeasymark doctor` lists the current effective set so the operator can see any drift.
  - `mode: allowlist` approves only the entries listed in `allowlist`.
- `allowlist` (array of structured entries, required when `mode: allowlist`, absent otherwise).
- `resolved_component_snapshot` (array of structured component references, populated for new `mode: all` records and otherwise optional only for migrated older records): the exact component set that `mode: all` resolved to when the operator made the decision. Each entry records `kind`, `identifier`, `network_class`, and a configuration content hash or version when available. If the current configured set for the class differs from the snapshot, `doctor` requires re-confirmation before the approval can be reused.
- `decision_at` (UTC ISO 8601 ms, required): when the approval or denial was recorded.
- `decided_by` (string, required): operator identity (login or initials) at decision time.
- `decided_by_role` (enum `maintainer|delegated_reviewer`, required): role used for public-schema redaction.
- `rationale` (string, required): free-text justification recorded with the decision.

The folder-scope external artifact archive identity approval is a separate `approvals_record` entry under `.noeasymark-state/_folders/<output-folder-name>/approvals.json`. It owns `archive_id`, `retention_expectation`, grant metadata, and the durable approval/decline decision, and it has no `mode` or `allowlist`. The per-run `content_handling.external_artifact_archive` record above gates whether this run may use the already-approved archive endpoints and references the folder-scope record through `folder_approval_reference`.

CLI form `--perimeter-mode {all|allowlist|deny}` maps to the persisted record as `--perimeter-mode all` -> `granted: true, mode: all`; `--perimeter-mode allowlist` -> `granted: true, mode: allowlist, allowlist: [...]`; `--perimeter-mode deny` -> `granted: false`. The internet mode flag uses the same mapping. External artifact archives have two decisions: `--external-artifact-archive {approve,decline,prompt}` controls the folder-scope archive identity and retention approval, while `--external-artifact-archive-mode {all|allowlist|deny}` controls the per-run `content_handling.external_artifact_archive` network-class record with the same mode mapping as perimeter and internet. An approved folder-scope archive with per-run archive mode `deny` remains recorded but is unusable for that run. Allowlist-entry flags accept one structured JSON object per occurrence, or an `@<path>` reference to a JSON or TOML file containing one entry or an array of entries. For convenience, a bare `kind:identifier` value is accepted only for entries with no `scope` and no `notes`; the runtime expands it to `{ "kind": "<kind>", "identifier": "<identifier>" }` before validation. Non-interactive init invocations that supply any approval or mode decision flag must also supply `--approval-decider <identity>`, `--approval-decider-role {maintainer,delegated_reviewer}`, and `--approval-rationale <text>`; `init` copies those audit fields into every generated content-handling decision record unless an imported bundle supplies per-record audit fields. When `--external-artifact-archive approve` is used, the folder-scope archive grant defaults `granted_at` to the same timestamp, `granted_by` to the same identity, and `granted_by_role` to the same role as the decision unless archive-specific grant fields are supplied by the imported bundle. The init_bundle discriminated-union form `{decision: approve|decline|prompt}` (with `mode`, `allowlist`, `decided_by`, `decided_by_role`, and `rationale` nested under concrete approve/decline decisions) is expanded by `noeasymark init` into the persisted record above; `{decision: prompt}` falls through to interactive collection. The `perimeter` and `internet` network classes each carry exactly one such object. The external artifact archive carries two parallel decisions in the bundle, mirroring its two CLI flags: an `external_artifact_archive_identity` decision (`{decision: approve|decline|prompt}` with `archive_id`, `retention_expectation`, `granted_by`, `granted_by_role`, `granted_at`, and `rationale`, and no `mode`/`allowlist`) that `init` expands into the folder-scope `approvals_record` under `.noeasymark-state/_folders/<output-folder-name>/approvals.json`, and a separate `external_artifact_archive` per-run decision (the same `{decision, mode, allowlist, decided_by, decided_by_role, rationale}` shape as the other network classes, plus `folder_approval_reference`) that `init` expands into the per-run `content_handling.external_artifact_archive` record. This lets a non-interactive import approve an archive identity while independently denying or allowlisting any given run; `prompt` on either decision falls through to interactive collection for that decision only. For `mode: all`, `init` derives `resolved_component_snapshot` after loading the current committed component configs; imported bundles may not claim a broader snapshot than current config resolves to. `noeasymark doctor` refuses any persisted approval that has `granted: true` without a `mode`, has `mode: allowlist` without a populated `allowlist`, has `mode: all` with a missing or stale `resolved_component_snapshot`, has any missing identity/role field, or has any field with an empty `rationale` (an approval cannot be implicitly empty and an audit chain cannot be implicitly anonymous).

When `mode: allowlist` is selected, the allowlist is a sequence of structured entries. Each entry has:

- `kind`: one of `litellm_provider`, `cli_agent_backend`, `engine`, `artifact_archive`, or `endpoint`.
- `identifier`: the LiteLLM provider identifier, CLI agent backend YAML name, engine YAML name, artifact archive name, or named endpoint registered in configuration.
- `scope` (optional): a sub-scope record matching the `endpoint_identifiers` schema declared by the component. See **Endpoint Identifier Schema**.
- `notes` (optional): free-text annotation visible in audit reports.

Identifier normalization is declared by component configuration rather than inferred by adapters:

| Allowlist `kind` | Identifier source | Normalization rule |
| --- | --- | --- |
| `litellm_provider` | LiteLLM backend `provider` field | Use the backend's `provider_identifier_case_policy` (`case_sensitive` or `case_insensitive`). |
| `cli_agent_backend` | CLI backend configuration id / YAML name | Use the backend's `identifier_case_policy`; shipped examples default to `case_sensitive` unless the CLI vendor documents otherwise. |
| `engine` | `engine_id` | Use the engine's `identifier_case_policy`; shipped engine ids are case-sensitive lowercase ids. |
| `artifact_archive` | `archive_id` | Use the archive config's `identifier_case_policy`; shipped archive ids are case-sensitive lowercase ids. |
| `endpoint` | Named endpoint identifier or endpoint `scope` | Use the endpoint matcher's own canonicalization rules; hostname-like matchers use the host canonicalization below. |

Every backend, engine, and artifact archive configuration whose identifier can appear in an allowlist must declare the relevant identifier case policy. `noeasymark doctor` refuses a config that omits the policy or applies case-insensitive matching without an explicit declaration.

Allowlist matching is exact after applying the declared identifier policy and, for hostnames, the host canonicalization below. Host comparisons use a single canonical host form: parse URL values with `urllib.parse.urlsplit()` when the source value is a URL, read only the `.hostname` component, reject missing or unparsable hosts, drop userinfo and ports, IDNA-encode Unicode domain labels to ASCII/punycode, lowercase the result, and strip one trailing root dot. The runtime never writes an environment variable's raw URL value to state, logs, diagnostics, or public output; only the environment-variable name and canonical host may be recorded. Wildcards are not allowed unless the component configuration declares a `matcher_type` that explicitly supports them. A broad endpoint pattern such as `*.example.com` must be represented as a named endpoint with its matcher type, owner, and rationale in configuration before it can appear in an allowlist.

### Endpoint Identifier Schema

Each entry in a component's `endpoint_identifiers` block, and each entry in an allowlist's `scope` field, is a discriminated-union record:

- `matcher_type`: one of `hostname`, `hostname_suffix`, `tenant_id`, `deployment_name`, `container_name`, `endpoint_name`, or `env_url_host`.
- `value`: the typed value for the chosen matcher type. `hostname` requires a literal DNS name; `hostname_suffix` requires a leading-dot suffix (`.example.com`) and matches only canonical hosts ending at that DNS-label boundary, not the apex host or a lookalike such as `badexample.com`; `tenant_id`, `deployment_name`, `container_name`, and `endpoint_name` are exact strings; `env_url_host` requires the name of an environment variable whose runtime value the component parses as a URL, and the matcher applies against only the canonical host component. The environment-variable name itself is not source-derived and never carries a secret; the resolved host, not the raw URL, participates in allowlist matching, so an `env_url_host` entry is safe to commit even when the underlying URL is operator-private.
- `notes` (optional): free-text annotation visible in audit reports.

Wildcards are only honored where the matcher type explicitly supports them (currently `hostname_suffix`). `noeasymark doctor --backends` validates every identifier against the schema and against the component it scopes; for `env_url_host` entries, doctor additionally verifies that the named environment variable is set in the current process environment and that its value parses as a URL whose host can be extracted, and reports an actionable exit-code-3 error otherwise. A backend or engine that omits `endpoint_identifiers` may still appear in an allowlist by `kind`+`identifier` only; an allowlist entry that supplies a `scope` for such a component fails `doctor` because there is no identifier set against which the scope can be matched.

`noeasymark doctor --backends` and `noeasymark doctor` match each configured component's `network_class` and identity against the relevant approval state and reports any mismatch. The pipeline refuses to issue any call or archive upload to a component that fails the match.

## Final Output Shape

The tree below is the canonical target structure for deliverables. Existing converted-reference examples, when available, are illustrative references rather than prerequisites for understanding or implementing the output shape.

```text
<output_root>/<output-folder-name>/
  .gitattributes                  # deliverable-local Git LFS rules for PDFs and generated image assets
  README.md                       # deliverable overview: title, summary, navigation pointer to markdown/
  .noeasymark/
    approvals.json                # redacted approvals_record_public copy
  original/
    source.pdf                    # original PDF via Git LFS when within lfs_max_file_bytes
    source.part-NNN.pdf           # emitted instead of source.pdf when the source is page-split
    source.split-manifest.json    # reassembly record emitted with split source parts
  markdown/
    README.md                     # Markdown navigation index: chapter list, generation metadata
    00-front-matter.md            # source-document front matter when present (cover, copyright, TOC preface); omitted otherwise
    01-<chapter-or-section>.md    # see Identifier Conventions for the chapter slug
    02-<chapter-or-section>.md
    figures.md                    # emitted when figure records exist
    tables.md                     # emitted when table records exist
    equations.md                  # emitted when equation records exist
  images/
    ch01-page-0012-fig-01.png
  manifests/
    source-manifest.json           # redacted, share-safe copy of source-manifest fields
    outline.json                   # redacted, share-safe copy of the approved outline/chapter map
    custom-anchors.json            # emitted when explicit custom anchors are used
    external-links.json            # emitted when final deliverables contain approved external URLs
    chapters/
      chapter-01.json
    chunks/
      ch01-chk01.json
  evidence/
    00-front-matter.evidence.json # emitted when markdown/00-front-matter.md is emitted
    01-<chapter-or-section>.evidence.json
    figures.evidence.json          # emitted when markdown/figures.md is emitted
    tables.evidence.json           # emitted when markdown/tables.md is emitted
    equations.evidence.json        # emitted when markdown/equations.md is emitted
  glossary/
    candidates.yaml
    approved.yaml
  reports/
    pilot.json
    tool-profile.json
    chapter-01.json
    aggregate-quality.json        # machine-readable aggregate quality/progress report
    aggregate-quality.html        # final aggregate-quality review surface
    chapter-NN-review.html        # per-chapter review surface
    status-<run_id>.md            # local run-status surface for the active run
  reviews/
    manual/
      <chunk_id>.md               # one file per chunk routed to MANUAL_REVIEW
    waivers/
      <waiver_id>.json            # schema-valid manual-review waiver records
```

The `01`, `chapter-01`, `ch01`, and `chk01` fragments in the tree are representative examples. Their exact width follows the chapter-id, chunk-id, and filename width rules in **Identifier Conventions**, so very large documents widen these examples deterministically.

PDFs must not be committed as ordinary Git blobs. If the original PDF is committed, it must use Git LFS.

The `original/` directory always holds the source PDF in the deliverable: a single `source.pdf` (Git LFS) when the file is within `lfs_max_file_bytes`, or page-split `source.part-NNN.pdf` files plus a `source.split-manifest.json` reassembly record when it exceeds that size. The folder is written commit-ready; the runtime does not commit it. Normalized PDFs, OCR-derived PDFs, full-page renders, temporary crops, raw engine outputs, and raw model logs are not part of the committed deliverable tree unless a later artifact policy explicitly promotes a narrowly scoped item and records why it is safe to review.

NoEasyMark writes a `.gitattributes` file at the deliverable root so the folder remains commit-ready even if the operator copies it into another repository. The deliverable file uses the global text-normalization rule followed by the canonical binary-asset LFS block defined in **Phase 0** under `.gitattributes` normalization. This section intentionally does not repeat the full extension list; the Phase 0 block is the source of truth for tracked binary extensions and for the intentional `*.svg` exclusion.

The runtime never commits. It writes the deliverable to `output_root` and leaves committing to the operator, who is responsible for their rights, licenses, and source-document terms when they commit. No commit-creating subcommand exists; there is no repository-storage approval.

The pipeline always writes its deliverable to `output_root` (default `./output/`) as a commit-ready folder and never commits it; there is no separate repository mode and no repository-storage approval.

Keep pipeline code, schemas, prompts, judge backend configurations, engine configurations, artifact archive configurations, and reusable configuration in the repository. Keep bulky interim artifacts gitignored unless external artifact archive storage is explicitly approved and selected.

The repository layout matches the upstream `franklesniak/copilot-repo-template` template, with NoEasyMark-specific additions:

```text
pyproject.toml
pipeline.toml                 # repo-level limits and defaults
src/
  noeasymark/
tests/
schemas/
config/
  prompts/
    default_judge/
      v0001.md
    escalation_judge/
      v0001.md
    fallback_judge/
      v0001.md
    triage_model/
      v0001.md
    independent_auditor/
      v0001.md
    alternate_auditor/
      v0001.md
    glossary_build/
      v0001.md
    manual_review_reingest/
      v0001.md
  judge_backends/
    codex.yaml
    claude-code.yaml
    litellm-anthropic.yaml
    litellm-azure.yaml
    litellm-openai.yaml
    litellm-gemini.yaml
    litellm-ollama.yaml
  engines/
    marker.yaml
    marker-forced-ocr.yaml
    docling.yaml
    pymupdf4llm.yaml
    tesseract.yaml
    tesseract-region-crop.yaml
    markitdown.yaml
    markitdown-with-azure-di.yaml
    azure-document-intelligence.yaml
    mistral-ocr.yaml
  artifact_archives/
    local-only.yaml
    cloud-archive.yaml
  render_profiles/
    default-300dpi.yaml
tools/
  generate_schema_hooks.py    # regenerates the delimited schema-hook blocks
corpus/                       # operator-supplied source PDFs and reference materials (Git LFS for PDFs)
  <output-folder-name>/
    source.pdf
    reference/                # optional; copied verbatim from operator's source repository
.noeasymark-state/            # gitignored unless explicitly promoted
.noeasymark-artifacts/        # gitignored
.implementation-state/        # gitignored; development-time gate-record artifacts only
output/                       # default output_root for produced deliverables; gitignored in this repo
  <output-folder-name>/
```

Use these explicit storage roots:

| Root | Purpose | Git status |
| --- | --- | --- |
| `.noeasymark-state/<run_id>/` | Mutable internal run state, ledgers, manifests, tool profile, queues, and blocking-gate records. | Gitignored. |
| `.noeasymark-artifacts/<run_id>/` | Bulky per-run scratch artifacts, renders, crops, candidates, raw logs, run-scoped response cache when selected, and CLI-agent temp dirs. | Gitignored. |
| `.noeasymark-artifacts/_shared/<output-folder-name>/responses/` | Folder-scoped local response cache shared by runs of the same document when `response_cache_scope: folder`. | Gitignored. |
| `<output_root>/<output-folder-name>/` (default `./output/<output-folder-name>/`) | The reviewable, commit-ready deliverable tree: final Markdown, final images, redacted reports, redacted tool-profile snapshot, manual review records, and lightweight evidence sidecars. | Written by the runtime, never committed by it; the operator commits it wherever they choose. The default `./output/` is gitignored in the NoEasyMark tool repository. |

Internal reports that are not review deliverables live under `.noeasymark-state/<run_id>/reports/`. Public review reports live under `<output_root>/<output-folder-name>/reports/`. When a phase says it emits a report without a full path, it means the public report path if the report is reviewable and the internal state path if the report is operational scratch.

Evidence sidecars are source-derived review artifacts. They map final Markdown blocks and asset records to source pages, layout regions, crop references, candidate spans, adjudication records, and confidence/review status. They are written into the deliverable under `<output_root>/<output-folder-name>/evidence/`; the runtime does not commit them.

Full adjudication records are internal run-state artifacts because they may contain chosen source text, rejected candidate text, reviewer-supplied corrections, and source-derived evidence in the `reason` field. They live under `.noeasymark-state/<run_id>/adjudication/` and are referenced from the deliverable's evidence sidecars by stable `adjudication_id`. The deliverable's evidence sidecars and chapter reports include adjudication ids, decision source, confidence, review status, backend identity, prompt/schema versions, hashes, and redacted reason summaries that contain no source-derived text. Full adjudication records are not written into the deliverable unless an explicitly approved artifact policy promotes that record class.

Mutable state files under `.noeasymark-state/<run_id>/` remain the canonical working records during a run. When this specification says source manifests, outline records, chapter manifests, or chunk manifests are made reviewable, it means schema-validated redacted copies under `<output_root>/<output-folder-name>/manifests/`. Redacted manifest copies may omit absolute local paths, raw source excerpts, private endpoint names, and other fields not permitted by the active content-handling approvals; each redacted copy records the internal state file hash it was derived from so the audit chain remains intact.

### Public Manifest Redaction

Every record that is emitted in two forms ships two sibling schemas: the internal schema (for example `source_manifest`, `tool_profile`, `approvals_record`) validates the full mutable record under `.noeasymark-state/`; the public-schema sibling (for example `source_manifest_public`, `tool_profile_public`, `approvals_record_public`) validates the redacted copy written into the deliverable under `<output_root>/<output-folder-name>/`. The two schemas share a single source of truth: the internal schema carries `nem_redaction` annotations (and optional `nem_public_transform` rules) on every field that must not appear or must be transformed in the public view, and the public sibling schema is generated deterministically from those annotations. The full generation rules, the `tests/test_public_schema_consistency.py` CI check, and the override mechanism for narrowly-scoped public-only deviations live in the schema section below; that text is the source of truth for the generation pipeline. This section is the operator-facing summary of why two siblings exist and what the redaction guarantee is.

Redaction is performed by **omission**, not by null-substitution or sentinel-string replacement: a redacted field is absent from the public copy, and the public schema marks it as not present. This keeps both forms schema-valid without requiring `nullable` unions in the internal schema. Each public copy records the SHA-256 of the internal state file it was derived from as `derived_from_hash` so the audit chain remains intact, and records the list of field paths that were redacted as `redactions_applied` so reviewers can see what was omitted without seeing the omitted values themselves. These two public-only audit fields are generated only for schemas whose internal source declares a maintainer-authored `nem_public_override` for them with fixture coverage; otherwise the public-schema generator may not introduce fields that are absent from the internal schema. The shipped redaction profile is the set of `nem_redaction` annotations on the internal schema at adoption time; operators may not extend or relax it without authoring an explicit `nem_public_override` annotation on the internal schema, recording the deviation in the tool profile, and shipping a fixture that proves the override's emitted output is still consistent with the redaction guarantee.

## Output and Review Surface

The runtime always writes the same commit-ready deliverable folder to `<output_root>/<output-folder-name>/` (with `output_root` defaulting to `./output/`) and never commits it:

- Final Markdown, images, evidence sidecars, reports, manifests, glossary files, redacted approvals, the reviewable original-source copy, and manual-review files are written into the deliverable folder.
- The per-chapter review surface is a local HTML report at `<output_root>/<output-folder-name>/reports/chapter-NN-review.html` that renders Markdown, embeds image crops, and surfaces review markers and chapter metrics. Reports display paths relative to the deliverable root. The reviewer opens the report locally.
- A local run-status file at `<output_root>/<output-folder-name>/reports/status-<run_id>.md` provides run-state visibility (run metadata, metrics, relative paths). It contains no source-derived content beyond what the deliverable already holds.
- `glossary apply`, manual-review re-ingest, and cross-chapter finalization all run normally and write into the deliverable.
- If the operator wants the deliverable in a repository (for example under a project's `references/` directory), they copy the folder into the target repository path or explicitly stage the chosen path themselves. When their repository intentionally ignores the output root, they must choose the non-ignored destination, adjust the ignore rule, or force-add the reviewed files themselves; nothing in the runtime performs that step.

Every local review surface, including chapter reports, aggregate reports, status files, manual-review files, and supervising-agent review requests, displays deliverable-root-relative paths unless the owning schema field explicitly names another base. Absolute host-local paths are not rendered into review surfaces.

Writing only to the local `output_root` does not weaken any other guardrail. Anti-spin controls, ledgers, caching, manual-review severity blocking, network-class approvals, and pilot gating all remain in force.

## CLI Surface

`noeasymark` is the single Typer CLI for the pipeline. Phases below reference subcommands by name; this section is the canonical definition of each subcommand's purpose and flags. Every pipeline step is a subcommand, and the agent invokes the CLI rather than reimplementing pipeline logic inside chat reasoning.

Every run starts with a schema-valid run configuration. `noeasymark init` creates or updates `.noeasymark-state/<run_id>/run_config.json` and records the private source PDF input record, canonical working source path, source SHA-256, document title, generated or overridden output folder name, run id, `output_root`, `lfs_max_file_bytes`, topic description, content-handling choices, approved allowlists with explicit `mode`, the per-run external archive mode and folder approval reference, optional ceilings (spend, invocation, runtime, wall-clock, artifact bytes, minimum free space), pilot-page hints, seed glossary terms, restricted-diversity acknowledgment, and pre-pilot backend preferences when known. When an external artifact archive identity or retention decision is supplied, `init` creates or updates the folder-scope `approvals_record` entry rather than storing that identity in `run_config.json`. Active backend bindings are finalized in `tool_profile.json` after pilot validation. It supports both non-interactive flags and interactive prompts. A later command that needs run configuration must refuse to continue when no active run is selected or when the active run configuration fails schema validation.

The active run is resolved by:

1. The explicit `--run-id <id>` flag on the command, if present.
2. Otherwise the contents of the `.noeasymark-state/active_run` pointer file.
3. If neither is present, the command stops with a clear "no active run" message and points the operator at `noeasymark init`.

Conversion-run gates are cleared through local schema-valid gate records (no remote issue or PR mirrors; the runtime has no remote surface). Every gate record lives under `.noeasymark-state/<run_id>/gates/<gate_id>.json`, validates against `gate_record`, and records `gate_id`, `gate_name`, `status` (`pending`, `approved`, `changes_requested`, `blocked`, `superseded`, or `resolved`), `opened_at`, `opened_by_unit_id`, `scope`, `review_packet_paths`, `operator_instructions`, `decision_log`, and the state-file hashes the gate protects. Human-review gates (`CHAPTER_MAP_REVIEW`, `PILOT_PLAN_REVIEW`, `PILOT_GLOSSARY_REVIEW`, and manual-review waivers) move forward only when the operator runs `noeasymark gate approve <gate_id>` or `noeasymark gate request-changes <gate_id>` after editing the referenced review packet or state files as needed. Automatic-block gates that require an explicit retry (`JUDGE_BACKEND_BLOCKED`, `ENGINE_BLOCKED`, `SCHEMA_MIGRATION_BLOCKED`, `STALE_WORK_UNIT_BLOCKED`, `FINALIZATION_BLOCKED`, budget ceilings, cache-cap halts, and anti-spin halts) are retried with `noeasymark gate retry <gate_id>` after the operator corrects credentials, config, quotas, dependencies, or limits; `retry` re-runs `doctor --quick` plus the gate's own preflight and changes the gate to `resolved` only when the preflight passes. Gates with command-specific resumption rules, such as `OCRMYPDF_NOT_AVAILABLE` and `SUPERVISING_AGENT_REVIEW_PENDING`, follow the named-signal text instead of accepting a generic retry; `SUPERVISING_AGENT_REVIEW_PENDING` clears on a fresh successful quality-check run (the operator reruns the failed command after the supervising coding agent applies a fix) or an explicit operator action, never through `gate approve`/`request-changes`. Per-chapter and final report walkthroughs are checklist items inside the local reports, not gate records. Conversion-run gates are entirely local. The only GitHub-backed gate anywhere in this project is the development-time `SCHEMA_DESIGN_REVIEW` gate used while *building* `noeasymark` (its PR-review approval rule is defined in Phase 0 and runs through the GitHub MCP connector, not the conversion runtime).

Conversion-run subcommands:

- `noeasymark init` - create or update a run configuration from prompts, flags, or an imported TOML/JSON file; generate the output folder name; display and re-confirm carried-forward folder-scope approvals; record content-handling choices; create initial state directories; and validate that required inputs are present before processing begins. **Behavior when a run is already active for the same folder.** Plain `noeasymark init` (without `--fresh`, `--reuse`, or `--import`) exits with usage-error exit code 1 and prints the active `run_id` plus the three explicit choices: `noeasymark init --reuse <run_id>` to attach, `noeasymark init --fresh` to create a new run, or `noeasymark init --import <path>` to seed from an init_bundle. The default does not silently reuse, silently create, or interactively prompt; the operator must say which they want. This rule applies only when an active run resolves through `.noeasymark-state/active_run` for the same output folder; if no active run resolves, plain `init` proceeds normally. Canonical non-interactive flags are `--source <path>`, `--title <document-title>`, `--topic-description <text>`, `--output-folder-name <name>`, `--local-processing {acknowledge,prompt}` (required to be `acknowledge` for non-interactive use because local processing acknowledgment is the one mandatory true approval), `--pilot-pages <csv-or-range-list>`, `--seed-glossary-term <term>` (repeatable), `--output-root <dir>`, `--lfs-max-file-bytes <bytes-or-suffixed-size>`, `--perimeter-mode {all,allowlist,deny}`, `--perimeter-allowlist-entry <json|@path|kind:identifier>` (repeatable), `--internet-mode {all,allowlist,deny}`, `--internet-allowlist-entry <json|@path|kind:identifier>` (repeatable), `--external-artifact-archive {approve,decline,prompt}`, `--external-artifact-archive-id <archive_id>`, `--external-artifact-archive-retention <text>`, `--external-artifact-archive-mode {all,allowlist,deny}`, `--external-artifact-archive-allowlist-entry <json|@path|kind:identifier>` (repeatable), `--approval-decider <identity>`, `--approval-decider-role {maintainer,delegated_reviewer}`, `--approval-rationale <text>`, `--restricted-diversity-acknowledgment`, `--unknown-cost-acknowledgment`, `--spend-ceiling-usd <amount>`, `--invocation-ceiling <count>`, `--runtime-ceiling-seconds <seconds>`, `--wall-clock-ceiling-seconds <seconds>`, `--artifact-bytes-ceiling <bytes-or-suffixed-size>`, `--rendered-page-bytes-ceiling <bytes-or-suffixed-size>`, `--minimum-free-space-bytes <bytes-or-suffixed-size>`, `--diagram-text-policy-default {crop_only,transcribe}`, `--ocr-policy {conditional_redo_then_force,always_redo,always_force,always_redo_then_force,none}`, and `--pre-pilot-backend <role=backend_id>`. Any non-interactive approval or mode decision flag requires the shared audit flags `--approval-decider`, `--approval-decider-role`, and `--approval-rationale`. For perimeter and internet approvals, the mode flag is the decision flag: `all` grants every configured component in that network class, `allowlist` grants only supplied entries and requires at least one matching allowlist-entry flag, and `deny` records `granted: false`; supplying a perimeter or internet allowlist entry without the matching mode flag, or with mode `all` or `deny`, exits with code 2. For external artifact archives, `--external-artifact-archive {approve,decline,prompt}` controls only the folder-scope archive identity/retention decision, while `--external-artifact-archive-mode` controls this run's archive network-class record. Non-interactive `--external-artifact-archive approve` requires `--external-artifact-archive-id`, `--external-artifact-archive-retention`, and `--external-artifact-archive-mode`; if the mode is `allowlist`, at least one archive allowlist entry must validate. Supplying archive id, retention, mode, or allowlist companion flags with `--external-artifact-archive decline`, or supplying archive mode or allowlist flags without any archive decision flag, exits with code 2 and names the missing or incompatible decision. Interactive prompts (where a flag is omitted entirely) are not subject to the non-interactive companion-flag rule because the prompt itself collects the decision before companion fields. Lifecycle flags are `--fresh` to force a new `run_id` when one already exists for the same folder, `--reuse <run_id>` to attach to an existing run, and `--import <path>` to seed from a TOML or JSON file. Interactive prompts use the same field names and semantics as these flags.
- `noeasymark doctor` - environment self-check; default checks Python packages, external binaries, tool versions, schema validity for all backend and engine configuration files, activation state for judge backends and engines, artifact archive targets, credentials for enabled components, writable artifact paths, governing instruction-file inventory, artifact storage configuration, content-handling approvals, and committed-state freshness. Tool checks are capability-based rather than version-string-only. Because the conversion runtime performs no GitHub automation, `doctor` does not check `gh` or any GitHub capability; it verifies that configured CLI-agent backends expose the exact flags recorded in their backend YAML (`--help` output or an equivalent machine-readable capability command). If a vendor renames a flag, the backend YAML must be updated and revalidated before the backend can be bound in a tool profile.
- `noeasymark doctor --quick` - fast subset suitable for session start; skips long network calls and GPU probes.
- `noeasymark doctor --gpu` - includes CUDA visibility, `nvidia-smi`, driver compatibility, and any local VLM/OCR model preflight; intended for devcontainer creation and any session that may use local GPU engines.
- `noeasymark doctor --devcontainer-fingerprint` - compares the active devcontainer's observed base-image digest, Python, `uv`, native binaries, global npm agent CLIs, and declared toolchain records against `.devcontainer/toolchain-fingerprint.json`. A mismatch reports `TOOLCHAIN_DRIFT` and exits with doctor finding code 3; the nightly byte-identity job treats this as drift requiring a deliberate fingerprint and expected-output refresh, not as a conversion regression.
- `noeasymark doctor --backends` - exercises a small synthetic smoke call against every enabled judge backend and every backend bound in the active tool profile to verify auth, CLI presence, schema-output behavior, network-class approval and allowlist compliance, endpoint identity, timeout behavior, declared transcript persistence, and client-side schema validation. Disabled example configurations are schema-validated but not smoke-called. `--all-configured` extends smoke calls to every backend configuration whose declared activation state allows testing, and is intended for maintainers updating shipped examples. Smoke calls must not include source-derived content, document titles, candidate text, crops, or source metadata. The shipped synthetic smoke-call payload is fixed and is the single source of truth for backend health: the prompt body is `Respond with a JSON object that exactly matches the supplied schema. The object has two fields: "ok" (the boolean true) and "echo" (the string "noeasymark-doctor-smoke"). Emit nothing outside the JSON.`, and the supplied JSON Schema is `{"type":"object","additionalProperties":false,"required":["ok","echo"],"properties":{"ok":{"const":true},"echo":{"const":"noeasymark-doctor-smoke"}}}`. The fixed `echo` token lets the runner detect a backend that ignored the schema and returned chat-style filler. Doctor records the backend's actual response against this payload in the doctor report, including timing and any schema-validation failures, but never copies the response into the response cache (it is not a real adjudication record).
- `noeasymark status` - prints next undone unit, current spend/runtime, progress rate, blocked gates, latest chapter state, content-handling mode, and current tool profile including backend bindings; runs `doctor --quick` first by default. `--skip-doctor` is an explicit read-only escape hatch for diagnostics when `doctor --quick` is failing: it reads only atomically written local state, prints a prominent `not preflight-verified` banner, performs no repairs or probes, and must not be treated as evidence that the environment is ready to continue processing.
- `noeasymark gate list|show|approve|request-changes|retry|reopen` - inspect and clear schema-valid gate records for the active run. `approve` requires `--reviewer <name-or-login>` and accepts optional `--note <text>`; it refuses to approve a gate whose protected state-file hashes no longer match the files named in the review packet, forcing the operator to re-run the producing command so the gate packet reflects the edited state. `request-changes` requires `--reviewer <name-or-login>` and `--note <text>`, records both in `decision_log`, and leaves the producing work unit blocked. `retry` is available only for automatic-block gates and never approves a human-review gate. `reopen` supersedes a previously approved or resolved gate when the protected state changes after approval.
- `noeasymark preprocess` - source inspection, normalization, OCR-derived copies, page renders, source manifest.
- `noeasymark chunk` - chapter splitting and processing-chunk planning.
- `noeasymark segment` - layout-region detection for every page; multi-column reading-order resolution.
- `noeasymark convert <chunk_id>` - run engine ensemble for a chunk. `<chunk_id>` uses the canonical chunk-id form defined in **Identifier Conventions**.
- `noeasymark figures <chunk_id>` - image, caption, table, and equation extraction for a chunk.
- `noeasymark judge <chunk_id>` - adjudicate disputed spans for a chunk; the sole entry point for paid or agent-driven judgment calls.
- `noeasymark glossary build` - build candidate glossary from pilot output.
- `noeasymark glossary apply` - protected-zone-aware deterministic rewrites against the approved glossary. `noeasymark publish-chapter` invokes this step automatically after chapter QA passes and before chapter-review report emission; the standalone subcommand exists so the operator can re-apply the glossary after editing `approved.yaml` without re-running QA.
- `noeasymark pilot` - drive the pilot sample end-to-end and emit pilot report plus tool profile. `--plan-only` selects and records the proposed pilot page set and then stops at the optional `PILOT_PLAN_REVIEW` gate without sending source-derived content to engines or backends. `--require-plan-review` persists `run_config.require_pilot_plan_review: true` before planning so subsequent pilot invocations keep pausing at the plan gate until the gate is approved or the operator clears the run-config field.
- `noeasymark publish-chapter <chapter_id>` - run chapter QA, invoke `glossary apply`, write the chapter's final Markdown, images, and evidence sidecar into the deliverable folder under `output_root`, and refresh the local chapter-review HTML report. It never opens a pull request. `<chapter_id>` uses the canonical `ch<NN>` form defined in **Identifier Conventions** (for example `ch01`, `ch017`); the chunk-level commands `noeasymark convert`, `noeasymark figures`, and `noeasymark judge` continue to take canonical `<chunk_id>` arguments.
- `noeasymark finalize` - cross-chapter consistency, indexes, and the final aggregate report, all written into the deliverable folder under `output_root`. It never opens a pull request.
- `noeasymark run` - orchestrator. Default behavior is to advance exactly one next undone unit per invocation so each invocation produces a reviewable diff (equivalent to `--once`). `--until <gate>` advances units until the named gate is hit. `--continuous` loops until the next blocking gate, completion, or anti-spin halt. All variants consult the response cache, honor the spend, invocation, runtime, and wall-clock ceilings, and respect anti-spin halts. `--require-chapter-map-review` forces a pause at `CHAPTER_MAP_REVIEW` even when `outline_status` is `OUTLINE_PRESENT`.
- `noeasymark migrate` - apply pending schema migrations for the active run. `--dry-run` previews the migration plan without modifying state. Halts with `SCHEMA_MIGRATION_BLOCKED` if any step is missing.
- `noeasymark cache prune` - manually evict response-cache entries when `response_cache_eviction_policy` is `none` or when the operator wants to reclaim space, and reclaim disk used by chapter artifacts that have been superseded by re-conversion. Accepts `--older-than <duration>` (e.g., `7d`, `90d`), `--backend <id>`, `--chapter <chapter_id>`, `--superseded`, `--archive`, and `--dry-run` filters. The response-cache filters (`--older-than`, `--backend`, `--chapter`) target entries under `.noeasymark-artifacts/_shared/<output-folder-name>/responses/` or `.noeasymark-artifacts/<run_id>/responses/` depending on `response_cache_scope`. The `--superseded` filter targets a distinct artifact class: the chapter-supersession directory `.noeasymark-state/<run_id>/superseded/<UTC timestamp>/` populated by re-conversion under the **Reconversion artifact supersession** closed handoff decision. The two artifact classes are pruned separately because they have different retention semantics; combining `--superseded` with any of `--older-than`, `--backend`, or `--chapter` in a single invocation is a usage error (exit code 1) because those filters do not apply to the supersession directory. `--dry-run` previews either action without modifying state. Refuses to evict response-cache entries that back unpublished adjudication records without first archiving their provenance. **"Unpublished" for prune purposes** means the adjudication record's owning chapter has `chapter_manifest.status` outside the terminal set `{published}`. Specifically, entries backing chapters in `planned`, `in_progress`, `converted`, `figures_extracted`, `adjudicated`, `blocked`, or `superseded` states are not eligible for direct eviction; the operator must either wait for chapter publication or pass `--archive` to copy the entry's provenance into `cache_provenance_archive` first. A chapter reaches its terminal `published` state when its chapter-review report and deliverable outputs have been written successfully. `--superseded` does not consult the unpublished-adjudication check because it operates on already-superseded chapter artifacts rather than response-cache entries.
- `noeasymark unlock <run_id>` - manually release a single-writer lock whose holder has crashed or been killed. Displays the lock holder's `pid`, `hostname`, command name, safe flag names, redacted command argument values, and start timestamp, then requires the operator to confirm the holder is not running before clearing the lock. Path-like values, source/title values, approval identities, reviewer identities, credential names beyond approved public env-var names, and any value supplied to a non-boolean option are redacted in terminal output as `<redacted:path>`, `<redacted:identity>`, or `<redacted:value>` according to field class. The raw lock file remains available only on local disk under the private state root for the operator's direct inspection. The runtime never automatically steals locks based on PID inspection, because PID reuse and cross-host execution make automated lock-stealing unsafe.

Developer-only subcommands:

- `noeasymark dev regenerate-schema-hooks` - developer-only command that regenerates the `# noeasymark: begin schemas` / `# noeasymark: end schemas` delimited block in `.pre-commit-config.yaml` and in `.github/workflows/data-ci.yml` from `schemas/_index.yaml`. `--check` runs the regeneration without writing and exits non-zero when the regenerated output diverges from the committed files; CI invokes this form. The command refuses to write outside the delimited region and refuses to touch the template-owned `validate-template-sync-*` hooks. This subcommand is namespaced under `dev` because it is not part of any conversion run.
- `noeasymark dev regenerate-public-schemas` - developer-only command that regenerates every committed public sibling schema from the corresponding internal schema by applying the deterministic `nem_redaction` / `nem_public_transform` transformation. `--check` exits non-zero when any committed public schema diverges from the generated form. CI invokes the `--check` form from `data-ci.yml` and `cross-platform-check.yml`. The canonical pytest is `tests/test_public_schema_consistency.py`.
- `noeasymark dev regenerate-render-profile` - developer-only command that regenerates `config/render_profiles/default-300dpi.yaml` by reading `pymupdf.__version__` from the active interpreter and writing it as the YAML's `renderer_version` field. `--check` exits non-zero when the committed YAML's `renderer_version` differs from the active runtime. CI invokes `--check` from `cross-platform-check.yml` on every OS so any renderer drift surfaces as a CI failure.

## Judge Backends

A judge backend is the runtime mechanism through which `noeasymark judge` produces a schema-conformant adjudication record for a disputed span. The pipeline supports two backend families. Operators install one or more backend configurations under `config/judge_backends/<name>.yaml`; roles in `.noeasymark-state/<run_id>/tool_profile.json` bind to a backend by name plus per-role parameters.

Every backend YAML, regardless of family, carries `change_notes`: a structured object with `vendor_doc_freshness` (array, required for shipped examples that depend on vendor or CLI facts) and `operator_notes` (array of dated free-text rationale records for local additions such as failure-classification overrides). The `vendor_doc_freshness` entry shape is the one defined in **Premise** and is part of the `judge_backend_record` schema.

Every backend YAML with a non-empty `transport_only_parameters` list also carries `transport_only_parameter_proofs`, a closed object keyed by exact parameter name. The key set must match `transport_only_parameters` exactly. Each proof records `classification` (`timeout`, `retry`, `streaming`, `same_model_routing`, `transport_metadata`, or `other_output_neutral`), `evidence_kind` (`vendor_doc`, `installed_help`, `adapter_contract`, or `operator_rationale`), `evidence_ref` (a URL, installed-tool evidence id, adapter test id, or dated local rationale id), `checked_at_utc`, `checked_by`, and a short rationale explaining why the parameter cannot change generated content for that backend. Proofs are backend-specific and are not inherited between providers, CLI agents, or Router modes. A tool-profile `transport_only_parameter_additions` entry must carry the same proof shape in the role binding under `transport_only_parameter_addition_proofs`; the role-binding proof key set must match the additions exactly.

### Judge Response Schema Portability

Judge response schemas are authored once as Pydantic v2 models and exported to canonical JSON Schema for repository validation. Runtime backend adapters do not pass that canonical schema blindly to every provider. They compile it to a conservative backend-compatible profile and record the emitted schema hash in provenance.

The portable profile favors required object properties, explicit `additionalProperties: false` where supported, bounded strings, bounded arrays, enums, simple discriminated unions, and clear descriptions. It avoids schema features known to vary across native structured-output implementations unless a backend-specific adapter has a tested lowering rule. Unsupported features include unconstrained maps, open-ended `anyOf`/`oneOf` nests, regex-dependent semantics that the provider does not enforce, nonstandard formats, recursive references, and implicit optional fields whose omission would change adjudication meaning.

`noeasymark doctor --backends` validates all three layers for every enabled or tool-profile-bound backend. Disabled example configurations must pass schema validation, but they do not need credentials or network access:

1. The canonical Pydantic-exported JSON Schema validates with the repository schema tooling.
2. The backend adapter can compile the canonical schema to that backend's declared structured-output mechanism.
3. A synthetic smoke call returns JSON that passes both backend-native enforcement when available and the pipeline's mandatory client-side validation.

Phase 0 commits backend-specific schema-lowering fixtures for every shipped backend family under `tests/fixtures/judge_backends/<backend-family>/schema_lowering/`. Each fixture names the canonical input schema, backend family, backend structured-output mode, expected emitted schema or expected rejection, diagnostic id, diagnostic summary, and whether credentials or network access are required. Lowering fixtures never include source-derived prompts or real model calls; they exercise the adapter compiler and validation diagnostics offline. Each shipped backend family has at least one supported-feature fixture, one unsupported-feature fixture for every profile feature the adapter intentionally rejects, and one fixture covering prompt-enforced JSON when that family permits it. The canonical pytest is `tests/test_judge_schema_lowering.py`; it asserts byte-identical emitted schemas where deterministic lowering is expected and asserts stable diagnostic ids and human-readable messages for unsupported features. `noeasymark doctor --backends` reuses the same diagnostic ids when it reports a lowering failure so a backend smoke failure can be matched to the committed fixture.

If a backend only supports prompt-enforced JSON, the adapter may still be used, but the tool profile records `schema_support: prompt_enforced_and_validated`, pilot metrics must include schema-failure rate, and repeated schema-output failure follows the normal backend-failure anti-spin path.

### LiteLLMBackend

Wraps a LiteLLM call. Suitable for providers with HTTP APIs that LiteLLM supports, such as OpenAI direct, Azure OpenAI, Anthropic API, Vertex/Gemini, Bedrock, Mistral, Ollama local, Groq, xAI, Databricks, and others. Backend YAML records:

- `kind: litellm`
- `enabled`: boolean; shipped example files default to `false` until an operator enables them or binds them in the active tool profile.
- `provider`: LiteLLM provider identifier.
- `provider_identifier_case_policy`: `case_sensitive` or `case_insensitive`; controls allowlist normalization for `kind: litellm_provider`.
- `model_id`: provider-specific model identifier.
- `model_snapshot_or_version`: optional pinned version.
- `default_parameters`: provider-accepted parameter bag using provider-native key names; the schema is parameter-bag style so provider-specific keys pass through unchanged. There is no special-cased `reasoning_effort` field; each provider's native parameter for effort or thinking budget (`reasoning_effort`, `thinking`, `thinking_config`, and so on) lives in this bag, and the merged bag is what `model_parameters_hash` covers in cache identity.
- `transport_only_parameters`: an explicit list of keys in `default_parameters` (or addable to the parameter bag at tool-profile or per-call time) that must be excluded from `model_parameters_hash` because they do not affect generated output. The shipped LiteLLM examples set this to `[timeout_seconds, stream, request_timeout, max_retries]` plus the LiteLLM Router transport keys (`num_retries`, `fallbacks`) when Router is enabled and same-model routing constraints are satisfied. Every key must have a matching `transport_only_parameter_proofs` entry. Every key not in this list is treated as output-affecting; the conservative default keeps unrecognized provider-native parameters in cache identity.
- `response_format`: `json_schema` with a path to the canonical schema and the backend-compiled emitted schema. `enable_json_schema_validation` is an optional boolean (default `false`) that controls whether LiteLLM's own client-side schema-validation hook is engaged in addition to the pipeline's mandatory post-call validation. The pipeline always validates responses client-side; this flag exists only to opt into LiteLLM's pre-decode enforcement on providers that do not natively support strict schema output.
- `prompt_caching`: provider-specific cache-control hints, such as explicit cache breakpoints for Anthropic or `automatic` for providers that cache transparently.
- `prompt_cache_profile`: a closed provider profile used by `prompt_caching`, prompt rendering, and cache-metric normalization. It declares cache enablement mode (`automatic`, `explicit_breakpoints`, or `unsupported`), minimum cacheable prompt-token requirements by model/platform when the provider publishes them, maximum explicit breakpoint count, supported TTL or retention-policy values, cacheable request surfaces (`tools`, `system`, `messages`, images, documents, structured-output schema, and similar provider-native blocks), invalidation-sensitive request fields, required byte-identical prefix rules, request fields that should be set consistently for routing such as OpenAI `prompt_cache_key` when the provider supports them, and the response usage JSON paths that prove read, write, and uncached input tokens. A shipped profile may declare a metric as `unavailable`, but only with a fixture showing the provider or transport does not expose a stable field.
- `vendor_cache_metrics`: `native`, `inferred`, or `unavailable`.
- `router`: disabled by default. If enabled, records LiteLLM Router retry/fallback/load-balancing behavior. The actual backend/model selected by Router must be captured in call provenance.
- `credentials`: declares which environment variables must be present. The backend never stores credentials in the file.
- `network_class`: `device`, `perimeter`, or `internet`. Examples: `litellm-ollama` is `device`; `litellm-azure` against a private VNet tenant may be `perimeter`; `litellm-anthropic` against the public API is `internet`. If the operator cannot justify `device` or `perimeter`, classify as `internet`.
- `endpoint_identifiers` (optional): a list of discriminated-union records following the **Endpoint Identifier Schema**. Used for allowlist scoping.
- `input_modalities`: array of `text` or `image` declaring which content-block kinds the bound model accepts. Shipped LiteLLM examples set this from the chosen `model_id`+`model_snapshot_or_version` against the provider's published capability documentation at Phase 0 freshness check time; default for unknown text-only models is `[text]`. The runtime never sends an image content block to a backend whose `input_modalities` does not include `image`; `noeasymark doctor --backends` refuses to bind such a backend to a role whose `required_modalities` (recorded in `tool_profile.json` role binding) includes `image`.
- `failure_classification`: an ordered list of match rules evaluated against a normalized failure event. Each rule has `match_type` (`http_status`, `provider_error_code`, `exception_class`, `stderr_regex`, `stdout_regex`, `json_path`, `timeout`, or another schema-declared event field), `pattern`, `category` (`retryable`, `correctable`, or `fatal`), and `signature_template` used to normalize failure signatures for anti-spin accounting.
- `cost_tracking`: common cost-tracking record with `mode`, optional mode-specific fields, and billing-freshness checklist when the mode depends on vendor billing behavior. LiteLLM backends usually use `native`, `estimated_by_model`, `runtime_only`, or `unknown`.
- `change_notes`: the common structured backend change-note object described above.

Pipeline-owned retries and fallback routing are the default. LiteLLM Router retries/fallbacks are allowed only when explicitly enabled in backend YAML, and their behavior must be visible in the ledger and adjudication provenance to avoid invisible backend switching. When Router is enabled, its retry and fallback policy is constrained to the **same** `(provider, model_id, model_snapshot_or_version)` tuple as the original call: same-provider transient-error retries against an alternate region, deployment, or load-balanced endpoint are allowed only when the configured targets declare the same model snapshot/version and compatible endpoint identifiers. Cross-provider transitions (e.g., OpenAI → Anthropic, Azure OpenAI → Bedrock), cross-model transitions, and cross-snapshot transitions are not Router behavior. Cross-provider or cross-model fallback is the responsibility of the pipeline's role-binding fallback path (`fallback_judge`), which produces its own adjudication record with its own cache identity. `noeasymark doctor --backends` refuses to bind a LiteLLM backend whose `router` configuration declares fallback targets that change `provider`, `model_id`, or `model_snapshot_or_version`; the change must be expressed as a separate backend YAML and bound to the appropriate role in the tool profile. The complete Router target set is covered by `backend_config_hash`, and provenance records the selected target's endpoint identifier, deployment/region label when available, retry count, fallback reason, and LiteLLM hidden model id when LiteLLM exposes one. This keeps the cache key meaningful: `rendered_prompt_hash` and `backend_id` together describe a single provider/model/snapshot surface, and a Router-driven retry against an alternate same-provider endpoint produces bytes whose response shape is replayable from the same cache entry.

### CLIAgentBackend

Wraps a CLI coding agent, such as Codex CLI, Claude Code headless, or a future agent CLI, through a documented subprocess contract. Backend YAML records:

- `kind: cli_agent`
- `enabled`: boolean; shipped example files default to `false` until an operator enables them or binds them in the active tool profile.
- `identifier_case_policy`: `case_sensitive` or `case_insensitive`; controls allowlist normalization for the CLI backend configuration id / YAML name.
- `argv_template`: an array of argv tokens with placeholders for allowed values such as `{schema_path}`, `{schema_json}`, `{prompt_file}`, `{output_path}`, and `{work_dir}`. This is never a shell string. `{schema_json}` is the minified inline JSON Schema string and is used only for CLI flags that require the schema body rather than a file path; it never carries source-derived content.
- `shell: false`, required.
- `placeholder_allowlist`: the only placeholders allowed in `argv_template`.
- `prompt_passing`: one of `stdin`, `file`, or `arg`. `stdin` and `file` are the only modes allowed for any prompt containing source-derived content. `arg` is allowed only for fixed non-sensitive instructions or paths (for example, the schema file path or a deterministic system-prompt flag) and must never carry source text, rendered OCR, Markdown, crop descriptions, candidate spans, captions, or any other source-derived content. `noeasymark doctor --backends` refuses a backend whose `argv_template` interpolates `arg`-class placeholders into source-derived call sites.
- `output_capture`: one of `stdout_json`, `stdout_jsonl_final`, or `file_json`.
- `schema_support`: `native`, `prompt_enforced_and_validated`, or `autodetect`. Native support must name the concrete structured-output flag or mode and the emitted schema profile used by that CLI version. `autodetect` is allowed only for disabled shipped examples and for backends that `doctor --backends` can immediately resolve to either `native` or `prompt_enforced_and_validated` before any source-derived call.
- `model_id` and `model_snapshot_or_version`: required in the effective tool-profile role binding. Backend YAML may omit them only when the installed CLI has no static model field and `doctor --backends` can discover a stable actual model id plus snapshot/version from a non-source smoke call, CLI metadata, or operator-supplied binding before the first source-derived call. If the CLI cannot report stable model identity, the operator must supply explicit values or the backend remains unbindable for source-derived judging.
- `default_parameters`: provider-native parameter bag, identical in shape to the LiteLLM backend's `default_parameters`. CLI agents that support an effort or thinking control declare it here using the agent's own flag or environment-variable name; the adapter is responsible for translating bag entries into argv tokens or environment values. There is no special-cased `reasoning_effort` field.
- `transport_only_parameters`: identical in semantics to the LiteLLM backend's `transport_only_parameters` list. For CLI agents the shipped list typically contains transport-layer flags such as `timeout_seconds` and any agent-specific connection or retry knob; output-shaping flags (effort, thinking, sandbox modes that change generation behavior) remain output-affecting and are not listed here. Every listed key must have a matching `transport_only_parameter_proofs` entry.
- `sandbox`: agent-specific sandbox mode; default to the most restrictive setting that still permits the agent to read supplied evidence and emit JSON.
- `ambient_config`: `disabled`, `minimal`, or `default`. Default is `disabled`; the runner passes flags such as Codex `--ignore-user-config`, Codex `--ignore-rules`, Codex `--ephemeral`, Claude `--bare`, Claude `--no-session-persistence`, or vendor equivalents when supported. If a backend requires ambient config, the exception must be explicit in backend YAML and visible in provenance.
- `allowed_tools`: the CLI-agent tools or capabilities enabled for the call. Default is read-only access to copied evidence and write access only to the declared output path.
- `session_persistence`: `disabled` by default. Persistent CLI-agent sessions are not used for judging unless explicitly enabled and included in cache identity and provenance.
- `transcript_persistence`: `none`, `local_tool_cache`, `vendor_account`, or `unknown`. If a CLI may persist prompts or responses outside the per-call temp directory, the backend must declare the location class and retention expectation, and `doctor --backends` must surface it before source-derived calls are allowed.
- `repo_access`: `none` or `read_only_repo`. Default is `none`; a judge backend should not need repository access.
- `artifact_access`: `copied_read_only`, `read_only_artifacts`, or `none`. Default is `copied_read_only`, meaning the runner copies only required evidence into the per-call temp directory.
- `write_access`: `temp_only` or `none`. Default is `temp_only`, limited to the per-call temp directory.
- `working_directory_strategy`: per-call OS temp scratch directory containing the schema file, prompt file, output file, and read-only evidence copies. Symlinks are allowed only when the runner resolves the target first, verifies it remains under a declared NoEasyMark-owned state/artifact root, records the logical relative target in provenance, and rejects any traversal, dangling link, or link to a host path outside the declared roots. Otherwise evidence is copied.
- `env_allowlist`: exact environment variables passed to the subprocess. The runner records variable names and presence/absence only; it never records secret values, token prefixes, or inherited environment dumps.
- `credentials`: declares required environment variables or prior interactive login, such as `codex login` or `claude auth login`.
- `timeout_seconds`: per-call timeout.
- `vendor_cache_metrics`: usually `unavailable` unless the CLI exposes native metrics.
- `cost_tracking`: common cost-tracking record with billing-freshness checklist when the mode depends on vendor subscription, credit-pool, pricing, or usage-reporting behavior. CLI agents on subscription plans may not report per-token billing, so the spend ledger records invocation count, wall-clock runtime, and `cost_tracking.mode`: `zero_subscription`, `subscription_credit_pool`, `estimated_by_model`, `runtime_only`, or `unknown`.
- `failure_classification`: an ordered list of match rules, identical in shape to the common normalized failure-event model. CLI-specific rules usually match `exit_code`, `signal`, `timeout`, `stderr_regex`, or `stdout_regex`, but may also match parsed JSON error fields when the CLI emits structured diagnostics.
- `network_class`: `device`, `perimeter`, or `internet`, declared based on where the bound CLI agent sends content. A CLI agent bound to a local model is `device`; one that sends content to the agent vendor's hosted model is `internet`, or `perimeter` if the operator has configured the agent against a private endpoint. If unsure, classify as `internet`.
- `endpoint_identifiers` (optional): same shape as for LiteLLMBackend; see the **Endpoint Identifier Schema**.
- `input_modalities`: array of `text` or `image` declaring which content kinds the bound CLI agent accepts in its prompt-passing channel. Shipped CLI-agent examples set this from the underlying CLI's documented capabilities and installed-help probes at Phase 0 freshness check time. Image support is never inferred from the product family alone: a backend may declare `image` only when `doctor --backends` verifies the installed CLI accepts an image-bearing prompt channel and the bound model identity supports image input, preferably through a synthetic non-source smoke call. Otherwise the conservative declaration is `[text]`. The runtime never sends an image to a backend whose `input_modalities` lacks `image`; `noeasymark doctor --backends` refuses the corresponding role binding.
- `change_notes`: the common structured backend change-note object described above.

The subprocess runner must not invoke a shell. It creates a per-call scratch directory under the platform-appropriate temp root (`tempfile.gettempdir()`), writes the prompt and schema as files when needed, passes only allowlisted environment variables plus explicitly declared credential variables, enforces timeouts, captures stdout/stderr, validates output client-side, and records all non-secret invocation metadata. Before deleting scratch, the runner writes the durable call bundle under `.noeasymark-artifacts/<run_id>/agent-runs/<unit_id>/<call_id>/` with relative paths only. The bundle includes the emitted schema file or schema hash, prompt/evidence hashes, captured output, normalized failure event when present, redacted stdout/stderr or their hashes according to the active content-handling mode, and a manifest naming any source-derived files copied into scratch. The argv recorded in provenance may include file paths, flags, and non-source schema text passed through `{schema_json}`, but it must not include source-derived prompt content. Environment provenance records allowlisted variable names, credential variable names, and boolean presence results only. The runner spawns every subprocess inside a noeasymark-owned process container so the OS can clean up child trees when noeasymark exits: on POSIX (Linux, macOS) by creating a new process group with `start_new_session=True` (equivalent to `os.setsid`) and addressing the group via `os.killpg(-pgid, signal)`; on Windows by creating the child with `creationflags=CREATE_NEW_PROCESS_GROUP | CREATE_SUSPENDED`, attaching it to a job object created with `JOB_OBJECT_LIMIT_KILL_ON_JOB_CLOSE`, and resuming it. The optional dependency that exposes Win32 job-object creation (`pywin32`) is declared in the core dependency set with a `sys_platform == "win32"` marker so it installs only where it is used. On operator interruption (Ctrl+C, OS shutdown signal, or programmatic termination of the `noeasymark` process), the runner issues a **cooperative shutdown signal** to every live child subprocess (`SIGTERM` to each process group on POSIX; `CTRL_BREAK_EVENT` to each console child plus `TerminateJobObject(..., 1)` on Windows for non-console children), waits a 30-second grace period for cooperative shutdown, then issues a **forced termination** to any still-running children (`SIGKILL` to each process group on POSIX; `TerminateJobObject(..., -1)` on Windows) before exiting. The runner records the interruption as a `normalized_failure_event` with `category: correctable`, signature `interrupted_by_operator:<primitive_name>` (for example `interrupted_by_operator:sigterm` on POSIX, `interrupted_by_operator:ctrl_break_event` on Windows), and a `platform_primitive` provenance field naming the OS-native primitive that fired, so the affected unit can resume cleanly on the next `noeasymark run` invocation and cross-host traces remain comparable. If the CLI can write transcripts, caches, or session state outside the temp directory despite `session_persistence: disabled`, the backend configuration must declare that persistence behavior and the run must treat those artifacts as source-derived. If an enabled CLI-agent backend omits an isolation flag because the installed CLI release lacks it, the backend YAML must truthfully record the resulting `ambient_config`, `session_persistence`, and `transcript_persistence` mode; `noeasymark doctor --backends` refuses source-derived calls when the YAML claims stricter isolation than the smoke probe can verify.

For CLI agents with native schema flags, `noeasymark doctor --backends` detects support from the installed CLI's own help output plus a synthetic smoke call and records the evidence in `change_notes.vendor_doc_freshness[]`. The current reference docs and local Codex help check show `codex exec --output-schema <path-to-schema.json>` for Codex structured final output, while the current Claude Code CLI reference documents print-mode `--json-schema <inline-json-schema>` with JSON output. These examples are starting points, not durable contract text: if an installed release renames, removes, or changes a flag's argument form, the shipped YAML must use the verified current form or remain disabled. Even when native schema output is used, the pipeline validates the response client-side after the call, because CLI envelopes and provider-enforced schema subsets differ.

#### Subscription Credit Accounting

Backends and source-content-receiving engines share one `cost_tracking` record so budget gates do not depend on backend family. `cost_tracking.mode` is one of:

- `native` - the backend reports authoritative usage and dollar-denominated cost for the call.
- `zero_subscription` - usage is included in the subscription plan with no per-call accounting.
- `subscription_credit_pool` - usage draws from a finite monthly credit pool that is separate from interactive usage. The spend ledger records invocation count and runtime; cost-per-call is estimated where the vendor exposes credit consumption, otherwise marked unknown.
- `estimated_by_model` - cost is estimated by approximating tokens against published model pricing using the `litellm.token_counter()` helper as the default tokenizer router. Each backend YAML records the actual `estimator_id` used (for example `tiktoken/cl100k_base` for OpenAI/Azure, `anthropic/approx` for Anthropic non-tokenizer-exposed models, `vertex/gemini_approx` for Gemini); the estimator id is copied into every adjudication record's provenance so cross-provider comparisons can be adjusted for tokenizer drift.
- `estimated_by_page` - source-content engine cost is estimated by page count against published or configured page pricing.
- `runtime_only` - no dollar estimate is attempted; invocation count and wall-clock runtime are enforced.
- `none` - the component has no expected variable usage cost, though runtime and artifact usage are still recorded.
- `unknown` - no reliable cost signal is available; the run records this explicitly rather than fabricating a number. A backend declared `unknown` is only eligible when `run_config.json` records `unknown_cost_acknowledgment: true` and supplies both `invocation_ceiling` and `runtime_ceiling_seconds`. `noeasymark doctor --backends` refuses any unknown-cost backend that lacks either the acknowledgment or both mandatory ceilings.

The optional dollar spend ceiling in `run_config.json` is enforced against calls whose `cost_tracking.mode` is `native`, `estimated_by_model`, or `estimated_by_page`. For `zero_subscription`, `subscription_credit_pool`, `runtime_only`, and acknowledged `unknown` backends, operators set per-run `invocation_ceiling` (max number of backend invocations) and `runtime_ceiling_seconds` (max accumulated wall-clock seconds spent in backend calls); these are enforced in the same pre-call check as the dollar ceiling and produce the same `BUDGET_THRESHOLD_HIT` halt. For acknowledged `unknown` backends and for `subscription_credit_pool` backends both ceilings are required, because both classes can run silently expensive when the vendor exposes no per-call credit signal; `noeasymark doctor --backends` refuses any backend in either class whose run-config bindings lack both ceilings. For `zero_subscription` and `runtime_only` backends both ceilings remain optional but recommended.

When a backend or source-content engine cost mode depends on current vendor billing behavior, its YAML carries `cost_tracking.billing_freshness`. The checklist records `plan_type` (`per_call`, `subscription_included`, `subscription_credit_pool`, `runtime_metered`, `unknown`, or `none`), `per_call_usage_signal` (`native_usage`, `estimated_tokens`, `estimated_pages`, `runtime_only`, `not_exposed`, or `not_applicable`), `credit_pool_visibility` (`native_balance`, `monthly_quota_visible`, `not_exposed`, or `not_applicable`), `pricing_source_ref` (a `vendor_doc_freshness[]` id, installed-tool evidence id, or operator-provided dated pricing record), `required_ceilings` (array drawn from `spend_ceiling_usd`, `invocation_ceiling`, and `runtime_ceiling_seconds`), `checked_at_utc`, `checked_by`, and `verification_status` (`verified`, `updated`, or `verification_deferred`). The checklist is required for `native`, `zero_subscription`, `subscription_credit_pool`, `estimated_by_model`, `estimated_by_page`, `runtime_only`, and `unknown`; it is omitted only for `mode: none`. It documents billing assumptions only and does not weaken the existing spend, invocation, or runtime gates.

Hosted document-service engines also write a normalized usage-counter record for every request, even when dollar cost remains estimated. The record stores `engine_id`, `provider`, `provider_request_id` when available, `raw_usage_ref`, `raw_usage_hash`, `usage_source` (`native_response`, `response_headers`, `request_estimate`, or `unavailable`), `pages_submitted`, `pages_returned`, nullable `pages_billed`, `request_count`, `source_bytes_submitted`, `output_bytes_received`, nullable `provider_cost_estimate_usd`, `cost_estimate_mode`, `pricing_source_ref`, and provider-specific normalized counters. Mistral OCR maps the response `usage_info` into `raw_usage_ref` and counts returned page objects plus submitted source bytes. Azure Document Intelligence stores the raw response and estimates pages billed from the submitted page range or PDF/TIFF page count when per-call billing usage is not returned. Azure Content Understanding maps the response `usage` object into tiered counters such as `documentPagesMinimal`, `documentPagesBasic`, `documentPagesStandard`, token groups, audio hours, and video hours when present. The budget ledger and chapter report use the normalized counters for cross-provider totals while retaining the raw provider usage blob for audit.

Operators selecting Claude Code as a backend should verify current subscription and credit-pool behavior in current vendor documentation and in `noeasymark doctor --backends`; the reference notes are not the authority for billing behavior.

### Pre-Shipped Backend Configurations

The repository ships example configuration files for common setups. Shipped examples are disabled by default and must be enabled explicitly or bound in the active tool profile before they are eligible for pilot or full-run calls. Schema validation applies to every shipped file; credential checks, network-class allowlist checks, and smoke calls apply only to enabled or bound backends unless the operator runs `noeasymark doctor --backends --all-configured`. Any shipped example that depends on a vendor model id, model alias, model snapshot, modality claim, billing claim, or CLI flag is an illustrative seed only. Phase 0 must refresh those fields from official vendor documentation or installed-tool help before the YAML becomes bindable; when the authoritative check is unavailable, inconclusive, or stale, the YAML stays disabled, keeps a conservative placeholder or `_shipped_example_` sentinel for the unverified field, and records `verification_deferred`.

- `codex.yaml` - Codex CLI exec mode. The shipped `argv_template` contains `codex exec --output-schema {schema_path}` plus only those isolation flags that Phase 0 verifies against the installed Codex release with `codex --help` and `codex exec --help`. Current reference checks expect isolation flags such as `--ephemeral`, `--ignore-user-config`, `--ignore-rules`, and `--skip-git-repo-check`; if any are unavailable, leave that flag out and set the backend's `ambient_config`, `session_persistence`, or `repo_access` fields to the observed relaxed mode rather than claiming stricter isolation. `model_id` and `model_snapshot_or_version` may be omitted from the disabled shipped YAML, but the backend cannot be bound until `doctor --backends` or the operator supplies a stable effective model identity for the tool profile.
- `claude-code.yaml` - Claude Code print/headless mode. The shipped `argv_template` uses `claude -p --output-format json --json-schema {schema_json}` when the installed Claude Code release expects an inline schema body. If a later release documents a file-path schema flag, `{schema_path}` is also allowed, but Phase 0 must choose the form supported by `claude --help`. Include `--no-session-persistence`, `--bare`, or any other isolation/persistence flag only when the installed release documents that flag and `doctor --backends` smoke-calls the resulting backend successfully; otherwise record the relaxed persistence or ambient-config mode explicitly. `model_id` and `model_snapshot_or_version` may be omitted from the disabled shipped YAML, but the backend cannot be bound until `doctor --backends` or the operator supplies a stable effective model identity for the tool profile.
- `litellm-anthropic.yaml` - Anthropic API via LiteLLM. Shipped `model_id` and `model_snapshot_or_version` are the literal sentinel `_shipped_example_`; shipped `input_modalities` is the conservative `[text]` until Phase 0 verifies the selected Anthropic model's official capabilities. Phase 0 consults Anthropic's published model documentation, selects the exact model id and snapshot/version the operator wants to pilot, records the doc URL and access date in `change_notes.vendor_doc_freshness[]`, and updates `input_modalities` only for capabilities the selected model documentation actually supports.
- `litellm-azure.yaml` - Azure OpenAI via LiteLLM. Shipped `model_id` is the operator-supplied Azure deployment name placeholder `azure/<deployment-name>` (operator replaces `<deployment-name>` with the actual Azure deployment); shipped `model_snapshot_or_version` is the literal sentinel `_shipped_example_`.
- `litellm-openai.yaml` - OpenAI direct API via LiteLLM. Shipped `model_id` and `model_snapshot_or_version` are the literal sentinel `_shipped_example_`; shipped `input_modalities` is the conservative `[text]` until Phase 0 verifies the selected OpenAI model's official capabilities. Phase 0 consults OpenAI's official model documentation, selects the exact model id and snapshot/version the operator wants to pilot, records the doc URL and access date in `change_notes.vendor_doc_freshness[]`, and updates `input_modalities` only for capabilities the selected model documentation actually supports.
- `litellm-gemini.yaml` - Google Vertex/Gemini via LiteLLM. Shipped `model_id` and `model_snapshot_or_version` are the literal sentinel `_shipped_example_`; shipped `input_modalities` is the conservative `[text]` until Phase 0 verifies the selected Gemini/Vertex model's official capabilities. Phase 0 consults Google's published model documentation, selects the exact model id and snapshot/version the operator wants to pilot, records the doc URL and access date in `change_notes.vendor_doc_freshness[]`, and updates `input_modalities` only for capabilities the selected model documentation actually supports.
- `litellm-ollama.yaml` - Local Ollama instance via LiteLLM, typically running a GPU-resident model. Shipped `model_id` is the illustrative local placeholder `ollama/llama3.1:8b`; shipped `model_snapshot_or_version` is the literal sentinel `_shipped_example_`. Phase 0 or the operator may replace it with any locally installed Ollama model, and `doctor --backends` verifies the local endpoint and model availability before binding.

`noeasymark doctor --backends` refuses to bind a backend whose effective `model_id`, `model_snapshot_or_version`, structured-output flag, model identity, or required modality is still the literal sentinel `_shipped_example_`, an angle-bracket placeholder such as `azure/<deployment-name>`, or a `verification_deferred` field into the active tool profile or invoke that backend for a real adjudication call. The smoke call (`doctor --backends` itself) may still validate disabled examples with synthetic prompts when credentials and network approvals are available. The sentinel forces the operator to explicitly choose and verify a concrete model surface at pilot time; the shipped values are illustrative defaults intended to validate schema and CLI flow only.

Operators may add backend configurations for any other LiteLLM-supported provider or any other CLI agent that satisfies the CLIAgentBackend contract. No backend, provider, or model is required by default.

### Role Binding

The judge roles in **Phase 8 - Adjudication** are abstract. `.noeasymark-state/<run_id>/tool_profile.json` binds each enabled role to a backend by name plus the per-role parameters needed at call time. At pilot time the operator evaluates available backends against the role requirements and records the chosen bindings.

Fallback binding is capability-based rather than family-name based. The `fallback_judge` should have a different failure domain from the default judge whenever practical, such as a different provider, different transport, different credential pool, different region, or different CLI/API path. The `independent_auditor` must be materially independent of the default judge when enabled, preferably a different vendor or model family. The tool profile records each role's `failure_domain`, `network_class`, backend family, model family, and independence rationale.

Bindings may be changed later by re-running `noeasymark pilot` or by editing the tool profile and re-running `noeasymark doctor --backends`. Any post-pilot change that can affect output behavior, such as backend id, model id, model version, prompt version, schema version, model parameters, enabled judge roles, engine set, artifact strategy, render-profile alias, or quality thresholds, must be recorded as a tool-profile revision. The pipeline must review cache-key impact and run a targeted mini-pilot before the changed binding is used at full scale, unless the operator records an explicit waiver. The mini-pilot scope is the intersection of the change-affected pages with the existing pilot set, capped at 20 pages; if the intersection is empty, the mini-pilot falls back to a fresh stratified sample of equivalent size.

Shipping with no runnable judge backend is intentional. Disabled shipped backend YAML files validate schema and document common configurations, but none is bindable until the operator replaces the `_shipped_example_` sentinel, supplies credentials where needed, records content-handling approvals, and approves a tool profile. If no judge backend is bound, `doctor --backends` exits 0 after reporting `no enabled or tool-profile-bound backends to smoke-call`, and disputed spans route to `MANUAL_REVIEW` with complete evidence packets. This manual-review-only path can complete local smoke tests and a limited pilot, but it is expected to be review-heavy and cannot satisfy a full-run scale-up gate unless the operator explicitly approves the resulting manual-review load at `PILOT_GLOSSARY_REVIEW`.

### Backend Identity in Caching and Provenance

`backend_id`, `backend_config_hash`, model identity, model parameters, rendered prompt content, and evidence content together determine cache identity; full provenance is recorded alongside for audit. A response produced by one backend is not the same cache entry as one produced by another backend, even if every other input is identical.

## Engine Configurations

Every conversion engine that may receive source-derived content has a configuration under `config/engines/<name>.yaml`. Engine activation has three distinct states. `enabled` is the operator-authored YAML flag that makes a non-initial engine eligible for consideration. `dependency_eligible` is a doctor-computed report field showing that the engine's local binaries, Python packages, credentials, network-class approval, and allowlist checks are currently satisfied. `tool_profile.active_engines` is the approved runtime roster selected during pilot and is the only state that makes an engine participate in conversion. Shipped Initial-set engines may become `dependency_eligible` even while their YAML `enabled` field remains `false`; pilot can then include them in `tool_profile.active_engines` without mutating their YAML. Non-initial engines require `enabled: true` before doctor probes them for runtime eligibility. `noeasymark doctor` validates every engine configuration against schema, but dependency probes, credential checks, network-class allowlist checks, and engine smoke tests are required only for Initial-set engines, enabled non-initial engines, and engines selected by the active tool profile.

Engine YAML records:

- `engine_id`
- `identifier_case_policy`: `case_sensitive` or `case_insensitive`; controls allowlist normalization for this `engine_id`.
- `enabled`: boolean; shipped files default to `false`. For Initial-set engines this means "not operator-forced"; doctor may still compute `dependency_eligible` and pilot may still select the engine into `tool_profile.active_engines`. For non-initial engines, `enabled: false` means schema-validate only; the engine is not dependency-probed, smoke-called, or selectable until the operator flips it to `true`.
- `initial_set`: boolean; true only for engines named in the Phase 6 Initial engine set. Operator-added engines must set this to `false`; Initial-set membership cannot be conferred outside the shipped configuration.
- `kind`: for example `local_cli`, `python_library`, `litellm_document_service`, `azure_document_intelligence`, `mistral_ocr`, or `markitdown_wrapper`
- `family`: one of `layout_ml`, `layout_heuristic`, `raw_ocr`, `document_ai`, or `vision_llm`. Used by the diversity rule. Network class is tracked separately and must not be encoded into the family value.
- `delegates_to` (optional): the `engine_id` of another configured engine to which this engine routes source-derived content. When present, the engine inherits the delegated engine's `network_class` and `family` for diversity counting, and both engines' allowlist approvals must hold. A delegating engine is not `dependency_eligible` unless the target engine exists, is schema-valid, does not itself delegate, passes its own credential, endpoint, network-class, allowlist, dependency, and smoke checks, and records the delegated target id plus target config hash in the doctor report. The delegated target's `enabled` flag is not flipped merely because a wrapper is selected, and the target does not have to be in `tool_profile.active_engines` as a standalone candidate, but delegation never bypasses the target's checks.
- `network_class`: `device`, `perimeter`, or `internet`; if unsure, classify as `internet`. Overridden by the delegated engine when `delegates_to` is set.
- `endpoint_identifiers` (optional): a list of records following the **Endpoint Identifier Schema**, used for allowlist matching.
- `credentials` (optional), declaring required environment variables without storing secrets
- `supported_python` (optional): version specifier for the engine wrapper or local package.
- `supported_platforms` (optional): operating systems, architectures, and container assumptions.
- `requires_gpu` (optional): boolean or structured requirement when the engine needs CUDA or another accelerator.
- `required_binaries` (optional): external CLI tools that `doctor` must find.
- `timeout_seconds`
- `retry_policy`
- `resource_budget`: closed object with `max_workers`, `max_cpu_threads`, `scheduler_concurrency`, `requires_gpu`, nullable `gpu_device_policy`, nullable `gpu_memory_min_mb`, nullable `gpu_memory_soft_limit_mb`, nullable environment overrides such as thread-limit variables, and references to the Phase 2 spend/runtime/artifact ceilings that constrain the engine.
- `adapter_smoke_contract`: source-free fixture and expected call-shape contract used by doctor and tests to assert arguments, API keyword arguments, disabled remote features, expected output files, sidecar schemas, lineage fields, and failed-candidate envelopes.
- `output_contract`: expected files and sidecar JSON records. Output-contract verification failures populate `normalized_failure_event.output_contract_failure` (for example `missing_output`, `empty_output`, `invalid_sidecar_json`, or `schema_invalid_sidecar`) and `output_contract_path` with a relative path when a specific output path is involved.
- `text_source_policy`: declares which source lineages the engine may use, such as `embedded_text_layer`, `ocr_redo_pdf`, `ocr_force_pdf`, `rendered_page_ocr`, `layout_region_crop_ocr`, `document_ai_ocr`, `local_vlm_inference`, or `mixed`.
- `failure_classification`: ordered list of match rules evaluated against the normalized failure-event model. Local CLI engines usually match `exit_code`, `signal`, `timeout`, `stderr_regex`, or `stdout_regex`; Python-library engines usually match `exception_class` or `exception_message_regex`; hosted document services usually match `http_status`, `provider_error_code`, or `json_path`.
- `cost_tracking`: common cost-tracking record with billing-freshness checklist when the mode depends on vendor pricing or usage-reporting behavior; engines usually use `none`, `runtime_only`, `native`, `estimated_by_page`, or `unknown`.
- `provider_usage_mapping` (optional): for hosted document-service engines, maps provider-native usage fields into the normalized document-service usage counter record used by budget ledgers and reports.
- `documentation_urls`: array of schema-valid documentation link records for upstream engine/tool docs. Each entry carries `url`, `purpose` (`upstream_home`, `api_reference`, `cli_reference`, `installation`, `model_cache`, `license`, or `other`), `component_version_scope` (nullable), `canonical_url` (nullable until link-check), `reference_group` (`pdf_conversion_tool` or `engine_adapter`), and optional `freshness_id` linking to `change_notes.vendor_doc_freshness[]`.
- `change_notes`: a structured object with `vendor_doc_freshness` (array, required for shipped examples that depend on vendor or CLI facts) and `operator_notes` (array of dated free-text rationale records for local additions such as failure-classification overrides). The `vendor_doc_freshness` entry shape is the one defined in **Premise** and is part of the `engine_record` schema.

Examples:

- Local Tesseract is `network_class: device`, `family: raw_ocr`.
- Local PyMuPDF/PyMuPDF4LLM is `network_class: device`, `family: layout_heuristic`.
- Marker or Docling running fully locally is `network_class: device`, `family: layout_ml`.
- Azure Document Intelligence through a private endpoint may be `perimeter`, `family: document_ai`; otherwise classify as `internet`.
- Mistral OCR public API is `network_class: internet`, `family: document_ai`.
- Any engine with unclear routing is `internet`.
- Shipped disabled MarkItDown wrappers: `markitdown.yaml` uses only local PDF parsing and is `network_class: device`, `family: layout_heuristic`; `markitdown-with-azure-di.yaml` delegates to `azure-document-intelligence`, inherits that engine's effective `network_class` and `family`, and counts as `document_ai` for diversity purposes.
- Future MarkItDown extension wrappers, such as OCR-plugin, Azure Content Understanding, or image-description variants, must be separate engine YAML files. Each one declares its delegated service or plugin, endpoint identifiers, credentials, network class, family, and required approvals before it is eligible for `doctor` probing or pilot selection.

`noeasymark doctor` validates engine configurations, resolves the single permitted `delegates_to` hop, refuses any chain longer than one hop or any self-delegation, refuses a delegating engine whose target is missing or fails target checks, and refuses to run an engine whose effective `network_class` is not approved by the operator's content-handling choices.

Every shipped engine YAML has committed output-contract fixtures under `tests/fixtures/engines/<engine_id>/output_contract/`. The fixture set includes at least one successful synthetic output bundle, one missing-output diagnostic, one empty-output diagnostic, one invalid-sidecar-JSON diagnostic when the engine writes sidecars, and one schema-invalid-sidecar diagnostic when the sidecar schema has required fields. Fixtures use synthetic non-source documents and do not call the real engine or any network service. The canonical pytest is `tests/test_engine_output_contracts.py`; it validates expected files, sidecar schemas, relative paths, and stable `normalized_failure_event.output_contract_failure` values plus readable diagnostics for each negative fixture. `noeasymark doctor` reuses those diagnostic ids when an enabled engine's smoke output violates its contract.

The shipped disabled cloud/document-service examples use conservative, schema-valid placeholders rather than trying to discover operator infrastructure. `config/engines/azure-document-intelligence.yaml` declares `engine_id: azure-document-intelligence`, `kind: azure_document_intelligence`, `family: document_ai`, `enabled: false`, `network_class: internet` by default, `credentials.required_env` containing `AZURE_DOCUMENT_INTELLIGENCE_ENDPOINT` and `AZURE_DOCUMENT_INTELLIGENCE_KEY`, an `endpoint_identifiers` entry whose `matcher_type` is `env_url_host` and whose value source is `AZURE_DOCUMENT_INTELLIGENCE_ENDPOINT`, and `default_parameters` including `model_id: prebuilt-layout` plus an explicit API-version field named `api_version` with the value chosen from the current Azure SDK/REST documentation at Phase 0 adoption. Its `provider_usage_mapping` records raw response storage plus `estimated_by_page` counters derived from the submitted page range or source page count when native per-call billing usage is unavailable. An operator using a private endpoint may change only `network_class` to `perimeter` and keep the same endpoint-matcher structure; `doctor` then validates the endpoint against the perimeter allowlist instead of the internet allowlist.

`config/engines/mistral-ocr.yaml` declares `engine_id: mistral-ocr`, `kind: mistral_ocr`, `family: document_ai`, `enabled: false`, `network_class: internet`, `credentials.required_env` containing `MISTRAL_API_KEY`, an `endpoint_identifiers` entry for host `api.mistral.ai`, and `default_parameters` including `model: mistral-ocr-latest`, `document_transport: document_base64`, `include_image_base64: false`, `extract_header: false`, `extract_footer: false`, and `table_format: markdown`. Its `provider_usage_mapping` stores Mistral's `usage_info` as the raw usage record and normalizes returned page count, request count, source bytes, output bytes, and any native usage fields that the current API exposes. `document_transport: document_base64` is a NoEasyMark adapter enum, not a raw vendor request field; Phase 0 maps it to the current Mistral SDK/REST request shape for a base64 PDF and records that mapping in `change_notes.vendor_doc_freshness[]`. The base64 transport is the shipped default so NoEasyMark does not require publishing a source PDF at a public URL merely to call a hosted OCR service; the internet-processing approval and allowlist still apply because the PDF bytes leave the device.

Before committing any shipped backend YAML or cloud/document-service engine YAML, Phase 0 runs a vendor-documentation freshness audit. The audit verifies every model id, model alias, endpoint host, API-version field, documented modality, CLI isolation flag, transport option, and billing/credit assumption against the current official vendor documentation or installed-tool help available at adoption time, then records the result in `change_notes.vendor_doc_freshness[]`. Disabled `_shipped_example_` records still require this freshness note because stale examples mislead operators even when they are not bindable. If a vendor page or tool-help source is unavailable, the YAML remains disabled and conservative, and the deferred verification is appended to `_TODO_AFTER-BUILD-MANUAL-STEPS.md` rather than being silently treated as true.

The shipped MarkItDown wrappers are limited to `config/engines/markitdown.yaml` and `config/engines/markitdown-with-azure-di.yaml`. `markitdown.yaml` uses local PDF parsing only. `markitdown-with-azure-di.yaml` declares `engine_id: markitdown-with-azure-di`, `kind: markitdown_wrapper`, `enabled: false`, `delegates_to: azure-document-intelligence`, no separate credentials, and wrapper parameters only for MarkItDown behavior. Its effective `family`, `network_class`, endpoint identifiers, and approval requirements come from the delegated Azure Document Intelligence engine.

Future MarkItDown wrappers for OCR plugins, Azure Content Understanding, image-description LLM clients, or other optional integrations are extension examples, not shipped configurations. Each must be added as its own engine YAML before use. Every MarkItDown engine variant calls the narrowest local-file or stream conversion path that can handle the pipeline-owned input artifact, passes only NoEasyMark-selected local paths or streams, and disables ambient plugin discovery unless the exact plugin/extra is declared in that engine YAML and verified by `doctor`. URL conversion, recursive archive traversal beyond the supplied artifact, image-description LLM clients, and other MarkItDown features that can read arbitrary files or make network calls are forbidden unless a separate engine YAML declares the resulting content movement, endpoint identifiers, credentials, network class, and required approvals. If a future MarkItDown release exposes a different Azure plugin name or configuration surface, Phase 0 records the observed plugin id in `change_notes.vendor_doc_freshness[]` without changing the delegated-engine target.

### Default `failure_classification` rule lists

Every shipped backend YAML and every shipped engine YAML ships an ordered `failure_classification` list. The lists are not free invention: the spec mandates the per-family default lists below as the shipped starting point. Each shipped YAML's actual list is the relevant per-family default list verbatim. Operators may add YAML-specific rules above the defaults to customize anti-spin behavior, but never remove a default rule without an explicit documented rationale in the YAML's own `change_notes.operator_notes[]`.

Every list ends with the same catch-all safety net so any signature not matched by an earlier rule is still recorded deterministically: `{ match_type: stderr_regex, pattern: ".*", category: retryable, signature_template: "unmatched:{event_type}:{component_id}:{signature_stem}" }`. The catch-all is `retryable` so a transient unknown failure does not immediately retire the component; the `NO_PROGRESS_HALT_SIGNATURE` halt catches the case where the same unmatched signature genuinely cycles.

Failure-classification `pattern` semantics are fixed before any source-derived call. For `stderr_regex` and `stdout_regex`, `pattern` is a Python standard-library `re` pattern evaluated with `re.search` against the redacted excerpt. Inline flags such as `(?i)` are allowed; locale-dependent flags are forbidden. For `exception_class`, `provider_error_code`, `output_contract_failure`, `signal`, `http_status`, `exit_code`, and `timeout`, the event field is converted to its canonical string form and the Python `re` pattern must match with `re.fullmatch`; numeric fields use their decimal representation and `timeout` uses the literal string `timeout` when the timeout boolean fired. For `json_path`, `pattern` is not a regular expression: it is a restricted JSONPath of the form `$` followed by dot-separated identifier segments and optional `[integer]` array indices, and it matches only when the path exists and the resolved value is neither `null` nor `false`. `signature_template` uses Python `str.format` named placeholders, and every placeholder must be a declared `normalized_failure_event` field or the derived `signature_stem`. `noeasymark doctor` compiles or validates every shipped and operator-added rule, rejects invalid patterns, rejects unknown placeholders, rejects a template that would render an empty signature, and reports a doctor finding before any engine or backend receives source-derived content.

**Per-family default `failure_classification` lists.**

LiteLLMBackend (any LiteLLM provider): authentication and permission failures are `correctable`; rate-limit and capacity failures are `retryable` with anti-spin backoff; quota-exhausted failures are `correctable`; schema-output failures are `correctable` with the canonical schema-validation signature; the catch-all closes the list.

```yaml
failure_classification:
  - { match_type: http_status, pattern: "401",  category: correctable, signature_template: "litellm_http:{http_status}:auth_failed" }
  - { match_type: http_status, pattern: "403",  category: correctable, signature_template: "litellm_http:{http_status}:permission_denied" }
  - { match_type: http_status, pattern: "404",  category: correctable, signature_template: "litellm_http:{http_status}:model_not_found:{model_id}" }
  - { match_type: http_status, pattern: "408",  category: retryable,   signature_template: "litellm_http:{http_status}:request_timeout" }
  - { match_type: http_status, pattern: "409",  category: retryable,   signature_template: "litellm_http:{http_status}:conflict" }
  - { match_type: http_status, pattern: "425",  category: retryable,   signature_template: "litellm_http:{http_status}:too_early" }
  - { match_type: http_status, pattern: "429",  category: retryable,   signature_template: "litellm_http:{http_status}:rate_limited" }
  - { match_type: http_status, pattern: "500",  category: retryable,   signature_template: "litellm_http:{http_status}:server_error" }
  - { match_type: http_status, pattern: "502",  category: retryable,   signature_template: "litellm_http:{http_status}:bad_gateway" }
  - { match_type: http_status, pattern: "503",  category: retryable,   signature_template: "litellm_http:{http_status}:unavailable" }
  - { match_type: http_status, pattern: "504",  category: retryable,   signature_template: "litellm_http:{http_status}:gateway_timeout" }
  - { match_type: http_status, pattern: "529",  category: retryable,   signature_template: "litellm_http:{http_status}:overloaded" }
  - { match_type: provider_error_code, pattern: "insufficient_quota",   category: correctable, signature_template: "litellm_provider_error:{provider_error_code}" }
  - { match_type: provider_error_code, pattern: "context_length_exceeded", category: correctable, signature_template: "litellm_provider_error:{provider_error_code}" }
  - { match_type: timeout,     pattern: ".*",  category: retryable,   signature_template: "litellm_timeout:{component_id}" }
  - { match_type: exception_class, pattern: "litellm\\..*RateLimitError",      category: retryable,   signature_template: "litellm_exception:{exception_class}" }
  - { match_type: exception_class, pattern: "litellm\\..*Authentication.*",   category: correctable, signature_template: "litellm_exception:{exception_class}" }
  - { match_type: exception_class, pattern: "litellm\\..*BadRequest.*",       category: correctable, signature_template: "litellm_exception:{exception_class}" }
  - { match_type: json_path,   pattern: "$.schema_validation_failure", category: correctable, signature_template: "schema_validation_failure:{response_schema_id}:{response_schema_version}" }
  - { match_type: stderr_regex, pattern: ".*", category: retryable, signature_template: "unmatched:{event_type}:{component_id}:{signature_stem}" }
```

CLIAgentBackend (Codex CLI, Claude Code, future CLI agents): exit-code 0 is success and is not modeled here; nonzero exit codes split into auth/quota/network/cli-bug categories; the catch-all closes the list.

```yaml
failure_classification:
  - { match_type: exit_code, pattern: "1",   category: correctable, signature_template: "cli_exit:1:{signature_stem}" }
  - { match_type: exit_code, pattern: "2",   category: correctable, signature_template: "cli_exit:2:usage_error" }
  - { match_type: exit_code, pattern: "126", category: correctable, signature_template: "cli_exit:126:not_executable" }
  - { match_type: exit_code, pattern: "127", category: correctable, signature_template: "cli_exit:127:not_found" }
  - { match_type: signal,    pattern: "SIGKILL", category: correctable, signature_template: "cli_signal:SIGKILL" }
  - { match_type: signal,    pattern: "SIGTERM", category: correctable, signature_template: "cli_signal:SIGTERM" }
  - { match_type: timeout,   pattern: ".*",  category: retryable,   signature_template: "cli_timeout:{component_id}" }
  - { match_type: stderr_regex, pattern: "(?i)not (logged in|authenticated)|please .* login|expired session", category: correctable, signature_template: "cli_auth_failed:{component_id}" }
  - { match_type: stderr_regex, pattern: "(?i)rate.?limit|too many requests|quota .* exceeded",              category: retryable,   signature_template: "cli_rate_limited:{component_id}" }
  - { match_type: stderr_regex, pattern: "(?i)network|connection refused|dns",                                category: retryable,   signature_template: "cli_network:{component_id}" }
  - { match_type: stderr_regex, pattern: "(?i)schema|json (validation|parse)",                                category: correctable, signature_template: "schema_validation_failure:{response_schema_id}:{response_schema_version}" }
  - { match_type: stderr_regex, pattern: ".*", category: retryable, signature_template: "unmatched:{event_type}:{component_id}:{signature_stem}" }
```

Local CLI engines (Tesseract, ocrmypdf, qpdf, mutool, ghostscript):

```yaml
failure_classification:
  - { match_type: output_contract_failure, pattern: "missing_output|empty_output|invalid_sidecar_json|schema_invalid_sidecar", category: correctable, signature_template: "local_cli_output_contract:{output_contract_failure}:{component_id}" }
  - { match_type: exit_code, pattern: "1",   category: correctable, signature_template: "local_cli_exit:1:{signature_stem}" }
  - { match_type: exit_code, pattern: "2",   category: correctable, signature_template: "local_cli_exit:2:usage_error" }
  - { match_type: exit_code, pattern: "127", category: correctable, signature_template: "local_cli_exit:127:not_found" }
  - { match_type: signal,    pattern: "SIGKILL", category: correctable, signature_template: "local_cli_signal:SIGKILL" }
  - { match_type: timeout,   pattern: ".*",  category: retryable,   signature_template: "local_cli_timeout:{component_id}" }
  - { match_type: stderr_regex, pattern: "(?i)corrupt|damaged|password.protected", category: fatal, signature_template: "local_cli_input_unrecoverable:{component_id}" }
  - { match_type: stderr_regex, pattern: ".*", category: retryable, signature_template: "unmatched:{event_type}:{component_id}:{signature_stem}" }
```

Python-library engines (PyMuPDF, pymupdf4llm, marker-pdf, docling, Pillow): exceptions usually carry actionable classes.

```yaml
failure_classification:
  - { match_type: exception_class, pattern: "FileNotFoundError",       category: correctable, signature_template: "py_exception:{exception_class}" }
  - { match_type: exception_class, pattern: "PermissionError",          category: correctable, signature_template: "py_exception:{exception_class}" }
  - { match_type: exception_class, pattern: "MemoryError",              category: correctable, signature_template: "py_exception:{exception_class}" }
  - { match_type: exception_class, pattern: "TimeoutError",             category: retryable,   signature_template: "py_exception:{exception_class}" }
  - { match_type: exception_class, pattern: "pymupdf\\..*EmptyFileError", category: fatal,    signature_template: "py_exception:{exception_class}" }
  - { match_type: exception_class, pattern: "ValueError",               category: correctable, signature_template: "py_exception:{exception_class}" }
  - { match_type: stderr_regex,    pattern: ".*", category: retryable,   signature_template: "unmatched:{event_type}:{component_id}:{signature_stem}" }
```

Document-AI engines (Mistral OCR, Azure Document Intelligence, Azure Content Understanding, and any delegated wrapper): same shape as LiteLLMBackend's HTTP-status block plus document-specific failures.

```yaml
failure_classification:
  - { match_type: http_status, pattern: "400",  category: correctable, signature_template: "docai_http:{http_status}:bad_request" }
  - { match_type: http_status, pattern: "401",  category: correctable, signature_template: "docai_http:{http_status}:auth_failed" }
  - { match_type: http_status, pattern: "403",  category: correctable, signature_template: "docai_http:{http_status}:permission_denied" }
  - { match_type: http_status, pattern: "404",  category: correctable, signature_template: "docai_http:{http_status}:resource_not_found" }
  - { match_type: http_status, pattern: "408",  category: retryable,   signature_template: "docai_http:{http_status}:request_timeout" }
  - { match_type: http_status, pattern: "413",  category: correctable, signature_template: "docai_http:{http_status}:payload_too_large" }
  - { match_type: http_status, pattern: "415",  category: correctable, signature_template: "docai_http:{http_status}:unsupported_media_type" }
  - { match_type: http_status, pattern: "429",  category: retryable,   signature_template: "docai_http:{http_status}:rate_limited" }
  - { match_type: http_status, pattern: "5..",  category: retryable,   signature_template: "docai_http:{http_status}:server_error" }
  - { match_type: provider_error_code, pattern: "InvalidImage",                  category: fatal,       signature_template: "docai_provider_error:{provider_error_code}" }
  - { match_type: provider_error_code, pattern: "UnsupportedLanguage",           category: correctable, signature_template: "docai_provider_error:{provider_error_code}" }
  - { match_type: timeout,     pattern: ".*",  category: retryable,   signature_template: "docai_timeout:{component_id}" }
  - { match_type: stderr_regex, pattern: ".*", category: retryable,   signature_template: "unmatched:{event_type}:{component_id}:{signature_stem}" }
```

These lists are the shipped defaults. The signature templates use the `normalized_failure_event` placeholders defined in **Closed Handoff Decisions**. Operators add YAML-specific rules above the defaults in any shipped YAML; the matching order is "operator-added first, default list second, catch-all last." The lists are deliberately conservative on the retire-vs-retry boundary: catastrophic input problems (`corrupt|damaged|password.protected`, `InvalidImage`) are `fatal` so the engine retires for the unit immediately; everything else is at most `correctable`, leaving the anti-spin caps to halt cycling.

## Cache Key Canonicalization

`cache_key` is reproducible across implementations. Each output-affecting input is hashed independently into a named component digest, and the final key is the SHA-256 hex digest of a canonical-JSON object that maps each component name to its hex digest:

```text
cache_key = sha256_hex( canonical_json( { component_name: component_hex, ... } ) )
```

Canonical JSON in this specification is RFC 8785 (JSON Canonicalization Scheme, JCS): UTF-8 encoding with no byte-order mark, no insignificant whitespace, sorted object keys (lexicographic on raw unescaped property-name strings represented as UTF-16 code-unit arrays, as required by JCS), arrays preserved in declared order, JSON string data preserved as-is without Unicode normalization by the JCS canonicalizer, and JSON numbers serialized exactly per the JCS number-to-string algorithm (which is itself the ECMAScript 262 `Number.prototype.toString` algorithm). When another field-level rule in this specification explicitly says to NFC-normalize or otherwise normalize a standalone string before hashing, that normalization happens before constructing the value to hash and is not part of the JCS canonicalizer. The implementation uses a conforming JCS library such as the `rfc8785` Python package; `noeasymark doctor` exercises the library against the published JCS test vectors before any cache-key computation is permitted. The runtime rejects any value that JCS cannot serialize portably, including `NaN`, positive infinity, negative infinity, lone Unicode surrogates, non-string object keys, duplicate object keys, and schema fields whose JSON representation would depend on host locale or Python object identity. Non-finite numeric operational values such as unknown costs are represented as `null` plus an explicit mode enum, never as floating-point sentinels. The same rule applies to every nested object hashed below.

The top-level `cache_key` envelope (`{component_name: component_hex, ...}`) is a string-to-string mapping and therefore does not exercise the JCS number-serialization path, but every nested component object that is hashed in turn (notably the merged parameter bag used to compute `model_parameters_hash`, which can carry numeric values such as `temperature`, `top_p`, or `thinking.budget_tokens`) does.

The component hashes are:

- `cache_key_version` - integer; initial release ships `cache_key_version = 1`; bumped any time the set of cache-key components changes, so an additional component becomes an observable invalidation event rather than a silent stale hit. Hashed as its decimal string representation under name `cache_key_version`.
- `backend_id` - the literal backend YAML name (file basename without extension). Hashed as UTF-8 NFC bytes.
- `backend_config_hash` - SHA-256 of the backend YAML file's UTF-8 bytes after newline normalization only (CRLF and CR mapped to LF). No other whitespace, blank-line, comment, key-order, anchor, alias, tag, or scalar-style normalization is applied before hashing. This is intentionally conservative: an editor-only comment or whitespace edit can invalidate cache, but output-affecting YAML scalar content cannot be hidden by a hash normalizer. Formatting hooks and schema validation own unwanted whitespace cleanup before a backend is bound. The initial release does not accept a parsed canonical YAML projection for cache identity. If a future version adopts such a projection, it must bump `cache_key_version` or add a new component name and ship fixtures proving comments, anchors, aliases, scalar styles, key order, duplicate-key rejection, and scalar value preservation cannot create stale cache hits.
- `model_id` and `model_snapshot_or_version` - hashed as UTF-8 NFC bytes of the literal strings; absent snapshot hashes to the literal string `_absent_`.
- `model_parameters_hash` - SHA-256 of canonical JSON of the active per-call parameter bag, filtered to exclude every key listed in the call's effective `transport_only_parameters` set. The parameter bag is the merged result of backend YAML `default_parameters`, tool-profile per-role overrides, and any per-call overrides; provider-native key names are preserved verbatim because they are part of the cache identity (see **Backend Execution Parameters**). The effective `transport_only_parameters` set for a call is the union of the backend YAML's `transport_only_parameters` list and the tool-profile role's `transport_only_parameter_additions` list. Every key not in that union is treated as output-affecting; the conservative default keeps unrecognized provider-native parameters in the cache identity. The exclusion list actually applied to each call is recorded in provenance.
- `response_schema_id` and `response_schema_version` - hashed as UTF-8 NFC bytes of the literal strings. A response validated against a different schema identity or version is never a cache hit, even if the prompt text is unchanged.
- `backend_emitted_schema_hash` - SHA-256 of canonical JSON of the backend-compatible schema profile actually supplied to the backend, or the literal string `_prompt_enforced_` when no backend-native schema is supplied and the schema is enforced only by prompt plus client-side validation.
- `rendered_prompt_hash` - SHA-256 of the exact bytes sent as the prompt for HTTP backends, or the exact bytes written to stdin or the prompt file for CLI-agent backends. `prompt_id` and `prompt_version` are recorded in provenance but intentionally excluded from `cache_key`; a prompt metadata edit that does not change rendered prompt bytes does not invalidate the cache.
- `evidence_content_hash` - SHA-256 of canonical JSON of the fixed-shape envelope `{candidate_spans, page_image_crops, layout_regions, crop_refs, extra_evidence}` where each key is always present and each value is an array, even when the array is empty. Each list element is the SHA-256 of the underlying artifact's canonical content projection, sorted ascending by the artifact's declared stable id field: `span_id` for `candidate_spans`, `crop_id` for `page_image_crops`, `region_id` for `layout_regions`, `crop_ref_id` for `crop_refs`, and `extra_evidence_id` for `extra_evidence`. The canonical content projection is defined per artifact class so two implementations seeing the same source evidence produce identical hashes: for `page_image_crops` the initial release projection is the exact encoded image bytes of the crop artifact (PNG bytes for the default PNG render profile, or the encoded bytes for another explicit render-profile image format), rendered at the active `render_profile_id`; for `candidate_spans`, `layout_regions`, `crop_refs`, and `extra_evidence` the projection is canonical JSON (per the JCS rules above) of the record with its `audit_metadata` block omitted. A decoded-pixel projection is deliberately not accepted in cache identity for the initial release. If a future version adopts decoded-pixel hashing, it must add a new component name or bump `cache_key_version`, record both decoded-pixel and encoded-artifact hashes in provenance for migration audit, and ship fixtures proving equivalent pixels with different encoders do not create stale cache hits. The `audit_metadata` block on every evidence-record schema declares the bookkeeping fields that do not enter cache identity: `created_at`, `created_by`, `updated_at`, `reviewer_notes`, `provenance_links`, and any field annotated `nem_evidence_excluded: true` in the schema. Every other field — including all geometry, lineage, content text, confidence values, and region references — participates in the projection. Each owning schema declares its stable id as a required string field with a deterministic project-prefixed format (for example `cs-<14-char>`, `cr-<14-char>`, `rg-<14-char>`) so that two implementations seeing the same source evidence produce identical sort orders and therefore identical hashes. Stable ids are generated by the producing pipeline step using a deterministic rule (typically the SHA-256 prefix of the upstream artifact id plus the element's index within its parent) and are recorded once; they never depend on processing-order non-determinism.
- `topic_description_hash` - SHA-256 of NFC bytes of the operator-supplied topic description after Unicode whitespace stripping at the start and end. When no topic description is set, when the description is absent, or when the description is entirely whitespace, the hash is the SHA-256 of the empty byte string.
- `glossary_context_hash` - SHA-256 of canonical JSON of `{glossary_id, glossary_version, glossary_entries_hash}`. `glossary_entries_hash` is the SHA-256 of canonical JSON of the approved glossary entry projection sorted by `entry_id` ascending. The projection includes exactly `entry_id`, `surface_form`, `normalized_form`, `aliases`, `misreadings`, `domain`, `protected_zone`, `status`, and `decision_rationale`; it excludes evidence references, approver identity, timestamps, confidence, and reviewer notes because those are audit metadata rather than prompt/output-affecting glossary context. `aliases` and `misreadings` are each sorted ascending after NFC normalization and duplicate removal. Only entries whose `status` is `approved` appear in the projection. Before an approved glossary exists, `glossary_id` is the literal `_none_`, `glossary_version` is `v0000`, and `glossary_entries_hash` is the SHA-256 of the empty byte string. This component makes the no-glossary first-pilot state and every later approved-glossary change explicit in cache identity even though the rendered prompt also contains the glossary summary text.
- `render_profile_hash_full` - the full SHA-256 hex digest of canonical JSON of the render profile fields, as defined in Phase 5. This full digest is the cache-identity component. The short `render_profile_id` remains the 12-character lowercase hexadecimal prefix used for human-facing references, filenames, reports, crop records, evidence sidecars, and image assets.

`tool_profile_hash` is recorded in provenance but is not part of `cache_key`. It is computed as the SHA-256 of canonical JSON of the active `tool_profile.json` content. The compact provenance/log field is named `tool_profile_hash_compact` and carries the first 16 lowercase hexadecimal characters of that digest; the untruncated digest is named `tool_profile_hash_full`. A tool-profile change that alters output-affecting fields must change one of the explicit cache-key components above or bump `cache_key_version`; otherwise `doctor` fails the tool-profile revision as an unsafe cache-key change.

### Backend Execution Parameters

Backend YAML carries provider-native execution parameters under a single `default_parameters` mapping rather than special-cased top-level fields such as a generic `reasoning_effort`. Each backend adapter forwards the merged parameter bag to its provider using the provider-native key names (for example, OpenAI `reasoning_effort`, Anthropic `thinking.budget_tokens`, Gemini `thinking_config`, Codex CLI `--reasoning-effort`, Claude Code `--thinking`). Tool-profile per-role overrides and per-call overrides use the same parameter-bag shape and are merged with last-writer-wins semantics. The merged bag is what `model_parameters_hash` hashes, so a parameter change for any provider is captured naturally in the cache key without coercion between vendor vocabularies. The pilot tool profile records which provider-native parameter (if any) the operator intends as the effort control for each role so that comparative reports remain interpretable.

Each backend YAML declares a `transport_only_parameters` list of keys that must be excluded from `model_parameters_hash`. Tool-profile role bindings may extend that list additively through `transport_only_parameter_additions`; they may not shorten the backend's list. The hash is computed over the merged parameter bag after dropping every key in the effective union of the two lists. The conservative default is "every key not listed is output-affecting," so an unrecognized provider-native parameter is always captured by cache identity rather than silently bypassed. A `transport_only_parameters` change in backend YAML is a backend-config change and is captured by `backend_config_hash`; a `transport_only_parameter_additions` change in the tool profile is captured by `tool_profile_hash` and, because the effective exclusion set is recorded per call in provenance, the change is auditable even though it never enters `cache_key` directly. `noeasymark doctor --backends` rejects a backend whose `transport_only_parameters` list overlaps a key the backend YAML's own examples describe as output-shaping, whose proof object does not match the excluded key set exactly, whose proof evidence is stale or missing for a shipped example, or whose role binding adds an exclusion without a matching role-binding proof.

## Phase 0 - Template Adoption, Repo Scaffolding, and Devcontainer

The repository is initialized from the `franklesniak/copilot-repo-template` template. Creating `franklesniak/NoEasyMark` through GitHub's **Use this template** flow establishes the baseline tooling: `pyproject.toml`, `package.json`, `.pre-commit-config.yaml`, `.markdownlint.jsonc`, `.remarkrc.mjs`, `.remarkignore`, `.yamllint.yml`, the GitHub Actions workflows under `.github/workflows/`, modular instruction files under `.github/instructions/`, `.github/copilot-instructions.md`, `.cursor/rules/repository-instructions.mdc`, `.hermes.md`, `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md`. NoEasyMark adds tool-specific files on top of that baseline and modifies template-shipped files only as part of bounded downstream adoption, an intentional downstream override, or an upstream template sync.

The implementing agent starts Phase 0 inside a clean clone of `franklesniak/NoEasyMark` after the operator has created the repository from the template and copied this file to `docs/specification.md`. The target repository copy is the authority for implementation. If the operator has not copied the spec into the target repository, the agent's first action is to create `docs/`, copy the spec content provided by the operator into `docs/specification.md`, and commit that source-of-truth file before changing inherited template files. The agent does not read implementation requirements from `research-misc` after the target repository contains `docs/specification.md`.

Before Phase 0 mutates files, the agent verifies the local Git context: the current working tree is a clone whose `origin` resolves to `franklesniak/NoEasyMark`, the default branch is `main`, the current branch is an `implementation/...` branch or can be created from `main`, and the GitHub MCP connector is installed for the target repository with access to read repository metadata, create labels, open PRs, post comments, query reviews, query status checks, and read branch state. A wrong repository, wrong remote, detached head that cannot create the phase branch, missing target-repository connector installation, or branch namespace mismatch stops Phase 0 before adoption edits with `MANUAL_SETUP_INCOMPLETE` when it is an operator setup gap, or with usage/configuration exit code `2` when the invocation itself named the wrong path or branch.

**Scope of implementation.** The implementing agent's job is bounded by the **end-to-end definition** in the **Implementation milestoning** closed handoff decision. The agent builds the tool, ships the scaffolding, lands all specified tests and CI, and proves the runtime is ready by completing the **deferred-content init smoke** described in that same decision. The agent does **not** execute a first real conversion run against the operator-supplied corpus; that is an operator-driven follow-up task that begins after the development handoff PR merges. Wherever this spec describes runtime behavior for a real run (preprocess against a real PDF, pilot, chapter conversion, judge calls, chapter PRs, finalize), the agent's implementation responsibility is to make the code work and to validate it against synthetic fixtures and unit/integration tests, not to perform that run end-to-end during implementation.

**Phase 0 freshness record.** Phase 0 writes `.implementation-state/phase-0-freshness.json` and includes a generated Markdown table with the same rows in every Phase 0 PR body that introduces or changes the recorded facts. The record is schema-valid and each row carries `freshness_id`, `fact_group` (`os_lifecycle`, `github_action_pin`, `tool_version_pin`, `devcontainer_base`, `windows_installer`, `template_upstream`, `backend_or_engine_vendor_fact`, or `installed_tool_help`), `spec_field`, `observed_value`, `status` (`verified`, `updated`, or `verification_deferred`), `checked_at_utc`, `checked_by`, `evidence_kind` (`official_doc`, `installed_help`, `api_probe`, or `operator_record`), `evidence_ref`, and nullable `todo_ref`. Required rows cover the upstream template commit, supported OS lifecycle windows, the CUDA and CPU devcontainer base tags and observed digests, the `astral-sh/setup-uv` action major, the pinned `uv` resolver version, every load-bearing package pin in **Pinned Dependency Policy**, every Windows installer route in **Cross-platform Toolchain Installation (Host-Native)**, and every backend or cloud/document-service engine vendor fact that already requires `change_notes.vendor_doc_freshness[]`. For backend/engine YAML facts, the YAML `vendor_doc_freshness[]` entry and the Phase 0 freshness row share the same `freshness_id` so reviewers can trace the PR-body table back to the committed configuration. A deferred row must also create or update `_TODO_AFTER-BUILD-MANUAL-STEPS.md` with the exact verification still needed.

**Concurrent operator activity on `main`.** The agent operates exclusively on `implementation/...` branches and never pushes to `main` directly. The maintainer may merge unrelated PRs into `main` while the agent is mid-implementation (Dependabot bumps, hotfixes from other tasks, etc.); these merges are not an agent-halt condition. To detect divergence at handoff time, the first commit of Phase 0 writes `.implementation-state/initial-main-head.json` recording the `main` HEAD SHA, the timestamp, and the verifying tool identity. When the agent opens the final development-handoff PR (`implementation/noeasymark` → `main`), it queries `main`'s current HEAD through the GitHub MCP connector, compares against the recorded value, and — if `main` has advanced — adds a single advisory paragraph at the top of the final PR body naming the recorded SHA, the current SHA, and the count of intermediate commits, so the maintainer is aware that a rebase or merge may be required before fast-forward is possible. The advisory is informational; it does not block the PR open. Concurrent activity on `main` is a maintainer-side merge concern, not an agent state-machine concern.

**Manual-setup verification.** Before any template-adoption work, the agent runs a `manual_setup_verification` precheck that uses its GitHub MCP connector to confirm every frontmatter `manual_setup_steps` item that is observable through GitHub's API: repository exists with the expected `franklesniak/NoEasyMark` owner/repo pair, visibility matches the `repository_visibility` frontmatter field, default branch is `main`, Issues are enabled, Discussions are enabled, private vulnerability reporting is enabled, Dependabot alerts and security updates are enabled, secret scanning and push protection are enabled, Actions workflow permissions include `contents: write` and `pull-requests: write`, and the labels enumerated in the `manual_setup_steps` block all exist (the agent is authorized to create the missing labels through the MCP connector with the fallback colors recorded in that block — this is the single mutation the precheck is allowed to make, because the spec explicitly authorizes it). Items that cannot be observed through the API (the LFS test push, the upstream-template-visibility check when the upstream is private, operator behavioral steps, and any setting for which the current connector lacks a read surface) are reported as `unverifiable_assumed_complete` only after the precheck writes a row naming the exact setting, the expected value, the GitHub web-UI path or manual command the operator should use to verify it, and the frontmatter rationale that makes it required. Any verifiable item that is wrong stops Phase 0 with `MANUAL_SETUP_INCOMPLETE` and prints a single consolidated remediation table naming each missing item, the exact GitHub MCP action or web-UI path to fix it, and which frontmatter rationale references the requirement. The precheck does not toggle Discussions, vulnerability reporting, Dependabot, secret scanning, or workflow permissions on the operator's behalf, because those changes affect repository policy and should be operator-visible decisions. The precheck is rerunnable; the operator fixes the missing item and re-launches the agent.

The default Phase 0 stack decision is to remove PowerShell and Terraform. If the operator wants either stack retained, the operator gives that instruction before Phase 0 begins in the implementation task itself; no pre-existing `.template-sync/marker.yml` is required. The implementing agent then creates the downstream marker during Phase 0 with the corresponding module retained in `included_modules`.

Initial template adoption uses this bounded stack selection:

- Retain Python, Markdown, JSON/YAML/schema validation, GitHub Actions linting, Dependabot validation, pre-commit, markdownlint, remark link validation, and the agent instruction framework.
- Retain the template-sync-support module by default so future upstream template updates can be applied deliberately. The upstream template ships `.template-sync/manifest.yml`, `.template-sync/scripts/`, `schemas/template-sync-marker.schema.json`, and matching fixtures. Downstream repositories that retain sync support create `.template-sync/marker.yml` during adoption using the exact schema shape inherited from the adopted template commit. The marker is not a NoEasyMark-owned schema extension point; if the upstream template schema changes, the marker follows the inherited schema rather than the example below. For the template commit inspected while authoring this specification, the downstream marker shape is:

  ```yaml
  template_sync:
    source_repo: https://github.com/franklesniak/copilot-repo-template.git
    last_reviewed_template_commit: "<full 40-char SHA resolved at Phase 0 — see the paragraph after this block>"
    included_modules:
      - baseline
      - python
      - markdown
      - json
      - yaml
      - schema
      - github-actions
      - github-platform
      - github-templates
      - agent-instructions
      - template-sync-support
    local_overrides:
      - path: templates/
        reason: NoEasyMark copies needed Python/schema starter content into load-bearing locations and removes template scaffolds during Phase 0.
        default_decision: REMOVE-LOCAL
      - path: COPILOT_CHAT_PROMPTS.md
        reason: Template-maintainer prompt catalog removed during downstream adoption following the same pattern as the inherited GETTING_STARTED_*, OPTIONAL_CONFIGURATIONS, TEMPLATE_MAINTENANCE, and TEMPLATE_DESIGN_DECISIONS removals; downstream maintainers may reauthor a NoEasyMark-specific prompt catalog later if needed.
        default_decision: REMOVE-LOCAL
      - path: .github/workflows/powershell-ci.yml
        reason: PowerShell stack is not retained.
        default_decision: REMOVE-LOCAL
      - path: .github/workflows/terraform-ci.yml
        reason: Terraform stack is not retained.
        default_decision: REMOVE-LOCAL
      - path: .github/instructions/powershell.instructions.md
        reason: PowerShell stack is not retained; protected-file change is pre-authorized by docs/specification.md.
        default_decision: PROTECTED-REVIEW
      - path: .github/instructions/terraform.instructions.md
        reason: Terraform stack is not retained; protected-file change is pre-authorized by docs/specification.md.
        default_decision: PROTECTED-REVIEW
      - path: .claude/hooks/session-start.sh
        reason: Terraform install block removed because NoEasyMark does not retain the Terraform stack; hook itself is preserved for future tool installs.
        default_decision: PROTECTED-REVIEW
      - path: .github/instructions/pdf-conversion.instructions.md
        reason: NoEasyMark-owned instruction file. The upstream template's catch-all `.github/instructions/*.instructions.md` mapping classifies this file as `agent-instructions` for sync purposes; this override entry surfaces the file explicitly so future syncs do not silently treat it as a removed-upstream file.
        default_decision: SKIP
  ```

  The implementing agent does not hardcode `last_reviewed_template_commit`. At Phase 0 it resolves the upstream template's current default-branch HEAD — the full 40-character commit SHA whose inherited files it is reconciling against — through the implementing platform's GitHub MCP connector, and writes that value into `.template-sync/marker.yml`. The value must be a resolved full 40-character SHA, never a branch name, tag, or other moving ref. If the maintainer supplied a specific pin in the implementation task, the agent uses that instead. This follows the first-adoption procedure in the upstream `TEMPLATE_UPDATE_PROCEDURE.md` (set the base SHA at adoption; advance it only on later reviewed syncs). Because the repository is created from the template's current HEAD via "Use this template", the resolved HEAD and the inherited working tree normally match; if upstream commits landed between repo creation and adoption, the agent reconciles them per the first-sync procedure before recording the SHA. Before any edit to a file in the **agent instruction inventory** or **agent runtime configuration inventory** described below, the agent writes a `protected_file_decisions` entry into `.template-sync/marker.yml` using the upstream marker schema's real enum (`TAKE`, `MERGE`, `SKIP`, `REMOVE-LOCAL`, `DEFER`, `PROTECTED-REVIEW`) with `adoption_mode: minimal-preservation`, plus the schema-required `authorization_basis` (the relevant section of `docs/specification.md`), `authorized_scope`, and `reason`; the protected-file edit and its matching marker entry land in the same commit. NoEasyMark's entire adoption stays within `adoption_mode: minimal-preservation` (identity substitution, module-trim, the single intended Source-of-Truth customization, and metadata refresh); file deletions such as the PowerShell/Terraform instruction files use `REMOVE-LOCAL` with an `authorization_basis` that names the file and the word "removal". The marker field name is the schema's `protected_file_decisions`. This makes the standing pre-authorization recorded against `@franklesniak` (see the prose paragraph that introduces protected-file authorization later in this section) self-contained in the repository, so a fresh agent reviewing the diff later can confirm every protected-file change without consulting an external task description.

  The initial `local_overrides` list is the one shown above; later phases append to `local_overrides` only for template-owned paths they intentionally modify or remove, plus the narrow set of NoEasyMark-owned paths that happen to match the upstream template's catch-all glob mappings. NoEasyMark-owned paths such as `src/noeasymark/`, `config/`, `tools/`, and `.devcontainer/` are ordinary downstream project files unmapped by the upstream template; they are not listed in `.template-sync/manifest.yml`, and future template-sync tooling must not remove unmapped downstream files merely because the upstream template does not know about them. `.github/instructions/pdf-conversion.instructions.md` is a NoEasyMark-owned file but matches the template's `.github/instructions/*.instructions.md` catch-all and is therefore listed in `local_overrides` with `default_decision: SKIP` so future syncs surface it as an explicit downstream-owned file that intentionally does not adopt an upstream replacement. Module names in `included_modules` must exactly match `.template-sync/manifest.yml`; convenience aliases such as `precommit`, `markdownlint`, `dependabot`, or `github_actions` are not valid marker values.
- Retained template-sync tooling is adopted as downstream tooling, not as immutable upstream text. Keep `.template-sync/manifest.yml`, `.template-sync/marker.yml`, `.template-sync/scripts/**`, `schemas/template-sync-marker.schema.json`, `schemas/template-sync-manifest.schema.json`, the template-sync schema examples, `tests/test_generate_sync_candidates.py`, `tests/test_template_manifest.py`, and `tests/test_validate_marker.py`. Keep `TEMPLATE_UPDATE_PROCEDURE.md`, but rewrite it during Phase 0 so its module table, path-mapping table, and sync procedure describe the adopted NoEasyMark module set and the downstream-owned-file policy. Remove template onboarding documents that are not part of retained sync tooling (`GETTING_STARTED_NEW_REPO.md`, `GETTING_STARTED_EXISTING_REPO.md`, `OPTIONAL_CONFIGURATIONS.md`, `TEMPLATE_MAINTENANCE.md`, `.github/TEMPLATE_DESIGN_DECISIONS.md`) after applying them. Remove `COPILOT_CHAT_PROMPTS.md` during Phase 0, update `.template-sync/manifest.yml` and `tests/test_template_manifest.py` so its absence is expected, and record the removal in `.template-sync/marker.yml` `local_overrides` with `default_decision: REMOVE-LOCAL` and rationale "Template-maintainer prompt catalog removed during downstream adoption following the same pattern as the inherited GETTING_STARTED_*, OPTIONAL_CONFIGURATIONS, TEMPLATE_MAINTENANCE, and TEMPLATE_DESIGN_DECISIONS removals; downstream maintainers may reauthor a NoEasyMark-specific prompt catalog later if needed." No retained test may require a file that Phase 0 removes.
- Update the retained template-sync tests so they distinguish template-owned files from downstream-owned files. The manifest remains authoritative only for files that are still template-owned or intentionally governed by a template catch-all. Markdown files under downstream-owned NoEasyMark paths (`docs/specification.md`, NoEasyMark reports, NoEasyMark design docs, and future `docs/noeasymark/**` material) are not manifest omissions merely because the upstream template did not map them. The tests must still fail when a retained template-owned file lacks a manifest mapping, when a manifest mapping references a removed template-owned file without an override, or when a downstream-owned file matches a retained upstream catch-all and lacks the explicit local override required by this specification.
- Rename the template example package under `src/copilot_repo_template/` to `src/noeasymark/`, update `pyproject.toml`, and replace project placeholders. The shipped identity values are: repository `franklesniak/NoEasyMark`, package and distribution name `noeasymark`, GitHub owner handle `@franklesniak`. The shipped `SECURITY.md` directs reporters to GitHub's private vulnerability reporting flow (Security tab -> Report a vulnerability) and contains no email address. The shipped `CODE_OF_CONDUCT.md` directs reporters to contact the repository maintainers through the contact information published on their GitHub profile pages and contains no email address. The implementing agent is authorized to replace the template's placeholder `.github/CODEOWNERS` during Phase 0 with the sole entry `* @franklesniak`; this routes chapter-PR auto-review to the maintainer and is the canonical CODEOWNERS state at adoption time. The operator may edit `.github/CODEOWNERS` later to add additional maintainers. No security-contact email is ever shipped; the implementing agent fails CI rather than insert one.
- Convert the inherited Python packaging workflow from pip-based installation to `uv` during Phase 0. Commit `uv.lock`; keep the root `pyproject.toml` as the dependency source of truth; place development-only dependencies in the `dev` dependency group; place optional engine stacks in named extras; and update `python-ci.yml` to install `uv` with the official `astral-sh/setup-uv` action, run `uv lock --check`, run `uv sync --locked --dev`, and invoke `mypy` and `pytest` through `uv run`. The action reference follows the template's GitHub Action pinning policy: use the current stable `astral-sh/setup-uv` major at adoption time, do not use `@main` or an unqualified latest alias, and configure Dependabot to surface later major bumps. Confirm during Phase 0 that the inherited `.github/dependabot.yml` already contains a `package-ecosystem: github-actions` entry that covers `.github/workflows/`; if it does not, add one in the same Phase 0 PR so that `astral-sh/setup-uv` (and every other pinned action this repository introduces) gets routine Dependabot bumps. Also ensure the pinned `uv` toolchain version supplied to the `astral-sh/setup-uv` action's `version:` input matches the resolver that produced the committed `uv.lock`; the pinned `uv` version is bumped only together with a refreshed lockfile. The `uv` tool version itself is pinned with the action's `version:` input so CI uses the same resolver that produced the committed `uv.lock`; the pinned `uv` version is bumped only together with a refreshed lockfile. Do not use `uv sync --all-extras` in core CI, because optional engine extras may require large native binaries, GPUs, service SDKs, or external tools that are intentionally outside the core support window. Optional-stack workflows may install explicit extras by name when validating that stack. Preserve the template's Python-version matrix in `python-ci.yml`, but make that per-PR workflow run on `ubuntu-latest` only; the operating-system matrix belongs in `cross-platform-check.yml`. Remove the template's initial `continue-on-error` posture for mypy once NoEasyMark source replaces the placeholder package; type-check failures are real CI failures unless an explicit temporary waiver is recorded in the Phase 0 PR body. The Phase 0 PR body explicitly calls out this deliberate divergence from the live template's pip-based install, optional-dependency posture, advisory mypy setting, and per-PR OS shape, so reviewers can distinguish intentional downstream policy from accidental template drift.
- Configure a cross-platform CI matrix during Phase 0. Per-PR-blocking jobs run on `ubuntu-latest` only to keep PR feedback fast and macOS CI minute consumption bounded. A nightly cron job, a manually-dispatchable workflow, and a pull-request label trigger named `cross-platform-check.yml` run the Python test suite (`uv run pytest`), the schema-example tests (`uv run pytest tests/test_schema_examples.py`), the schema-hook regeneration check (`uv run noeasymark dev regenerate-schema-hooks --check`), the public-schema regeneration check (`uv run noeasymark dev regenerate-public-schemas --check`), and the render-profile regeneration check (`uv run noeasymark dev regenerate-render-profile --check`) on the matrix `os: [ubuntu-latest, macos-latest, windows-latest]`. A separate nightly job builds the CPU devcontainer from its floating base tag, verifies that the observed digest and toolchain match `.devcontainer/toolchain-fingerprint.json`, then runs the synthetic-fixture conversion suite and asserts byte-identity against committed expected outputs only after that fingerprint match passes; the matrix runners themselves assert schema-equivalent, evidence-comparable output per the cross-platform output equality rules in **Cross-platform Path and Encoding Conventions**. The workflow uses `pull_request` activity types `opened`, `synchronize`, `reopened`, and `labeled`, with a job-level condition that runs the matrix only when the PR has the `cross-platform-check` label or the event was a manual dispatch/nightly schedule. If commits are pushed while the label remains present, the `synchronize` event runs the matrix for the updated head. Because GitHub manual `workflow_dispatch` events are available only after the workflow file exists on the default branch, Phase 0's own PR is responsible for installing and syntax-validating the workflow; the first required hosted cross-platform run occurs after Phase 0 lands. The Phase 0 wrap-up PR body **and** `_TODO_AFTER-BUILD-MANUAL-STEPS.md` both carry the same explicit operator instruction: "After this PR merges, label any subsequent PR (the next phase's PR is a natural candidate) with `cross-platform-check` and confirm the matrix passes on `ubuntu-latest`, `macos-latest`, and `windows-latest`. This is the first required hosted cross-platform run; it must be green before any later phase PR is expected to land." The `_TODO_AFTER-BUILD-MANUAL-STEPS.md` entry has its own checkbox and the operator removes it on completion; the Phase 0 PR-body instruction is permanent. For avoidance of doubt, Phase 0 is complete when it installs and syntax-validates `cross-platform-check.yml`; the hosted matrix pass is a post-Phase-0, pre-later-phase-merge requirement and remains a final development handoff requirement. Cross-platform failures from the nightly cron open issues labeled `cross-platform-regression` so they surface against the next PR cycle. Pre-commit, markdownlint, and the template-sync-support hooks remain Linux-only because they validate text content rather than runtime behavior. PR authors who change runtime semantics (signal handling, file locking, atomic writes, subprocess management, path handling) add the `cross-platform-check` label to trigger the matrix on their PR before merge. The "OS-sensitive paths" whose modification automatically requires the label are: `src/noeasymark/lock/**`, `src/noeasymark/atomic/**`, `src/noeasymark/subprocess/**`, `src/noeasymark/path/**`, `src/noeasymark/time/**`, `src/noeasymark/render/**` (because PyMuPDF renderer behavior is OS-sensitive), any `.devcontainer/**` change, any `.github/workflows/cross-platform-check.yml` change, and any change that adds or removes a binary-asset extension to `.gitattributes`. A NoEasyMark-owned pre-commit hook `enforce-cross-platform-label` (declared in a NoEasyMark-owned hook block of `.pre-commit-config.yaml`, not the template-sync block and not the generated schema-hook block) verifies at PR-CI time that any PR whose diff touches an OS-sensitive path carries the `cross-platform-check` label; local pre-commit runs lack PR event payload and label visibility, so the hook reports an advisory or skips label enforcement locally unless the CI environment exposes the pull-request event payload needed to evaluate labels. PRs without the label fail this PR-CI check and the agent (or PR author) adds the `cross-platform-check` label through the GitHub MCP connector and re-runs CI. The matrix is also a required check on the final development-handoff PR (`implementation/noeasymark` → `main`) regardless of which files it touches, because cumulative cross-platform readiness is a definition-of-done criterion.
- Audit every retained pre-commit hook for Windows-native compatibility during Phase 0. Hooks that invoke bash directly are either replaced with a Python equivalent (Python is already a hard dependency) or scoped to repositories where pre-commit runs in Git Bash or WSL. The shipped audit result is recorded in `.pre-commit-config.yaml` as a comment block listing each hook and its cross-OS status. macOS native pre-commit and Linux native pre-commit are treated as the supported defaults; Windows-native pre-commit must work through Git for Windows' bundled bash or the operator's PowerShell + Python toolchain. The acceptance test for the audit is concrete: after stack pruning and hook rewiring, `pre-commit run --all-files` must pass on a Windows host or Windows GitHub Actions runner with only the documented NoEasyMark host-native prerequisites installed, and it must not require WSL unless the hook is explicitly documented as optional and inactive by default. The implementing agent does not need separate operator approval for the audit when every retained hook passes this test without weakening validation. It must stop and request maintainer input only if a retained hook would need to be disabled, made Linux-only, made WSL-only, or weakened relative to the Linux/macOS enforcement path.
- Remove the optional PowerShell and Terraform example source, tests, templates, workflows, scripts, docs, and instruction references. NoEasyMark is Python-only, so PS and TF carry no functional weight. An operator who needs either stack states that retention requirement before Phase 0 begins; the implementing agent then keeps the matching module in `.template-sync/marker.yml` `included_modules` while applying the rest of Phase 0. The specific pruning at Phase 0 removes `tests/test_terraform_hooks.py`, `.github/scripts/terraform_hooks.py`, the inherited `docs/terraform/` directory, the entire `tests/PowerShell/` directory, and `tests/test_example.py` (paired with `src/copilot_repo_template/example.py`, which is also removed by the package rename below). The template-sync-support tests `tests/test_generate_sync_candidates.py`, `tests/test_template_manifest.py`, and `tests/test_validate_marker.py` are retained because template-sync support is retained. `tests/test_check_prohibited_placeholders.py`, `tests/test_dependabot_schema.py`, and `tests/fixtures/dependabot/auto-assignment.yml` are retained because their supporting infrastructure is retained. `tests/test_schema_examples.py` is rewired to auto-discover the new NoEasyMark schemas rather than removed.
- Retain `check-placeholders.yml` permanently as a guard against future template-sync regressions that could reintroduce `OWNER/REPO` or `@OWNER` placeholders. The upstream workflow short-circuits in the template repository itself via a repository-name guard, so once placeholders are replaced in the downstream repository the workflow simply runs and passes; there is no transitional removal step and no reason to ever remove it.
- Customize `.github/ISSUE_TEMPLATE/` during Phase 0 so the template's commented `triage` label entries are activated where appropriate, `OWNER/REPO` links are replaced with `franklesniak/NoEasyMark`, and the Discussions contact link is uncommented only if the operator has enabled Discussions for the repository. If Discussions are not enabled, keep the support/discussion link commented and do not claim that issue templates reference Discussions.
- Retain `auto-fix-precommit.yml` during Phase 0 by default because the inherited workflow is narrowly scoped to Copilot coding-agent branches and does not affect NoEasyMark runtime behavior. Preserve its `copilot/**` branch and bot-identity scope exactly; the fact that it does not run on `implementation/**` or `conversion/**` branches is intentional and is not a Phase 0 defect or `_TODO_AFTER-BUILD-MANUAL-STEPS.md` item. If the maintainer later knows GitHub Copilot coding-agent branches will never be used, remove the workflow in a separate cleanup PR and record that downstream override in `.template-sync/marker.yml`.

Stack removal must follow the template module boundaries rather than deleting only the obvious source files. If PowerShell or Terraform support is not retained, remove the corresponding workflows, tests, templates, lint settings, `.pre-commit-config.yaml` inline blocks, `data-ci.yml` or `precommit-ci.yml` setup blocks that call stripped hooks, and references from protected instruction files after explicit authorization. As a general rule, for every removed stack the agent scans every retained file in the inherited template surface (`.github/workflows/`, `.github/instructions/`, `.pre-commit-config.yaml`, `.claude/hooks/`, and any other retained file that carries upstream-shipped inline blocks) for `# template-sync: begin <module>-only` / `# template-sync: end <module>-only` delimited regions whose `<module>` matches the removed stack, and strips each delimited region in the same Phase 0 PR that removes the stack. The `.github/workflows/auto-fix-precommit.yml` workflow's `template-sync: begin terraform-only` block is one current instance; the general rule catches future template additions of the same shape without requiring a spec edit. Remove the `templates/` directory entirely once the stack scaffolds it carries are no longer needed; NoEasyMark does not author new stack scaffolds in this directory. The `templates/` removal must follow the salvage-first ordering so the working tree is never in a state where neither the source nor the copy of a needed configuration block exists:

1. Copy every load-bearing block out of `templates/` into its downstream location in a single commit. The known load-bearing blocks are the full `[tool.ruff]` and `[tool.ruff.lint]` configuration from `templates/python/pyproject.toml` into the root `pyproject.toml`, the `[tool.mypy]` configuration from the same file into the root `pyproject.toml`, and the schema-example pytest from `templates/python/tests/test_schema_examples.py` into `tests/test_schema_examples.py` if the downstream `tests/test_schema_examples.py` does not already exist. Before copying a TOML table, parse the root `pyproject.toml` as TOML and detect whether the destination table already exists. If it exists, merge only missing keys after comparing values; conflicting existing values are kept when they are already stricter for NoEasyMark, otherwise the Phase 0 PR body calls out the conflict and the agent chooses the template value only when tests prove it is required. Never append duplicate TOML tables.
2. Run `pre-commit run --all-files` and the schema-example pytest against the new locations to confirm the copies are wired in.
3. Delete the `templates/` directory in a follow-up commit; CI must still pass after the deletion because step 1 has already moved the dependencies.

When NoEasyMark's schemas replace the template's worked example schema, perform the swap as a single change so that no validator or test points at a removed alias:

- Delete `schemas/example-config.schema.json`, `schemas/examples/example-config/valid/*`, and `schemas/examples/example-config/invalid/*`.
- Split the inherited `.pre-commit-config.yaml` schema hook region before deleting hook aliases: the retained upstream `template-sync` schema hooks remain in template-owned `template-sync: begin schema-template-sync-support-only` blocks, while all NoEasyMark schema-example and schema self-validation hooks are generated into adjacent `# noeasymark: begin schemas` / `# noeasymark: end schemas` blocks. The generated block is owned only by `noeasymark dev regenerate-schema-hooks`; the template-owned block is never rewritten by that command.
- Remove `.pre-commit-config.yaml` hook entries `validate-example-config-schema` and `validate-example-config-valid-examples` only after their replacements exist in the NoEasyMark-generated block.
- Remove any `data-ci.yml` step that names the example-config hook aliases, then add the NoEasyMark-generated schema-hook check in the same change so CI never points at a removed alias.
- Rewire `tests/test_schema_examples.py` to auto-discover the new `schemas/<name>.schema.json` files and their `schemas/examples/<name>/{valid,invalid}/` fixture trees.
- Retain `schemas/template-sync-marker.schema.json`, its `schemas/examples/template-sync-marker/{valid,invalid}/` fixtures, the related `validate-template-sync-*` pre-commit hooks, and `tests/test_dependabot_schema.py` so that the `vendor.dependabot` schema lock and the template-sync workflow continue to work.

Add `.noeasymark-state/`, `.noeasymark-artifacts/`, `.implementation-state/`, and the default deliverable output root `/output/` to `.gitignore`. The `.implementation-state/` directory holds development-time gate-record artifacts (currently just `gates/schema-design-review.json`) that the implementing agent uses to track development-phase blocking gates; it never carries conversion-run state or source-derived content. Normalize `.gitattributes` during Phase 0 so text-classified files use LF line endings on every host, then add Git LFS tracking for the binary asset extensions NoEasyMark expects to accumulate (PDFs, plus the image formats generated by Phase 7 figure extraction and the image formats commonly present in operator-supplied reference materials) unconditionally. Place the global text rule before binary and LFS extension overrides, and place the LFS rules after it so binary assets remain `-text`. The shipped block below is the canonical binary-asset LFS block; other sections cross-reference it instead of redefining the extension list:

```gitattributes
* text=auto eol=lf

*.pdf filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
*.gif filter=lfs diff=lfs merge=lfs -text
*.tiff filter=lfs diff=lfs merge=lfs -text
*.tif filter=lfs diff=lfs merge=lfs -text
*.webp filter=lfs diff=lfs merge=lfs -text
*.bmp filter=lfs diff=lfs merge=lfs -text
```

These rules are harmless when no binary files of a given type are ever committed and prevent the common failure mode where a future repository-storage-approved commit lands a multi-megabyte binary as an ordinary Git blob (which permanently bloats history). Text-based image formats are deliberately omitted from the LFS block: `*.svg` is XML and benefits from ordinary Git diff-and-merge. The existing `.gitattributes` binary classification rules are not by themselves Git LFS tracking.

The Phase 0 `.gitattributes` edit is idempotent. If the operator used the alternate corpus-upload step to add these rules before the implementing agent ran, Phase 0 detects semantically equivalent existing rules, normalizes them into one canonical block, and does not append duplicates. If an existing rule differs materially (for example it marks PDFs `binary` but lacks `filter=lfs diff=lfs merge=lfs -text`), Phase 0 preserves unrelated attributes, adds the missing LFS tracking rule in the canonical block, and records the normalization in the Phase 0 PR.

The `.gitattributes` normalization authority does not authorize broad protected-file churn. Protected instruction files and agent runtime configuration files may receive LF normalization only when that exact file is already being edited under one of the protected-file authorizations below; line-ending-only rewrites of otherwise untouched protected files are out of scope and must be left for an explicitly authorized cleanup.

Initial template adoption follows the upstream template's `GETTING_STARTED_NEW_REPO.md` checklist for every file shipped by the template that is not specifically called out by this specification. That checklist covers the replacement or update of `README.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `LICENSE`, `.github/CODEOWNERS`, `.github/ISSUE_TEMPLATE/`, `.github/pull_request_template.md`, `.github/dependabot.yml`, `.vscode/settings.json`, and `docs/PR_REVIEW_PROMPTS.md`, and the removal of template-maintainer onboarding documents once they have been read and applied. Phase 0 of this specification adds only the NoEasyMark-specific deltas on top of that checklist; an item not addressed here is governed by the upstream checklist unless it conflicts with the retained-template-sync rules above.

**Required content per file.** The agent does not author free prose for these files; each authored file must contain at minimum the elements below. Beyond the minimum the agent may inherit the template's prose with NoEasyMark identity swapped in.

- `README.md`: project title (`NoEasyMark`); a one-paragraph description matching the **Premise** section of `docs/specification.md`; a "Status: in development" badge or text marker; a link to `docs/specification.md` as the authoritative implementation contract; a prerequisites pointer (`uv`, Git LFS); a devcontainer-quickstart pointer (`.devcontainer/cpu/devcontainer.json` for non-GPU work, `cuda/` for GPU); a license pointer; and the upstream-template attribution linking to the public `franklesniak/copilot-repo-template` repository.
- `CONTRIBUTING.md`: development workflow (clone, `uv sync --locked --dev`, `pre-commit install`, branch-naming conventions for `implementation/` vs `conversion/`); the pre-commit discipline reminder inherited verbatim from the template's `.pre-commit-config.yaml` header; links to `.github/copilot-instructions.md`, `.github/instructions/pdf-conversion.instructions.md`, and `docs/specification.md`; a "How to report a bug" pointer that says "Open an issue using the bug-report template (a `triage` label is auto-applied)."
- `CODE_OF_CONDUCT.md`: the Contributor Covenant 3.0 text inherited from the current upstream template, with the reporting section completed to direct reporters to the repository maintainers through the contact information published on their GitHub profile pages (`https://github.com/franklesniak` for the sole maintainer at adoption time) per the **Identity placeholders** closed handoff decision. Remove upstream `[NOTE: ...]` placeholders and template-only customization comments. No email address.
- `SECURITY.md`: GitHub private vulnerability reporting flow per the **Identity placeholders** closed handoff decision. No email address.
- `LICENSE`: MIT text inherited from the template, with the year updated to the Phase 0 adoption year and the copyright holder set to "Frank Lesniak".
- `.github/pull_request_template.md`: short body covering "Summary", "Changes", "Testing performed", and a checklist that includes "I ran `pre-commit run --all-files` locally" and "The relevant CI jobs are green." This template serves the build-time development PRs; the conversion runtime opens no pull requests.
- `.vscode/settings.json`: minimal Python/Markdown editor settings consistent with the template's existing configuration; no operator-personal preferences.

**Agent entry-point file rewrites.** `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, `.hermes.md`, and `.cursor/rules/repository-instructions.mdc` start from the upstream template's body for each file (so structure, protected-file metadata blocks, and template-sync compatibility are preserved). The agent makes only identity-string substitutions (`copilot-repo-template` -> `NoEasyMark`, `@OWNER` -> `@franklesniak`, `OWNER/REPO` -> `franklesniak/NoEasyMark`, `<OWNER>` -> `franklesniak`) plus a protected-file metadata refresh in these five entry points. **It does NOT add or replace a project-guidance section in them.** Verified against the live template at spec-authoring time, the entry points are thin and contain no "Project-Specific Guidance" section; they also preserve any upstream instruction-contract anchors the template ships. The live template's instruction-contract validation is known to constrain `CLAUDE.md` and may constrain additional entry points in future template revisions, so the conservative downstream rule is to avoid section injection in all five entry points. The NoEasyMark project pointer is recorded once, in the `.github/copilot-instructions.md` "Source of Truth" section (surgical-edit #2). The replacement body is exactly:

```text
Read **[`docs/specification.md`](../docs/specification.md)** before making changes.

If any instruction here conflicts with the spec, **the spec wins**.
```

This pointer is recorded only in `.github/copilot-instructions.md` (surgical-edit #2). The five entry points are not otherwise modified, which preserves any upstream instruction-contract anchors they carry.

**Surgical-edit constraint on protected files.** Protected files (the agent instruction inventory and agent runtime configuration inventory enumerated below) are edited *only* through the enumerated authorized changes; the implementing agent does not rewrite, restructure, or paraphrase any other passage. The complete authorized change set for the initial adoption is:

1. **Identity-string substitution.** Replace `copilot-repo-template` -> `NoEasyMark`, `@OWNER` -> `@franklesniak`, `OWNER/REPO` -> `franklesniak/NoEasyMark`, and `<OWNER>` -> `franklesniak` everywhere these literals appear. This is mechanical text replacement; no surrounding prose is rewritten.
2. **Source-of-truth pointer.** In `.github/copilot-instructions.md` only, replace the template's placeholder `## Source of Truth` section body (the blockquote that begins with "**Customize this section** for your project.") with the literal three-line block: a heading line `Read **[`docs/specification.md`](../docs/specification.md)** before making changes.`, followed by a blank line, followed by `If any instruction here conflicts with the spec, **the spec wins**.`. No surrounding prose is rewritten.
3. **Entry-point content.** The five thin agent entry points (`.cursor/rules/repository-instructions.mdc`, `.hermes.md`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`) receive no section-level edit. The template does not give them a dedicated project-specific section, and they carry instruction-contract required anchors that must be preserved; the NoEasyMark project pointer lives only in `.github/copilot-instructions.md` (edit #2). These entry points receive only identity substitution (edit #1) and protected-file metadata refresh (edit #5).
4. **Discarded-stack reference removal.** For each discarded optional stack (PowerShell and Terraform under the default Phase 0 stack-pruning choice), the agent removes lines that *enumerate or list-reference* the discarded stack by name in the following bounded surfaces only: rows of stack tables in `.github/copilot-instructions.md`, bullet entries that name the stack in stack-roster lists, and the instruction-file row in any stack inventory. Free-prose paragraphs that discuss general repository hygiene without naming the discarded stack are not edited. The PowerShell and Terraform instruction files themselves (`.github/instructions/powershell.instructions.md`, `.github/instructions/terraform.instructions.md`) are deleted in their entirety rather than edited.
5. **Protected-file metadata refresh.** The template's protected files carry `Version` and `Last Updated` metadata fields. The agent bumps `Version` once per protected-file content change following the template's documented bump rule (typically a patch-version increment for the identity/section substitution), and sets `Last Updated` to the date the Phase 0 PR opens. No other metadata field is changed.
6. **`.claude/hooks/session-start.sh` Terraform block removal.** Remove the `TERRAFORM_VERSION` constant and the inline Terraform download/verify/install block (architecture/OS detection, HashiCorp releases download, SHA256 checksum verification, install to `/usr/local/bin`) plus related comments. The live template installs Terraform inline rather than via a named helper. All other hook content is preserved verbatim, including the `ensure_pre_commit` and `persist_path_prepend` helpers and the guard that exits before the install body when `CLAUDE_CODE_REMOTE` is not `true`.

The agent does NOT, in any protected file: rewrite or paraphrase explanatory prose, restructure headings, reorder sections, change the canonical NoEasyMark Source-of-Truth pointer from the verbatim form shown above, introduce new sections, change tone or voice, expand or compress passages for readability, fix grammar or spelling in inherited content, modernize examples, or alter cross-references that the template owns. The agent stops with `PROTECTED_FILE_SCOPE_EXCEEDED` and requests explicit maintainer instruction before performing any protected-file change that does not match one of the six bullets above.

Every protected-file edit is recorded in `.template-sync/marker.yml` under the marker schema's `protected_file_decisions` field, using the schema's `decision` enum (`TAKE`, `MERGE`, `SKIP`, `REMOVE-LOCAL`, `DEFER`, `PROTECTED-REVIEW`) with `adoption_mode: minimal-preservation` and the required `authorization_basis` and `authorized_scope`, plus the `docs/specification.md` section name that authorized the change. The full-file deletions of the PowerShell and Terraform instruction files are recorded as `REMOVE-LOCAL` decisions whose authorization explicitly names the file and the removal. This makes the audit chain self-contained and lets the next template-sync confirm that no broader edit was made.

Two related-but-distinct file inventories survive adoption. The first is the **agent instruction inventory**: `.github/copilot-instructions.md`, `.github/instructions/*.instructions.md`, `.cursor/rules/repository-instructions.mdc`, `.hermes.md`, `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md`. These are declarative documents that describe project conventions to the implementing agent. They are not modified unless the maintainer explicitly authorizes the exact change. The second is the **agent runtime configuration inventory**: `.claude/` (including `.claude/hooks/` and `.claude/settings.json`), `.codex/` (including `.codex/config.toml`), `.cursor/` beyond its `rules/repository-instructions.mdc` file, and `.vscode/` beyond its `settings.json`. These are operational settings (permissions, hooks, environment, editor preferences) that affect how the implementing agent or human editor behaves at runtime. They are also not modified without explicit maintainer authorization, for the same reason: changes silently alter implementing-agent behavior and break template-sync. Each inventory's contents are listed in the template-sync marker (`.template-sync/marker.yml`) so that future upstream syncs can detect and surface conflicts.

Some template-shipped workflow files are also effectively protected because they encode cross-stack assumptions; modifications during stack pruning are authorized as a bounded set. The initial adoption is granted standing pre-authorization (recorded against the maintainer `@franklesniak` in `.template-sync/marker.yml`) for the protected-file changes enumerated below. Template-owned workflow and hook-policy path changes that are not themselves in the protected instruction or runtime configuration inventories are recorded under `local_overrides` so future template syncs see the audit trail. Edits to files in the protected instruction inventory or protected agent runtime configuration inventory are recorded under `protected_file_decisions`; when such a file also needs future sync path-policy visibility, the same Phase 0 commit records both the `protected_file_decisions` entry and the matching `local_overrides` entry. Any protected-file change outside this enumerated set still requires explicit maintainer authorization at the time of the change. Instruction precedence is: this specification governs NoEasyMark product behavior, runtime architecture, schema contracts, content-handling policy, and the Phase 0 rewrite authorizations; inherited template instruction files govern general repository hygiene, protected-file handling, coding style, and template-sync process wherever this specification is silent. The new NoEasyMark instruction file (`.github/instructions/pdf-conversion.instructions.md`) is additive only: it must not replace, neuter, or supersede any inherited template instruction file. Where the NoEasyMark instruction file itself conflicts with an inherited instruction file, the inherited file wins unless the maintainer records an explicit override authorization in `.template-sync/marker.yml`; where this specification conflicts with an inherited instruction file on a NoEasyMark-specific implementation matter, this specification is the recorded override.

- Instruction files: update the source-of-truth section to point to this specification, remove references to discarded optional stacks, keep remaining agent entry points as thin summaries that defer to `.github/copilot-instructions.md`, and update protected-file metadata required by the template.
- Workflow files: strip hook aliases and setup blocks from `.github/workflows/data-ci.yml` and `.github/workflows/precommit-ci.yml` that target removed stacks (PowerShell, Terraform) or removed example schemas, with the rationale "stack not retained" or "example schema replaced by NoEasyMark schemas" recorded in the change description so the next upstream sync can be merged without confusion.
- Pre-commit configuration: remove `.pre-commit-config.yaml` inline blocks for non-retained stacks and replace the example-config schema-validation hooks with their NoEasyMark equivalents in the same change.
- Agent runtime hooks: edit `.claude/hooks/session-start.sh` to remove the Terraform install block (the `TERRAFORM_VERSION` constant, the `install_terraform`-style call site, and any related comments). The hook remains a no-op when `CLAUDE_CODE_REMOTE` is not `true`; for remote sessions, the hook is retained so the upstream template may later reintroduce other tool installs through the same hook, and its safe-PATH-prepend helpers remain useful. The rationale "Terraform stack pruned from NoEasyMark; hook left in place for future tool installs" is recorded in `.template-sync/marker.yml` under `protected_file_decisions` for authorization and under `local_overrides` with `default_decision: PROTECTED-REVIEW` for future template-sync path-policy visibility.

Add a NoEasyMark-specific entry under `.github/instructions/` (for example `pdf-conversion.instructions.md`) for guidance that applies to the conversion-pipeline source and configuration, alongside the stack-specific instruction files inherited from the template (`docs.instructions.md`, `python.instructions.md`, `json.instructions.md`, `yaml.instructions.md`, `gitattributes.instructions.md`).

The shipped `.github/instructions/pdf-conversion.instructions.md` content is:

````markdown
---
applyTo: "src/noeasymark/**,tests/**,schemas/**,config/**,pipeline.toml,tools/**"
description: "NoEasyMark conversion-pipeline rules: evidence-first, deterministic, resumable, and content-handling aware."
---

# NoEasyMark Conversion Pipeline Instructions

These instructions apply to NoEasyMark pipeline code, tests, schemas, prompts,
backend and engine configuration, render profiles, artifact-archive
configuration, and helper tools. The repository-wide constitution and
language-specific instructions remain authoritative. If this file conflicts
with inherited template instructions, the inherited instruction wins unless the
maintainer records an explicit override in `.template-sync/marker.yml`.

## Source Of Truth

Read `docs/specification.md` before changing conversion behavior. The spec is
the implementation contract for runtime gates, content handling, schemas,
artifact storage, cache identity, branch layout, and review surfaces.

## Evidence And Determinism

- Treat the original PDF and rendered page evidence as canonical. Normalized
  PDFs, OCR-derived PDFs, candidate Markdown, and model outputs are evidence
  sources, not replacement truth.
- Every source-derived output must be traceable to source pages, layout regions,
  candidate records, and adjudication or manual-review records where applicable.
- Prefer deterministic, schema-validated transformations. Any transformation
  that can change source meaning must go through judge or manual review.
- Preserve deterministic identifiers, hash inputs, timestamp formats, and cache
  key rules exactly as specified.

## Content Handling

- Never move source-derived content across a network class or into repository,
  remote-comment, or archive storage unless the active run records the required
  approval and allowlist match.
- Do not place absolute local paths, source excerpts, crops, candidate text, raw
  prompts, or raw model responses in public reports or remote comments unless
  the corresponding approval explicitly permits it.
- Redact public sibling records by omission, not by placeholder values.

## Runtime Boundaries

- Implement pipeline behavior in the `noeasymark` CLI and reusable library
  modules. Do not rely on chat reasoning, manual file edits, or unrecorded
  operator decisions for runtime conversion work.
- Backend and engine subprocesses must use structured argv, never shell strings,
  for source-derived calls.
- State-mutating commands must honor the single-writer lock, atomic-write, and
  resumability rules in the spec.

## Prompt And Backend Configuration

- Prompts live under `config/prompts/` and are versioned files, not Python string
  constants.
- Backend and engine YAML files are examples until enabled or bound in the
  active tool profile. Disabled examples must still validate against schemas.
- Credential values never appear in committed configuration, state, prompts,
  tests, logs, or reports.
````

Before generating code, the agent reads `.github/copilot-instructions.md`, the active agent entry point for the runtime being used, and the relevant modular instruction files under `.github/instructions/` for the files it will modify.

### Pinned Dependency Policy

The implementing agent pins every NoEasyMark-added Python dependency in `pyproject.toml` and `uv.lock` at Phase 0 adoption time using whatever version is current stable on PyPI on the day the lockfile is generated. Subsequent bumps go through Dependabot or an explicit operator action; they are not the implementing agent's standing authority once Phase 0 is complete. The template's current `check-jsonschema` pin is preserved verbatim until the upstream template revises it (the `vendor.dependabot` schema lock depends on the exact pin matching the dependabot-schema test fixture). At spec-authoring verification on 2026-06-03, the upstream pin is `check-jsonschema==0.37.2`; if the upstream template advances that pin before Phase 0 adoption, NoEasyMark adopts the upstream value current at adoption time and records the observed template commit and pin in the Phase 0 PR body. The agent's chosen `uv` toolchain version (passed to the `astral-sh/setup-uv` action's `version:` input) is bumped only together with a refreshed `uv.lock`.

A small set of packages are **load-bearing** for cache identity, schema/state portability, signal handling, or render-profile identity. An upstream version bump on any of these is treated as a tool-profile change and triggers the mini-pilot rule, even when Dependabot opens the PR. Each load-bearing package carries an explicit minimum-version floor in the spec; the agent's exact pin at Phase 0 may be that floor or a later release. The load-bearing set is:

| Package | Why it's load-bearing | Minimum version floor | Phase-0 pin selection rule |
| --- | --- | --- | --- |
| `anyascii` | Drives transliteration in output-folder-name derivation; bump can change generated slugs. | `0.3.3` | Current stable on PyPI at Phase 0; record the pinned version in `tool_profile.json`'s build provenance. |
| `rfc8785` | Drives JCS canonicalization for every cache-key component hash. | `0.1.4` | Current stable; doctor exercises the pinned library against the published JCS test vectors before any cache-key computation. |
| `filelock` | Default single-writer lock implementation. | `3.29.0` | Current stable; `lock_backend: filelock` is the spec default. |
| `portalocker` | Alternate lock backend when shared-reader semantics are needed. | `2.10.1` | Current stable; only required when `lock_backend: portalocker`. |
| `pywin32` | Win32 job-object creation for cross-host process supervision; installed only under `sys_platform == "win32"`. | `307` | Current stable; declared in the core dependency set with the platform marker. |
| `check-jsonschema` | Template's vendor-schema lock; pin matches the dependabot-schema test fixture. | Upstream template pin current at Phase 0 adoption (`0.37.2` verified on 2026-06-03). | Preserve the template's pin verbatim and record the observed upstream commit/pin in the Phase 0 PR body. |
| `pymupdf` | Renderer for the default render profile; `pymupdf.__version__` is recorded as `render_profile.renderer_version` and participates in `render_profile_hash_full` and `render_profile_id`. | `1.24.0` | Current stable; bumping creates a new render-profile hash/id pair, which is intentionally a tool-profile change. |
| `pydantic` | Schema authoring; v2 only. | `2.7.0` | Current stable in the v2 line. |
| `litellm` | Backend transport for every shipped `LiteLLMBackend`. | `1.50.0` | Current stable; bumping is captured by `backend_config_hash` via the lockfile hash, not by `model_parameters_hash`. |
| `rapidfuzz` | Drives `overlap_divergence_threshold` and the normalized-fuzzy consensus comparator. | `3.9.0` | Current stable; the specific comparator used (`rapidfuzz.distance.Indel.normalized_similarity`) is pinned in `pipeline.toml` so other comparators can drift without affecting gates. |
| `reportlab` | Generates the synthetic init-smoke fixture `tests/fixtures/smoke/synthetic-source.pdf`; its exact resolved version is what produced the committed bytes. | `4.2.0` | Declared in `[dependency-groups.dev]` with the compatible-release form `reportlab~=X.Y.Z` (e.g., `reportlab~=4.2.0`), where `X.Y.Z` is the current stable at Phase 0 adoption. The committed `uv.lock` pins the exact resolved patch. Dependabot may land same-minor patch bumps automatically when `tests/test_synthetic_fixture_reproducibility.py` still passes after qpdf canonicalization; same-minor patch failures and any minor/major bump require deliberate maintainer action to regenerate the committed PDF in the same PR that lands the version change. |

Other packages (Typer, Rich, Pillow, OCRmyPDF, pymupdf4llm, marker-pdf, docling, latex2mathml, etc.) are not load-bearing for cache identity and follow only the general policy above.

The implementation language is Python `>=3.13,<3.15` initially, matching the template's current support window. The CLI is built with Typer. Schemas are authored with Pydantic and exported to JSON Schema for validation through the template's schema tooling. Code formatting follows the template defaults (Black at line length 100, Ruff at line length 100). The template's root `pyproject.toml` ships Black configuration but does not ship a `[tool.ruff]` block; the Ruff configuration lives only in `templates/python/pyproject.toml` and is currently applied via the pre-commit hook's `--line-length=100` argument. NoEasyMark copies the full `[tool.ruff]` and `[tool.ruff.lint]` configuration from `templates/python/pyproject.toml` into its own root `pyproject.toml` so that running `ruff check` locally and running the pre-commit hook produce the same result. Type checking uses mypy as configured by the template's `python-ci.yml` workflow after that workflow is converted to `uv` and made enforcing for NoEasyMark. The template's `check-jsonschema` pin keeps `vendor.dependabot` schema alignment with the dependabot schema test; NoEasyMark preserves the upstream pin current at adoption time and re-evaluates it whenever the upstream template bumps it. When the upstream template updates its Python support window, update `pyproject.toml`, `uv.lock`, Black targets, mypy version, and the Python CI matrix together.

Markdown deliverables use GitHub Flavored Markdown. Display math is fenced as `$$ … $$` and inline math as `$ … $`. The `markdownlint` and `remark` configurations from the template are the lint authority; NoEasyMark may add tool-specific custom rules but does not weaken the template defaults. Because the upstream template's `remark` setup validates links but does not parse math by default, Phase 0 adds `remark-math` to `package.json` and imports it in `.remarkrc.mjs` before `remark-validate-links`; this makes math-delimiter parsing part of the retained Markdown CI surface.

Create the Typer CLI defined in **CLI Surface** above. The agent invokes the CLI rather than reimplementing pipeline logic inside chat reasoning.

Use dependency profiles instead of installing every heavy tool up front. Runtime feature profiles (`ocr`, `engines`, `engines-restrictive`, `math`, `judge-litellm`, `local-vlm`, and `experimental`) are published optional extras under `[project.optional-dependencies]`. The development toolchain is the local-only `dev` group under top-level `[dependency-groups]`, so it is installed for CI and contributors but is not exposed as a published package extra. The partition rule is: any Python package directly imported by a shipped engine YAML's adapter goes in `engines` when its license is permissive (MIT, Apache-2.0, BSD, MPL) and in `engines-restrictive` when its license is non-permissive (GPL-3.0-or-later, AGPL, or similar copyleft); aspirational engines not yet bound to a shipped YAML go in `experimental`; the rest follow their natural functional category. The license split exists because NoEasyMark itself is MIT and operators distributing builds may need to exclude copyleft extras.

The `core` dependency `pymupdf` is **dual-licensed AGPL-3.0-or-commercial** by Artifex, not MIT-compatible permissive. The NoEasyMark source code remains MIT, but any operator distributing NoEasyMark builds (binary wheel, container image, or hosted service) inherits PyMuPDF's AGPL obligations on the bundled PyMuPDF portion: source availability to recipients for local distribution, and the AGPL §13 network-use clause for hosted service distribution. Operators distributing under a closed-source license must obtain the Artifex commercial license for PyMuPDF separately. NoEasyMark's own MIT license is unaffected by this — `pip install noeasymark` will pull PyMuPDF transitively, and the user assumes the AGPL obligations on the PyMuPDF portion that ordinary Python dependency installation always implies. `noeasymark doctor` enumerates the SPDX license expression of every installed package (core and every active extra) in a "License Footprint" section so distributors can audit the effective license set before publishing builds. There is no permissive drop-in replacement for PyMuPDF at the quality level NoEasyMark needs; moving rendering off PyMuPDF would require redesigning the render-profile model and is out of scope for the initial spec.

- `core` (required): `typer`, `pydantic` (>=2.7), `rich`, `pymupdf` (>=1.24), `Pillow`, `rapidfuzz` (>=3.9), `anyascii` (>=0.3.3), `rfc8785` (>=0.1.4), `filelock` (>=3.29), `pywin32` (>=307; `sys_platform == "win32"` only). These are not optional.
- `ocr`: `ocrmypdf`, `pdfplumber`, Tesseract-facing helpers.
- `engines` (permissive licenses only): `pymupdf4llm`, `docling`, `markitdown[pdf]`.
- `engines-restrictive` (copyleft licenses; isolate so MIT-compatible builds can exclude): `marker-pdf` (GPL-3.0-or-later as of this writing; verify current license at adoption time and re-classify if upstream relicenses).
- `math`: optional equation tools such as `latex2mathml`.
- `judge-litellm`: `litellm` (>=1.50) plus any provider-side optional dependencies the operator's configured LiteLLM backends require.
- `local-vlm`: CUDA-enabled local model support for experiments on a local GPU.
- `experimental`: optional tools such as Nougat, MinerU, Surya, Mistral OCR client, `azure-ai-formrecognizer`/`azure-ai-documentintelligence`, Azure Content Understanding client, or specialized math OCR. Packages move from `experimental` to `engines` (or `engines-restrictive`) when a shipped engine YAML's adapter starts importing them directly.
- `dev`: pre-commit hooks, mypy, ruff, black, pytest, `check-jsonschema` (the upstream template pin current at Phase 0 adoption; `0.37.2` verified on 2026-06-03), `reportlab~=X.Y.Z` (compatible-release form against the Phase 0 stable version; the committed `uv.lock` pins the exact resolved patch and is the audit-authoritative record of which ReportLab version produced the committed synthetic fixture), and the schema-example fixtures harness. This local-only dependency group is declared under top-level `[dependency-groups]`, not under `[project.optional-dependencies]`. Core CI installs it via `uv sync --locked --dev` (or the equivalent explicit form `uv sync --locked --group dev`).

Optional-stack CI jobs install only the narrow extra they validate (`--extra ocr`, `--extra engines`, `--extra engines-restrictive`, `--extra judge-litellm`, etc.); `--all-extras` is intentionally not used by any CI workflow.

#### Install Profiles for Operators

The dependency partition above produces several install profiles. NoEasyMark documents three named profiles so an operator does not have to invent a combination of extras from scratch; `noeasymark doctor` detects which profile is active (via `importlib.metadata.distributions()` introspection of installed packages) and advises the operator to advance to the next profile when an attempted step requires it. The profiles are advisory shortcuts; an operator may always pick an arbitrary subset of extras.

| Profile | Command | Intended use | Capabilities it unlocks |
| --- | --- | --- | --- |
| `ci-development` | `uv sync --locked --dev` | NoEasyMark CI, type/lint/test runs, contributor onboarding | Build, type-check, lint, run pytest including the synthetic-fixture init smoke. Does not unlock real OCR, engine ensemble, or LiteLLM backends. |
| `operator-pilot-ready` | `uv sync --locked --dev --extra ocr` | An operator running the first pilot against a source PDF whose text layer may be poor | All of `ci-development` plus OCR redo and force passes via `ocrmypdf`. Sufficient to run preprocess, single-engine baseline conversion, and CLI-agent judge backends through Codex or Claude Code. |
| `operator-full-pilot` | `uv sync --locked --dev --extra ocr --extra engines --extra judge-litellm` | An operator running a full-diversity pilot against an unfamiliar corpus | All of `operator-pilot-ready` plus the permissive-license engine ensemble (Docling, PyMuPDF4LLM, MarkItDown) and LiteLLM-backed cloud judges. Operators distributing builds under a closed-source license must verify the LiteLLM dependency surface against their license obligations. |

Operators who need GPL-licensed engines (`marker-pdf` as of the spec's authoring) add `--extra engines-restrictive` to the relevant profile and accept the resulting license footprint; `noeasymark doctor`'s "License Footprint" section surfaces the obligation explicitly. Operators with local GPU resources add `--extra local-vlm`. Operators using experimental engines (Nougat, MinerU, Surya, Mistral OCR client, Azure Document Intelligence, Azure Content Understanding) add `--extra experimental`. None of these extra profiles are installed by default.

`noeasymark doctor`'s "Install Profile" section reports the detected profile by name (`ci-development`, `operator-pilot-ready`, `operator-full-pilot`, or `custom` when the active set matches none of the named profiles), enumerates the missing extras to advance to the next profile, and prints the exact `uv sync` command that would advance.

CLI agent backends are external binaries. Dependency profiles do not install them. `noeasymark doctor` reports whether configured CLI agents are present on `PATH` and authenticated.

Optional engine profiles declare `supported_python`, `supported_platforms`, `requires_gpu`, `required_binaries`, and any external-service SDK constraints. The default devcontainer targets Python 3.13 for the core package. If an optional engine is not installable or runnable under the active Python/runtime, `doctor` disables that engine with an actionable report instead of weakening the core Python support window. A tool profile that explicitly requires the disabled engine remains blocked until the runtime is changed, the engine configuration is replaced, or the requirement is waived.

Create schemas for `state_record` (the base mixin used by every persisted state file), `run_config`, `pipeline_config`, `init_bundle`, `schemas_index`, `work_queue`, `work_unit`, `gate_record`, `source_manifest`, `source_manifest_public`, `outline`, `outline_public`, `render_profile`, `chapter_manifest`, `chapter_manifest_public`, `chunk_manifest`, `chunk_manifest_public`, `layout_region`, `crop_record`, `candidate_output`, `candidate_lineage`, `figure_record`, `table_record`, `equation_record`, `evidence_sidecar`, `custom_anchor_registry`, `external_link_allowlist`, `adjudication_record`, `triage_decision`, `audit_decision`, `review_marker`, `review_marker_link`, `manual_review_input`, `manual_review_waiver`, `independent_auditor_waiver`, `mini_pilot_waiver`, `pilot_plan`, `glossary_entry`, `glossary_candidates`, `adjudication_response`, `triage_response`, `audit_response`, `glossary_candidates_response`, `gold_set`, `quality_thresholds`, `prompt_record`, `prompt_role_registry`, `judge_response_schema_profile`, `judge_backend_record`, `engine_record`, `artifact_archive_record`, `content_handling_record`, `approvals_record`, `approvals_record_public`, `tool_profile`, `tool_profile_public`, `budget_ledger`, `progress_ledger`, `artifact_usage_ledger`, `response_cache_entry`, `cache_provenance_archive`, `chapter_report`, `aggregate_quality_report`, `run_status_snapshot`, `endpoint_identifier`, `normalized_failure_event`, `failure_match_rule`, `signal_registry`, and `phase0_freshness_record`.

A subset of these schemas is called out here as load-bearing across the rest of the spec because their fields drive cache identity, gate behavior, or branch automation: `run_config`, `pipeline_config`, `tool_profile`, `adjudication_record`, `evidence_sidecar`, `work_queue`, `chapter_manifest`, `chunk_manifest`, `source_manifest`, and `approvals_record`. The Phase 0 milestone PR for the schema set pauses at the named development-time gate `SCHEMA_DESIGN_REVIEW`, which surfaces the field list, required-vs-optional choices, default values, and downstream cache-key impact for **every** schema named in this Phase 0 schema list, not only the load-bearing subset. The maintainer reviews the load-bearing schemas in detail and is expected to spot-check the rest; surfacing every schema in the gate prevents a schema that turns out to be load-bearing later from advancing unreviewed. The gate-record state file's `schemas_under_review` list contains the full schema set. The gate is recorded as a hybrid PR + state-file artifact:

- The agent opens the milestone PR as ready for review, not as a draft, with the body containing the load-bearing schema summary, the operator checklist for clearing `SCHEMA_DESIGN_REVIEW`, and the exact statement that "Ready for review" is not an approval. The agent applies the `schema-design-review` label so the gate is visible in the PR listing.
- The agent writes `.implementation-state/gates/schema-design-review.json` (a development-time-only file under a new gitignored directory) with fields `gate_id`, `gate_name: "SCHEMA_DESIGN_REVIEW"`, `opened_at`, `pr_url`, `pr_number`, `pr_head_ref`, `pr_head_sha`, `base_ref`, `schemas_under_review` (the full Phase 0 schema set), `load_bearing_schemas` (the subset named above), `status: "pending"`, and `satisfied_by` (nullable until the gate clears; later `approved_review` or `merged_pr`).
- The agent then returns control to the operator so it does not burn agent CLI session time or vendor credits while waiting. Headless runtimes that support process exit status exit with the `SCHEMA_DESIGN_REVIEW` development-time exit code. Interactive runtimes that do not expose a meaningful process exit code stop work after writing the gate file and opening the PR; they may leave the operator at the prompt or exit normally. The gate file plus GitHub PR state, not the shell status alone, are the durable signal. No polling loop runs while the gate is open. The operator may run the usual PR review loop while the gate is open: Copilot, Claude, human reviewers, or the maintainer may comment, request changes, and push follow-up commits to the PR branch. The remote PR head is authoritative during this waiting period.
- Gate-satisfying actions are deliberately narrow. Marking a draft PR "Ready for review" only changes the PR stage and never clears `SCHEMA_DESIGN_REVIEW`. An open milestone PR clears the gate only when the current PR head has (a) a submitted GitHub PR review whose state is `APPROVED` and whose `commit_id` equals the current PR `head.sha`, (b) no later `CHANGES_REQUESTED` review still reflected in GitHub's current review decision, and (c) no required or reported current-head status check in a failing or pending state. A merged milestone PR also clears the gate when GitHub reports the recorded `pr_number` as merged into the expected `base_ref`; this is treated as maintainer acceptance even if the explicit approval review is not separately visible at resumption time.
- On resumption the agent reads `.implementation-state/gates/schema-design-review.json` first. If that file is missing (fresh clone, devcontainer rebuilt without a persisted `.implementation-state/` volume, hosted runner, etc.), the agent runs exactly one GitHub MCP query for open PRs labeled `schema-design-review` and, if necessary, closed merged PRs with the same label from the current implementation cycle (returning number, head ref, head SHA, base ref, review decision, reviews, merge state, and URL). It reconstructs the state file from a single matching PR. If zero PRs match, the agent treats the gate as not yet opened and proceeds with Phase 0's pre-gate work normally; if more than one plausible gate PR matches, the agent stops with a usage-error exit and prints every matching PR number so the operator can close stale duplicates or name the intended PR before re-launch.
- Once the state file is present (either read or reconstructed), if `status` is still `pending` or `changes_requested`, the agent queries the recorded `pr_number` through the GitHub MCP connector for PR metadata, current head ref, current head SHA, base ref, draft state, merge state, reviews, review decision, and current-head status checks. If the PR is open, the agent fetches the recorded remote head, ensures the local working tree is clean, switches to the local phase branch, fast-forwards it to the remote PR head, and verifies that local `HEAD` equals GitHub's current `head.sha`. If the PR head moves between query and fetch, the agent re-queries once and repeats the fast-forward; if it moves again, the agent stops and prints both observed SHAs so the operator can let the review loop settle before re-launching. If the local branch has unpushed commits or cannot fast-forward to the remote PR head, the agent stops without rebasing or resetting and prints the local and remote SHAs; remote review-loop commits are accepted only through a clean fast-forward.
- If the recorded milestone PR is merged before resumption, the agent fetches the expected base branch, fast-forwards the local base branch to the remote base tip when possible, records `satisfied_by: "merged_pr"`, `merged_at`, `merged_by`, and `merge_commit_sha` from GitHub PR metadata when available, re-validates the gate file, and continues from the updated base branch or from a new stacked branch rooted at that base tip. Because GitHub's `merge_commit_sha` can represent a merge commit, squash commit, or rebase result depending on the merge method, the agent treats GitHub's recorded merged state plus the expected base ref as the acceptance signal rather than requiring the original PR head SHA to be an ancestor of the base branch.
- If the milestone PR remains open and the current-head approval/check conditions above are satisfied, the agent updates the gate file with `status: "approved"`, `satisfied_by: "approved_review"`, `approved_at`, `approver_login`, `approval_review_id`, `approved_head_sha` (the current PR head SHA), and the current status-check summary, re-validates the file against the gate-record schema, and continues with the remaining Phase 0 work and the later phases. If the current review decision is `REVIEW_REQUIRED`, `null`, or equivalent no-review state, or if the latest approving review targets an older commit than the current PR head, the agent leaves `status: "pending"` and stops again using the same runtime-specific gate behavior; the operator re-launches once the approval or merge is recorded. The canonical devcontainer variants mount a `noeasymark-implementation-state` named Docker volume at `.implementation-state/` so successive launches in the same container reuse the state file without re-querying GitHub; the label-discovery fallback exists precisely so the gate workflow remains correct when that volume is unavailable.
- The agent never advances any later phase that consumes a load-bearing schema while `status` is `pending` or `changes_requested`. Between exit and resumption, the agent may have already committed independent adoption cleanup or documentation work that cannot be invalidated by schema changes; that work survives across resumptions because the work queue is the source of truth.
- If GitHub's current review decision is `CHANGES_REQUESTED`, the agent updates `status` to `changes_requested`, fetches and fast-forwards to the current remote PR head as above, addresses the requested changes in additional commits to the same PR branch, pushes them, updates `pr_head_sha`, stops again using the same runtime-specific gate behavior, and waits for the next operator-driven review cycle and re-launch. Any prior approval is stale after those commits unless a later gate-satisfying approval review targets the new head SHA or the PR is merged.

`SCHEMA_DESIGN_REVIEW` is the only development-time gate enumerated by this specification; conversion-run gates remain enumerated under **Named Pipeline Signals**. Non-load-bearing schemas advance without a separate gate but follow the same shape and migration rules.

**Load-bearing rule.** A schema is "load-bearing" — and therefore in scope for the `SCHEMA_DESIGN_REVIEW` PR's detailed review packet — when any of the following holds: (a) it is referenced by another internal schema via `$ref`; (b) it is named as a component of `cache_key` in **Cache Key Canonicalization**; (c) it is named in `adjudication_record`'s provenance group; (d) it participates in the `state_record` mixin chain consumed by `noeasymark migrate`; (e) it owns a public sibling (`*_public`) generated by `tools/generate_public_schemas.py`; or (f) it owns a deterministic-prefix identifier from the table in **Identifier Conventions**. Schemas that meet none of these criteria are still authored, exported, fixture-covered, and migration-registered, but the `SCHEMA_DESIGN_REVIEW` packet covers them as a single "ride-along" group rather than reviewing each in detail. `schemas/_index.yaml` carries a `load_bearing` boolean on every entry; `tools/generate_schema_hooks.py` writes the value mechanically from the rule above and `noeasymark dev regenerate-schema-hooks --check` fails CI when the flag drifts. The `SCHEMA_DESIGN_REVIEW` PR body's table-of-schemas section groups load-bearing entries first with full field-set callouts and lists ride-along entries second under a single heading.

**Phase 0 pre-gate / post-gate partition.** While `SCHEMA_DESIGN_REVIEW` is pending the agent may continue any Phase 0 work that does not import a schema module and whose tests do not load a fixture under `schemas/examples/`. The pre-gate list enumerated below is illustrative, not exhaustive; the underlying rule is the authority for items not enumerated.

Pre-gate Phase 0 work (may land in `implementation/phase-00a-template-adoption` or a similarly named pre-schema branch and merge without waiting for `SCHEMA_DESIGN_REVIEW`):

- `.template-sync/marker.yml` authoring with the adopted module set and `local_overrides`.
- Stack pruning per the bounded list above (removing PowerShell and Terraform workflows, tests, instructions, lint configs, `.pre-commit-config.yaml` inline blocks, `data-ci.yml` setup blocks, and `.claude/hooks/session-start.sh` Terraform install block).
- `.github/CODEOWNERS` replacement to `* @franklesniak`.
- `.gitattributes` normalization including the LFS rules block.
- `.gitignore` additions for `.noeasymark-state/`, `.noeasymark-artifacts/`, `.implementation-state/`, and the default deliverable output root `/output/`.
- Agent-instruction file rewrites (`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, `.hermes.md`, `.cursor/rules/repository-instructions.mdc`) and `.github/copilot-instructions.md` source-of-truth updates.
- The new `.github/instructions/pdf-conversion.instructions.md`.
- Pip→`uv` conversion of `pyproject.toml`, the lockfile generation, and `python-ci.yml` rewiring to `astral-sh/setup-uv` plus `uv run` invocation.
- `.devcontainer/cpu/devcontainer.json` and `.devcontainer/cuda/devcontainer.json` authoring including the named credential and `noeasymark-implementation-state` volume mounts.
- `cross-platform-check.yml` workflow authoring (label / dispatch / cron triggers).
- `.github/ISSUE_TEMPLATE/` customization, `pull_request_template.md` customization, `dependabot.yml` audit for github-actions ecosystem coverage, and the inherited `auto-fix-precommit.yml` / `check-placeholders.yml` updates.
- Removal of template onboarding documents (`GETTING_STARTED_NEW_REPO.md`, `GETTING_STARTED_EXISTING_REPO.md`, `OPTIONAL_CONFIGURATIONS.md`, `TEMPLATE_MAINTENANCE.md`, `.github/TEMPLATE_DESIGN_DECISIONS.md`, `COPILOT_CHAT_PROMPTS.md`) once their applicable text has been applied, and rewriting of `TEMPLATE_UPDATE_PROCEDURE.md` for the adopted module set.
- `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, `LICENSE` (year and copyright holder), and `.vscode/settings.json` authoring.
- The salvage-first `templates/` directory removal (copy load-bearing blocks into the root, run pre-commit + schema-example pytest, delete `templates/`).
- The schemas themselves authored as Pydantic models and exported `.schema.json` files plus `schemas/examples/<name>/{valid,invalid}/` fixture trees: these are authored *into the schema PR* (which is the gate-bearing PR) but are not consumed by other code yet, so authoring is pre-gate work whose merge approval *is* `SCHEMA_DESIGN_REVIEW`.

Post-gate Phase 0 work (waits for `SCHEMA_DESIGN_REVIEW` approval before merging):

- Schema-hook block generation in `.pre-commit-config.yaml` and `.github/workflows/data-ci.yml` (`# noeasymark: begin schemas` / `# noeasymark: end schemas` regions emitted by `tools/generate_schema_hooks.py` from `schemas/_index.yaml`).
- Rewiring `tests/test_schema_examples.py` to auto-discover the NoEasyMark schema set.
- `tests/test_public_schema_consistency.py` (verifies committed public sibling schemas match the deterministic transformation of internal schemas).
- `pipeline.toml` materialization (the file is validated against `pipeline_config`, so committing populated values depends on the approved schema).
- `noeasymark` CLI subcommands that import any schema module: `init`, `doctor`, `status`, `gate list|show|approve|request-changes|retry|reopen`, `dev regenerate-schema-hooks`, the work-queue scaffolding, and the deferred-content init smoke harness.
- Initial committed `config/` content that validates against the approved schemas: prompts under `config/prompts/<role>/v0001.md`, backend YAMLs under `config/judge_backends/`, engine YAMLs under `config/engines/`, render-profile YAMLs under `config/render_profiles/`, and artifact-archive YAMLs under `config/artifact_archives/`.
- The `tests/fixtures/smoke/synthetic-source.pdf` fixture and the deferred-content init smoke harness it backs.

The schema field sets below are the normative minimum for the schemas they name, not the exclusive design space for the full Phase 0 schema list. For every schema named in the Phase 0 list that does not receive its own subsection here, the implementing agent must still author a complete Draft 2020-12 schema, fixtures under `schemas/examples/<schema-name>/{valid,invalid}/`, and a concise design summary in the `SCHEMA_DESIGN_REVIEW` PR. Those schemas are within the implementing agent's Phase 0 design authority: the agent must not pause merely because this document does not enumerate every field. It derives the fields from the record's producing/consuming prose, the schema-wide conventions below, the cache-key section when relevant, and the smallest closed shape that preserves auditability and migration safety. When several shapes would satisfy the prose, prefer the shape that is stricter, easier to redact into a public sibling, and less likely to change cache identity accidentally. Any deliberately open extension point must be named `extensions`, must be isolated from first-class fields, and must not participate in cache identity unless the schema summary explicitly says so. Maintainer review at `SCHEMA_DESIGN_REVIEW` is the approval channel for those proposed field sets.

Schema-wide conventions apply to every schema unless explicitly overridden:

- **Strict-mode posture.** Every schema object declares `additionalProperties: false` at its own top level unless this specification explicitly names a first-class extension map or dynamic-key map for that schema. Named extension maps, such as `pipeline_config.extensions`, `run_config.extensions`, and `prompt_record.extensions`, may allow additional string keys only inside that map and only for the purpose stated by the field. Other map-shaped first-class fields with dynamic keys, such as `tool_profile.role_extensions`, declare their value schema or pattern and are not generic extension bags. Operator-defined keys must not collide with first-class fields.
- **Enums vs open strings.** Every enumerated field uses a closed JSON Schema enum. Open strings are permitted only when the schema labels the field `free_text` explicitly. The only shipped exception is the `category` key on review markers, which is operator-defined free text.
- **Identifier format.** Every project-prefixed deterministic id (`cs-`, `cr-`, `rg-`, `crf-`, `xev-`, etc.) is shaped as `<prefix><14-char lowercase hex of SHA-256 of the canonical-JSON of the parent context plus the element's position within its container>`, where `<prefix>` is one of the table prefixes in **Identifier Conventions** and includes the trailing hyphen, unless that section lists a producer-specific hash preimage for the field. Run-scoped identifiers (`run_id`, `chapter_id`, `chunk_id`, `unit_id`) follow the rules in **Identifier Conventions** rather than this prefix rule.
- **Timestamps.** All timestamp fields use UTC ISO 8601 with millisecond precision and a `Z` suffix (pattern `^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}\.\d{3}Z$`). A shared JSON Schema definition `nem_iso8601_ms_utc` is referenced by every timestamp field so the format constraint is centralized.
- **Hash field formats.** All `*_hash` fields are 64-character lowercase hexadecimal SHA-256 digests unless the field name explicitly carries the `_short` suffix (12-character truncation) or `_compact` suffix (16-character truncation). Truncated hashes always retain a sibling field carrying the full digest for audit (for example, `tool_profile_hash_compact` is paired with `tool_profile_hash_full`).
- **File-path fields.** Path fields in deliverable records (under `<output_root>/<output-folder-name>/`) are relative to the deliverable root. Path fields in per-run internal state (under `.noeasymark-state/<run_id>/`) are relative to the run's state directory unless the field is explicitly a private operator input path such as `source_pdf_input`. Path fields in folder-scoped internal state (under `.noeasymark-state/_folders/<output-folder-name>/`) are relative to that folder-state directory. Path fields in artifact records (under `.noeasymark-artifacts/<run_id>/`) are relative to the run's artifact directory. Each path field's documentation block names its base. Absolute private input paths never appear in public sibling schemas.
- **Cross-record references.** A reference to another record uses the raw string id (e.g., `chapter_id: "ch01"`) when the referenced record type is unambiguous from the field name. When the same field can refer to records of different schemas, the structured envelope `{ "schema_id": "...", "record_id": "..." }` is used.
- **`state_record` mixin.** Every persisted run-state file inherits the `state_record` mixin (`schema_id`, `schema_version`, `created_by`, `created_at`, `migration_history`, optional `input_hashes` or `depends_on`). Ledger files (`budget_ledger`, `progress_ledger`, `artifact_usage_ledger`) inherit a slimmer `state_record_append_only` variant that omits `migration_history` because ledgers append rather than migrate; their schema bumps require generating a new ledger file with a fresh start rather than mutating prior entries.

### `run_config` schema field set

Required: `schema_id`, `schema_version`, `created_by`, `created_at`, `run_id`, `output_folder_name`, `source_pdf_input` (structured private record with `supplied_path`, `resolved_path_private`, `path_kind` enum `repo_relative`, `absolute_in_repo`, `absolute_external`, or `relative_external`, `resolved_under_repo` boolean, and a redaction policy), `source_pdf_working_path` (normally `original/source.pdf`, relative to `.noeasymark-artifacts/<run_id>/`), `source_pdf_sha256` (full 64-char hex), `document_title`, `local_processing_acknowledgment` (boolean true), `time_source_id` (string, default `system_clock_utc`; identifies the wall-clock and monotonic time authority for every timestamp written by this run, so cross-host traces remain comparable), `content_handling` (object containing the three persisted approval records `perimeter`, `internet`, and `external_artifact_archive`; each record has `granted` boolean, `decision_at`, `decided_by`, `decided_by_role`, `rationale`, plus `mode: all|allowlist` when `granted: true`, `allowlist` when `mode: allowlist`, and `resolved_component_snapshot` when `mode: all` for newly written records; the `external_artifact_archive` record additionally carries `folder_approval_reference` pointing into the corresponding `approvals_record` entry so doctor can refuse a run-scope `granted: true` whose folder-scope approval is missing or declined), `restricted_diversity_acknowledgment` (boolean), `unknown_cost_acknowledgment` (boolean), `output_root` (string; default `./output/`; the directory under which the deliverable folder is written), `lfs_max_file_bytes` (integer; default 2147483648, i.e. 2 GiB; the original PDF is page-split above this size), `response_cache_scope` (`folder` or `run`), `diagram_text_policy_default` (`crop_only` or `transcribe`; default `crop_only`). The runtime carries no `repository_storage_approval_reference`, `integration_branch`, `chapter_branch_prefix`, `remote_issue_comment_source_content_approval`, or `absolute_path_disclosure_approval` fields.

Optional: `topic_description` (free_text, max 280 chars), `output_folder_name_override`, `pilot_pages` (array of `source_page_index`), `require_pilot_plan_review` (boolean, default `false`; when true, `noeasymark pilot` behaves as though `--plan-only` was supplied until the `PILOT_PLAN_REVIEW` gate is approved), `seed_glossary_terms` (array of free_text), `spend_ceiling_usd` (positive number), `invocation_ceiling` (positive integer), `runtime_ceiling_seconds` (positive integer), `wall_clock_ceiling_seconds` (positive integer), `artifact_bytes_ceiling` (positive integer), `rendered_page_bytes_ceiling` (positive integer), `minimum_free_space_bytes` (positive integer), `pre_pilot_backend_preferences` (object mapping role → backend_id), `render_profile_alias_override` (a named alias under `config/render_profiles/`; default is `default-300dpi`), `ocr_policy_override` (same enum as `ocr_policy_default`; set by `--ocr-policy` and used only for this run), `ocr_languages` (ordered non-empty array of Tesseract language identifiers; if omitted, the run uses `pipeline.toml`'s `ocr_languages_default`), `extensions` (free-form additional keys, validated only that keys are strings).

`pre_pilot_backend_preferences` is a **soft hint**, not an enablement action. Recording a preference for a role does not flip a backend's `enabled` field, does not register an allowlist entry, and does not bypass `noeasymark doctor --backends` smoke validation. `noeasymark init` accepts preferences for backends that are currently disabled in `config/judge_backends/<name>.yaml`, but it prints an advisory row for each disabled or non-bindable preference and includes the same advisory under `pre_pilot_backend_preference_warnings` when `--json` is present. `noeasymark doctor --backends` surfaces each preference, names the targeted backend, and reports whether the backend is currently enabled and tool-profile-bindable. At pilot time, the pilot binds a preference only after the targeted backend is explicitly enabled (operator action on the YAML or maintainer-approved tool-profile change) and the doctor smoke call succeeds; if a preference still references a disabled backend, the pilot proceeds with the next preference candidate or the operator's interactive selection at `PILOT_GLOSSARY_REVIEW` and records the unmet preference in the pilot report.

### `init_bundle` schema field set

`init_bundle` is an operator-input schema, not a persisted run-state schema. It is accepted only by `noeasymark init --import <path>` and is expanded by `init` into `run_config`, `approvals_record`, and folder-state records after normal validation and prompts. Required fields: `schema_id` (`init_bundle`), `schema_version`, `source_pdf_path` (string exactly as supplied by the operator), `source_pdf_path_kind` (`repo_relative`, `absolute_in_repo`, `absolute_external`, or `relative_external`), `document_title`, and `local_processing_acknowledgment` (must be `true`). A bundle that omits `local_processing_acknowledgment`, sets it to `false`, or supplies any non-boolean value fails schema validation before prompts and exits with usage/configuration exit code 2; `init` never prompts to override a false local-processing acknowledgment imported from a bundle. Optional fields match the operator-facing inputs and CLI flags rather than internal resolved fields: `topic_description`, `output_folder_name`, `pilot_pages`, `require_pilot_plan_review`, `seed_glossary_terms`, `spend_ceiling_usd`, `invocation_ceiling`, `runtime_ceiling_seconds`, `wall_clock_ceiling_seconds`, `artifact_bytes_ceiling`, `rendered_page_bytes_ceiling`, `minimum_free_space_bytes`, `diagram_text_policy_default`, `perimeter_processing_approval`, `internet_processing_approval`, `external_artifact_archive_identity`, `external_artifact_archive`, `restricted_diversity_acknowledgment`, `unknown_cost_acknowledgment`, `output_root`, `lfs_max_file_bytes`, `pre_pilot_backend_preferences`, `render_profile_alias_override`, `ocr_policy_override`, and `ocr_languages`. Approval fields use the same discriminated-union shape as interactive init: `{decision: approve|decline|prompt}` for binary approvals, and `{decision: approve, mode: all|allowlist, allowlist: [...], decided_by, decided_by_role, rationale}` or `{decision: decline, decided_by, decided_by_role, rationale}` or `{decision: prompt}` for network-class and archive approvals. The external artifact archive is supplied as two parallel fields mirroring its two CLI flags: `external_artifact_archive_identity` carries the folder-scope identity/retention decision (`{decision: approve|decline|prompt}` with `archive_id`, `retention_expectation`, and audit fields, and no `mode`/`allowlist`), while `external_artifact_archive` carries the per-run network-class decision using the same `mode`/`allowlist` shape as the other network classes; `init` expands them into the folder-scope `approvals_record` and the per-run `content_handling.external_artifact_archive` record respectively, per the expansion rules above. The bundle may also supply a top-level `approval_audit_default` object (`decided_by`, `decided_by_role`, `rationale`) that fills those audit fields for concrete approval decisions that omit them. The bundle must not include resolved absolute paths, source hashes, run ids, branch names, ledgers, cache entries, tool-profile bindings, or any field under `.noeasymark-state/`; `init` derives those values so importing a bundle cannot clone stale state. A `{decision: prompt}` value causes `init` to fall through to the interactive prompt for that approval as if the field had been omitted; `{decision: decline}` records the declined state without prompting.

The init bundle optional fields include `render_profile_alias_override` and `ocr_languages`; OCR language values use the same ordered-array form as `run_config.ocr_languages` and are validated during `preprocess` after Tesseract language data is discoverable. A minimal JSON bundle illustrates the discriminated-union approval shape; the TOML form follows identical key structure with the same nested objects:

```json
{
  "schema_id": "init_bundle",
  "schema_version": "v0001",
  "source_pdf_path": "corpus/comed-blue-book-2025/source.pdf",
  "source_pdf_path_kind": "repo_relative",
  "document_title": "ComEd Blue Book 2025",
  "local_processing_acknowledgment": true,
  "topic_description": "ComEd electrical distribution standards; service entrance, metering, and grounding requirements.",
  "output_folder_name": "comed-blue-book-2025",
  "approval_audit_default": {
    "decided_by": "franklesniak",
    "decided_by_role": "maintainer",
    "rationale": "Initial NoEasyMark run configuration approved by the repository maintainer."
  },
  "perimeter_processing_approval": { "decision": "decline" },
  "internet_processing_approval": {
    "decision": "approve",
    "mode": "allowlist",
    "allowlist": [
      { "kind": "litellm_provider", "identifier": "anthropic" }
    ]
  },
  "external_artifact_archive_identity": { "decision": "decline" },
  "external_artifact_archive": { "decision": "decline" },
  "restricted_diversity_acknowledgment": { "decision": "decline" },
  "unknown_cost_acknowledgment": { "decision": "decline" }
}
```

`schemas/examples/init_bundle/valid/` and `schemas/examples/init_bundle/invalid/` are the authoritative fixture set for the full bundle surface; the example above is illustrative only.

### `pipeline_config` schema field set

Required: `schema_id`, `schema_version`, `created_by`, `created_at`, plus every constant listed in **Default Constants and Thresholds** with its declared owner `pipeline.toml`. Each constant carries the type and default from the constants table.

Optional: `extensions` (free-form additional keys for operator-defined repository-scope tunables).

### `tool_profile` schema field set

`.noeasymark-state/<run_id>/tool_profile.json` does not exist until `noeasymark pilot` constructs it. Subcommands that need a tool profile but find none report "no active tool profile for run `<run_id>` — run `noeasymark pilot --plan-only` to begin" and exit with a usage-error exit code; they do not synthesize a placeholder profile. The deferred-content init smoke succeeds without a tool profile because `init` does not construct one. The work queue's first runnable unit after `init` is `preprocess`, which also runs without a tool profile; the tool profile is first required at `convert` time. The schema below describes the *populated* record produced by pilot; before pilot, the absence of the file is the canonical "no profile" state.

Required: state_record mixin, `tool_profile_id`, `pilot_run_id` (the run that produced this tool profile), `pilot_plan_id`, `render_profile_alias`, `time_source_snapshot` (the approved `run_config.time_source_id`, clock implementation id, and verification evidence used for provenance timing), `quality_thresholds` (full thresholds map per **Starter quality thresholds**), `approved_metric_specs` (the comparator, transform, numerator/denominator, and unavailable/not-applicable rules approved at `PILOT_GLOSSARY_REVIEW`), `gold_set_status` (`pending`/`approved`/`waived`/`superseded`), `glossary_match_profile`, `roles` (object mapping each enabled role to a `role_binding` record), `active_engines` (array of `engine_id`s plus per-engine activation rationale), `effective_parameters_snapshot` (frozen merged parameter bag per role at the time the profile was approved; used to detect drift), `prompts_in_use` (array of `prompt_id`s with versions), `tool_profile_hash_compact`, `tool_profile_hash_full`.

Each `role_binding` carries: `backend_id`, `model_id`, `model_snapshot_or_version`, `effort_parameter_name` (the provider-native parameter the operator intends as the effort control), `default_parameters_overrides` (merged into the backend's `default_parameters` to produce the effective bag), `transport_only_parameter_additions` (an additive list of keys to merge into the backend's `transport_only_parameters` set; may not shorten the backend's list), `transport_only_parameter_addition_proofs` (closed proof object whose key set exactly matches `transport_only_parameter_additions`), `required_modalities` (array of `text` and optionally `image`; the modalities the role's prompts will actually emit at call time, computed at pilot time from the chosen prompt template and the evidence shape of the chunks the role will see), `failure_domain` (free_text describing the role's independence profile), `network_class` (snapshot at pilot time), `independence_rationale` (free_text), `optional_waiver_id` (when waiver applies). `noeasymark doctor --backends` refuses a binding whose `required_modalities` is not a subset of the bound backend's `input_modalities` or whose transport-only addition proofs are missing, extra, stale, or inconsistent with the backend's output-neutrality evidence.

`effective_parameters_snapshot` drift detection is a canonical per-role comparison. For each role, the runtime recomputes the merged parameter bag from backend YAML defaults, role overrides, and per-call defaults, removes no keys, canonicalizes it with the same JCS rules used for cache components, and compares the full SHA-256 digest with `effective_parameters_snapshot.<role>.parameters_hash`. If the hash differs, `doctor --backends` prints a per-key diff that classifies changes as added, removed, changed, or transport-only-list changed. Output-affecting drift blocks scale-up until the operator approves a new tool profile or runs the mini-pilot path. Transport-only-only drift is allowed to proceed only when the backend YAML and provenance both record why the key is transport-only.

Optional: `independent_auditor_waiver_id`, `mini_pilot_waiver_id`, `role_extensions`, `revisit_at`. `role_extensions` is an object keyed by extension role name; extension role names must match `ext_[a-z0-9][a-z0-9_]{2,63}`, must not collide with shipped role names, and must declare `description`, `allowed_prompt_directory`, `allowed_decision_sources`, `enabled_by_gate_id`, and `pilot_rationale`. Extension roles are inactive unless named here and approved through the pilot gate.

### `schemas_index` schema field set

Required: `schema_id`, `schema_version`, `schemas` (array). Each entry carries `schema_id`, `schema_path`, `valid_fixtures_path`, `invalid_fixtures_path`, `load_bearing` (boolean; computed mechanically from the load-bearing rule in **Phase 0** by `tools/generate_schema_hooks.py`), and optional `public_schema_id`. Paths are repository-relative and must point under `schemas/`. `schemas/_index.yaml` validates against `schemas/schemas_index.schema.json` before `tools/generate_schema_hooks.py` emits schema hook blocks. `noeasymark dev regenerate-schema-hooks --check` exits non-zero when any committed `load_bearing` value diverges from the recomputed value.

### `prompt_record` schema field set

Required: state_record mixin (`schema_id`, `schema_version`, `created_by`, `created_at`), `prompt_id`, `prompt_version` (zero-padded `v0001`-style integer matching the `<version>` segment of `prompt_id`), `role`, `description` (free_text), `compatible_role` (defaults to the value of `role`), `intended_model_families` (array of free_text identifiers, e.g., `gpt`, `claude`, `gemini`, `mistral`, `llama`), `intended_cli_agents` (array of free_text identifiers, e.g., `codex`, `claude_code`), `schema_output_expectations` (closed enum: `native_structured`, `prompt_enforced_and_validated`, or `autodetect`), `response_schema_id`, `response_schema_version`, and `change_notes` (free_text). Optional: `deprecated_at`, `superseded_by` (a sibling `prompt_id`), and `extensions` (free-form additional keys for prompt-specific metadata that does not participate in cache identity).

`role` and `compatible_role` use a controlled extension surface rather than a free-form string. Valid values are the shipped prompt roles (`default_judge`, `escalation_judge`, `fallback_judge`, `triage_model`, `independent_auditor`, `alternate_auditor`, `glossary_build`, and `manual_review_reingest`) plus extension role names that match the `ext_[a-z0-9][a-z0-9_]{2,63}` pattern and appear in the active `tool_profile.role_extensions` registry. A prompt file for any shipped role must live under `config/prompts/<role>/`, and a prompt file for an extension role must live under `config/prompts/<extension-role>/`; in both cases `prompt_record.role` must match that directory. The repository schema accepts the shipped enum plus the extension-name pattern, but pilot/runtime validation rejects any extension role not registered in the active tool profile. `glossary_build` and `manual_review_reingest` are shipped pipeline prompt roles, not tool-profile judge bindings. This keeps the schema closed for shipped behavior while allowing pilot-authorized additions without a schema migration.

`prompt_role_registry` is committed at `config/prompts/prompt-role-registry.yaml`. Required fields: state_record mixin, `roles` (array), `response_schema_mappings`, `generated_at`, and `generation_metadata`. Each role entry carries `role`, `prompt_directory`, `shipped` (boolean), `tool_profile_bindable` (boolean), `compatible_roles`, `allowed_decision_sources`, `response_schema_id`, `response_schema_version`, `minimum_prompt_version`, and `expected_prompt_files`. `noeasymark dev validate-prompt-role-registry --check` fails when a `config/prompts/<role>/` directory is missing from the registry, a registry role is absent from the `prompt_record.role` enum/extension pattern, a shipped prompt file's frontmatter role or response schema disagrees with the registry, a response-schema mapping is missing, or a role is marked tool-profile-bindable when it is a pipeline-only role such as `glossary_build` or `manual_review_reingest`.

`prompt_id` intentionally uses the human-readable `<role>/<version>` form (e.g., `default_judge/v0003`) rather than the project-prefixed deterministic id scheme used by per-record schemas in the **Identifier Conventions** table. The role-and-version form is human-meaningful at audit time, matches the on-disk path `config/prompts/<role>/<version>.md`, and cannot collide because the role directory is the unique key. The deterministic-prefix scheme is therefore not applied to prompts. Every `adjudication_record`, `triage_decision`, `audit_decision`, and cache `provenance` bundle that names a prompt cites the `prompt_id` and `prompt_version` literally; the underlying prompt-file body is hashed into `rendered_prompt_hash` (a cache-key component) so a prompt body edit that does not also bump `prompt_version` is detected through cache invalidation rather than identifier change.

### `adjudication_record` schema field set

Grouped for readability; all groups are required unless noted.

Decision group: `adjudication_id`, `decided_at`, `role` (enum), `decision_source` (`consensus_byte_exact`, `consensus_with_normalization`, `default_judge`, `escalation_judge`, `fallback_judge`, `triage_model`, `independent_auditor`, `manual_review`), `chosen_text`, `confidence`, `confidence_method`, `review_flag` (enum: `accepted`, `accepted_with_flag`, `disputed`, `manual_review`), `reason` (free_text).

Evidence group: `chunk_id`, `region_refs` (array of `region_id`), `candidate_spans` (array of `span_id`), `source_page_indices`, `lineage_summary` (array of `primary_text_source` values present across candidates).

Backend identity group: `backend_id`, `backend_config_hash`, `model_id`, `model_snapshot_or_version`, `model_parameters_hash`, `model_parameters_excluded_keys` (the effective `transport_only_parameters` set actually applied to this call, computed as the union of the backend YAML list and the tool-profile role's `transport_only_parameter_additions`), `model_parameters_exclusion_proof_refs` (the proof ids or evidence refs that justified each excluded key at call time), `response_schema_id`, `response_schema_version`, `backend_emitted_schema_hash`.

Provenance group: `prompt_id`, `prompt_version`, `rendered_prompt_hash`, `evidence_content_hash`, `topic_description_hash`, `glossary_context_hash`, `render_profile_id`, `render_profile_hash_full`, `glossary_id`, `glossary_version`, `tool_profile_hash_compact`, `tool_profile_hash_full`, `tool_profile_snapshot_path` (relative to internal state), `router_state` (nullable; populated when LiteLLM Router was active), `cache_key`, `cache_hit` (boolean), `invocation_started_at`, `invocation_completed_at`, `cost_tracking_mode`, `estimated_cost_usd` (nullable), `estimator_id` (nullable).

Audit group: `reviewer_identity` (nullable; populated only for `manual_review` source), `reviewer_confidence` (nullable), `reviewer_text_diverges_from_source` (nullable boolean), `divergence_reason` (nullable), `fallback_used` (boolean), `escalation_reason` (nullable).

### `triage_decision` schema field set

Required: state_record mixin, `triage_id`, `chunk_id`, `span_refs` (array of `span_id` values), `routing_class` (`accept_with_consensus`, `route_to_default_judge`, `route_to_escalation_judge`, or `route_to_manual_review`), `confidence`, `confidence_method`, `reason`, `evidence_content_hash`, `prompt_id`, `prompt_version`, and `tool_profile_hash_full`.

When `triage_model` is enabled, `tool_profile.json` must also include `triage_routing_policy`. The policy declares which routing classes are allowed and how optional targets resolve when a reserved role is unbound. `route_to_escalation_judge` is allowed only when `escalation_judge` is bound, unless the policy explicitly maps it to `route_to_manual_review`. It must not silently demote to `route_to_default_judge`, because the triage prompt uses that class for high-impact spans. `noeasymark doctor --backends` rejects a tool profile that binds `triage_model` without a complete `triage_routing_policy` covering every class the prompt can emit.

### `audit_decision` schema field set

Required: state_record mixin, `audit_id`, `adjudication_id`, `chunk_id`, `auditor_role` (`independent_auditor` or `alternate_auditor`), `backend_id`, `model_id`, `decision` (`agree`, `dissent`, or `needs_manual_review`), `confidence`, `confidence_method`, `reason`, `alternative_span_id` (nullable), `evidence_refs`, `prompt_id`, `prompt_version`, `cache_key`, and `tool_profile_hash_full`.

### `review_marker` and `review_marker_link` schema field sets

`review_marker` required fields: state_record mixin, `review_marker_id`, `chapter_id`, `chunk_id`, `marker_source` (`consensus_with_normalization`, `judge_low_confidence`, `judge_flagged`, `manual_review`, `qa_check`, or `human_authored`), `status` (`open`, `resolved`, `waived`, or `superseded`), `severity` (`low`, `normal`, or `high`), `category` (free_text), `reason` (free_text), `source_page_indices`, `primary_page_index`, `region_refs`, `span_refs`, `adjudication_refs`, `block_refs`, `markdown_file_path`, `marker_open_offset` (nullable until Markdown assembly writes the marker), `marker_close_offset` (nullable until Markdown assembly writes the marker), `created_from` (enum naming the producing phase or manual-review path), and `audit_metadata`.

`review_marker_link` required fields: state_record mixin, `review_marker_id`, `target_kind` (`evidence_block`, `candidate_span`, `adjudication_record`, `figure_record`, `table_record`, `equation_record`, `manual_review_input`, `manual_review_waiver`, or `chapter_report`), `target_id`, `relationship` (`wraps`, `flags`, `resolved_by`, `waived_by`, `supersedes`, or `references`), `created_at`, and `audit_metadata`. Links are append-only audit records; resolving or waiving a marker writes a new link and updates the marker `status` rather than deleting the original link.

### `manual_review_input` and `manual_review_waiver` schema field sets

`manual_review_input` is serialized as YAML frontmatter in `reviews/manual/<chunk_id>.md`; the resolved Markdown body is not duplicated in frontmatter. Required frontmatter fields: state_record mixin, `manual_review_id`, `chapter_id`, `chunk_id`, `review_scope` (`entire_chunk`, `span_set`, or `block_set`), `review_item_ids`, `review_marker_ids`, `adjudication_ids`, `source_page_range`, `affected_block_ids`, `disputed_span_ids`, `candidate_markdown_paths`, `page_crop_paths`, `layout_region_crop_paths`, `evidence_sidecar_paths`, `requested_decision` (`supply_resolved_markdown`, `choose_candidate`, `confirm_missing_caption`, `confirm_blank`, `waive_or_explain`, or `other`), `severity` (`low`, `normal`, or `high`), `status` (`pending`, `submitted`, `accepted`, `invalid`, `superseded`, or `waived`), `created_at`, `submitted_at` (nullable), `accepted_at` (nullable), `reviewer_identity` (nullable), `reviewer_role` (nullable), `reviewer_confidence` (nullable), `body_scope` (`entire_chunk_markdown`, `replacement_for_listed_spans`, or `replacement_for_listed_blocks`), `submission_format` (`resolved_markdown_body` or `structured_patch_set`), `structured_patches` (array; empty unless `submission_format: structured_patch_set`), `body_sha256` (nullable), `accepted_body_sha256` (nullable), `body_byte_length`, `body_markdown_profile`, `validation_errors`, `supersedes_manual_review_id` (nullable), and `waiver_id` (nullable). The schema permits empty bodies only while `status: pending`; `submitted` requires non-empty body bytes, `submitted_at`, `reviewer_identity`, `reviewer_confidence`, and a matching `body_sha256`; `accepted` additionally requires `accepted_at` and `accepted_body_sha256` matching the body snapshot that was actually ingested.

Manual-review Markdown files are UTF-8 with LF line endings and exactly one YAML 1.2 frontmatter document between the opening and closing `---` fences. The frontmatter loader used by `noeasymark` rejects duplicate keys, anchors, aliases, merge keys, explicit tags, multiple YAML documents, and non-printable control characters. `body_sha256` is computed over the normalized body bytes after the closing frontmatter fence, with LF line endings and one final LF. All paths in a `manual_review_input` are relative paths from an allowed field-specific root; absolute paths, URLs, path traversal, and symlink escapes are invalid. Most fields resolve against the deliverable root or the run-state root (`.noeasymark-state/<run_id>/`); `page_crop_paths` and `layout_region_crop_paths` additionally permit the artifact root (`.noeasymark-artifacts/<run_id>/`) because page-image and layout-region crops are stored there per the **Use these explicit storage roots** contract, and copying those bulky crops into the deliverable or state root is neither required nor desired.

When `submission_format: structured_patch_set`, each `structured_patches[]` entry carries `patch_id`, `target_kind` (`span` or `block`), `target_id`, `expected_old_sha256`, `replacement_section_anchor`, `replacement_sha256`, and `rationale`. The body contains one deterministic section per patch headed `## Patch <patch_id>`; the normalized Markdown under that heading, up to the next patch heading or end of body, is the replacement text whose hash must match `replacement_sha256`. Re-ingest applies a patch only when the target still belongs to the declared review scope and the current source-derived target hash still equals `expected_old_sha256`; otherwise the review remains blocked and records a validation error rather than applying a stale patch.

`manual_review_waiver` records are written as schema-valid JSON files under `reviews/waivers/<waiver_id>.json`. Required fields: state_record mixin, `waiver_id`, `waived_item_ids`, `review_marker_ids`, `manual_review_ids`, `severity`, `affected_pages`, `affected_blocks`, `affected_spans`, `waiver_scope` (`item`, `chunk`, `chapter`, or `run`), `rationale`, `approver`, `approver_role`, `approved_at`, `final_publication_allowed`, `follow_up_owner` (nullable), `follow_up_due_at` (nullable), `expires_at` (nullable), `revisit_at` (nullable), `supersedes_waiver_id` (nullable), and `status` (`active`, `superseded`, or `revoked`). Waivers update review markers through `review_marker_link` records and never delete the original marker or review input.

### `glossary_candidates` schema field set

Required: state_record mixin, `glossary_id`, `glossary_version`, `source_scope` (`pilot`, `chapter`, or `run`), `topic_description_hash`, and `entries` (array of `glossary_entry` records whose `status` is `candidate`, `rejected`, or `deferred`). The approved glossary uses the same envelope shape but permits only `approved` entries and is validated as a `glossary_entry` collection by `glossary_entry` plus the glossary-file invariant checks.

### `evidence_sidecar` schema field set

One file per committed Markdown file. Chapter Markdown files use `<output_root>/<output-folder-name>/evidence/<chapter_id_digits>-<chapter-or-section>.evidence.json`; the reserved front-matter file uses `<output_root>/<output-folder-name>/evidence/00-front-matter.evidence.json`; the generated index pages use `<output_root>/<output-folder-name>/evidence/figures.evidence.json`, `<output_root>/<output-folder-name>/evidence/tables.evidence.json`, and `<output_root>/<output-folder-name>/evidence/equations.evidence.json`, each emitted whenever its corresponding `markdown/` index page is emitted. Required: state_record mixin, `chapter_id` (nullable for `markdown/00-front-matter.md` and for the generated index pages `markdown/figures.md`, `markdown/tables.md`, and `markdown/equations.md`; all chapter Markdown evidence sidecars carry a non-null `ch<NN>` value), `markdown_file_path` (relative to the deliverable root), `block_entries` (array). Index-page sidecars describe their entries with the `index_link` `block_kind` and the `generated` `acceptance_status`, and their `block_entry.region_refs`/`candidate_refs` point back to the `figure_record`, `table_record`, or `equation_record` evidence the index row was generated from. Each `block_entry` carries: `block_id`, `block_kind` (`paragraph`, `heading`, `list`, `table`, `figure_ref`, `equation`, `caption`, `code`, `front_matter`, `index_link`), `markdown_offset_start`, `markdown_offset_end`, `source_page_indices`, `region_refs`, `candidate_refs` (array of `span_id`), `normalizations_applied` (array of enum values: `whitespace`, `line_wrap`, `soft_hyphen_join`, `unicode_nfc`, `list_marker_normalize`, `page_furniture_strip`, `markdown_escape`), `normalization_events` (array of records naming the normalization type, source span refs, source offsets when available, output byte range, original text or original text hash, emitted text or emitted text hash, and rationale), `adjudication_refs` (array of `adjudication_id`), `review_marker_ids` (array of `review_marker_id`), `acceptance_status` (enum: `consensus`, `consensus_with_normalization`, `judge_accepted`, `judge_flagged`, `manual_review_resolved`, `restricted_diversity_accepted`, `generated`), `confidence`, `confidence_method`, `extra_notes` (nullable free_text).

### `custom_anchor_registry` and `external_link_allowlist` schema field sets

`custom_anchor_registry` is emitted as `manifests/custom-anchors.json` only when explicit custom anchors are used. Required fields: state_record mixin, `registry_id`, `markdown_files` (array), `anchor_entries` (array), `collision_results`, `emitted_syntax_profile`, and `generation_metadata`. Each `anchor_entry` carries `anchor_id`, `markdown_file_path`, `heading_block_id`, `heading_text_hash`, `source_block_refs`, `source_page_indices`, `generated_heading_anchor`, `custom_anchor`, `emitted_syntax` (`generated_inert_html_anchor`), `raw_html_policy_exception` (`custom_anchor_registry_only`), and `collision_status` (`unique`, `duplicate_custom_anchor`, `collides_with_generated_anchor`, or `unused`). The only custom-anchor HTML the Markdown assembler may emit is `<a id="..."></a>` immediately before the registered heading, with no `href`, text, style, event attributes, or additional attributes. Any raw HTML anchor not present in this registry is unsafe raw HTML and fails assembly. Finalization validates that custom anchors are unique within the deliverable, do not collide with GitHub-compatible generated heading anchors or review-marker ids, point to existing heading block ids, and are referenced only by links whose fragments match the registry exactly.

`external_link_allowlist` is emitted as `manifests/external-links.json` only when final deliverables contain external URLs. Required fields: state_record mixin, `allowlist_id`, `approval_refs`, `entries`, `observed_links`, `validation_results`, and `generation_metadata`. Each `entries[]` record carries `link_purpose` (`source_reference`, `license_reference`, `publisher_reference`, `operator_added_reference`, or `other`), `scheme` (`https` unless an approved artifact policy extends the enum), `target_domain`, optional `path_prefix`, optional `exact_url_sha256`, `source` (`source_pdf`, `reference_material`, `operator_input`, or `generated_policy`), `approval_id`, and `content_handling_ref`. Each `observed_links[]` record carries the Markdown file path, link text hash, target URL hash, resolved domain, source block id, source page ids when source-derived, matched allowlist entry id, and validation result. Link validation never follows external URLs during finalization; it parses and inventories them locally. An external URL that does not match the allowlist remains visible text only when an approved artifact policy explicitly chooses that degradation; otherwise it blocks finalization.

### `work_queue` schema field set

Stored as one canonical file `.noeasymark-state/<run_id>/work_queue.json` plus an append-only event log `.noeasymark-state/<run_id>/work_queue_events.jsonl`. The canonical file is rewritten using the portable atomic-write protocol described in **Phase 1** (write to a sibling temp file in the same directory, flush, then `os.replace`) on each unit state change; the event log accumulates every state transition for audit and crash recovery.

`work_queue` required fields: state_record mixin, `units` (array of `work_unit`). Each `work_unit` carries: `unit_id`, `unit_type` (enum from **Identifier Conventions**), `scope_id`, `chapter_id` (nullable), `chunk_id` (nullable), `dependencies` (array of `unit_id`), `input_hashes` (array of content hashes the unit consumed), `output_paths` (array of paths produced), `status` (enum: `pending`, `runnable`, `running`, `succeeded`, `failed`, `blocked_gate`, `stale`, `migration_required`, `waived`, or `superseded`), `attempt_count`, `last_attempt_at` (nullable), `last_failure_signature` (nullable), `last_failure_event_id` (nullable, pointer into `normalized_failure_event`), `blocking_gate` (nullable enum from **Named Pipeline Signals**), `stale_dependency_hashes` (nullable array of `{dependency_unit_id, recorded_hash, observed_hash}`), `stale_resolution` (nullable enum: `rerun`, `migration`, `waiver`, or `superseded`), `stale_resolution_ref` (nullable id such as a gate id, migration id, waiver id, or superseding unit id), and `succeeded_at` (nullable).

Each event in the event log is a `work_queue_event` record: `event_id`, `unit_id`, `occurred_at`, `transition` (enum: `created`, `enqueued`, `started`, `succeeded`, `failed`, `retried`, `blocked`, `unblocked`, `marked_stale`, `migration_required`, `waived`, `migrated`, `stale_requeued`, or `superseded`), `actor` (CLI command + version), `dependency_hashes_before` (nullable), `dependency_hashes_after` (nullable), `resolution_path` (nullable enum: `rerun`, `migration`, `waiver`, or `superseded`), `resolution_ref` (nullable), and `notes` (free_text, nullable).

When a completed unit's recorded upstream hashes no longer match the current hashes, the next mutating command marks every affected downstream unit `stale`, writes a `marked_stale` event with the old and new hashes, clears any stale output from publication eligibility, and refuses to publish the owning chapter until the unit is resolved. A stale unit returns to `runnable` only through `stale_requeued` after the operator or runtime chooses re-run. If the stale state is caused by a schema version gap with no registered migration chain, the unit becomes `migration_required` and the run uses `SCHEMA_MIGRATION_BLOCKED`. If stale work blocks a command that cannot safely choose a resolution path, the runtime writes a `gate_record` with `gate_name: STALE_WORK_UNIT_BLOCKED`, `scope` naming the unit/chapter/run, the stale dependency hashes, and the permitted resolution paths; `gate retry` rechecks the hashes and resolves only after every named unit is re-run, migrated, superseded, or linked to a schema-valid waiver whose scope permits publication. A `waived` unit must name the waiver id in `stale_resolution_ref`; waivers do not delete stale events and do not make source-fidelity metrics pass silently.

Crash recovery is deterministic and work-queue based. A read-only command may report a `running` unit whose writer lock is no longer held, but it does not repair state. The next mutating command that acquires the run lock first normalizes every orphaned `running` unit before doing new work: it appends a `normalized_failure_event` with category `correctable`, signature `interrupted_without_lock:<unit_id>`, and the last recorded actor, transitions the unit through `failed`, increments `attempt_count`, and then either moves it back to `runnable` when attempts remain or blocks it under the appropriate anti-spin or manual-review gate when attempts are exhausted. `noeasymark unlock <run_id>` only clears a stale lock file after operator confirmation; it never edits `work_queue.json` directly, so crash recovery remains auditable through the event log.

### `chapter_manifest` schema field set

Required: state_record mixin, `chapter_id`, `chapter_id_width`, `chunk_id_width`, `chapter_slug`, `chapter_slug_derivation` (normalized title, truncation decision, collision suffix kind/value, and chapter-title hash when used), `chapter_index` (1-based numeric), `source_page_range` (start and end `source_page_index`), `chunks` (array of `chunk_id`), `proposed_chunk_boundaries` (array of `{boundary_page, cascade_layer_that_fired, evidence_ref, candidate_score, scoring_breakdown, tie_break_reason, protected_content_intersections, tail_handling}`), `status` (enum: `planned`, `in_progress`, `converted`, `figures_extracted`, `adjudicated`, `published`, `superseded`, or `blocked`), `chapter_report_path`, `markdown_file_path`, `evidence_sidecar_path`, `manual_review_blocking_items` (array of waiver/review pointers), `superseded_artifacts` (array of `{path, moved_to, moved_at, reason}` records), `published_at` (nullable). `published_at` is populated only when `status` is `published`; all other statuses carry `published_at: null`.

### `chapter_report` schema field set

`chapter_report` required fields: state_record mixin, `chapter_id`, `run_id`, `report_json_path`, `report_html_path`, `generated_at`, `source_page_range`, `source_manifest_hash`, `chapter_manifest_hash`, `tool_profile_hash_full`, `markdown_file_path`, `evidence_sidecar_path`, `summary`, `qa_check_results`, `metrics`, `flags`, `links`, `reviewer_checklist`, and `generation_metadata`. Each `qa_check_result` carries `check_id`, `check_name`, `status` (`pass`, `warn`, `fail`, `not_applicable`, `unavailable`, or `waived`), `severity` (`info`, `warning`, or `blocking`), `metric_value` (nullable), `threshold` (nullable), `affected_pages`, `affected_blocks`, `evidence_refs`, `waiver_id` (nullable), `failure_signature` (nullable), `blocking` (boolean), and `next_action` (nullable free_text). The HTML report is rendered from this JSON record; the JSON is the audit authority.

### `aggregate_quality_report` schema field set

`aggregate_quality_report` required fields: state_record mixin, `run_id`, `report_json_path`, `report_html_path`, `generated_at`, `source_manifest_hash`, `outline_hash`, `tool_profile_hash_full`, `chapter_manifest_hashes`, `chapter_report_hashes`, `finalization_preflight`, `chapter_summaries`, `manual_review_summary`, `quality_metrics`, `glossary_summary`, `link_validation_results`, `anchor_validation_results`, `asset_inventory`, `orphan_asset_results`, `indexes_emitted`, `content_handling_summary`, `finalization_gate_id` (nullable), `reviewer_checklist`, and `generation_metadata`. `finalization_preflight` records the complete chapter set considered, chapter states, unresolved-marker counts by severity, waiver ids applied, source-page coverage, manifest hash set, and the pass/fail reason when the run is blocked. `manual_review_summary` records unresolved counts by severity and status, aging buckets (`0-24h`, `1-3d`, `4-7d`, `>7d`), median and p95 time to submitted/accepted/waived states, waiver counts by severity, per-chapter review load, and conflict counts; it does not include per-reviewer productivity metrics or direct reviewer identities in public reports. The HTML report is rendered from this JSON record; the JSON is the audit authority.

### `chunk_manifest` schema field set

Required: state_record mixin, `chunk_id`, `chapter_id`, `source_page_range`, `overlap_range` (nullable), `overlap_owner` (enum: `earlier_chunk`, `later_chunk`; defaults to `earlier_chunk`), `diagram_text_policy` (`crop_only` or `transcribe`; defaults from `run_config.diagram_text_policy_default`), `engine_runs` (array of `{engine_id, run_id, status, candidate_id_if_success, candidate_id_if_failed, failure_event_id_if_failed, runtime_seconds}`), `candidate_outputs` (array of successful `candidate_id` values), `failed_candidate_outputs` (array of failed `candidate_id` values), `judge_calls` (array of `adjudication_id`), `accepted_spans_count`, `disputed_spans_count`, `manual_review_spans_count`, `status` (enum: `planned`, `in_progress`, `converted`, `figures_extracted`, `adjudicated`, `published`, `superseded`). In each `engine_runs[]` record, exactly one of `candidate_id_if_success` or `candidate_id_if_failed` is populated when the run reaches a terminal engine status; `failure_event_id_if_failed` is populated only with `candidate_id_if_failed`.

### `layout_region` and `crop_record` schema field sets

`layout_region` required fields: state_record mixin, `region_id`, `source_page_index`, `page_label` (nullable), `pdf_user_space_bbox`, `canonical_page_bbox`, `rendered_pixel_bbox`, `render_profile_id`, `region_type`, `reading_order_index`, `column_group` (nullable), `confidence`, `confidence_method`, `detection_source`, `detector_contributions`, `parent_region_id` (nullable), `child_region_ids` (array of `region_id`), `text_hash` (nullable), `content_refs` (array), and `audit_metadata`. `region_type` is a closed enum whose initial values are `page`, `column`, `paragraph`, `heading`, `list`, `table`, `table_cell`, `figure`, `caption`, `equation`, `code`, `sidebar`, `footnote`, `header`, `footer`, and `unknown`. `detection_source` is a closed enum: `docling`, `marker`, `pymupdf_text_blocks`, `projection_xy_cut`, `visual_detector`, `manual_review`, or `merged`. Each `detector_contribution` carries `detector_id`, `detector_priority`, nullable `native_region_id`, nullable `native_region_type`, nullable `native_bbox`, nullable `native_coordinate_origin`, `normalized_bbox`, nullable `confidence`, `accepted` (boolean), `contribution_role` (`primary`, `secondary`, `audit`, `merged`, `rejected_overlap`, `contained_parent`, or `contained_child`), and nullable `notes`.

`crop_record` required fields: state_record mixin, `crop_id`, `crop_ref_id`, `region_id`, `source_page_index`, `render_profile_id`, `source_pdf_sha256`, `pdf_user_space_bbox`, `canonical_page_bbox`, `rendered_pixel_bbox`, `crop_image_path`, `crop_image_sha256`, `image_format`, `created_for`, `consumer_refs` (array), and `audit_metadata`. `created_for` is a closed enum: `layout_region_crop_ocr`, `judge_evidence_crop`, `manual_review_packet`, `figure_extraction`, or `qa_visual_diff`. A `crop_record` is the durable evidence object for a rendered crop; `crop_ref_id` is the stable cross-record pointer used inside cache-key evidence envelopes and candidate lineage when the crop image itself is stored once but referenced by multiple consumers.

### `candidate_output` and `candidate_lineage` schema field sets

`candidate_output` required fields: state_record mixin, `candidate_id`, `chunk_id`, `engine_id`, `engine_run_id`, `engine_family`, `status` (`succeeded` or `failed`), `candidate_markdown_path` (nullable), `candidate_markdown_sha256` (nullable), `sidecar_json_path`, `sidecar_json_sha256`, `spans` (array), `candidate_lineage`, `failure_event_id` (nullable), `failure_summary` (nullable), `retry_metadata` (nullable), `diagnostic_artifact_refs` (array), and `audit_metadata`. A successful candidate has non-null Markdown path/hash fields and at least one span unless the source pages are confirmed blank. A failed candidate has `status: failed`, null Markdown path/hash fields, an empty `spans` array, a non-null `failure_event_id`, and enough diagnostic artifact references for manual review and anti-spin replay.

Each `candidate_output.spans[]` entry has required fields: `span_id`, `candidate_id`, `span_index`, `offset_start`, `offset_end`, `text`, `source_page_indices`, `region_refs` (array of `region_id`), `lineage_ref`, `confidence` (nullable), `confidence_method` (nullable), `protected_zone` (boolean), and `normalizations_applied` (array). `candidate_id` identifies the whole engine output (`co-<14-char>`); `span_id` identifies the adjudication unit (`cs-<14-char>`) and is the stable id used by `candidate_spans`, `span_refs`, evidence sidecars, and cache-key evidence sorting. `span_index` is deterministic within the candidate output and is recorded for debugging only; cross-record references use `span_id`.

`candidate_lineage` required fields: state_record mixin, `lineage_id`, `candidate_id`, `span_id` (nullable for candidate-scope lineage, populated for span-scope lineage), `primary_text_source`, `source_artifact_refs`, `source_artifact_hashes`, `intermediate_toolchain`, `embedded_text_layer_quality_ref` (nullable), `delegated_service_id` (nullable), `lineage_confidence`, `lineage_confidence_method`, and `audit_metadata`.

### `figure_record`, `table_record`, and `equation_record` schema field sets

`figure_record` required fields: state_record mixin, `figure_id`, `chapter_id`, `visual_block_type` (`pure_illustration` or `technical_diagram`), `source_page_indices`, `region_refs`, `crop_refs`, `asset_paths` (array), `asset_image_format` (`png`), `asset_path_derivation`, `bounding_boxes`, `caption_status` (`paired_text`, `embedded_in_image`, `missing`, `uncertain`, or `not_applicable`), `caption_span_refs` (array of `span_id`), `caption_pairing_evidence`, `stitching_group_id` (nullable), `related_visual_ids` (array), `visual_asset_qa_results` (array), `crop_padding` (object with requested/applied point and pixel padding plus clipped edges), `confidence`, `confidence_method`, and `review_flags`.

`table_record` required fields: state_record mixin, `table_id`, `chapter_id`, `source_page_indices`, `region_refs`, `crop_refs`, `asset_paths` (array), `asset_image_format` (`png` when an image fallback exists), `structure_representation` (`markdown`, `sanitized_html`, `image_only`, or `manual_review`), `representation_decision_reason` (array), `representation_rubric_version`, `structured_table_path` (nullable), `structured_table_sha256` (nullable), `row_count` (nullable), `column_count` (nullable), `row_span_count`, `column_span_count`, `missing_cell_count`, `nested_content_present`, `rotated_or_oblique`, `cell_bounding_refs` (array), `caption_status`, `caption_span_refs` (array of `span_id`), `caption_pairing_evidence`, `stitching_group_id` (nullable), `related_visual_ids` (array), `visual_asset_qa_results` (array), `crop_padding`, `confidence`, `confidence_method`, and `review_flags`. If `structure_representation` is `sanitized_html`, the emitted HTML is restricted to table-structure elements and inert text/inline formatting; scripts, event-handler attributes, style attributes, external URLs, forms, iframes, embedded objects, and active content are forbidden. The sanitizer profile id and sanitized output hash are recorded in `structured_table_path` metadata. If safe structure cannot be produced, the table is preserved as an image crop and flagged for review rather than emitting unsafe HTML.

`equation_record` required fields: state_record mixin, `equation_id`, `chapter_id`, `equation_kind` (`display` or `inline`), `source_page_indices`, `region_refs`, `crop_refs`, `asset_paths` (array), `asset_image_format` (`png` when an image fallback exists), `transcription_format` (`latex`, `verbatim`, `image_only`, or `manual_review`), `transcription_text` (nullable), `transcription_validation_profile`, `parse_status`, `symbol_coverage`, `caption_or_label_span_refs` (array of `span_id`), `visual_asset_qa_results` (array), `crop_padding`, `confidence`, `confidence_method`, `protected_zone` (boolean; always true for equation content), and `review_flags`.

### `source_manifest` schema field set

Required: state_record mixin, `source_pdf_working_path` (relative to the run artifact directory), `source_pdf_public_path` (nullable until the deliverable copy has been written; then populated with the deliverable-root-relative path `original/source.pdf` or the relevant split-source manifest/path), `source_pdf_sha256`, `page_count`, `outline_status` (enum), `normalization_record` (status, command argv arrays, tool versions, intermediate artifact hashes, final `normalized.pdf` hash when produced, invariant-check results, and skip/failure reason when normalization is discarded), `ocr_language_config` (resolved language array, source `run_config` or `pipeline_default`, Tesseract `-l` argument string, `tesseract --list-langs` evidence id, and missing-language diagnostics when validation fails), `derived_artifacts` (object mapping artifact name to `{path, sha256, tool_id, tool_version, created_at}`), `aggregate` (object containing `text_layer_quality_aggregate` summary), `per_page` (array of `source_page_record` records ordered by `source_page_index` ascending).

`per_page` has no padding element: array element 0 is the record whose `source_page_index` is 1. Implementations must still look up pages by the explicit `source_page_index` field and must reject duplicate, missing, or out-of-order page indices so a 1-based page id never becomes an implicit 0-based array contract.

Each `source_page_record` carries: `source_page_index`, `page_label` (nullable), `mediabox`, `cropbox`, `rotation`, `rendered_pixel_dimensions`, `render_profile_id`, `text_layer_quality` (the full per-page quality block per Phase 3), `blank_page_status` (`not_blank`, `detected_blank`, `intentionally_blank`, or `unknown`), `blank_page_evidence` (nullable object with text, raster, and layout-region signals), and `ocr_runs` (array of `{policy, mode, languages, language_arg, output_path, sha256, glyph_anomaly_rate}` records).

### `approvals_record` schema field set

Required: state_record mixin, `output_folder_name`, `external_artifact_archive` (object: `granted` boolean, `decision_at`, `decided_by`, `decided_by_role`, `rationale`, nullable `archive_id` populated only when `granted` is true, nullable `retention_expectation` populated only when `granted` is true, nullable `granted_at`, nullable `granted_by`, nullable `granted_by_role`). The folder-scope archive record owns the archive identity and retention expectation only; per-run network-class scoping of which archive endpoints are approved lives under `run_config.content_handling.external_artifact_archive` and references this record through `folder_approval_reference`. A declined approval is represented by `granted: false`, a populated decision timestamp, decider, decider-role, and rationale, and null grant-only fields; it is not represented by omitting the approval object or by a pseudo-mode such as `none`. The role fields exist so the public sibling can carry accountability information (`decided_by_role`, `granted_by_role`) without exposing operator identity literals.

Public sibling `approvals_record_public` (used only if the operator later commits the deliverable; the runtime itself does not publish it): same shape minus `granted_by` (which can contain operator email or username), `decided_by` (same identity risk), `retention_expectation` (free_text that may name an internal vendor), and any `notes` fields. Approver and decider identity in the public copy are reduced to `granted_by_role` and `decided_by_role` respectively, each constrained to the closed enum `{maintainer, delegated_reviewer}`. The reduction is symmetric so the redaction policy is uniform across every identity-bearing field on the record. The internal `approvals_record` retains the literal `decided_by` and `granted_by` values for audit; the redaction transformation drops the literals and substitutes the corresponding `_role` field whose value the agent records at decision time alongside the literal identity.

This schema does not carry content-handling approvals; those live in `run_config` because they are per-run rather than per-folder.

Public-schema redaction is declared inline in each internal schema rather than in a separate document. NoEasyMark treats `nem_redaction`, `nem_public_transform`, and `nem_public_override` as project-owned JSON Schema annotation keywords; ordinary Draft 2020-12 validators are not expected to enforce their semantics. Pydantic declarations use project helper annotations rather than ad hoc raw dictionaries at call sites: `NemRedacted()` for `nem_redaction: true`, `NemRedacted(profile="<profile_name>")` for profile-specific redaction, `NemPublicTransform(rule=...)` for public transforms, and `NemPublicOverride(rationale=...)` for the rare maintainer-authorized exception. The helpers are implemented with Pydantic v2 `Field(json_schema_extra=...)` / `Annotated[...]` so the emitted JSON Schema carries the NoEasyMark annotations deterministically while the Python model remains readable.

Every field whose value must not appear in the public sibling carries a `nem_redaction: true` tag (or `nem_redaction: <profile_name>` for more granular control) in the JSON Schema. Redaction tags are the authority; hand-written public-schema descriptions in this specification are explanatory summaries and must not be treated as a separate source of truth. The public sibling schema is generated from the internal schema by a deterministic transformation over the emitted schema document: transform `$defs` entries once, recursively transform object `properties`, array `items`, tuple `prefixItems`, and composition branches (`allOf`, `anyOf`, `oneOf`, `not`, `if`, `then`, `else`), and preserve `$ref` strings after their referenced `$defs` entry has been transformed. When a property is dropped, it is also removed from the containing object's `required` list; if `required` becomes empty, the `required` keyword is omitted. A redacted field is omitted from the public schema rather than represented as `null`, an empty string, or a placeholder. `nem_public_transform` is a closed structured rule with `kind` constrained to `copy_role_only`, `hash_literal`, `relative_path_only`, `boolean_summary`, or `enum_generalize`; every kind has an explicit input-type and output-type contract documented in `tools/generate_public_schemas.py` and fixture-covered in `tests/test_public_schema_consistency.py`. Public schemas may not introduce fields that are absent from the internal schema unless the internal field or containing schema carries `nem_public_override` with a maintainer rationale and a fixture proving the public-only field cannot leak private content.

A CI test (`tests/test_public_schema_consistency.py`) verifies that committed public schemas match the generated forms exactly, that every public field is type-compatible with its internal source field or documented transform output, that every `nem_redaction` tag has a fixture proving the redacted value is absent from generated public output, and that every `nem_public_transform` kind is exercised by at least one fixture. If a hand-authored public sibling conflicts with `nem_redaction` tags, generation fails and the redaction tags win unless the internal schema carries a documented `nem_public_override` with maintainer rationale.

### `response_cache_entry` and `cache_provenance_archive` schema field sets

`response_cache_entry` required fields: state_record mixin, `entry_id`, `cache_key`, `cache_key_version`, `component_hashes` (object with exactly the component names from **Cache Key Canonicalization**), `response_schema_id`, `response_schema_version`, `backend_id`, `model_id`, `model_snapshot_or_version`, `stored_at`, `last_accessed_at`, `access_count`, `response_payload_path` (relative to the containing response-cache directory), `response_payload_sha256`, `provenance` (the same provenance envelope mirrored into `adjudication_record`), and `eviction_state` (`active`, `archived_for_unpublished_adjudication`, `pruned`). Optional fields: `expires_at`, `superseded_by_entry_id`, `diagnostic_failure_event_id`.

Only schema-valid successful backend responses are stored as active `response_cache_entry` records. Native-schema failures, client-side schema-validation failures, malformed JSON, transport errors, and anti-spin retry exhaustion are recorded as `normalized_failure_event` diagnostics and may populate `diagnostic_failure_event_id` on a later related cache record, but the invalid payload itself is not replayable from the local response cache.

`cache_provenance_archive` required fields: state_record mixin, `archive_id`, `archived_at`, `archive_reason` (`eviction`, `manual_prune`, `cache_migration`, or `superseded_reconversion`), `source_cache_scope` (`folder` or `run`), `source_cache_directory`, `cache_key`, `response_cache_entry_hash`, `adjudication_refs` (array of adjudication ids or review-marker ids that still rely on the provenance), `provenance_snapshot`, `response_payload_hash`, `response_payload_archived` (boolean), `archive_path`, and `retention_policy`. The archive schema permits omitting the raw response payload when the payload is still available in an unpruned cache entry or when content-handling approvals do not permit copying it; provenance itself remains available for audit replay.

### `artifact_usage_ledger` schema field set

`artifact_usage_ledger` is append-only and stored at `.noeasymark-state/<run_id>/ledgers/artifact-usage.jsonl`. Each event inherits `state_record_append_only` fields and carries `event_id`, `run_id`, `artifact_id`, `artifact_class` (`deliverable`, `private_working_copy`, `render`, `temporary_crop`, `candidate_output`, `ocr_sidecar`, `raw_model_log`, `adjudication_scratch`, `agent_run`, `response_cache`, `tool_cache`, `superseded_chapter_artifact`, or `external_archive_copy`), `content_sensitivity` (`public_reviewable`, `redacted_reviewable`, `source_derived_private`, `source_derived_raw`, or `secret_bearing`), `relative_path`, `path_root` (`deliverable_root`, `state_root`, `artifact_root`, `shared_artifact_root`, or `external_archive`), `sha256` (nullable until file creation completes), `size_bytes` (nullable until file creation completes), `created_by_unit_id`, `retention_class` (`commit_ready`, `retain_until_chapter_published`, `retain_until_run_finalized`, `retain_until_manual_prune`, `cache_policy`, or `external_policy`), `retention_expires_at` (nullable), `archive_required_before_prune` (boolean), `lifecycle_event` (`created`, `referenced`, `archived`, `pruned`, `superseded`, or `verified`), `archive_id` (nullable), `archive_object_ref` (nullable), `archive_manifest_hash` (nullable), `source_artifact_id` (nullable), and `notes` (nullable free_text). No ledger event may record credentials, signed URLs, or absolute local paths.

### `artifact_archive_record` schema field set

`artifact_archive_record` is a committed configuration record under `config/artifact_archives/`. Required fields: state_record mixin, `archive_id`, `identifier_case_policy` (`case_sensitive` or `case_insensitive`; controls allowlist normalization for this `archive_id`), `enabled` (boolean), `network_class` (`device`, `perimeter`, or `internet`), `endpoint_identifiers` (array of `endpoint_identifier` records), `credential_env_vars` (array of environment variable names, never values), `eligible_artifact_classes` (array using the `artifact_usage_ledger.artifact_class` enum), `raw_source_derived_allowed` (boolean), `retention_policy`, `encryption_at_rest_required` (boolean), `allowed_encryption_modes`, `provider_verification_method` (`device_path_check`, `head_object`, `object_inventory`, `provider_api`, or `manual_attestation`), `verification_adapter_config`, `archive_path_template`, `allowlist_match_fields`, `max_object_bytes` (nullable), `multipart_threshold_bytes` (nullable), and `change_notes`. `enabled: true` is allowed only when `doctor --artifacts` can verify the provider, credentials, approval chain, and configured verification method for the active run.

`active_run_pointer` is a plain UTF-8 text file at `.noeasymark-state/active_run` containing exactly the active `run_id` followed by a single LF newline and is therefore not schema-validated; the runtime trims surrounding whitespace when reading, rejects any content that does not match the `run_id` grammar, and never writes embedded whitespace inside the id. A pointer that resolves to a non-existent run directory is treated as "no active run" and surfaces the explicit `noeasymark init` hint to the operator. `schemas/_index.yaml` is a build-input file consumed by `tools/generate_schema_hooks.py` whose shape is a single top-level `schemas:` sequence, each entry naming `schema_id`, the path to the `.schema.json` file, the path to the valid-fixture directory, the path to the invalid-fixture directory, and the optional `public_schema_id` of the sibling redaction schema when one exists; `tools/generate_schema_hooks.py` itself validates the index against `schemas/schemas_index.schema.json` before emitting hook blocks.

The `state_record` mixin declares the fields every persisted state file carries: `schema_id`, `schema_version`, `created_by` (tool name and version that wrote the record), `created_at` (UTC ISO 8601 timestamp), `migration_history` (ordered list of `migration_id` strings that have been applied to the file), and optional `input_hashes` or `depends_on` arrays. Schemas that store run-state inherit `state_record` so that the migration runtime can read and update the shared bookkeeping fields without inspecting each schema individually.

The `candidate_output` schema must include per-span `region_refs` so each emitted span links to the `layout_region` records it derives from. Adjudication relies on this to fetch the correct crop when a span is disputed, especially on multi-column pages.

Every schema that exposes a `confidence` field uses a float in the closed interval `[0.0, 1.0]`, where `1.0` is full confidence. Each record that writes a confidence value also writes a `confidence_method` string identifying the producer's calibration source (for example, `engine_native`, `judge_self_reported`, `consensus_agreement_ratio`, `reviewer_attested`, or `not_calibrated`). Aggregations across records never mix calibration sources without recording the mix.

All schemas include `schema_id`, `schema_version`, and deterministic object identifiers where practical. Schema, prompt, judge-backend YAML, engine YAML, and `cache_key` use monotonically increasing zero-padded integer versions; the `noeasymark` Python package itself uses semantic versioning. Resumable state files include a `created_by` tool/version record and an `input_hashes` or `depends_on` field identifying the upstream artifacts that affected them. All timestamps anywhere in the system are UTC ISO 8601 with millisecond precision (`YYYY-MM-DDTHH:MM:SS.sssZ`); the runtime uses a single time source for both monotonic and wall-clock timestamps and records the source identity in the run config so cross-host traces remain comparable. If a schema changes incompatibly during a run, the tool must either migrate the affected state with a recorded migration id or mark the run blocked for operator review.

Schema migrations are first-class artifacts. Each forward migration lives in the package at `noeasymark.migrations.<schema_id>.v<from>__v<to>` with a source path such as `src/noeasymark/migrations/<schema_id>/v<from>__v<to>.py`, a module-level `migrate(record: dict) -> dict` function, a `migration_id` constant equal to the file basename without extension (for example `v0001__v0002`), and a `description` constant. Migrations are unidirectional (no down-migration is required by the spec). The migration registry is populated automatically from package resources at import time using `importlib.resources.files("noeasymark.migrations")` plus `pkgutil.iter_modules`/`importlib.import_module` over migration packages; it must work in editable installs and installed wheels and must not assume a physical `src/` directory exists at runtime. No manual registration step is required. On load, the runtime compares the file's recorded `schema_version` against the installed schema version and:

1. If they match, load normally.
2. If the file is older and a chain of registered migrations covers every intervening step, run them in order, write the result back atomically, append the chain of `migration_id` values to the file's `migration_history` array, and continue.
3. If any step is missing or any migration raises, mark the affected run blocked with `SCHEMA_MIGRATION_BLOCKED`, post a tracking update naming the schema, the file, the source and target versions, and the missing migration step, and stop without modifying the file.
4. If the file is newer than the installed schema, refuse to load and direct the operator to upgrade the `noeasymark` package.

`noeasymark migrate` is the operator-facing subcommand that previews and applies pending migrations for the active run; `noeasymark migrate --dry-run` reports the plan without modifying state. Repository-scope state (the `pipeline.toml` file, the shipped engine and judge-backend YAMLs, the shipped render-profile YAMLs, and any other file validated against a `pipeline_config`-style repo-wide schema) has its own migration registry under `src/noeasymark/migrations/pipeline_config/v<from>__v<to>.py` with the same module surface (`migrate`, `migration_id`, `description`). `noeasymark migrate --pipeline` previews and applies repository-scope migrations; the runtime refuses to load a `pipeline.toml` whose schema version is older than the installed schema when no migration chain covers the gap and surfaces the gap as `SCHEMA_MIGRATION_BLOCKED`. Migrations are exercised by schema-example tests so a migration regression fails CI before it reaches an operator.

Wire schemas into the template's schema tooling by following the layout `schemas/<name>.schema.json` plus `schemas/examples/<name>/valid/*` and `schemas/examples/<name>/invalid/*` fixture subdirectories for every schema. For each schema, the generated tooling creates two `.pre-commit-config.yaml` aliases: a `check-metaschema` entry that validates the schema itself, and a `check-jsonschema` entry scoped to `schemas/examples/<name>/valid/` that validates the positive fixtures against the schema. NoEasyMark provides a `tools/generate_schema_hooks.py` helper that emits both the bounded `.pre-commit-config.yaml` schema block and the matching bounded `.github/workflows/data-ci.yml` schema-step block from `schemas/_index.yaml`; the generated blocks are delimited by `# noeasymark: begin schemas` and `# noeasymark: end schemas` comments.

The script is exposed through the developer-facing Typer subcommand `noeasymark dev regenerate-schema-hooks`, which writes the regenerated blocks in place; the same script powers a no-write `--check` mode used by CI. The script is *not* a pre-commit hook (silent file rewrites during a commit are surprising) and is *not* run automatically by any conversion subcommand. The developer's workflow is: add or remove a schema -> run `noeasymark dev regenerate-schema-hooks` -> commit. The per-PR Linux host for `noeasymark dev regenerate-schema-hooks --check` is `.github/workflows/data-ci.yml`, because the check validates schema/data-file wiring rather than Python runtime behavior. `cross-platform-check.yml` also runs the same check on Linux, macOS, and Windows before final development handoff to catch path and line-ending assumptions. CI fails the build if the regenerated output diverges from the committed files; the canonical test name is `tests/test_generated_schema_hooks.py`.

The regenerated block is bounded: `generate_schema_hooks.py` reads, replaces, and writes back only content between `# noeasymark: begin schemas` and `# noeasymark: end schemas` (in `.pre-commit-config.yaml`) and between `# noeasymark: begin schemas` and `# noeasymark: end schemas` (in `.github/workflows/data-ci.yml`). Template-owned hooks that the spec instructs Phase 0 to retain — notably `validate-template-sync-manifest-schema`, `validate-template-sync-marker-schema`, `validate-template-sync-manifest`, and `validate-template-sync-marker` — live *outside* those delimiters; the regeneration script must refuse to write anything outside the delimited region and a CI assertion verifies the template-owned hook block is byte-identical to the upstream template's shape after Phase 0 adoption.

**Required placement of the NoEasyMark schema-hook block.** In both `.pre-commit-config.yaml` and `.github/workflows/data-ci.yml` the NoEasyMark-owned block (`# noeasymark: begin schemas` through `# noeasymark: end schemas`) appears immediately after the inherited template-sync schema-validation hooks (after the `# template-sync: end schema-template-sync-support-only` line in pre-commit-config; after the analogous template-sync schema step in data-ci) and before any language-stack hooks or data-file lint steps. This co-locates all schema-related validation in one section and keeps the natural read order "schema validators → language-stack hooks → data-file linters." The placement is required, not merely conventional: a CI assertion verifies that no non-NoEasyMark hook appears between the template-sync schema block and the NoEasyMark schema block.

**Public-schema generation tooling.** Public sibling schemas are generated by `tools/generate_public_schemas.py` (mirroring `tools/generate_schema_hooks.py`) and exposed through the developer-facing Typer subcommand `noeasymark dev regenerate-public-schemas`, which writes regenerated public schemas in place. The same script powers a no-write `--check` mode used by CI: `noeasymark dev regenerate-public-schemas --check` exits non-zero when any committed public schema diverges from the deterministic transformation of its internal source. The check runs in `.github/workflows/data-ci.yml` and in `cross-platform-check.yml`. The canonical test name is `tests/test_public_schema_consistency.py`; it loads every internal schema, applies the same transformation in-process, and asserts byte-identity with the committed public sibling. Both `tools/generate_schema_hooks.py` and `tools/generate_public_schemas.py` write JSON output using `json.dumps(obj, sort_keys=True, indent=2, separators=(",", ": "), ensure_ascii=False)` followed by exactly one terminal `\n`; YAML output uses a single canonical writer that emits block-style mappings, no flow style, sorted keys, double-quoted strings, and exactly one terminal `\n`. Both tests assert byte-identity using those exact formatters, and the `check-jsonschema` pin used by NoEasyMark's hook block is identical to the one used by the template-sync block — NoEasyMark does not maintain a separate pin; an upgrade is initiated only when the upstream template advances its pin. At spec-authoring verification on 2026-06-03 that shared pin is `0.37.2`.

**Render-profile generation tooling.** The shipped `config/render_profiles/default-300dpi.yaml` is regenerated by `tools/generate_render_profile.py`, exposed as `noeasymark dev regenerate-render-profile`. The script reads `pymupdf.__version__` from the active interpreter and writes the YAML with the resolved value as `renderer_version`. `--check` mode exits non-zero when the committed YAML's `renderer_version` differs from the active runtime; CI's `cross-platform-check.yml` runs `--check` on every platform so a renderer drift surfaces as a CI failure rather than a silent `render_profile_hash_full` and `render_profile_id` change. Because `render_profile_hash_full` is the SHA-256 of the canonical-JSON of the profile fields and `render_profile_id` is its 12-character lowercase hexadecimal prefix, a `renderer_version` bump produces a new hash/id pair and is treated as a tool-profile change per the **Render profile** closed handoff decision.

Invalid fixtures are exercised by `tests/test_schema_examples.py`, which auto-discovers `schemas/<name>.schema.json` and the `schemas/examples/<name>/{valid,invalid}/` trees and asserts the expected outcome for each fixture. Ordinary `files:` regex changes to existing generated hooks are picked up through pre-commit; alias additions, removals, and renames are made by regenerating both bounded blocks in the same change. Keep generated JSON deterministic and sorted where practical.

`pipeline.toml` lives at the repository root and is validated against the `pipeline_config` schema. Phase 0 writes a populated `pipeline.toml` containing every constant whose owner is `pipeline.toml` in **Default Constants and Thresholds**; the table remains the canonical prose source and the file is the committed machine-readable materialization of those defaults. Schema defaults exist only as a fallback for future optional fields and tests, not as a reason to ship a blank or skeletal `pipeline.toml`. The file records repository-scope defaults and limits: chunk-size band, hard-maximum chunk size, pilot-sample band, gold-set floor, mini-pilot cap, engine and backend signature caps, no-progress windows, response cache size cap (`0` means no cap) and eviction policy (`lru` is the default), artifact disk warning thresholds, minimum-free-space warning thresholds, the chapter cap on unresolved low-severity review markers (high- and normal-severity markers block independently), crop-padding defaults, default OCR policy, default OCR languages, the `delegates_to` chain-length limit, and any other repository-scope tunable surfaced in the constants table. `pipeline.toml` does not duplicate per-run state; ceilings, content-handling approvals, storage choices, per-run OCR language choices, and pre-pilot backend preferences live in `run_config.json`; active bindings and quality thresholds live in `tool_profile.json`. When a repository default is intentionally overridable per run, the run field is named as an override and its precedence over `pipeline.toml` is documented in both schemas.

Prompts live under `config/prompts/<role>/v0001.md`, `v0002.md`, and so on, where `<role>` is the exact judge-role or pipeline-step name used elsewhere in this specification. The shipped roles are `default_judge`, `escalation_judge`, `fallback_judge`, `triage_model`, `independent_auditor`, `alternate_auditor`, `glossary_build`, and `manual_review_reingest`. Additional auditor or routing roles enabled by later pilots use the same role identifier they receive in `tool_profile.json`. Each file's frontmatter records prompt id, prompt version, compatible role, intended model families and CLI agents, schema-output expectations, response schema id and version, and change notes. The canonical `prompt_id` is `<role>/<version>` (for example `default_judge/v0003`). Every `adjudication_record` cites the prompt id and version it used. Prompts are version-controlled and reviewable; no prompt is ever inlined as a Python constant.

Prompt `response_schema_id` values point to backend-facing response schemas, not to persisted state-record schemas. The shipped model-facing schemas are `adjudication_response` for `default_judge`, `escalation_judge`, `fallback_judge`, and `manual_review_reingest`; `triage_response` for `triage_model`; `audit_response` for `independent_auditor` and `alternate_auditor`; and `glossary_candidates_response` for `glossary_build`. These response schemas contain only fields the model is expected to produce. The executor validates the model output against the response schema, then adds runtime-owned state, backend identity, provenance, cache, timing, cost, and reviewer-audit fields before validating and persisting the corresponding `adjudication_record`, `triage_decision`, `audit_decision`, or `glossary_candidates` record.

Minimum model-facing response schema contracts:

| Response schema | Required model-produced fields | Forbidden runtime-owned fields |
| --- | --- | --- |
| `adjudication_response` | `chosen_text`, `confidence`, `confidence_method`, `review_flag`, `reason`, `source_support_summary`, `candidate_refs` | `adjudication_id`, `decision_source`, backend identity, cache fields, cost fields, timing fields, reviewer identity, absolute paths |
| `triage_response` | `routing_class`, `confidence`, `confidence_method`, `reason`, `span_refs` | `triage_id`, backend identity, cache fields, cost fields, timing fields, reviewer identity, absolute paths |
| `audit_response` | `audit_result`, `confidence`, `confidence_method`, `findings`, `reason` | `audit_id`, backend identity, cache fields, cost fields, timing fields, reviewer identity, absolute paths |
| `glossary_candidates_response` | `candidates` array whose items include `surface_form`, `normalized_form`, `aliases`, `domain`, `confidence`, `candidate_rationale`, and `source_refs` | `glossary_id`, `glossary_version`, approver identity, backend identity, cache fields, cost fields, timing fields, absolute paths |

All four response schemas set `additionalProperties: false`, forbid raw prompts, raw model responses, tool logs, credential names or values, local absolute paths, and source text outside the bounded field that the prompt explicitly asks the model to produce. They may carry source-page indices, candidate ids, region ids, crop refs, confidence numbers, enum values, and short reasons needed to transform the response into persisted state. Phase 0 commits valid and invalid fixtures under `schemas/examples/<response_schema_id>/`; invalid fixtures cover every forbidden runtime-owned field above, extra properties, missing required fields, wrong enum values, and attempts to return host-local paths. `tests/test_schema_examples.py` and `tests/test_prompt_response_schema_contracts.py` both load these fixtures so a prompt/schema edit cannot accidentally ask a model to emit runtime-owned state.

The v0001 content for every shipped role is enumerated below or explicitly aliased to another enumerated body. Each block is the verbatim file body the implementing agent writes to `config/prompts/<role>/v0001.md`; frontmatter values are populated by the implementing agent against the `prompt_record` schema. The body is structured as a stable prefix (durable rules), optional reusable middle blocks marked with `{{ reusable: ... }}` placeholders that the runtime fills in from cached context, and a variable suffix marked with `{{ variable: ... }}` placeholders filled at call time.

Reusable blocks have deterministic empty-state renderings. Before an approved glossary exists, `{{ reusable: glossary_summary_block }}` renders exactly:

```text
No approved glossary is available for this call.
```

The associated provenance values are `glossary_id: "_none_"`, `glossary_version: "v0000"`, and the `glossary_context_hash` defined in **Cache Key Canonicalization**. After approval, the block renders a stable, schema-derived summary of `glossary/approved.yaml` sorted by `entry_id`, and any glossary content change creates a new `glossary_version` plus a new `glossary_context_hash`. When no topic description is set, `{{ reusable: topic_description_block }}` renders an empty string and the existing `topic_description_hash` empty-string rule applies.

### `config/prompts/default_judge/v0001.md`

```text
You judge disputed text spans extracted from a PDF page. Your output is a single
JSON record matching the adjudication_response schema. Do not write any text outside
the JSON.

Source-page evidence is ground truth. Candidate texts are hypotheses produced by
different OCR, layout, or document-AI engines. If an image crop is included,
examine it first. If this is a text-only evidence packet, examine the structured
source-evidence fields first and treat the absence of pixels as a reason to lower
confidence or flag for review when the text evidence is not enough. Then examine
each candidate and decide.

Be especially careful with protected zones, where a single wrong character changes
meaning: numbers, units, signs, Greek letters and other symbols, equation syntax,
code-like text, citations, headings, figure/table/equation labels, caption text,
cross-references, and any domain term marked protected by the glossary. Inside a
protected zone, do not normalize or smooth a candidate; choose verbatim or flag for
review.

Different candidates come from different lineages (embedded text layer, OCR-redo,
OCR-force, document-AI, vision-LLM, rendered-page OCR). Lineages of different
quality should not be averaged. Prefer the candidate whose lineage best matches
the source page. When the embedded text layer for this page was labeled poor,
mixed, or unknown by preprocessing, distrust candidates that derive solely from
that lineage and prefer candidates with independent visual or service lineage.

When an approved glossary is provided and a candidate matches a glossary term,
prefer the glossary surface form over morphological variants. Never apply a
glossary rewrite inside a protected zone.

If evidence is insufficient to choose confidently, flag the span for review rather
than guess. Inventing missing content, units, labels, captions, or cross-references
is never acceptable.

Before emitting JSON, verify your chosen text against the source-page evidence one
more time.

{{ reusable: topic_description_block }}

{{ reusable: glossary_summary_block }}

{{ variable: disputed_span_evidence }}
```

### `config/prompts/escalation_judge/v0001.md`

```text
This span has been escalated to you because automated judging could not resolve
it. The cost of a wrong decision here is high; the cost of flagging for human
review is far lower. Take extra care.

Your output is a single JSON record matching the adjudication_response schema. Do
not write any text outside the JSON.

Source-page evidence is ground truth. Candidate texts are hypotheses. If an image
crop is included, examine it first. If this is a text-only evidence packet,
examine the structured source-evidence fields first and treat the absence of
pixels as a reason to lower confidence or flag for review when the text evidence
is not enough. Then examine each candidate and decide.

Be especially careful with protected zones, where a single wrong character changes
meaning: numbers, units, signs, Greek letters and other symbols, equation syntax,
code-like text, citations, headings, figure/table/equation labels, caption text,
cross-references, and any domain term marked protected by the glossary.

Before deciding, verify each of the following against the source-page evidence:
(a) every digit, (b) every unit, (c) every symbol, (d) every label,
(e) every cross-reference. Cite the evidence reference that supports the choice
in the JSON reason field so reviewers can audit your evidence.

Different candidates come from different lineages. If two candidates appear
equally consistent with the source, prefer the candidate whose engine lineage
is most independent from the others (per the candidate_lineage.primary_text_source
field) and record the lineage rationale in the reason field.

When an approved glossary is provided, glossary entries marked protected_zone are
never overridden by a candidate that diverges from them.

If after careful examination you remain uncertain, flag the span for human review.
Inventing missing content is never acceptable.

{{ reusable: topic_description_block }}

{{ reusable: glossary_summary_block }}

{{ variable: disputed_span_evidence }}
```

### `config/prompts/fallback_judge/v0001.md`

```text
You are running as the fallback judge. The primary judge backend was unavailable
or failed for this span. Apply the same care as the primary judge; the primary's
absence does not permit lower standards.

Your output is a single JSON record matching the adjudication_response schema. Do
not write any text outside the JSON.

Source-page evidence is ground truth. Candidate texts are hypotheses produced by
different OCR, layout, or document-AI engines. If an image crop is included,
examine it first. If this is a text-only evidence packet, examine the structured
source-evidence fields first and treat the absence of pixels as a reason to lower
confidence or flag for review when the text evidence is not enough. Then examine
each candidate and decide.

Be especially careful with protected zones, where a single wrong character changes
meaning: numbers, units, signs, Greek letters and other symbols, equation syntax,
code-like text, citations, headings, figure/table/equation labels, caption text,
cross-references, and any domain term marked protected by the glossary.

Different candidates come from different lineages. Prefer the candidate whose
lineage best matches the source page. When the embedded text layer was labeled
poor, mixed, or unknown by preprocessing, distrust candidates that derive solely
from that lineage.

When an approved glossary is provided and a candidate matches a glossary term,
prefer the glossary surface form over morphological variants. Never apply a
glossary rewrite inside a protected zone.

If evidence is insufficient, flag the span for review rather than guess. Inventing
missing content is never acceptable.

{{ reusable: topic_description_block }}

{{ reusable: glossary_summary_block }}

{{ variable: disputed_span_evidence }}
```

### `config/prompts/triage_model/v0001.md`

```text
You classify disputed text spans for routing. Do not decide the span itself;
choose exactly one routing class for each span:

- accept_with_consensus: all provided candidates agree after low-risk
  normalization, and the agreeing candidates have independent engine lineage,
  and the span is not inside a protected zone where even consensus requires
  judge review.
- route_to_default_judge: candidates disagree on prose with no apparent
  high-stakes content.
- route_to_escalation_judge: candidates disagree on protected-zone content
  (numbers, units, signs, Greek letters, symbols, equation syntax, code-like
  text, citations, headings, figure/table/equation labels, caption text,
  cross-references, or any term marked protected_zone in the glossary), or the
  span has been disputed multiple times, or the candidate-confidence spread is
  wide.
- route_to_manual_review: evidence is missing or insufficient to route
  meaningfully (no successful candidates, contradictory lineage, missing crop).

Do not emit a separate fallback routing class. Fallback judge selection is an
execution-layer recovery path used when the selected default or escalation judge
is unavailable, fails schema output repeatedly, or is blocked by credentials,
quota, or transient service state. The triage decision records the intended
semantic route; the judge executor records any fallback substitution in
provenance.

Your output is a single JSON record matching the triage_response schema. Do not
write any text outside the JSON. Spend minimal reasoning; this is the low-cost
routing path.

Worked examples:
- Two candidates produce the same body-prose sentence with different punctuation
  after normalization: accept_with_consensus.
- Two candidates produce different decimal values in a measurement: route_to_escalation_judge.
- All engine candidates failed: route_to_manual_review.
- One candidate present, no agreeing peers, prose region: route_to_default_judge.

{{ variable: disputed_span_minimal_evidence }}
```

### `config/prompts/independent_auditor/v0001.md`

```text
You are the independent auditor. You review acceptance decisions made by the
default judge to detect regressions. You are independent of the original judge;
do not anchor on its reasoning.

Examine the source-page evidence first, then the accepted text, then the
alternative candidates that were rejected, then decide whether you agree or
dissent.

Your output is a single JSON record matching the audit_response schema. Do not
write any text outside the JSON.

Be especially careful with protected zones, where a single wrong character
changes meaning: numbers, units, signs, Greek letters and other symbols, equation
syntax, code-like text, citations, headings, figure/table/equation labels,
caption text, cross-references, and any domain term marked protected by the
glossary.

Different candidates come from different lineages. Prefer the candidate whose
lineage best matches the source page. When the embedded text layer was labeled
poor, mixed, or unknown by preprocessing, distrust candidates that derive solely
from that lineage.

Record a confidence rating distinct from the original judge's confidence; do not
copy it. When you dissent, name the alternative candidate you would have chosen
and the evidence that supports it. When you agree, name the strongest piece of
evidence that confirms the acceptance.

{{ reusable: topic_description_block }}

{{ reusable: glossary_summary_block }}

{{ variable: acceptance_under_audit }}
```

### `config/prompts/alternate_auditor/v0001.md`

The alternate-auditor prompt body is byte-identical to `config/prompts/independent_auditor/v0001.md`. It is committed as a separate file with `prompt_record.role: alternate_auditor` and `compatible_role: alternate_auditor` so the prompt id, cache provenance, and audit records show which auditor slot ran. Any future divergence between the two auditor prompts must bump only the changed prompt's `prompt_version` and must be treated as a tool-profile change.

### `config/prompts/glossary_build/v0001.md`

```text
Extract candidate glossary terms from the provided pilot adjudicated output.

Your output is a single JSON record matching the glossary_candidates_response schema, a
list of glossary_entry candidates. Do not write any text outside the JSON.

Select candidates that fit at least one of:
- high-frequency technical terms (proper nouns, abbreviations, jargon),
- non-ASCII symbols and terms,
- repeated OCR-confused spellings that resolved to a consistent normalized form,
- cross-chunk spelling clusters,
- terms where engines frequently disagreed and a judge resolved consistently,
- terms suggested by the optional topic description.

For each candidate, populate: surface_form (the most common spelling, used as
the preferred form), normalized_form, optional aliases, optional misreadings,
domain tag, evidence_refs (every entry must cite at least one source-page
location; do not invent terms that lack evidence), protected_zone flag (true
for equations, code, file paths, citations, file extensions, and similar exact
sequences), and confidence reflecting how sure you are this is a real domain
term rather than a one-off OCR error.

Mark protected_zone=true for terms that, if rewritten by the glossary, could
change technical meaning.

Confidence values reflect the strength of evidence, not popularity. A single
high-confidence appearance can outweigh many low-confidence appearances.

{{ reusable: topic_description_block }}

{{ variable: pilot_adjudicated_output }}
```

### `config/prompts/manual_review_reingest/v0001.md`

```text
A human reviewer has filled in a manual_review_input file with corrected text
for a disputed chunk. Re-run adjudication treating the reviewer's text as the
highest-priority evidence source.

Your output is a single JSON record matching the adjudication_response schema. Do
not write any text outside the JSON.

The reviewer's text supersedes prior judge decisions for spans the reviewer
corrected. For each affected span, mark the span accepted as a manual-review
resolution. The runtime sets decision_source: manual_review and copies
reviewer_identity and reviewer_confidence from the input file into the persisted
adjudication record.

Where the reviewer's text resolves a previously disputed span, accept it. Where
the reviewer's text introduces material outside the original disputed range,
narrow the change to the disputed range and flag the extra material for separate
review. Where the reviewer's text leaves a span ambiguous (placeholder, TODO
marker, missing fill), preserve the original review marker.

Apply the same protected-zone awareness as the default judge. If the reviewer's
text changes a protected zone, accept the change because the reviewer is
authoritative, but log the change in the audit field.

Where the reviewer's text conflicts with the source-page image (apparent typo
by the reviewer), still accept the reviewer's text but emit
reviewer_text_diverges_from_source: true with a divergence_reason field
describing the conflict so a second reviewer can audit.

{{ reusable: topic_description_block }}

{{ reusable: glossary_summary_block }}

{{ variable: manual_review_input_with_original_evidence }}
```

Judge backend configurations live under `config/judge_backends/<name>.yaml` as defined in **Judge Backends**. Engine configurations live under `config/engines/<name>.yaml` as defined in **Engine Configurations**. Render-profile aliases live under `config/render_profiles/<name>.yaml`. The configurations are version-controlled and reviewable.

Devcontainer setup is part of scaffolding. The repository ships two selectable devcontainer variants using the standard one-level subdirectory layout: `.devcontainer/cpu/devcontainer.json` and `.devcontainer/cuda/devcontainer.json`. The `cpu/` variant is based on `mcr.microsoft.com/devcontainers/base:ubuntu-24.04` (the Microsoft devcontainers Ubuntu 24.04 Noble image) for lightweight local work. The `cuda/` variant is based on a verified NVIDIA CUDA devel image for Ubuntu 24.04, selected by the OCI Distribution discovery procedure described below; at the time this specification was authored, the current published tag the procedure selects is `nvidia/cuda:13.2.1-devel-ubuntu24.04`, but the discovery procedure is the authority, not the literal tag in this paragraph. Both variants install Python 3.13 the same way to keep the toolchain comparable: the `postCreateCommand` installs `uv` via the official install script, then runs `uv python install 3.13` to materialize a pre-built CPython 3.13 from `python-build-standalone`. The committed `.python-version` file and `requires-python` in `pyproject.toml` select the 3.13 support family; the exact Python patch version, `uv` version, native tool versions, npm-installed agent CLI versions, base image digest, and external binary versions are recorded in `.devcontainer/toolchain-fingerprint.json` during Phase 0. A top-level `.devcontainer/devcontainer.json` is not shipped during Phase 0; the absence of a top-level file makes the two variants appear as explicit choices in tooling that supports multiple configurations. `.devcontainer/README.md` states that `cpu` is the documented default and `cuda` is selected only when local GPU engines are needed. If a future Codespaces or editor workflow requires a top-level default, the top-level file must duplicate or forward to the CPU variant and the CUDA variant remains in `.devcontainer/cuda/`.

The committed devcontainer `FROM` lines use floating tags — `mcr.microsoft.com/devcontainers/base:ubuntu-24.04` for CPU and the selected newest `nvidia/cuda:<version>-devel-ubuntu24.04` for CUDA — so each rebuild pulls the current image. The agent does not pin image digests in `FROM`. The Ubuntu major (`24.04`) stays fixed because the apt package set and the CUDA tag family depend on it. Phase 0 records the observed base-image digests and the full toolchain in `.devcontainer/toolchain-fingerprint.json`; the nightly byte-identity job verifies the fingerprint and reports `TOOLCHAIN_DRIFT` on change rather than failing the build. Phase 0 still discovers the newest CUDA `-devel-ubuntu24.04` tag and records observed digests into the fingerprint using direct OCI Distribution requests (it does not depend on a local Docker daemon, which is generally not reachable from inside the bootstrap devcontainer). The CUDA tag-discovery procedure is: (1) walk `https://registry-1.docker.io/v2/nvidia/cuda/tags/list` (paginated) to enumerate live tags; (2) filter to tags whose suffix is exactly `-devel-ubuntu24.04` and whose prefix parses as a stable semantic version `<major>.<minor>.<patch>` with no prerelease or build suffix; (3) sort candidates by numeric `(major, minor, patch)` descending and select the greatest stable version (`13.2.1-devel-ubuntu24.04` at the time this specification was authored); (4) fetch a Bearer token from `https://auth.docker.io/token?service=registry.docker.io&scope=repository:nvidia/cuda:pull`; (5) GET `https://registry-1.docker.io/v2/nvidia/cuda/manifests/<selected-tag>` with the OCI and Docker manifest `Accept` headers to read the `Docker-Content-Digest` response header; (6) record the selected tag and observed digest in `.devcontainer/toolchain-fingerprint.json` and in the Phase 0 PR body for audit. Do not use registry lexical tag order as version order; OCI tag listing is lexical for pagination, not semantic-version selection. The CPU base digest is recorded the same way through the equivalent MCR endpoint. If the registry requests fail, the agent records the failure and the floating tag is used as-is — there is no registry-verification halt; `noeasymark doctor --gpu` reports the CUDA image digest as unrecorded until a later run records it.

An observed base-image digest alone does not freeze apt repository contents, npm registry contents, or native package resolver behavior. Phase 0 therefore writes `.devcontainer/toolchain-fingerprint.json` after the canonical variants are built. The fingerprint records the floating base image reference, the observed base-image digest, `uv --version`, the exact Python patch version reported by `uv python list --only-installed`, `dpkg-query` package names and versions for qpdf, MuPDF, Ghostscript, Tesseract, Poppler, Pandoc, build tools, Node, and Git LFS, `npm ls -g --depth=0 --json` entries for agent CLIs, and the executable-level `--version` output for each native binary. `noeasymark doctor --devcontainer-fingerprint` compares the active container against the committed fingerprint. The nightly devcontainer byte-identity job first requires a matching fingerprint; if the fingerprint drifts, the job reports `TOOLCHAIN_DRIFT` and does not classify output differences as conversion regressions until a maintainer refreshes the fingerprint, reruns the mini-pilot, and updates expected outputs deliberately.

Both canonical devcontainer files declare named Docker volumes that persist agent credentials and the implementing-agent's gate state across container rebuilds so the operator does not have to re-authenticate or re-discover the `SCHEMA_DESIGN_REVIEW` PR after every rebuild. The shipped `mounts` block on each variant binds three project-scoped named volumes to their respective locations:

```jsonc
"mounts": [
  "source=noeasymark-claude-creds,target=/home/vscode/.claude,type=volume",
  "source=noeasymark-codex-creds,target=/home/vscode/.codex,type=volume",
  "source=noeasymark-implementation-state,target=/workspaces/noeasymark/.implementation-state,type=volume"
]
```

The volume names use the `noeasymark-` prefix so they do not leak credentials or implementation state between unrelated projects on the same host. These mounts are Docker named volumes, not host bind mounts: the committed devcontainer files must not mount host paths such as `~/.config/gh`, `~/.claude`, `~/.codex`, `.ssh`, cloud-provider credential directories, or the Docker socket. If an operator wants to bind host credentials instead of using project-scoped named volumes, that is a private uncommitted devcontainer override and is outside the shared repository default. The bootstrap devcontainer does not need these mounts because it is short-lived; the canonical variants own the long-running credential and gate-state persistence. The `noeasymark-implementation-state` volume exists so the `SCHEMA_DESIGN_REVIEW` gate state file at `.implementation-state/gates/schema-design-review.json` survives container rebuilds; the label-discovery fallback described in **Phase 0** still applies on a host that has never built the container (or whose volume has been pruned).

Both canonical variants ship the same `customizations.vscode.extensions` block as the bootstrap variant in the document frontmatter:

```jsonc
"customizations": {
  "vscode": {
    "extensions": [
      "ms-python.python",
      "ms-python.vscode-pylance",
      "tamasfe.even-better-toml",
      "redhat.vscode-yaml",
      "DavidAnson.vscode-markdownlint",
      "GitHub.vscode-pull-request-github"
    ]
  }
}
```

The CUDA variant may add the NVIDIA Nsight VS Code extension (`nvidia.nsight-vscode-edition`) when the operator wants GPU debugging integration, but the shipped CUDA `customizations.vscode.extensions` block does not include it by default to keep both variants identical except for the GPU `runArgs`.

NoEasyMark's runtime is portable across the three supported OS families: mainstream supported distributions of Linux with the required native packages and comparable kernel/glibc capability (Ubuntu 24.04 LTS or later Ubuntu releases still in Canonical standard support, Debian 12 or later while under Debian security or LTS support for the target architecture, currently maintained Fedora releases rather than fixed EOL Fedora floors, and comparable mainstream distributions); supported macOS release lines (the current macOS major release and the two prior major release lines as reflected by Apple's current-version and compatibility documentation, only on Apple hardware those releases officially support); and supported Windows releases that Microsoft still lists as in support (Windows 11 24H2 and later while in support, Windows Server 2022 and later while in support, Windows 10 22H2 only when the operator is enrolled in Microsoft Extended Security Updates, and Windows 10 LTSC/LTSB editions only while their lifecycle remains active). Windows 10 22H2 without ESU/LTSC coverage is an unsupported host: `noeasymark doctor` reports it as such and directs the operator to use Windows 11, a supported Windows Server release, a Linux/macOS host, or one of the canonical devcontainers. The canonical devcontainer variants are one supported execution substrate; the same toolchain also runs host-native on each of the three OS families using the install paths described in **Cross-platform Toolchain Installation** below. `noeasymark doctor` enumerates the host OS, upstream support status when discoverable, the lock primitive in use (`fcntl.flock` or `msvcrt.locking`), the process-supervision primitive in use (POSIX process group or Windows job object), the available filesystem capabilities (long-path support, symlink creation rights), and the install state of every native tool dependency, so the operator can confirm the host is fully equipped before running any conversion phase. On Windows, `doctor` additionally reads the `HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\LongPathsEnabled` registry value and emits guidance when it is `0`; the runtime internally uses the `\\?\` extended-length path prefix for every filesystem operation under `.noeasymark-state/`, `.noeasymark-artifacts/`, and `.implementation-state/`, so the conversion pipeline operates correctly regardless of the registry setting, but reviewers viewing `references/` content through tools that do not understand `\\?\` may still encounter the historical 260-character limit and the existing `PATH_LENGTH_WARNING` at 240 characters helps surface that risk before publication.

Shared install scripts install `qpdf`, `mupdf-tools`, `ghostscript`, `tesseract-ocr`, `poppler-utils`, `pandoc`, build tools, Node, `uv`, and the npm-packaged agent CLIs `@anthropic-ai/claude-code` and `@openai/codex` in both variants. Including the agent CLIs in the canonical `postCreateCommand` makes the canonical devcontainer continuous with the bootstrap: an operator who wants to launch additional autonomous-mode agent work after Phase 0 (for example, to drive a later refactoring task) does not have to reinstall agent CLIs by hand. Operators who prefer a minimal container without agent CLIs may edit the committed `postCreateCommand` and rebuild; the spec default is "agent CLIs present." GitHub Actions run directly on hosted runners unless a later workflow explicitly opts into devcontainer execution. Either variant runs:

- The `cuda/` variant declares `runArgs: ["--gpus", "all"]`; the `cpu/` variant declares no GPU runArgs.
- If a CLI agent backend is in use, install or document install steps for that agent's CLI and provide a documented way to mount or pass through whatever credential the agent expects.
- Do not ask the user to provide a devcontainer volume path. The tool determines artifact storage automatically:
  - In a devcontainer, use a mounted Docker volume or workspace-local gitignored directory.
  - Outside a devcontainer, use a workspace-local gitignored directory such as `.noeasymark-artifacts/`.
  - Allow an advanced override in `pipeline.toml`, but do not require it.
- The repository includes `.gitignore` rules for generated artifacts. The devcontainer runs `noeasymark doctor --gpu` after creation. Missing CUDA, `nvidia-smi`, or GPU-only binaries disables GPU-dependent engines with an actionable report; it fails the scaffolding check only when the selected tool profile requires a GPU engine.
- Do not copy dangerous agent permission-bypass settings into the devcontainer by default. If a local workflow needs them, make them an explicit user-side customization rather than part of the shared project default.

Recommended default artifact paths:

```text
.noeasymark-artifacts/
  _shared/
    <output-folder-name>/
      responses/      # default folder-scoped response cache
  <run_id>/
    renders/
    candidates/
    ocr/
    crops/
    model-logs/
    agent-runs/     # per-call temp dirs for CLI agent backends
    responses/      # run-scoped response cache when selected
    cache/
```

### Cross-platform Toolchain Installation (Host-Native)

Operators who prefer host-native execution to a devcontainer install the same toolchain directly. The supported per-OS install paths are below; the resulting `noeasymark doctor` report is identical across the three OS families when the toolchain is fully installed.

**`uv` installation** (the Python toolchain manager that materializes Python 3.13 and resolves project dependencies):

- Linux and macOS: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Windows (PowerShell): `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
- All OSes alternative: `pipx install uv` when `pipx` is already present.

After `uv` is installed, `uv python install 3.13` materializes the same pre-built CPython 3.13 toolchain across all three OSes. `.python-version` and `requires-python` in `pyproject.toml` pin the interpreter version repository-wide.

**Native binary tool dependencies** (qpdf, mupdf-tools or its newer `mutool` packaging, ghostscript, tesseract-ocr with English language data, poppler-utils, pandoc; build essentials are required only when Phase 6 engines that compile from source are activated). `noeasymark doctor` checks each tool's presence on `PATH` and reports an actionable install command per OS:

- Linux (Debian / Ubuntu): `sudo apt-get install -y qpdf mupdf-tools ghostscript tesseract-ocr tesseract-ocr-eng poppler-utils pandoc build-essential`
- Linux (Fedora / RHEL): `sudo dnf install -y qpdf mupdf ghostscript tesseract tesseract-langpack-eng poppler-utils pandoc gcc gcc-c++ make`
- Linux (Arch / Manjaro): `sudo pacman -S --needed qpdf mupdf-tools ghostscript tesseract tesseract-data-eng poppler pandoc base-devel`
- macOS (Homebrew): `brew install qpdf mupdf ghostscript tesseract tesseract-lang poppler pandoc` (build tools are provided by Xcode Command Line Tools, installed via `xcode-select --install`)
- Windows (winget, recommended): `winget install -e --id qpdf.qpdf; winget install -e --id ArtifexSoftware.GhostScript; winget install -e --id UB-Mannheim.TesseractOCR; winget install -e --id JohnMacFarlane.Pandoc; winget install -e --id oschwartz10612.Poppler`. MuPDF (`mutool`) ships from `https://mupdf.com/releases`; download the latest Windows `.zip` and add the extracted directory to `PATH`.
- Windows (Chocolatey alternative): `choco install -y qpdf ghostscript tesseract poppler pandoc mupdf`
- Windows (Scoop alternative): `scoop install qpdf ghostscript tesseract poppler pandoc mupdf-tools`

Phase 0 verifies every Windows package-manager identifier and the MuPDF manual download route before treating the commands above as current operator guidance. The verification writes `windows_installer` rows into `.implementation-state/phase-0-freshness.json` for each route with `package_manager` (`winget`, `chocolatey`, `scoop`, or `manual_vendor_download`), `package_id`, `command`, `observed_status` (`available`, `renamed`, `deprecated`, `not_found`, or `verification_deferred`), `observed_version` when the package manager exposes one, `checked_at_utc`, and `evidence_ref`. If a package id has changed but an equivalent supported route exists, Phase 0 updates the command and records `status: updated`; if no current route can be verified, the command remains conservative, the row records `verification_deferred`, and `_TODO_AFTER-BUILD-MANUAL-STEPS.md` names the exact tool whose Windows install route needs maintainer confirmation.

Each per-OS command set above is reported by `noeasymark doctor` (selected by detected OS) with the specific missing tools enumerated. Tesseract's language data location varies by installer; doctor checks `tesseract --list-langs` and reports the language packs actually present.

**Agent CLIs (Codex CLI, Claude Code CLI)** are npm packages that install identically across all three OSes:

- All OSes: `npm install -g @anthropic-ai/claude-code @openai/codex`

On Windows, npm installs `.cmd` shims into the user's `%APPDATA%\npm\` directory; `noeasymark doctor` resolves these via `shutil.which` which understands Windows shim suffixes (`.cmd`, `.bat`, `.exe`, `.ps1`). The subprocess runner invokes them using their fully-resolved path so PATH-search variations do not affect call behavior. CLI authentication (`codex login`, `claude auth login`) is identical across OSes.

**`git-lfs`** installs identically through its vendor's documented per-OS paths and needs no NoEasyMark-specific guidance. NoEasyMark does not use the `gh` CLI.

**GPU support** is a Linux-and-Windows capability, not a macOS capability. The CUDA devcontainer variant requires either a Linux host with the NVIDIA Container Toolkit installed, or a Windows host running Docker Desktop with WSL2 backend and an NVIDIA driver that supports WSL2 GPU pass-through. macOS hosts (Apple Silicon and Intel) cannot run CUDA-dependent engines; `noeasymark doctor --gpu` on macOS reports the unavailability and the spec-defined fallback applies (CPU-only engines remain fully usable; the active tool profile is rejected only when it explicitly requires a CUDA-dependent engine). Apple Silicon hosts may use accelerated CPU paths through Metal-aware libraries when an engine declares Metal support, but the spec ships no Metal-bound engine in the initial engine set.

Definition of done: `noeasymark --help` lists all required subcommands, `noeasymark init --help` documents every required input and approval, `noeasymark doctor`, `noeasymark doctor --gpu`, `noeasymark doctor --backends`, and `noeasymark doctor --devcontainer-fingerprint` all report actionable tool status for the components in scope on every supported OS family or devcontainer variant, schemas validate, prompt files exist with frontmatter, judge backend YAML files validate against the `judge_backend_record` schema, engine YAML files validate against the `engine_record` schema, artifact archive YAML files validate against the `artifact_archive_record` schema, render-profile YAML files validate against the `render_profile` schema, `.devcontainer/toolchain-fingerprint.json` exists and matches the canonical CPU devcontainer, `pipeline.toml` validates against the `pipeline_config` schema, retained template-inherited per-PR workflows (`precommit-ci.yml`, `python-ci.yml`, `data-ci.yml`, `markdownlint.yml`, and any retained optional workflows) are green on their configured runners, `cross-platform-check.yml` has passed on `ubuntu-latest`, `macos-latest`, and `windows-latest` before the final development handoff per the Phase 0 timing rule, `check-placeholders.yml` passes after placeholders are resolved, and scaffolding CI passes. During Phase 0 it is valid for every shipped backend and engine example to remain `enabled: false`; in that state `doctor --backends` schema-validates every configuration, reports `no enabled or tool-profile-bound backends to smoke-call`, and exits 0 if no other actionable finding exists. On a host without GPU access (including every macOS host), `doctor --gpu` may report GPU features unavailable without failing the whole scaffolding check, but any local GPU engine remains disabled until the GPU check passes.

Phase 0 schema and fixture validation also proves the backend- and engine-specific contracts above: `.implementation-state/phase-0-freshness.json` validates against `phase0_freshness_record`, `tests/test_judge_schema_lowering.py` passes for every shipped backend family, `tests/test_engine_output_contracts.py` passes for every shipped engine, `tests/test_engine_adapter_smoke_contracts.py` asserts resolved call shapes and disabled remote features for every shipped engine adapter, `tests/test_document_service_usage_counters.py` covers Mistral, Azure Document Intelligence, Azure Content Understanding, and unavailable-usage examples, `tests/test_table_representation_rubric.py` covers every Phase 7 table representation decision, `tests/test_caption_pairing.py`, `tests/test_equation_transcription_validation.py`, `tests/test_visual_stitching_and_asset_qa.py`, and `tests/test_material_conflict_rubric.py` cover the closed visual and consensus rubrics, backend YAML schema validation rejects mismatched `transport_only_parameter_proofs`, tool-profile fixtures reject unmatched `transport_only_parameter_addition_proofs`, shipped backend and engine examples reject invalid `failure_classification` patterns or placeholders, model-facing response-schema fixtures reject runtime-owned fields, and backend or engine YAML examples reject missing `cost_tracking.billing_freshness` checklists for every vendor-dependent cost mode.

### Cross-platform Path and Encoding Conventions

NoEasyMark stores and operates on paths portably across Linux, macOS, and Windows. The runtime uses `pathlib.Path` (Python's OS-portable path abstraction) for every internal file operation; spec text uses `/` as the canonical reference form when documenting paths. Operator-facing CLI flags that accept paths (`--source`, `--output-folder-name` overrides, etc.) accept either separator and normalize on input via `Path` construction. The runtime never embeds a literal `\\` or `/` directly into a constructed path string when a `Path` operation is available. On Windows, the runtime additionally uses the extended-length `\\?\` prefix internally for every filesystem operation under `.noeasymark-state/`, `.noeasymark-artifacts/`, and `.implementation-state/`, so the pipeline operates correctly without requiring `LongPathsEnabled` to be flipped in the registry; the prefix is stripped before paths are displayed to the operator or written into public manifests.

Generated text outputs (Markdown, JSON, YAML, evidence sidecars, reports, manifests, ledger files) are written with LF line endings regardless of host OS. The same conversion run is expected to produce byte-identical text content on Linux, macOS, and Windows hosts only when it uses the same committed lockfile, render profile, tool profile, source manifest, cache inputs, and externally invoked tool versions. When host-native external tool versions differ, `doctor` records the drift and the cross-platform expectation becomes schema-equivalent, evidence-comparable output rather than byte identity; the devcontainer profiles provide the strongest byte-reproducibility target. The shipped `.gitattributes` reinforces the policy with `* text=auto eol=lf` for text-classified files, in addition to the binary LFS rules. Reviewers using Windows-native editors that historically required CRLF (notably classic Notepad before Windows 10's October 2018 build) need a modern editor; Visual Studio Code, modern Notepad, Sublime Text, JetBrains tools, and the IDE extensions referenced elsewhere in this spec all handle LF natively.

Filesystem case sensitivity is handled portably without runtime checks. Output folder names and chapter slugs are lowercase by construction; the output-folder collision check is case-insensitive so that `comed-blue-book-2025` and `ComEd-Blue-Book-2025` are recognized as the same folder on macOS APFS (case-insensitive by default), Windows NTFS (case-insensitive by default), and Linux ext4/xfs/btrfs (case-sensitive). Image asset filenames, chunk identifiers, chapter identifiers, and markdown filenames are also lowercase by construction so cross-OS clones of the same repository resolve identically.

No spec-defined behavior depends on symbolic link creation. The runtime does not create symlinks; it stores file copies where the spec language says "private working copy" and "public LFS copy." This avoids the Windows requirement that symlink creation needs either Developer Mode or administrator rights and the macOS / Linux semantics that vary by filesystem.

## Phase 1 - Session Resumption Protocol

At the start of every agent session:

1. Run `noeasymark status`.
2. `status` invokes `noeasymark doctor --quick` first by default; environment problems halt the session before any paid or backend-driven work begins. The explicit `status --skip-doctor` diagnostic path may print atomically written state while labeling it as not preflight-verified, but it is read-only and cannot advance work.
3. If no active run configuration exists, print the exact `noeasymark init` action needed and stop without processing source content.
4. Print the next undone unit, current spend/runtime, progress rate, blocked gates, latest chapter state, content-handling mode, and current tool profile including judge backend bindings.
5. If status shows a `BLOCKED` gate, update the local run-status file with the needed input and stop.
6. Otherwise continue with the next resumable unit.

The next undone unit is read from `.noeasymark-state/<run_id>/work_queue.json`. The queue is a dependency DAG, not a filename scan. Each completed unit records its output hashes; each downstream unit records the upstream hashes it depends on. If an upstream artifact changes, dependent units become `stale` and must be re-run, waived, superseded, or migrated before publication.

This makes the work survivable across context resets.

All wall-clock and monotonic time access goes through a single `noeasymark.time.Clock` abstraction so cross-host traces stay comparable. The default `Clock` implementation exposes `now_utc() -> datetime.datetime` (built on `datetime.datetime.now(timezone.utc)`) for every ISO timestamp the runtime writes, and `monotonic_ns() -> int` (built on `time.monotonic_ns()`) for every duration measurement. The runtime never derives ISO timestamps from `monotonic_ns()` and never calls `time.time()`, `datetime.utcnow()`, `datetime.now()` without an explicit `tz=` argument, or any other clock primitive directly outside the module that defines `noeasymark.time.Clock`. Doctor's startup check allows `datetime.datetime.now(timezone.utc)` and `time.monotonic_ns()` only in that clock module, rejects `time.time()` and `datetime.utcnow()` everywhere in runtime source, and rejects other direct clock primitives anywhere under `src/noeasymark/` except a narrowly enumerated test-only allowlist outside the installed package. The shipped default `Clock` records `time_source_id: "system_clock_utc"` in `run_config`; `run_config` owns that operator input. When pilot creates or updates `tool_profile.json`, it snapshots the active value as `tool_profile.time_source_snapshot.time_source_id` plus the clock implementation id and includes that snapshot in `tool_profile_hash_full`. Adjudication, triage, audit, cache, and progress provenance record the snapshot value used for the call. Changing `run_config.time_source_id` after tool-profile approval blocks source-derived backend calls until `doctor --backends` validates the new clock source and the operator approves a tool-profile revision or mini-pilot waiver. The abstraction exists so an operator coordinating multiple hosts can substitute a synchronized-clock implementation without touching the rest of the codebase; doing so is a tool-profile change because the time source enters every adjudication record's provenance.

Mutable state is single-writer by lock scope. A subcommand that mutates per-run state files under `.noeasymark-state/<run_id>/` acquires `.noeasymark-state/<run_id>/.lock`; a subcommand that mutates folder-scoped approvals acquires `.noeasymark-state/_folders/<output-folder-name>/.approvals.lock`; and a subcommand that mutates the folder-scoped response cache acquires `.noeasymark-artifacts/_shared/<output-folder-name>/.cache.lock`. Operations that must mutate more than one scope acquire every required lock before the first mutation, in lexicographic order of the normalized absolute lock-file paths, and release them in reverse order; this deterministic ordering prevents cache/run and approval/run deadlocks across concurrent invocations. Each lock acquisition is nonblocking (`timeout=0` and `blocking=False` for `filelock`, or the backend's exact equivalent), so contention always exits immediately with exit code 80 before any state mutation.

Every lock file records holder metadata after the OS-level lock is acquired: `pid`, `hostname`, command line, start timestamp, `lock_backend`, `lock_backend_version`, `os_lock_primitive`, and `lock_scope`. The default locking package is `filelock`, used through its hard OS-locking class in exclusive-writer mode only; the runtime never uses `SoftFileLock`, `SoftReadWriteLock`, or lock-file-existence polling as an authority. Its version is pinned in `pyproject.toml` and is bumped together with the rest of the dependency lockfile. With `lock_backend: filelock`, `os_lock_primitive` is `filelock:fcntl.flock` on Linux/macOS and `filelock:msvcrt.locking` on Windows. With `lock_backend: portalocker`, `os_lock_primitive` is `portalocker:fcntl.flock` on Linux/macOS and `portalocker:win32file.LockFileEx` on Windows. The spec's atomic-write + exclusive-only design has no shared-reader requirement, so `filelock`'s lack of shared-reader semantics is intentional rather than a workaround. `noeasymark doctor` validates locking end-to-end on the current host by acquiring each supported lock scope in nonblocking mode, spawning a worker subprocess that attempts the same acquisition, asserting that the worker fails immediately, and releasing the lock; doctor records the lock backend and primitive that fired so the operator can confirm cross-host comparability. An operator who later needs shared-reader semantics may swap in `portalocker` and record the substitution in `pipeline.toml` under `lock_backend`; the runtime treats the swap as a Phase 0-class change and re-runs `doctor`. A second mutating invocation that finds the OS-level lock held exits immediately with exit code 80 and a message naming the holder metadata if readable. Stale lock-file contents or a leftover lock-file path are never treated as contention; the OS-level acquisition result is authoritative, and the OS-level lock is released automatically when the holding process exits. Read-only commands (`status`, `doctor`, `migrate --dry-run`) read atomically-written snapshots without taking writer locks.

Atomic write semantics are required for every state and ledger file so a crashed writer leaves the previous valid state in place. The portable protocol is: (1) write the new content to a sibling temporary file in the same directory as the target (so the rename is intra-directory and intra-filesystem); (2) on POSIX, call `os.fsync` on the temporary file's descriptor and additionally `os.fsync` the parent directory's descriptor for durability; on Windows, call `os.fsync` on the temporary file's descriptor (Windows does not expose directory fsync, but the `os.replace` operation below is atomic at the NTFS/ReFS level); (3) call `os.replace(temp_path, target_path)` to atomically swap the new content over the old (`os.replace` is portable and atomic on Linux, macOS, and Windows for same-filesystem same-directory moves; unlike `os.rename`, it succeeds when the target exists on every supported OS). On Windows, transient `OSError` with `errno.EACCES`/`PermissionError`/`ERROR_SHARING_VIOLATION` from a concurrent reader (commonly caused by antivirus or filesystem-indexing tools holding the file open) is retried with exponential backoff up to 5 attempts; persistent failures are recorded as `normalized_failure_event` with category `correctable` and signature `windows_sharing_violation:<path>` so the operator can identify the responsible host process.

Phase 1 ships a dedicated atomic-write crash-consistency test suite. The canonical test module is `tests/test_atomic_write.py`, and cross-platform CI runs it on Linux, macOS, and Windows. The suite proves that the writer refuses a temporary path outside the target directory, leaves the previous file intact when interrupted before replace, leaves either the complete old file or the complete new file after replace with no partial bytes, reports unsupported parent-directory fsync without weakening POSIX durability when the platform lacks that primitive, records `windows_sharing_violation:<path>` after exhausting Windows sharing-violation retries, and rejects cross-filesystem temp-file usage. Tests use synthetic small JSON/Markdown files and monkeypatched fault points rather than killing the test runner unpredictably. Any helper that writes state, ledgers, reports, cache manifests, manual-review files, or generated Markdown must route through the same atomic-write primitive or have a test proving why an alternate protocol is equivalent.

## Phase 2 - Spend, Runtime, Caching, Backend Resilience, and Anti-Spin Controls

Maintain schema-validated `.noeasymark-state/<run_id>/budget.json`, `.noeasymark-state/<run_id>/progress.json`, and `.noeasymark-state/<run_id>/artifact-usage.json`.

Track per-engine runtime, resolved engine resource-budget consumption, per-backend and per-model spend, hosted document-service usage counters, per-chapter cost, wall-clock elapsed time, artifact bytes by class, rendered-page bytes, free disk space at phase boundaries, pages successfully advanced, retry count by failure signature, backend retry count by backend/model/operation, consecutive no-progress attempts, vendor cache metrics mode, vendor cache hit/miss counts when available, and local response cache hit/miss counts. For CLI agent backends whose `cost_tracking.mode` is `runtime_only`, `zero_subscription`, `subscription_credit_pool`, or `unknown`, the ledger records invocation count, wall-clock runtime, and the configured mode rather than per-token spend.

Hard ceilings are optional by default. The default is no hard spend ceiling and no hard global wall-clock ceiling. Even when hard ceilings are disabled, the ledgers still record spend, runtime, and artifact growth. The dollar spend ceiling applies only to calls whose `cost_tracking.mode` is `native`, `estimated_by_model`, or `estimated_by_page`. For `subscription_credit_pool` backends, `invocation_ceiling` and `runtime_ceiling_seconds` are mandatory whenever the backend is enabled or tool-profile-bound, because subscription-pool usage can run silently expensive. For acknowledged `unknown` backends, the same two ceilings are mandatory once the operator grants `unknown_cost_acknowledgment`. For `zero_subscription` and `runtime_only` backends, those ceilings remain optional unless the active tool profile makes them mandatory. Operators may also set hard `artifact_bytes_ceiling`, `rendered_page_bytes_ceiling`, and `minimum_free_space_bytes`; when absent, the repository warning thresholds still produce visible status warnings before large render or OCR batches. A local configured ceiling hit records `limit_subreason: configured_ceiling_exceeded` and uses `BUDGET_THRESHOLD_HIT`. Provider-side account quota, billing, or subscription-credit exhaustion records `limit_subreason: provider_quota_or_credit_exhausted` on the backend or engine failure event and uses `JUDGE_BACKEND_BLOCKED` or `ENGINE_BLOCKED` rather than pretending the operator's local ceiling was reached.

Caching has two independent layers:

- **Vendor-native prompt caching** reduces in-run or near-term repeated-call cost on the provider side when the backend/provider supports it. NoEasyMark does not promise cross-run provider-cache reuse; only the local response cache is a durable replay mechanism. Structure every paid prompt as a stable prefix, optional reusable middle blocks, and variable suffix. The stable prefix contains durable judge rules, system instructions, schema-output description, glossary version reference, and prompt metadata. The optional one-sentence topic description lives in a reusable middle block keyed by `topic_description_hash`, not in the stable prefix. Other reusable middle blocks contain large context that is reused across multiple calls for the same chunk or chapter. The variable suffix contains page-specific layout regions, crop references, candidate spans, disputed spans, and any other per-call evidence. For providers with explicit cache controls, place cache breakpoints at the end of reusable blocks according to provider rules. For providers that cache automatically, keep exact reusable prefixes identical across calls. For CLI backends, vendor cache metrics may be `native`, `inferred`, or `unavailable` depending on the backend.
- **Local response cache** sits in front of every paid or agent-driven judgment call. Cache entries are schema-validated (`response_cache_entry`) and stored at folder scope under the gitignored artifact directory at `.noeasymark-artifacts/_shared/<output-folder-name>/responses/` so that successive runs of the same document share lookups by `cache_key`. Per-run scratch and partial responses still live under `.noeasymark-artifacts/<run_id>/`. A run may opt out of the shared cache by setting `response_cache_scope: run` in `run_config.json`, in which case entries are stored under `.noeasymark-artifacts/<run_id>/responses/` and the run reads and writes only its own entries. Cache identity is fully captured by `cache_key`, so cross-run sharing never substitutes a response that was produced under a different backend, model, prompt, schema, evidence, or render profile. Each entry has a two-layer structure:
  - `cache_key` - a deterministic composite digest that drives lookup. Computed exactly as described in **Cache Key Canonicalization**, over these component hashes:
    - `cache_key_version`
    - `backend_id`
    - `backend_config_hash`
    - `model_id`
    - `model_snapshot_or_version`
    - `model_parameters_hash`, over the merged provider-native parameter bag (see **Backend Execution Parameters**)
    - `response_schema_id`
    - `response_schema_version`
    - `backend_emitted_schema_hash`
    - `rendered_prompt_hash`
    - `evidence_content_hash`
    - `topic_description_hash`
    - `glossary_context_hash`
    - `render_profile_hash_full`
  - `provenance` - the rich audit record attached to the cache entry and mirrored into `adjudication_record`. Includes prompt_id and prompt_version, response_schema_id and response_schema_version, backend-emitted schema hash, glossary_id, glossary_version, glossary_context_hash, `render_profile_id`, `render_profile_hash_full`, `tool_profile_hash_compact`, `tool_profile_hash_full`, tool_profile_snapshot_path, source_artifact_hashes, router_state when LiteLLM Router was enabled, vendor-cache metrics, and the invocation timestamp.

The cache is looked up by `cache_key` alone. Provenance may be extended freely as new audit fields are needed; provenance changes never invalidate the cache. Adding a new output-affecting field requires bumping `cache_key_version`, so additions become observable invalidation events rather than silent stale hits.

Vendor-cache metrics are normalized before they enter ledgers or provenance. The normalized field set is `vendor_cache_metrics_mode`, `vendor_cache_provider`, `vendor_cache_read_tokens`, `vendor_cache_write_tokens`, `vendor_cache_uncached_input_tokens`, `vendor_cache_retention_policy`, `vendor_cache_breakpoint_count`, and `vendor_cache_source_fields`. OpenAI direct backends read cached-token counts from the official usage field exposed by the active API shape, such as `usage.prompt_tokens_details.cached_tokens` for Chat-style envelopes or `usage.input_tokens_details.cached_tokens` for Responses-style envelopes when present; OpenAI automatic caching records `vendor_cache_write_tokens: null` because the API does not expose a separate write count. Anthropic direct backends map `usage.cache_read_input_tokens` to `vendor_cache_read_tokens` and `usage.cache_creation_input_tokens` to `vendor_cache_write_tokens`; when the response exposes cache duration or breakpoint details, the adapter records them as `vendor_cache_retention_policy` and `vendor_cache_breakpoint_count`. LiteLLM-backed providers either pass through the provider's native fields into the same normalized names or record `vendor_cache_metrics_mode: unavailable`; a LiteLLM adapter may use `inferred` only when a fixture proves the upstream response lacks native fields but the adapter can compute read/write/uncached token counts from documented passthrough metadata. CLI-agent backends record `unavailable` unless the CLI exposes stable machine-readable cache metrics. The adapter fixture set includes one provider-native example and one unavailable example for every shipped backend family that claims vendor-cache support.

Provider prompt-cache profiles are fixture-tested under `tests/fixtures/judge_backends/<backend-family>/prompt_cache_profiles/` and exercised by `tests/test_prompt_cache_profiles.py`. Each shipped profile includes positive fixtures for a cacheable request and metric extraction, plus negative fixtures for below-minimum prompt sizes, too many explicit breakpoints, cache-control on a changing suffix, retention or TTL values the provider does not support, and request-field changes that should invalidate or reduce cache reuse. OpenAI fixtures cover automatic caching, the 1024-token cacheability floor, `cached_tokens` extraction from the supported usage envelopes, `prompt_cache_key` routing identity, and the fact that write tokens may be unavailable. Anthropic fixtures cover explicit or automatic `cache_control`, provider maximum breakpoint count, model/platform-specific minimum token thresholds, `cache_read_input_tokens` and `cache_creation_input_tokens`, TTL handling, the provider's cacheable block ordering, and invalidation-sensitive fields such as tools, system content, message blocks, image presence, tool choice, and thinking controls. A backend may set `vendor_cache_metrics: unavailable` only when its fixture proves the response shape lacks stable metrics or the CLI wrapper cannot surface them; unavailable metrics still render as `not_applicable` in public reports rather than blocking correctness.

The response cache enforces the `response_cache_size_cap` and `response_cache_eviction_policy` declared in `pipeline.toml`. `response_cache_size_cap` is expressed in bytes; `0` means no size cap, so automatic eviction and `CACHE_CAP_HIT` do not fire. Positive values accept a bare integer byte count or a case-insensitive binary suffix: `K`, `KB`, and `KiB` mean 1024 bytes; `M`, `MB`, and `MiB` mean 1024^2 bytes; `G`, `GB`, and `GiB` mean 1024^3 bytes; `T`, `TB`, and `TiB` mean 1024^4 bytes. Decimal SI semantics are not used even for `KB`/`MB`/`GB`/`TB`. The resolved integer-bytes value is recorded in the active configuration record. The default eviction policy is `lru` (least-recently used by `cache_key` lookup time); operators may set `lfu` (least-frequently used) or `none` (no automatic eviction, manual prune required). Cache hits update `last_accessed_at` and `access_count` atomically under the folder-cache lock or run-cache lock before the cached response is replayed. Concurrent hits serialize through that lock, so no access counter increment is lost. For `lru`, eviction candidates are sorted by oldest `last_accessed_at`, then oldest `stored_at`, then lexicographic `cache_key`. For `lfu`, candidates are sorted by lowest `access_count`, then oldest `last_accessed_at`, then oldest `stored_at`, then lexicographic `cache_key`. Manual `cache prune` uses the same ordering after applying its filters. When the policy is `none` and a positive cap is reached, the run halts with `CACHE_CAP_HIT`, posts a tracking update naming the cap and the cache directory, and refuses further cache writes until the operator runs `noeasymark cache prune` or raises the cap. Eviction never removes a cache entry that backs an unpublished adjudication record whose owning chapter has not reached terminal `published` state without first archiving its provenance under `.noeasymark-state/<run_id>/cache_provenance/` so audit replay remains possible.

On a successful cache hit, the call is not issued and no spend is recorded; the prior schema-valid response is replayed with its full provenance. Cached responses are replayable only while every component of `cache_key` is unchanged. Classified failures may be recorded for diagnostics and anti-spin accounting, but they are not replayed as completed work.

All API and agent calls must be resumable and outage-tolerant.

Every backend client must include:

- Exponential backoff with jitter for retryable failures.
- `Retry-After` handling where the provider or HTTP layer supplies that signal; the chosen sleep duration and header value are recorded in the progress ledger.
- A maximum retry count per call.
- Classification of retryable vs. correctable vs. fatal failures.
- Durable recording of attempted calls in the progress ledger.
- No loss of completed work when a call fails.

Retryable failures include:

- HTTP 408, 425, 429, 500, 502, 503, and 504 for LiteLLM-based backends.
- HTTP 409 for LiteLLM-based backends only when the backend YAML has a provider-specific match rule that classifies the concrete conflict as transient and retryable; otherwise 409 is treated as correctable because generic HTTP conflict semantics can indicate a request/state mismatch.
- Provider outage signals, transient DNS failures, connection resets, and timeouts.
- For CLI agent backends, transient exit codes and stderr signatures classified as retryable in the backend YAML.

HTTP 425 (`Too Early`) is retryable only under the transport-safety rule from RFC 8470: the retry must not resend a non-idempotent model call in TLS early data. Shipped HTTP adapters either declare that their transport stack does not enable TLS 0-RTT/early data for model requests, or they expose a retry path that waits for the full TLS handshake before resending. The adapter fixture set includes a 425 response case for every shipped HTTP backend family that can emit or pass through HTTP status codes, and the fixture asserts the recorded retry metadata names the early-data policy used.

Potentially correctable failures pause the run instead of bricking it:

- invalid or expired API key
- authentication failure
- permission or quota error
- budget or subscription credit exhaustion
- malformed request caused by pipeline bug
- repeated schema-output failure from a model or agent
- repeated retryable failure after backoff is exhausted
- CLI agent backend not on `PATH` or not authenticated
- network-class approval missing for the selected backend or engine
- backend or engine identifier not present in the configured allowlist

Failure handling behavior:

1. Retry transient failures with exponential backoff and jitter.
2. If retries are exhausted or the issue is correctable rather than retryable, mark the current unit `JUDGE_BACKEND_BLOCKED` for judge failures or `ENGINE_BLOCKED` for engine failures.
3. Save current state.
4. Update the local run-status file with component id, model or engine details where relevant, operation, failure class, and suggested fix.
5. Pause the affected unit without discarding completed work.
6. Resume from `noeasymark status` after the issue is corrected.

The process must not discard successful candidates, rendered pages, adjudication records, manual-review files, response cache entries, or completed chapter outputs because of a backend or engine failure.

Anti-spin controls are mandatory. Each control fires independently; the first to fire halts the run, and each halt has its own named category so the operator can see which limit tripped:

- If the same engine fails on the same chunk with the same normalized failure signature 3 times, record a failed `candidate_output` for that engine attempt and continue with other engines.
- If the same backend fails on the same call with the same normalized failure signature 3 times, retire that backend for the current unit (chunk) and either fall back to the configured fallback judge or mark the unit `MANUAL_REVIEW`. Client-side schema validation failures produce the normalized signature `schema_validation_failure:<response_schema_id>:<response_schema_version>`. If the same backend is retired at unit level a second time within the run, it is promoted to `JUDGE_BACKEND_BLOCKED` for the run so the operator can replace it rather than letting the pipeline keep falling back.
- If the same chunk has 3 full attempts without producing a new diverse candidate, reducing review markers, producing a valid manual-review packet, or otherwise advancing schema-valid state, halt that chunk with `NO_PROGRESS_HALT_CHUNK` and mark it `MANUAL_REVIEW`. A **full attempt** is one complete `convert`+`judge` pass for the chunk that runs to terminal status without advancing any schema-valid state for that chunk. Re-runs that follow a corrected manual-review input, a tool-profile change, or a cache invalidation reset the attempt counter.
- If 20 paid or agent-driven model calls occur without any accepted span, reduced review-marker count, newly completed chunk, or newly filed manual-review packet, halt with `NO_PROGRESS_HALT_CALLS`.
- If 90 minutes of active processing elapse without any schema-valid state transition, halt with `NO_PROGRESS_HALT_TIME`. **Active processing** is wall-clock time since the most recent schema-valid state transition, excluding intervals during which the run is paused at a named gate, blocked waiting for operator input or manual-review file edits, sleeping in retry backoff, or otherwise not executing pipeline code. The runtime stamps `active_processing_started_at` and `active_processing_paused_at` markers in `progress.json` so the elapsed value is auditable.
- If 5 consecutive scheduled work units produce no measurable state advancement, halt with `NO_PROGRESS_HALT_UNITS`.
- If normalized failure signatures repeat across components in a way that does not fit the per-engine, per-backend, or per-chunk caps but still indicates cycling through the same root cause, halt with `NO_PROGRESS_HALT_SIGNATURE` and record the signature cluster in the gate record.

Per-run ceilings (optional dollar spend ceiling, invocation/runtime sub-ceilings for subscription, credit-pool, and acknowledged unknown-cost backends, optional wall-clock ceiling, optional artifact byte ceilings, optional minimum-free-space ceiling) live in `run_config.json`. Repository-scope limits live in `pipeline.toml` and are listed in **Default Constants and Thresholds**.

Before every paid, agent-driven, external engine, render batch, OCR batch, or archive-staging operation, the command about to perform the operation (`run`, `pilot`, `judge`, `convert`, `figures`, `preprocess`, or any future direct subcommand) consults the response cache when applicable, verifies the network-class approval and allowlist for the selected component, checks spend/runtime/artifact/free-space ceilings, and only then records the planned operation. On a cache miss or storage-budget check, if a configured hard ceiling would be exceeded, it marks `BUDGET_THRESHOLD_HIT`, posts the tracking update, and exits with a distinct status code.

## Phase 3 - Source Inspection and Preprocessing

The original PDF remains canonical.

Produce derived artifacts, each with hashes and tool versions:

- `.noeasymark-artifacts/<run_id>/original/source.pdf` is always the private canonical working copy produced by `noeasymark init` and is the path every downstream Phase 3 artifact reads from. The runtime also writes the reviewable copy into the deliverable at `<output_root>/<output-folder-name>/original/source.pdf` (Git LFS rules live in the deliverable's `.gitattributes`; the file is page-split above `lfs_max_file_bytes`); this deliverable copy never replaces or moves the private working copy, and the runtime does not commit it.
- `.noeasymark-artifacts/<run_id>/preprocess/normalized.pdf` when canonical normalization is accepted; otherwise `source_manifest.normalization_record` records the skip/failure reason and downstream work reads the private canonical working copy.
- `.noeasymark-artifacts/<run_id>/preprocess/ocr-redo.pdf`
- `.noeasymark-artifacts/<run_id>/preprocess/ocr-force.pdf`
- `.noeasymark-artifacts/<run_id>/renders/page-0001.png`
- `.noeasymark-state/<run_id>/source-manifest.json` (validates against the `source_manifest` schema)
- `.noeasymark-state/<run_id>/outline.json` (validates against the `outline` schema)

The state manifest and outline are mutable working records. Publication steps create public copies under `<output_root>/<output-folder-name>/manifests/` after applying the redaction rules in **Final Output Shape**.

Recommended processing:

1. Inspect original PDF metadata, page count, page labels, rotations, dimensions, outline, embedded images, and existing text layer quality.
2. Normalize a copy using the canonical `qpdf`/`mutool` sequence described below.
3. Produce OCR-derived copies with OCRmyPDF according to the resolved OCR policy: `run_config.ocr_policy_override` when present, otherwise `pipeline.toml`'s `ocr_policy_default`. The default policy is: run `ocrmypdf --mode redo` (the `ocr-redo.pdf` artifact) whenever the per-page `text_layer_quality` label is `poor`, `mixed`, or `unknown`; additionally run `ocrmypdf --mode force` (the `ocr-force.pdf` artifact) for any page where the `ocr-redo.pdf` output still produces `glyph_anomaly_rate` above the `force_ocr_glyph_anomaly_threshold` recorded in `pipeline.toml` (default 0.02). Pages with `text_layer_quality: good` get neither pass; when that good label is `quality_label_stage: provisional`, the source manifest keeps the provisional stage so later full-signal recomputation can route the page back to OCR or mark dependent work stale if the label worsens. Operators who want a uniform policy choose `always_redo`, `always_force`, or `always_redo_then_force` in `pipeline.toml`; `always_redo_then_force` produces both OCR-derived artifacts for every page by running the redo and force passes as separate commands. Generated OCRmyPDF commands use the consolidated `--mode redo` and `--mode force` forms; legacy `--redo-ocr` and `--force-ocr` aliases are accepted only when reading older state, operator notes, or external examples, and new command provenance normalizes them to `mode: redo` or `mode: force`. OCR languages resolve from `run_config.ocr_languages` when present, otherwise from `pipeline.toml`'s `ocr_languages_default` (default `["eng"]`). The resolved ordered language identifiers are validated against `tesseract --list-langs`, joined with `+` only for the OCRmyPDF/Tesseract `-l` argument, and recorded in `source_manifest.ocr_language_config` plus each `ocr_runs[]` record. When the policy targets only a subset of pages, NoEasyMark passes OCRmyPDF a deterministic `--pages` range list expressed in physical 1-based PDF page order (`source_page_index`), never printed page labels. The OCRmyPDF output remains a whole-document derived artifact; NoEasyMark passes `--output-type pdf --optimize 0` unless the active tool profile explicitly opts into PDF/A conversion or OCRmyPDF optimization, because OCRmyPDF may still apply whole-file PDF/A conversion and optimization even when `--pages` limits where OCR is performed. The chosen policy, generated page ranges, OCRmyPDF mode arguments, output-type and optimization arguments, resolved OCR languages, and any tool-profile override are recorded in the source manifest. Before any OCR call, `preprocess` confirms the `ocrmypdf` Python module is importable in the active interpreter, `python -m ocrmypdf --version` or the configured host-native OCRmyPDF executable succeeds, and `doctor --quick` has verified the requested Tesseract language data plus the OCRmyPDF rasterizer/PDF-output dependencies required by the active OCR mode. If that preflight fails, and the resolved policy would have called OCR for at least one page, the run halts with the `OCRMYPDF_NOT_AVAILABLE` halt category, prints the exact install command for the active install profile (`uv sync --extra ocr` is the canonical advance from `ci-development` or `operator-pilot-ready`-without-ocr to the profile that includes OCR), names the missing language packs or OCR dependencies, and offers the alternative remediation of disabling OCR at the field that actually supplied the resolved policy: when `run_config.ocr_policy_override` is set, clear it or set `ocr_policy_override: none` in `run_config.json` (while the override is present, changing `pipeline.toml`'s `ocr_policy_default` has no effect on the resolved policy); otherwise set `ocr_policy_default: none` in `pipeline.toml`. Runs whose resolved policy is `none` or whose pages are uniformly `text_layer_quality: good` do not trip this halt because no OCR call is attempted. A halt under `OCRMYPDF_NOT_AVAILABLE` preserves all completed Phase 3 work (inspection, normalization, render); resuming after the operator installs the extra or changes the policy continues from where the halt fired.
4. Render every page with PyMuPDF using the resolved render-profile alias.
5. Record all artifacts in the source manifest.

The canonical PDF normalization sequence for `.noeasymark-artifacts/<run_id>/preprocess/normalized.pdf` is deterministic and auditable:

1. `qpdf --deterministic-id --object-streams=generate --linearize -- <source_pdf_working_path> <preprocess>/normalized.qpdf-pass1.pdf`
2. `mutool clean -ggg -z <preprocess>/normalized.qpdf-pass1.pdf <preprocess>/normalized.mutool-clean.pdf`
3. `qpdf --deterministic-id --object-streams=generate --linearize -- <preprocess>/normalized.mutool-clean.pdf <preprocess>/normalized.pdf`

Every argv token, tool version, input hash, intermediate hash, final hash, and stderr/stdout excerpt hash is recorded in `source_manifest.normalization_record`; only relative artifact paths are stored. Normalization is accepted only when the final artifact opens successfully, has the same page count and page order, preserves each page's mediabox, cropbox, rotation, and page label, and keeps the per-page extracted-text hash equal after whitespace-only normalization. If any command returns nonzero, the final artifact fails to open, or an invariant check fails, the runtime discards `normalized.pdf`, records `normalization_record.status: skipped_semantic_drift` or `failed_tool_error` with the exact failed invariant, and downstream processing reads the private canonical working copy instead. A skipped normalization is not a source-fidelity waiver; the manifest preserves enough command evidence for a maintainer to reproduce or diagnose the decision.

The default render profile is `default-300dpi`; the phrase "300-400 DPI" means only that operators may choose an explicit named render-profile alias whose `dpi` is at most 400 for higher-detail work. The runtime never auto-promotes a run to 400 DPI from page content heuristics. A 400 DPI alias must be selected through `run_config.render_profile_alias_override` before pilot, or through a post-pilot tool-profile revision, and must pass an artifact-budget preflight before the first page render. The preflight estimates worst-case rendered bytes from page dimensions, render-profile DPI, color space, bit depth, image format, page count, and configured compression, then checks `rendered_page_bytes_ceiling`, `artifact_bytes_ceiling`, and minimum-free-space settings before rendering. If the estimate violates a hard ceiling, rendering halts with `BUDGET_THRESHOLD_HIT`; if only warning thresholds are crossed, the run records a status warning and continues. The resolved render-profile alias, hash, preflight estimate, and observed rendered bytes are recorded in the source manifest.

Existing text-layer quality is measured, not assumed. The source manifest records per-page and aggregate `text_layer_quality` with:

- `text_layer_present`: whether extractable text exists for the page.
- `extraction_method`: tool and version used to inspect the layer.
- `text_layer_density`: extracted characters per rendered page area and per detected text region.
- `glyph_anomaly_rate`: replacement characters, control characters, unexpected private-use code points, mojibake signatures, and impossible whitespace density.
- `visual_text_alignment_score`: sampled alignment between extracted text positions and rendered-page text regions when coordinates are available.
- `ocr_conflict_score`: divergence between the embedded text layer and at least one independent OCR or rendered-page text source when available.
- `quality_label`: `good`, `mixed`, `poor`, or `unknown`.
- `quality_label_stage`: `provisional` when any quality signal has not yet been computed, otherwise `full_signal` after every signal has either produced a value or been explicitly marked unavailable by the full preprocessing pass.
- `missing_quality_signals`: array naming unavailable signals that were treated as `good`, with each entry carrying `reason: not_yet_computed` or `reason: unavailable_after_full_pass`.
- `confidence` and `confidence_method`.
- `signals_consulted`: ordered list naming which of `glyph_anomaly_rate`, `visual_text_alignment_score`, and `ocr_conflict_score` were actually available when the label was computed (later signals are sometimes unavailable on the first preprocessing pass).

The `quality_label` is computed deterministically. If `text_layer_present` is false or `extraction_method` failed, the label is `unknown`, `quality_label_stage` is `full_signal`, and no further band evaluation is performed. Otherwise the label is the worst severity reached across the three signals using the bands defined by the `text_layer_quality_*` constants in **Default Constants and Thresholds**: each signal independently maps to `good`, `mixed`, or `poor`, and the overall label is the worst of the three. Missing signals (because a later phase has not yet computed them) are treated as `good` and therefore cannot worsen the label; every missing signal is listed in `missing_quality_signals`, and `quality_label_stage` is `provisional` until all three signals have been computed or explicitly marked unavailable after the full preprocessing pass. OCR policy, reviewer packets, and downstream consensus rules must preserve the stage so a provisional `good` label is distinguishable from a full-signal `good` label. If a later signal changes a provisional label's severity, the source manifest hash changes and dependent work follows the stale-work-unit path. Operators tuning a single signal change the corresponding `*_good_max`, `*_good_min`, `*_mixed_max`, or `*_mixed_min` value in `pipeline.toml`.

The quality label affects consensus. Pages or regions marked `poor`, `mixed`, or `unknown` may still use embedded text as one evidence source, but two candidates that ultimately derive from that same embedded text layer do not count as materially independent agreement for automatic acceptance.

### Per-Page Quality Signal Algorithms

`glyph_anomaly_rate` is computed as `(anomalous_chars / max(total_extracted_chars, 1))`. A character is anomalous when any of the following holds: it is `U+FFFD` (the Unicode replacement character), it falls in a Unicode private-use range (`U+E000–U+F8FF`, `U+F0000–U+FFFFD`, `U+100000–U+10FFFD`), it is a C0 control code (`U+0000–U+001F`) except `\t` (`U+0009`), `\n` (`U+000A`), `\r` (`U+000D`), or `U+000C` (form-feed), it is a C1 control code (`U+0080–U+009F`), or it appears as part of a known mojibake bigram from a small audited set whose initial value is `["Â", "Ã", "â", "â", "â", "ï¿½"]`. The mojibake set lives in `src/noeasymark/preprocess/mojibake.py` and is unit-tested. The numerator counts each anomalous character once; mojibake matches advance the scan by the length of the match.

`visual_text_alignment_score` is computed by sampling `text_layer_signal_sample_size` (default 20) extracted text spans uniformly without replacement from the per-page extracted text using a deterministic RNG seeded from the first 64 bits of the SHA-256 digest of the UTF-8 string `source_pdf_sha256 + ":" + str(source_page_index)`, interpreted as an unsigned big-endian integer. For each sampled span, the runtime prefers the per-character bbox from the exact extracted span record in `page.get_textpage().extractRAWDICT()`, transforms it to `rendered_pixel_bbox` against the active `render_profile_id`, and computes the intersection-over-union (IoU) of the transformed embedded-text box with independently detected rendered-page text-region boxes. The initial independent detector is Tesseract TSV output from the rendered PNG for the same page and render profile; word-level TSV boxes are merged to line/block regions before IoU is computed. `page.get_textpage().extractBLOCKS()` is not a visual ground-truth source for this signal because it derives from the embedded text layer; it may be recorded only as diagnostic embedded-layer geometry. `pymupdf.Page.search_for()` may be used only as a fallback for the embedded-text bbox when the occurrence can be disambiguated by the sampled block/line/span index and normalized text; ambiguous or missing search results make this signal unavailable for that page rather than allowing the runtime to guess a bbox. Pages with fewer than `text_layer_signal_sample_size` extracted spans use every available span; pages with no extracted text or no independent rendered-page text-region boxes record a `null` score and the `signals_consulted` array omits this signal. The score is in the closed interval `[0.0, 1.0]`.

`ocr_conflict_score` is computed by a hybrid policy. If `ocr-redo.pdf` already exists for this page (a previous `noeasymark preprocess` produced it under the run's `ocr_policy`), the runtime extracts the per-block text from the OCR-redo PDF using the same `extractBLOCKS()` call as the embedded layer, pairs blocks by best-fit `canonical_page_bbox` overlap, and for each paired block computes `normalized_levenshtein = levenshtein(embedded_text, ocr_text) / max(len(embedded_text), len(ocr_text), 1)`. The per-page score is the area-weighted mean of the per-block normalized distances. If `ocr-redo.pdf` does not exist on the first preprocess pass, the runtime runs Tesseract once against a 150 DPI render of the page (`tesseract <render.png> - -l <resolved-language-arg> --psm 6 -c preserve_interword_spaces=1`) and computes the per-page score as a single normalized-Levenshtein distance over the whole-page text without block pairing, then records `signals_consulted` as containing `ocr_conflict_score:first_pass_tesseract` so the operator knows the cheaper variant fired. Once `ocr-redo.pdf` is later produced, the score is recomputed against it and the signal annotation updates to `ocr_conflict_score:ocr_redo`. Pages with no embedded text record a `null` score and the signal is omitted from `signals_consulted`. The score is in the closed interval `[0.0, 1.0]`.

Canonical page and geometry rules:

- `source_page_index` is 1-based and refers to physical PDF page order in the original PDF. Printed page labels are recorded separately as `page_label`. Identifiers, asset filenames, and cross-system references always use `source_page_index`. Body-text cross-references that the source itself states in printed page-label form ("see page 7-12") are preserved verbatim; an evidence sidecar entry records the resolved `source_page_index` alongside the printed label so reviewers can navigate both.
- Every page records mediabox, cropbox, rotation, rendered pixel dimensions, and the `render_profile_id` used for page images and crops.
- Records distinguish three coordinate spaces:
  - `pdf_user_space_bbox`: PDF user-space points against the original page boxes before NoEasyMark's canonical rotation transform, retaining the source PDF coordinate convention for audit.
  - `canonical_page_bbox`: points in the visible reading orientation after applying the recorded mediabox, cropbox, and rotation rules, with origin at the top-left of the visible page. Layout regions and evidence references use this as their primary coordinate system.
  - `rendered_pixel_bbox`: integer pixel coordinates tied to a specific `render_profile_id`, with origin at the top-left of the rendered image.
- Every coordinate transform between these spaces is deterministic and recorded so a crop or evidence reference can be regenerated from the original PDF and render profile.
- Normalized and OCR-derived PDFs are evidence sources, but they never replace the original PDF as the canonical page-order and geometry authority.

The `outline` schema records, per outline node, the depth, title text, source page index, optional page label, parent and child node identifiers, detection source (PDF outline, TOC analysis, heuristic), per-signal score breakdown (`pdf_outline_score`, `typography_score`, `toc_score`, each in `[0.0, 1.0]`), and composite `confidence`. The composite confidence is `(outline_weight_pdf_outline * pdf_outline_score) + (outline_weight_typography * typography_score) + (outline_weight_toc * toc_score)`, where the three weights live in `pipeline.toml` and must sum to exactly 1.0. The `pdf_outline_score` is 1.0 when a PDF outline entry exists and resolves to a valid page, 0.0 otherwise. The `typography_score` is computed by detecting heading-like text spans on the boundary page using PyMuPDF span `size`, `font`, and `flags` fields (a span is heading-like when its average font size exceeds the page's median body font size by at least 25% and either the `TEXT_FONT_BOLD` flag is set or the normalized font name matches the audited bold-style pattern set in `src/noeasymark/preprocess/font_patterns.py`; the score is the fraction of detected boundary headings whose text matches the outline node's `title` after NFC normalization and whitespace collapse, in the closed interval `[0.0, 1.0]`); a page with no detected headings produces a `typography_score` of 0.0. The `toc_score` is 1.0 when a parsed TOC entry exists for the boundary, 0.0 otherwise; TOC parsing is opportunistic and may produce 0.0 for documents without a TOC even when the outline is otherwise sound. The source manifest records `outline_status` as one of:

- `OUTLINE_PRESENT` — recovered from the PDF outline or TOC with high confidence at every level needed for chapter splitting.
- `OUTLINE_PARTIAL` — recovered for some chapters but not all; Phase 4 must fill the gaps from heuristics.
- `OUTLINE_LOW_CONFIDENCE` — recovered fully but at least one chapter boundary has confidence below `outline_low_confidence_threshold` (default 0.85; see **Default Constants and Thresholds**).
- `OUTLINE_MISSING` — no outline recoverable; Phase 4 derives chapter boundaries from TOC analysis and heuristics.

At the end of preprocess, the runtime also validates any operator-supplied `run_config.pilot_pages` array against the now-known page count. Each entry must satisfy `1 <= source_page_index <= page_count`; duplicates are deduplicated with a `status` notice. If any operator-supplied page index falls outside that range, preprocess exits with schema-validation exit code 2 and prints the offending indices, the document's actual page count, and the path to `run_config.json` so the operator can correct the field and re-run. The runtime never silently clips out-of-range pilot pages because the operator may have intended a different document or a different page-labeling convention. Auto-selected pilot pages (those derived from layout analysis when the operator did not supply any) are constructed against the known page count and do not require this check.

Definition of done: source page count is known, `normalization_record` is populated with either an accepted normalized artifact or a skip/failure reason, render-profile selection and artifact-budget preflight are recorded, every page has a render for the resolved `render_profile_id`, derived artifact hashes are recorded, `outline_status` is set to one of the enumerated values, and any operator-supplied `pilot_pages` array has been validated against the page count.

## Phase 4 - Chapter Splitting and Processing Chunks

Separate chapter boundaries from processing chunks. A chapter is a publication and review unit. A chunk is a small processing unit.

If the PDF outline has a clear chapter hierarchy, use it and record the evidence. If not, analyze TOC pages, repeated headers, candidate chapter title pages, typography, page labels, and layout changes. Emit a proposed chapter map. Pause at `CHAPTER_MAP_REVIEW` when `outline_status` is `OUTLINE_PARTIAL`, `OUTLINE_LOW_CONFIDENCE`, or `OUTLINE_MISSING`, or when the operator explicitly requested the gate via `--require-chapter-map-review`. When `outline_status` is `OUTLINE_PRESENT` and the operator did not request the gate, record the proposed chapter map in the chapter manifest and continue without pausing; the chapter review report remains the primary review surface for the resulting splits.

The `CHAPTER_MAP_REVIEW` gate writes a packet under `.noeasymark-state/<run_id>/reviews/chapter-map/<gate_id>.json` plus a rendered Markdown companion. `gate_record.review_packet_paths` points to both files. The packet includes the proposed chapter list, `chapter_id`, slug, source-page range, page labels, detected title, per-boundary evidence refs, confidence scores and signal breakdowns, unresolved low-confidence or missing-outline items, proposed chunk plan per chapter, every forced or tail-handled chunk boundary with scoring details, coverage summary proving every page is assigned once, and exact operator instructions for `noeasymark gate approve <gate_id>` or `noeasymark gate request-changes <gate_id>`. When the operator requests changes, they edit the referenced outline/chapter-map state files or supply a replacement packet path; the next run validates that the edited chapter and chunk maps still cover all pages exactly once before resolving the gate.

Large chapters must still be split into bite-sized processing chunks. Use a target chunk-size band of `processing_chunk_target_pages_min`..`processing_chunk_target_pages_max` (defaults 10..15); the chunker aims at the upper edge and steps down toward the lower edge only when a natural section boundary inside the band would otherwise be split. Use a hard maximum of `processing_chunk_hard_max_pages` (default 25) unless an explicit override is recorded. If no natural section boundary appears before the hard maximum, force a processing split at the least-bad page boundary, preferably one that does not bisect a figure, table, equation, caption, list, or paragraph cluster.

Short and tail chapters have deterministic exceptions to the target band. A chapter whose total length is below `processing_chunk_target_pages_min` becomes one chunk, as long as it does not cross a chapter boundary. A trailing remainder below the minimum is merged backward into the previous chunk when the merged range would stay at or below `processing_chunk_hard_max_pages` and would not require moving the accepted boundary across protected content. If merging would exceed the hard maximum or would bisect a known or provisional figure, table, equation, caption, list, or paragraph cluster, the tail remains a short final chunk and `proposed_chunk_boundaries[].tail_handling` records `short_tail_kept_hard_max` or `short_tail_kept_protected_content`. If the merge succeeds, the boundary record uses `short_tail_merged_backward` and names the prior boundary that was removed. Fixture tests cover chapters below the minimum, exact-minimum chapters, ordinary tails, tails that merge, and tails kept for each exception reason.

"Natural section boundary" is detected by a layered cascade evaluated in this order; the first layer that yields a candidate boundary inside the target band wins, and `chapter_manifest.proposed_chunk_boundaries` records which layer produced each candidate and accepted boundary so reviewers can audit the choice. Within a winning layer, choose the candidate closest to `processing_chunk_target_pages_max` without exceeding it; ties choose the later source page so chunks stay as large as possible inside the band. If all candidates inside the target band would bisect a figure, table, equation, caption, list, or paragraph cluster, choose the highest-confidence non-bisecting candidate from the same layer even if it is farther from the upper edge; if every same-layer candidate bisects protected content, continue to the next layer before falling back to a forced split. Every candidate considered, its score, and the tie-break reason are recorded in `chapter_manifest.proposed_chunk_boundaries`; each resulting `chunk_manifest` stores only the accepted range, overlap metadata, and chunk execution state.

1. PDF outline node at depth 2 or below when the parent chapter has a schema-valid recovered outline subtree and the candidate node's composite confidence is at or above `outline_low_confidence_threshold`. `outline_status: OUTLINE_PARTIAL` may contribute only inside recovered subtrees; it does not authorize inventing outline candidates for missing chapters.
2. TOC-derived heading detected on a TOC page during preprocessing.
3. Typography-detected heading: a Phase 3 text-span or text-block heading candidate whose PyMuPDF span `size`, `font`, `flags`, or OCR/text-block style signal differs materially from body text and whose content matches heading patterns recorded in the chapter manifest. Phase 4 does not require Phase 5 `layout_region` records to exist yet.
4. Projection-profile vertical-whitespace gap larger than `chunk_boundary_min_gap_points` (default 24 points) between two preliminary text-block clusters on the same page, computed from Phase 3 PyMuPDF/OCR text geometry. The accepted boundary is later cross-checked against Phase 5 `layout_region` records during chapter QA.
5. Forced split at the least-bad page boundary (last resort when no earlier layer fires before the hard maximum).

Forced split scoring is deterministic and uses only Phase 3 inputs, plus provisional protected-content cues available before Phase 5/7 records exist. For each candidate page boundary from the target minimum through the hard maximum, compute the following lower-is-better score: `10000 * protected_bisection_flag + 2000 * paragraph_cluster_bisection_flag + 500 * caption_or_list_continuation_flag + 100 * semantic_continuity + 50 * low_whitespace_penalty + 10 * abs(candidate_chunk_pages - processing_chunk_target_pages_max) + tie_break_ordinal`. `protected_bisection_flag` fires for provisional figure, table, equation, or caption cues crossing the boundary; `paragraph_cluster_bisection_flag` fires when Phase 3 text-block geometry or extracted-text continuity shows a paragraph spans the boundary; `caption_or_list_continuation_flag` fires for list markers, caption labels, or obvious continuation punctuation crossing the boundary; `semantic_continuity` is `1.0 - lexical_window_cosine_distance` when that signal is available and `0.5` when unavailable; `low_whitespace_penalty` is `1.0 - min(vertical_whitespace_gap / chunk_boundary_min_gap_points, 1.0)`; and `tie_break_ordinal` is the candidate's source-page order divided by 1000 so identical scores choose the earlier boundary. If every candidate has `protected_bisection_flag: 1`, the lowest score still wins, but the boundary is marked `forced_split_protected_content` and chapter QA must re-check it after Phase 5 layout regions and Phase 7 asset records exist. Each score component is stored in `scoring_breakdown`.

Each chunk record stores its full page range and a separate `overlap_range` identifying any pages shared with an adjacent chunk. At chapter assembly time, overlap pages are emitted only once by deterministic rule: the earlier chunk owns shared pages unless an explicit per-chapter override is recorded in the chapter manifest and copied into the affected `chunk_manifest.overlap_owner` fields. A chapter-QA smoke check compares text across adjacent chunk boundaries and fails the chapter if duplicate content survives assembly. Independently of duplicate detection, the QA step compares the earlier-chunk and later-chunk text on every overlap page and emits an `OVERLAP_DIVERGENCE` warning when any page's normalized similarity falls below the configured threshold so the operator can record a per-chapter override picking the later chunk's text instead. The comparison record stores `per_page_scores` (one row per overlapped `source_page_index` with earlier/later text hashes and similarity), `minimum_similarity`, `pages_below_threshold`, `threshold`, `comparator_id`, `normalization_profile_id`, and the chosen `overlap_owner`; the warning is never based on an aggregate alone. The default similarity metric is `rapidfuzz.distance.Indel.normalized_similarity` computed after the closed `overlap_low_risk_v1` profile; the default threshold value is `0.95`, so the default warning condition is any per-page normalized similarity `< 0.95`.

`pipeline.toml` records overlap comparison under a closed `[overlap_divergence]` object: `comparator_id` (initial value `rapidfuzz_indel_normalized_similarity_v1`), `threshold`, `profile_id`, and `normalization_profile`. The profile object has exactly these fields: `unicode_normalization` (`NFC`), `strip_review_markers` (`true`), `line_breaks` (`space`), `whitespace` (`collapse_unicode_runs_to_ascii_space`), `trim` (`true`), `case_handling` (`preserve`), `protected_zones` (`preserve_verbatim`), and `rapidfuzz_processor` (`none`). Run overrides may lower or raise `threshold` and may select a different registered `profile_id`, but may not provide free-form processor code or unregistered profile fields. Profile fixtures cover each allowed value and prove that source-significant protected-zone text is not silently altered before comparison.

Definition of done: every source page is assigned to exactly one final coverage range, chunk records are schema-valid, no chunk crosses a chapter boundary, no chunk exceeds the hard maximum without an override, and overlap ranges are explicit wherever they exist.

## Phase 5 - Layout Segmentation and Multi-Column Defense

Before running text conversion engines, detect layout regions for every page. Multi-column layout is treated as a high-risk condition, not a formatting detail.

For each page, create `layout_region` records with `region_id`, `source_page_index`, optional `page_label`, `pdf_user_space_bbox`, `canonical_page_bbox`, `rendered_pixel_bbox`, `render_profile_id`, `region_type`, reading-order index, nullable column group, confidence, detection source, per-detector contributions, and parent/child relationships where a region contains captions, figures, table cells, sidebars, or footnotes. Detector-native coordinates and coordinate-origin metadata are recorded in `detector_contributions` and normalized into the three NoEasyMark coordinate spaces before any cache-key, crop, or adjudication reference is written.

Layout detection runs a layered cascade with explicit primary/secondary roles. The first successful layer creates the primary accepted region set for that page:

1. Docling layout boxes are the primary source whenever the Docling engine is activated and ran successfully on the page.
2. Marker layout data is the secondary source, consulted only on pages where Docling did not run successfully or did not return regions.
3. PyMuPDF text blocks are the tertiary source, consulted when neither Docling nor Marker produced regions.
4. Projection-profile or XY-cut heuristics are the quaternary source, consulted for pages where the above all failed.
5. Rendered-page visual analysis (vision-LLM or other visual detector) is the fifth source, consulted when explicitly enabled by the active tool profile and the prior sources all failed.

Visual layout detectors require a closed preflight record before they may inspect rendered source pages. The active tool profile must declare `visual_layout_detector_preflight` with `detector_id`, `network_class`, `input_modalities`, `content_handling_approval_ref` (nullable only for `device`), endpoint or model allowlist identity, local model availability evidence for device detectors, image-retention policy, page/invocation cap, runtime ceiling, spend ceiling, and transcript/persistence declaration. `noeasymark doctor --layout-detectors` smoke-tests only synthetic rendered pages and confirms that no source PDF bytes, rendered crops, candidate text, or document metadata are sent during doctor. A visual detector is dependency-eligible only when its network class is approved, any perimeter or internet endpoint is allowlisted, local model artifacts are present or explicitly prefetched before conversion, image retention is limited to declared local evidence/crop records, and the configured budget ceilings are non-null. If any check fails, the detector is disabled with an actionable `ENGINE_BLOCKED`-style diagnostic rather than silently falling back to an unapproved detector.

The cascade selects the primary accepted region set. Lower-priority detectors may still run in audit mode when `layout_detector_audit` is enabled or when an engine already emitted layout data for the page; audit-mode outputs do not directly replace primary regions. When reconciliation has outputs from more than one detector, the runtime computes pairwise Intersection-over-Union (IoU) after coordinate normalization. Winner-takes-all reconciliation applies only to same or compatible `region_type` candidates. When IoU `>= 0.7`, the higher-priority detector's region wins and the lower-priority detector's contribution is recorded in `detector_contributions` for audit. When IoU `< 0.7`, both regions are retained as distinct layout regions and reviewed at chapter QA for reading-order coherence. When regions overlap because one contains another with a different semantic type, such as a figure caption, table cell, sidebar, or footnote, retain both regions and record the parent/child relationship rather than dropping the contained region.

Audit-mode detector execution is separately bounded. `pipeline.toml` defines a closed `[layout_detector_audit]` object with `enabled`, `sampling_mode` (`off`, `stratified`, `qa_risk_pages`, or `all_until_cap`), `detector_allowlist`, `page_cap_per_chapter`, `runtime_ceiling_seconds`, `spend_ceiling_usd`, and deterministic `sample_seed_policy`. The initial defaults keep audit mode off, cap explicit audit runs at `layout_detector_audit_page_cap_per_chapter`, `layout_detector_audit_runtime_ceiling_seconds`, and `layout_detector_audit_spend_ceiling_usd`, and sample by stable source-page hash when `sampling_mode: stratified`. When a cap is exhausted, remaining audit work is skipped with an audit-budget diagnostic in the chapter QA result; primary layout extraction is not re-run merely to satisfy audit coverage.

Provider layout adapters are table-driven. Phase 0 commits `config/layout_adapters/docling.yaml` and `config/layout_adapters/marker.yaml`, each mapping provider-native labels and geometry fields into NoEasyMark `region_type`, `reading_order_index`, and nullable `column_group`. Each row declares `provider_label`, required native bbox/provenance fields, native coordinate origin, page-number base, confidence source, allowed NoEasyMark `region_type`, grouping rule, and downgrade behavior when required fields are absent. `column_group` may be derived only from stable provider structure plus normalized bounding boxes; no adapter may infer a native `column_group` field merely from the provider family. The fixture suite under `tests/fixtures/layout_adapters/<provider>/` includes at least one valid and one invalid example for every initially supported `region_type`, every grouping rule, missing bbox/provenance, ambiguous reading order, unsupported provider labels, and coordinate-origin conversion. Adapter tests assert exact `layout_region` records, exact downgrade diagnostics, and no source-derived fixture content.

The `render_profile` schema defines the fields whose canonical JSON is hashed to produce `render_profile_hash_full` and its short `render_profile_id` prefix:

- `dpi`: positive integer; default profile uses 300.
- `color_space`: closed enum `srgb`, `gray`, or `cmyk`; default `srgb`.
- `bit_depth`: closed enum integer `8` or `16`; default 8.
- `image_format`: closed enum `png`, `jpeg`, or `webp`; default `png`. The default profile emits PNG, lossless, 8-bit per channel, no alpha, and no embedded color profile.
- `image_compression`: closed enum of strings. For `png`, allowed values are `zlib-level-1` through `zlib-level-9` and the default is `zlib-level-6`; for `jpeg`, allowed values are `jpeg-quality-1` through `jpeg-quality-100`; for `webp`, allowed values are `webp-quality-1` through `webp-quality-100`.
- `mediabox_application`: closed enum `honor` or `ignore`; default `honor`.
- `cropbox_application`: closed enum `honor` or `ignore`; default `honor`.
- `rotation_application`: closed enum `apply` or `ignore`; default `apply`.
- `renderer_id`: closed enum; initial supported value is `pymupdf`.
- `renderer_version`: renderer version string, populated from `pymupdf.__version__` when `renderer_id` is `pymupdf`.

`render_profile_hash_full` is the full SHA-256 hex digest of the canonical-JSON representation of these fields. `render_profile_id` is the 12-character lowercase hexadecimal prefix of that digest. The schema also enforces cross-field compatibility: `color_space: cmyk` is valid only with `image_format: jpeg`; PNG and WebP profiles are restricted to `srgb` or `gray`. Render-profile aliases live under `config/render_profiles/<name>.yaml`. Changing a profile alias counts as a tool-profile change and triggers the mini-pilot rule.

Pages suspected of multi-column layout must be processed region-by-region in addition to any full-page engine output. For OCR, crop each column or region and OCR it separately. For judge calls, pass the relevant crop rather than the whole page when resolving a disputed span. Full-page output is never trusted when it conflicts with region-by-region reading order.

Add multi-column QA checks. Each check uses NoEasyMark's normalized `layout_region.column_group` field when it has been derived from detector evidence. Docling and Marker outputs are mapped into `column_group` only when their provider-specific structure and bounding boxes support a stable grouping; adapters must not assume that either provider emits a native `column_group` field. When no detector-derived grouping exists, the heuristic for that check is evaluated against the configured thresholds:

- Detect alternating left-column/right-column text interleaving when the per-page `column_group` distribution shows two or more groups intermixed in reading-order.
- Detect lines whose x-coordinates jump between columns while y-coordinates remain in the same band: heuristic threshold is a jump greater than `multi_column_x_jump_fraction` of page width (default 0.4) within a y-band narrower than `multi_column_y_band_points` (default 6.0).
- Compare full-page extraction against region-by-region extraction. The check emits a `full_vs_region_extraction_comparison` record per multi-column-risk page with `source_page_index`, full-page candidate refs, ordered region candidate refs, per-region normalized text hashes, per-region `rapidfuzz_indel_normalized_similarity_v1` scores, missing-span and extra-span summaries, reading-order inversion pairs, `minimum_region_similarity`, `regions_below_threshold`, `full_vs_region_alignment_min`, and status (`pass`, `review`, or `fail`). Similarity is computed after the same low-risk profile family used for overlap comparison, but region boundaries and protected zones are preserved as separate rows rather than flattened away. The check marks review when any region is below `full_vs_region_alignment_min` or when any reading-order inversion crosses column groups; it fails when the region-by-region output drops a protected block, table, figure caption, equation, or code block that exists in the full-page output.
- Flag abrupt semantic discontinuities near column boundaries: compute `lexical_window_cosine_distance` at each candidate column boundary. The default `multi_column_lexical_window_profile: ascii_alnum_v1` algorithm is: (1) take the `multi_column_cosine_window_chars` characters (default 50) immediately preceding the boundary in reading order, call it `L`; take the same number immediately following the boundary, call it `R`; (2) low-risk normalization on each window — NFC-normalize, lowercase using the locale-independent ASCII fold (Python `str.lower()` on an NFC string is locale-independent for ASCII letters; full Unicode case-folding is not used because it can change character counts), and strip every character that is not an ASCII letter `a–z` or a digit `0–9` (collapsed whitespace is removed in this strip); (3) extract every contiguous `multi_column_cosine_ngram_size` (default 3) character n-gram from each normalized window into a term-frequency vector with integer counts; (4) compute `cosine_similarity = dot(L_tf, R_tf) / (norm(L_tf) * norm(R_tf))` using L2 norms, treating either window with zero n-grams as `cosine_similarity = 1.0` (no signal); (5) return `lexical_window_cosine_distance = 1.0 - cosine_similarity` in the closed interval `[0.0, 1.0]`. The heuristic fires when the distance is greater than `multi_column_semantic_discontinuity_threshold` (default 0.5). The implementation lives in `src/noeasymark/segment/cosine.py`, is pure Python (no third-party dependency), and is unit-tested with golden fixtures for short headings, equation-like windows, punctuation-only windows, numeric-only fragments shorter than the n-gram size, and empty normalized windows. Those fixtures assert the zero-n-gram path returns `cosine_similarity = 1.0`, `lexical_window_cosine_distance = 0.0`, and a recorded `no_signal_zero_ngram` reason rather than a false discontinuity. This metric is implemented in NoEasyMark rather than delegated to `rapidfuzz`, because `rapidfuzz` supplies the overlap indel comparator but not a project-defined semantic cosine primitive.

Script-aware lexical-window profiles are opt-in and closed. A profile record declares `profile_id`, `unicode_normalization`, `case_strategy` (`ascii_lower`, `unicode_casefold`, or `none`), `character_class_policy` (`ascii_alnum`, `unicode_letter_digit`, or a named script allowlist), `ngram_unit` (`codepoint` only in the initial release), `offset_policy` (`preserve_codepoint_offsets` or `record_normalized_to_source_map`), and `cache_key_impact` (`layout_profile_hash`). The default `ascii_alnum_v1` remains the only profile selected by shipped config. Enabling a script-aware profile changes the layout-profile hash recorded in `layout_region.detector_contributions`, source-manifest QA records, and downstream cache evidence, so existing dependent work becomes stale rather than silently reusing ASCII-window decisions. Fixture tests cover Latin, non-Latin letter/digit windows, math-heavy zero-letter windows, mixed-script windows, and offset-map round trips.

- Fail or mark review when reading order cannot be established.

Heuristic thresholds live in `pipeline.toml` under `multi_column_detection` and may be overridden per run in `run_config.json`.

Definition of done: every page has at least one layout-region record, multi-column pages have explicit reading order, every region has reproducible crop coordinates, and any page with unresolved reading-order risk is marked for review before adjudication.

## Phase 6 - Engine Ensemble

Engines are organized into families. Diversity is enforced across families, not across instances of the same family. Each engine YAML declares its `family`; engines that delegate to another service inherit the delegated engine's family for diversity counting.

| Family identifier | Examples |
| --- | --- |
| `layout_ml` | Marker default, Marker forced OCR, Docling default |
| `layout_heuristic` | PyMuPDF4LLM, PyMuPDF/pdfplumber baseline, MarkItDown using only its built-in PDF parsing |
| `raw_ocr` | OCRmyPDF/Tesseract sidecar text, region-cropped Tesseract |
| `document_ai` | Mistral OCR, Azure Document Intelligence, Azure Content Understanding, MarkItDown-with-AzDI when configured to delegate |
| `vision_llm` | Vision-LLM-as-engine or OCR models selected during pilot, whether local GPU, perimeter-hosted, or internet-hosted; the component's `network_class` records where content moves |

Initial engine set: Marker default, Marker forced OCR, Docling default, PyMuPDF4LLM or PyMuPDF-based baseline, OCRmyPDF/Tesseract sidecar text, and region-cropped OCR for multi-column pages.

### Shipped Engine Activation Matrix

The following table is the single reference for "what does an unmodified first pilot pick up automatically, and what requires operator action first." It is a normative restatement of the **Engine activation policy** closed handoff decision plus the per-engine family/network-class declarations below. Phase 0 generates or validates the shipped per-engine YAML defaults against this table; any disagreement is a schema/configuration failure that must be fixed before the first pilot, not a precedence question.

| Shipped engine YAML | Family | Network class | In Initial set? | Activates automatically at first pilot when... | Operator action required before first activation |
| --- | --- | --- | --- | --- | --- |
| `marker.yaml` | `layout_ml` | `device` | yes | `marker-pdf` and its dependencies install successfully, LLM/hybrid enhancement is disabled, and the run's content handling permits device-class engines (always true) | None |
| `marker-forced-ocr.yaml` | `layout_ml` | `device` | yes | same as `marker.yaml`, plus Tesseract is on `PATH` and `ocr-force.pdf` was produced for the chunk | None |
| `docling.yaml` | `layout_ml` | `device` | yes | `docling` and its model weights are available; `requires_gpu` evaluates per the host | None for CPU; CUDA pass-through for GPU |
| `pymupdf4llm.yaml` | `layout_heuristic` | `device` | yes | `pymupdf` and `pymupdf4llm` import, and `doctor` verifies the OCR-disabled call path | None |
| `tesseract.yaml` | `raw_ocr` | `device` | yes | `tesseract` binary on `PATH`, English language pack present | None |
| `tesseract-region-crop.yaml` | `raw_ocr` | `device` | yes | same as `tesseract.yaml`, plus layout regions exist for the chunk and at least one page is multi-column or has uncertain reading order | None |
| `markitdown.yaml` | `layout_heuristic` | `device` | no | n/a | Operator flips `enabled: true` in the YAML before the first pilot |
| `markitdown-with-azure-di.yaml` | inherits `document_ai` via `delegates_to` | inherits `perimeter` or `internet` from the delegated AzDI engine | no | n/a | Operator flips `enabled: true` AND records the matching perimeter or internet approval AND adds the delegated AzDI endpoint to the relevant allowlist |
| `azure-document-intelligence.yaml` | `document_ai` | `perimeter` (private VNet) or `internet` (public endpoint) | no | n/a | Operator flips `enabled: true` AND records the matching approval AND lists the endpoint identifier in the allowlist |
| `mistral-ocr.yaml` | `document_ai` | `internet` | no | n/a | Operator flips `enabled: true` AND records the internet-processing approval AND lists the Mistral endpoint in the allowlist |

The matrix only enumerates the shipped engine YAMLs. Operator-added engines follow the same rules: Initial-set membership cannot be retroactively conferred; non-initial engines always require the explicit YAML enablement step before doctor will smoke-call them, regardless of family or network class.

Every shipped device-class engine is invoked only with NoEasyMark-owned local paths or streams under the active run directory. Vendor features that fetch source URLs, enable ambient plugins, call hosted LLM/image-description services, or otherwise move source-derived content outside the declared `network_class` are disabled in the shipped YAML. Re-enabling any such feature requires a separate engine YAML with explicit `network_class`, endpoint identifiers, credentials, cost tracking, lineage, and approval requirements.

Every shipped engine YAML includes an `adapter_smoke_contract` block so implementation tests assert the exact invocation shape rather than only imports. The contract declares the synthetic fixture input, expected command arguments or API keyword arguments, disabled remote or LLM features, expected output files, expected sidecar schema ids, expected lineage fields, and at least one failed-candidate envelope fixture. Adapter smoke tests run against source-free fixtures under `tests/fixtures/engine_adapters/<engine_id>/` with subprocess/API call capture. They assert, at minimum: Marker default omits LLM and hybrid options; Marker forced OCR uses the Phase 3 `ocr-force.pdf` artifact rather than Marker's own force-OCR path; PyMuPDF4LLM uses the installed-version OCR-disabled call path; MarkItDown's local variant does not delegate to remote services; delegated MarkItDown variants inherit the delegated engine family and network class; Tesseract engines emit the configured text/TSV or hOCR artifacts; cloud/document-AI wrappers require approval and endpoint allowlist evidence; and every adapter produces either a schema-valid candidate sidecar or a schema-valid failed `candidate_output` record with a linked failure event.

Marker ships as two distinct engine YAML files so OCR lineage is explicit. `config/engines/marker.yaml` reads the normalized PDF, disables LLM/hybrid enhancement, and does not pass `--use_llm` or configure an `llm_service`; any future Marker-with-LLM engine is a separate engine YAML whose effective `family`, `network_class`, credentials, endpoint identifiers, and approvals match the configured LLM service. `config/engines/marker-forced-ocr.yaml` uses the OCR-forced artifact (`ocr-force.pdf`) produced by Phase 3, declares `primary_text_source: ocr_force_pdf`, and records the OCR-derived artifact hash in candidate lineage. The shipped forced-OCR Marker file does not invoke Marker's own `--force_ocr` path; if a later pilot chooses that path, it is added as a separate engine YAML that records the Marker OCR model/toolchain, source artifact hashes, and `primary_text_source` as `rendered_page_ocr` or `mixed` according to the observed adapter output. Both shipped Marker files declare `family: layout_ml`, but they are separate `engine_id`s and must not be counted as two independent families for the diversity rule.

`config/engines/docling.yaml` records a closed `model_cache` and accelerator-doctor contract. The `model_cache` object declares `artifacts_path_source` (`explicit_path`, `DOCLING_ARTIFACTS_PATH`, or `default_user_cache`), nullable `artifacts_path`, `prefetch_command` (`docling-tools models download` or an approved equivalent), `offline_required`, `models_present`, and `missing_model_policy` (`recoverable_dependency_gap` unless an offline-required run lacks the models, then `engine_blocked`). No conversion run may rely on Docling's first-use model download; `noeasymark doctor --engines` either verifies the cache before pilot, records the prefetch action the operator must run, or marks Docling dependency-ineligible with an actionable diagnostic. The `accelerator` object declares requested device (`auto`, `cpu`, `cuda`, `mps`, or `xpu`), `requires_gpu`, observed CPU thread count, observed accelerator availability, resolved device, Docling version, and the installed PyTorch/accelerator evidence when available. CPU fallback is allowed and recorded when requested device is `auto` and `requires_gpu: false`; a missing requested GPU or incompatible CUDA/MPS/XPU stack is a correctable doctor finding, and a selected `requires_gpu: true` engine cannot activate until the matching accelerator check passes. The shipped Docling engine keeps remote services disabled; any Docling VLM or hosted-service variant must be a separate engine YAML with its own network class and approval requirements.

`config/engines/pymupdf4llm.yaml` is the shipped layout-heuristic baseline. PyMuPDF4LLM can automatically trigger OCR for image-only pages, so the shipped baseline must call the API with OCR disabled (`use_ocr: false`, or the installed-version equivalent verified by `doctor`) and must fail dependency eligibility if the installed version cannot reliably disable OCR. A future OCR-enabled PyMuPDF4LLM variant must be a separate engine YAML that declares the OCR dependency, records the OCR toolchain in `candidate_lineage`, and uses `primary_text_source: rendered_page_ocr` or `mixed` as appropriate.

Tesseract also ships as two distinct engine YAML files so full-page OCR and region-cropped OCR have separate lineage. `config/engines/tesseract.yaml` OCRs the page-level render or OCR sidecar path. `config/engines/tesseract-region-crop.yaml` uses `text_source_policy: layout_region_crop_ocr`, OCRs each layout region or column crop independently, records the source `layout_region` ids in candidate lineage, and declares the same `raw_ocr` family as `tesseract.yaml`. The region-crop YAML records a closed `tesseract_region_crop_profile` with `languages` (default `ocr_languages_default`, joined as `LANG[+LANG]` only for the CLI call), optional `script_packs`, `tessdata_dir` (nullable), `psm` (default `tesseract_region_crop_psm`), `oem` (default `tesseract_region_crop_oem`), `preserve_interword_spaces`, required output formats (`txt` plus `tsv`), optional `hocr_debug`, confidence source (`tsv_word_confidence`), and crop-to-span mapping (`word_boxes_to_span_offsets_v1`). `doctor --engines` validates every requested language or script against `tesseract --list-langs`; missing packs make the region-crop engine dependency-ineligible rather than falling back to English. The adapter writes text from the `txt` output, extracts word boxes and confidence from TSV, emits hOCR only when the profile enables debug evidence, and computes `candidate_lineage.lineage_confidence` as a glyph-count-weighted mean of TSV word confidences normalized to `[0.0, 1.0]`. Spans below `tesseract_region_crop_confidence_review_min` carry a low-confidence review flag with `lineage_confidence_method: tesseract_region_tsv_weighted_word_confidence_v1`. The two Tesseract engines may count as distinct configured engines only for the relaxed restricted-diversity rule; they never count as two independent families.

Engine resource budgets are part of engine activation, not an implementation tuning afterthought. Every engine YAML's `resource_budget` is resolved before pilot into `tool_profile.active_engines[].resource_budget_resolved`, using `engine_default_max_workers` and `engine_default_cpu_threads` only when the YAML does not specify a narrower cap. The scheduler refuses to launch more concurrent work for an engine than `max_workers`, `max_cpu_threads`, `scheduler_concurrency`, GPU requirement, and GPU memory hints allow; it also applies engine-declared environment limits such as thread-count variables when invoking local CLIs. Marker worker counts, Docling accelerator/device selection, Tesseract process fan-out, and hosted document-service concurrency all flow through this same object. Phase 2 spend/runtime/artifact ceilings remain authoritative: a resource budget can lower concurrency to protect the host, but it cannot raise or bypass any configured run ceiling.

Include MarkItDown in the pilot as a candidate or utility engine, not as the default high-fidelity converter. Its built-in PDF parsing is installed as `markitdown.yaml` (`family: layout_heuristic`). Variants that delegate to an OCR plugin, Azure Document Intelligence, Azure Content Understanding, or an LLM-vision service are installed as separate engine files (for example `markitdown-with-azure-di.yaml`) that declare the delegated service in `delegates_to` and inherit that target's effective family for diversity counting. Azure Document Intelligence and Azure Content Understanding wrappers therefore count as `document_ai`; an LLM-vision OCR plugin counts as `vision_llm` unless a future non-LLM OCR plugin declares a different reviewed family.

Optional engines such as Nougat, MinerU, Mistral OCR, Azure Document Intelligence, Azure Content Understanding, Surya, or specialized math OCR are added only when pilot metrics show a specific gap and the engine's effective network class is approved by the current content-handling choices.

Vision-LLMs are reserved for adjudication and are not enabled as text-conversion engines by default. This avoids having the same model class vote and then judge its own vote. This restriction does not prohibit a Phase 5 visual layout detector from producing `layout_region` evidence, because that detector does not emit candidate text or vote in adjudication; it still needs the separate detector preflight, content-handling approval, and network-class controls described in Phase 5. Enabling a vision-LLM-as-text-engine requires an explicit pilot finding and maintainer approval; the engine, when added, is classified under the `vision_llm` family. Its `network_class` is still `device`, `perimeter`, or `internet` according to where source-derived content is processed.

Every engine run must produce either a successful candidate Markdown plus sidecar JSON or a failed `candidate_output` record with error, timeout, logs, retry metadata, and a `failure_event_id`. Sidecar JSON includes per-span `span_id` and `region_refs` values linking text spans back to the `layout_region` records they were derived from.

Each successful candidate also records `candidate_lineage` at both candidate and span scope. Candidate-scope lineage records name `candidate_id`; span-scope lineage records name both `candidate_id` and `span_id`. The lineage record includes:

- `primary_text_source`: one of `embedded_text_layer`, `ocr_redo_pdf`, `ocr_force_pdf`, `rendered_page_ocr`, `layout_region_crop_ocr`, `document_ai_ocr`, `local_vlm_inference`, `manual_review`, or `mixed`.
- `source_artifact_refs`: source PDF, OCR-derived PDF, rendered page, crop, service response, or manual review artifact ids used to produce the span.
- `source_artifact_hashes`: content hashes for those artifacts where available.
- `intermediate_toolchain`: ordered tool ids and versions that transformed the source into the candidate span.
- `embedded_text_layer_quality_ref`: page or region quality record id when the source lineage uses the embedded text layer directly or indirectly.
- `delegated_service_id`: service identity when an engine wrapper delegates OCR or layout recognition to another system.
- `lineage_confidence` and `lineage_confidence_method`.

Two spans are materially independent for auto-acceptance only when their family diversity and lineage diversity both hold. They are not materially independent if they share the same poor, mixed, or unknown embedded text layer as their primary source, share the same OCR-derived PDF without an independent rendered-page or service check, or differ only by wrapper around the same delegated service output. They may be considered materially independent when one span derives from a rendered-page or crop OCR pass and another derives from a separate layout/document-AI service, or when a high-quality embedded text layer agrees with an independent visual/OCR lineage.

Adjudication requires diverse evidence, not merely candidate count. The default diversity rule requires at least one successful candidate from at least two different families. If no successful candidate exists at all for a chunk, mark the chunk `ENGINES_EXHAUSTED` and route it to `MANUAL_REVIEW` with the per-engine failed `candidate_output` records and their linked `normalized_failure_event` records attached.

If content-handling restrictions or local engine availability make it impossible to assemble two families, the chunk is marked `MANUAL_REVIEW` unless the operator has recorded `restricted_diversity_acknowledgment: true` in `run_config.json`. When the acknowledgment is present, the diversity rule relaxes to at least two successful candidates from at least two distinct configured engines; every span accepted under the relaxed rule carries a `RESTRICTED_DIVERSITY` review flag in its evidence sidecar and is enumerated in the chapter report.

Restricted-diversity reporting includes the evidence matrix behind the relaxed decision, not only a count. For each restricted acceptance, the evidence sidecar and chapter report record `restricted_diversity_matrix` with `span_id`, accepted-text hash, approving acknowledgment id, successful candidate ids, engine ids, configured engine families, primary text sources, lineage confidence methods, families represented, families unavailable, unavailable-family reason codes (`not_installed`, `blocked_by_content_handling`, `missing_credentials`, `dependency_ineligible`, `engine_failed`, `disabled_by_tool_profile`, or `not_applicable_to_page`), and any candidate conflicts that were manually or automatically rejected. Public report rendering redacts private paths, credential variable names, raw OCR text excerpts, and raw provider errors while preserving ids, hashes, reason codes, and counts so reviewers can judge the relaxed evidence without exposing local details.

## Phase 7 - Image, Caption, Table, and Equation Extraction

Classify visual/layout blocks before processing:

- Pure illustration: crop image and pair with caption.
- Technical diagram: crop the diagram tightly and preserve it as an image; do not transcribe interior diagram text by default.
- Caption: keep as text outside the image crop unless the caption is physically embedded inside the diagram.
- Table: preserve structure as Markdown or sanitized inert HTML when feasible; otherwise crop and flag for review.
- Display equation: transcribe separately, preferably as LaTeX, with image crop available for verification.
- Inline math or symbols: preserve visible notation, including Greek symbols such as `Δ` when the source uses the literal symbol.

Use rendered page crops as visual ground truth. Final reviewable image assets are stored deterministically as PNG with `asset_image_format: png` (the same field name used by the `figure_record`, `table_record`, and `equation_record` schema field sets); this deliverable asset format is separate from `crop_record.image_format`, which records the active render profile's evidence-crop format. If the active render profile uses CMYK, JPEG, WebP, or any other non-PNG-compatible crop surface, the final PNG asset is derived from the crop through an explicit sRGB or grayscale conversion recorded in the asset record. Image asset filenames use the pattern `images/<chapter_id>-page-<PPPP>-<kind>-<II>[-<hash>].png` where `<chapter_id>` is the `ch<NN>` form defined in **Identifier Conventions**, `<PPPP>` is the zero-padded `source_page_index` of the asset's primary page using width `max(4, number_of_digits(page_count))`, computed during preprocess, `<kind>` is `fig`, `tbl`, `eq`, or `dia`, `<II>` is a zero-padded two-digit per-page index, and the hash suffix appears only when needed to resolve a deterministic collision. When a visual block spans multiple pages, the filename uses the first page in the span and the asset record's `source_page_indices` array enumerates every page covered; linked crop records are written for every page in the span, `asset_paths` preserves page order unless an explicit reviewed composite image is created, and the chapter report lists multi-page assets so reviewers can spot reading-order regressions.

Table structure representation is selected by the closed `table_representation_rubric_v1`. Emit `markdown` only when the accepted table evidence is a rectangular row/column grid, `row_span_count == 0`, `column_span_count == 0`, `missing_cell_count == 0`, no nested block content is present, the table is not rotated or oblique, cell reading order is row-major, and confidence is at least `table_structure_confidence_min`. Emit `sanitized_html` when the table is structurally grid-like but needs row spans, column spans, row/column headers, footnotes, or other table-structure elements that GFM Markdown cannot represent, provided the sanitizer succeeds and confidence is at least `table_structure_confidence_min`. Emit `image_only` with a review flag when the table is visually identifiable but structure evidence is below the confidence threshold, rotated/oblique, sparse, or too ambiguous for safe text structure; the crop remains the fidelity source. Emit `manual_review` when adapters materially disagree about table boundaries, protected content would be lost, sanitizer output is unsafe or schema-invalid, or the table spans pages without enough evidence to join or split it deterministically. The table record stores every rubric input, the selected representation, and machine-readable reason codes, and fixtures cover row/column spans, nested content, rotated tables, missing cells, low confidence, sanitizer rejection, image-only fallback, and adapter disagreement.

Caption pairing uses `caption_pairing_v1`. Candidate caption regions are limited to `caption` layout regions, caption-like prose spans with a figure/table label token, or provider-native caption fields mapped through a layout adapter. A caption is `embedded_in_image` only when detector evidence places the caption text inside the visual block bbox or the provider marks it as an embedded child; embedded captions remain inside the crop and are not duplicated as separate prose unless a reviewer extracts them. A caption is `paired_text` when exactly one candidate is reading-order adjacent to the visual block, has compatible label text (`figure`, `fig.`, `table`, or `tbl.` plus matching ordinal when present), horizontally overlaps the block or aligns to its center band, is within `caption_pairing_max_vertical_gap_points`, and the combined geometry, label, reading-order, parent/child, and confidence score is at least `caption_pairing_min_confidence`. A caption is `missing` when no candidate exists in the search band and no embedded caption evidence exists. A caption is `uncertain` when multiple candidates score within 0.05 of the winner, label ordinals disagree, the candidate crosses a page/chunk boundary, the provider excludes captions from figure/table bounding regions without a stable sibling relation, or confidence falls below threshold. Uncertain captions create a review packet with the visual crop, candidate caption crops/text hashes, scores, and reason codes.

Equation transcription uses `equation_transcription_v1`. LaTeX is accepted only when the transcription parses through the configured math parser where available, preserves protected-zone status, has source crop evidence, and visible-symbol coverage is at least `equation_symbol_coverage_min`; parser diagnostics, normalized token classes, and coverage score are recorded. Verbatim is used when the visible source notation is plain enough to preserve literally but LaTeX would be speculative. `image_only` is used when the crop is clear but transcription evidence is insufficient. `manual_review` is required when independent candidates disagree on numbers, variables, signs, Greek symbols, subscripts/superscripts, equation labels, or relation operators, when parseability fails for a proposed LaTeX transcription, or when symbol coverage is below threshold. Equation records are always protected zones; deterministic normalization may not rewrite equation text except for explicitly recorded Markdown escaping.

Multi-page visual stitching uses `visual_stitching_v1`. Automatic logical stitching is allowed only for adjacent source pages with gap `<= visual_stitching_max_page_gap`, compatible visual type, stable label or table identifier when present, aligned crop geometry, and continuity evidence such as repeated table headers, row-number continuation, figure-panel labels, or explicit "continued" markers. A table may stitch into one logical `table_record` with ordered `asset_paths` when row/column structure and captions remain coherent; otherwise each page stays a separate record linked by `related_visual_ids`. A composite image is never created automatically: it requires reviewer approval recorded in the table or figure record, and the original per-page assets remain in `asset_paths` so the composite is additive evidence, not a replacement. Any stitching candidate that crosses a chapter boundary, conflicts with chunk ownership, lacks repeated-header/label evidence, or fails geometry continuity is kept as separate page assets with a review flag.

Visual asset QA runs after final PNG asset creation. For every figure, table, and equation asset, the runtime verifies that the file decodes, dimensions match the crop-record expectation after format conversion, the final asset bbox has IoU at least `visual_asset_crop_iou_min` with the source crop geometry, blank-border ratios stay below `visual_asset_blank_border_warn_ratio` unless the source crop itself has the same border, and optional perceptual hashes match the rendered source crop within the approved tolerance when that checker is enabled. File existence and SHA-256 checks are mandatory; perceptual hashes are advisory unless a future tool profile makes them blocking. QA fails when an asset is missing, undecodable, outside the deliverable root, or geometrically mismatched; it warns when excess border, neighboring prose, clipped content, or perceptual drift suggests the crop should be reviewed.

The asset-naming pattern is enforced as unique within a chapter; collisions are resolved by deterministically appending `-<collision suffix>`, where the suffix starts as the first 6 lowercase hexadecimal characters of the SHA-256 of the asset's primary `region_id` and is extended by two hexadecimal characters at a time until the filename is unique, up to the full 64-character digest if needed. The chosen suffix and digest length are recorded in the asset record so the name can be reproduced exactly.

Cropping policy: crop the minimum rectangle that contains the actual figure, diagram, table, or equation plus configured padding from `figure_crop_padding_points` and `figure_crop_padding_pixels`. The runtime applies whichever padding produces the larger absolute box at the active `render_profile_id`, so operators can tune the per-unit values independently. The padded rectangle is clamped to the page cropbox after coordinate normalization; the asset record stores requested padding, applied padding, any clipped edges, and whether page-boundary clipping occurred. Avoid including surrounding prose, captions, headings, or neighboring figures unless they are visually part of the same object. If the object crosses a page boundary, chunk boundary, or uncertain region boundary, create linked crop records and flag the crop for review. If the crop boundary is uncertain, record the uncertainty and flag the crop for review.

Definition of done: every visual block has a type, source page or pages, bounding box, asset path or paths when applicable, caption status, crop-padding metadata, and confidence.

## Phase 8 - Adjudication

Adjudication happens only through `noeasymark judge`, never as undocumented chat reasoning.

A **span** is a contiguous text region from a single candidate output, identified by its `span_id` and backed by its parent `candidate_id` plus an offset range within that candidate. Spans are the unit that adjudication compares, accepts, disputes, and records review markers against. Every span has at least one `region_refs` entry pointing at the `layout_region` records it derives from.

Judge roles are abstract; their concrete backend bindings live in `.noeasymark-state/<run_id>/tool_profile.json` and are reviewed at `PILOT_GLOSSARY_REVIEW`. The roles are:

- `default_judge` - handles normal disputed spans at the configured default effort or thinking-budget parameter for difficult prose.
- `escalation_judge` - reserved for low-confidence, high-impact, repeatedly disputed, or final-blocking spans; runs at the highest effort or thinking-budget setting available on the bound backend.
- `fallback_judge` - capability-compatible alternate used when the default judge backend is unavailable, rate-limited, or fails schema output repeatedly. Prefer a different failure domain from the default judge when practical.
- `triage_model` - cheap backend used only for low-risk classification, routing, and review-marker categorization.
- `independent_auditor` - different-vendor backend used for pilot spot-checks and regression sampling.
- `alternate_auditor` - optional second auditor slot used only when pilot evidence shows that a second independent failure domain materially improves regression detection. It uses the same v0001 body as `independent_auditor`, committed under its own prompt id.

The initial tool profile binds three roles: `default_judge`, `fallback_judge`, and a single `independent_auditor`. The `escalation_judge`, `triage_model`, and `alternate_auditor` roles are reserved in the schema and enabled only when pilot metrics show they earn the additional surface area. If the operator's installed backend set cannot satisfy the independence rationale for `independent_auditor` (for example, only one vendor is configured), `noeasymark doctor --backends` records the gap, the operator must either install an additional backend or record an `independent_auditor_waiver` record in the tool profile, and the pilot report flags the lower-confidence auditor binding so reviewers can weigh the result. The `independent_auditor_waiver` record carries `waiver_id`, `rationale`, `approver`, `approved_at`, `expires_at` (optional), and `revisit_at` (optional); the pilot report, the tool-profile public snapshot, and the local run-status file all surface the waiver verbatim.

Backend binding is operator-driven and is not constrained to a single provider family by this specification. A common configuration when a CLI agent is the most attractive option is to bind `default_judge` to a `cli_agent` backend such as Codex and bind `fallback_judge` and `independent_auditor` to LiteLLM-based backends; another common configuration uses LiteLLM-based backends throughout. `noeasymark doctor --backends` verifies that whatever bindings the operator selected actually work end-to-end before scale begins, confirms that the network class of each bound backend matches the operator's approvals, and records the failure-domain and independence rationale for the role.

Every adjudication record stores the role used, backend id, backend config hash, exact model id and model snapshot or version where available, prompt id and version from `config/prompts/`, configured model parameters including the provider-native effort or thinking-budget setting when present, response schema id and version, backend-emitted schema hash, rendered prompt hash, evidence content hash, the full provenance bundle, inputs, confidence, and decision.

Two-tier process:

1. Non-LLM pre-diff aligns candidates with `rapidfuzz.distance.Indel.opcodes()` after candidate outputs have been segmented into `span_id` units. Similarity scoring uses `rapidfuzz.distance.Indel.normalized_similarity` on the post-normalization text for the aligned span pair; the alignment opcode trace and similarity value are recorded in the adjudication evidence so a later implementation does not silently swap in a different comparator.
2. Consensus spans are accepted only when at least two successful candidates from different engine families agree, the agreed span maps to compatible `layout_region` references, the agreeing spans have materially independent `candidate_lineage`, and no successful independent family or lineage presents a material conflict. Agreement is evaluated in layers and the evidence sidecar records which layer accepted each span:
   - **byte-exact**: candidates match byte-for-byte after low-risk normalization; accepted as `consensus_byte_exact`.
   - **normalized-fuzzy**: non-protected spans whose normalized text is at least `fuzzy_consensus_min_chars` characters long and contains at least one Unicode letter or decimal digit match with `rapidfuzz.distance.Indel.normalized_similarity >= 0.98` after low-risk normalization; accepted as `consensus_with_normalization` and flagged for sampling in chapter QA. Shorter spans, protected spans, labels, headings, captions, equation/code-like text, and identifier-like spans require byte-exact consensus or route to judge/manual review.
   - **below threshold**: candidates differ above the fuzzy threshold; span goes to judge or manual review.

   Material-conflict blocking uses `material_conflict_rubric_v1` before any consensus span is accepted. Every successful candidate from an independent family or materially independent lineage that overlaps the proposed accepted span is compared against the consensus text and classified by span class. Protected classes (numbers, units, signs, Greek symbols, equations, code, citations, headings, figure/table/equation labels, captions, cross-references, file paths, identifiers, and domain glossary terms) require byte-exact post-normalization agreement; any deletion, insertion, substitution, reordering, or label/ordinal change is material. Prose spans may tolerate low-risk whitespace and line-wrap differences only when `rapidfuzz.distance.Indel.normalized_similarity >= material_conflict_prose_similarity_min` and the opcode trace contains no numeric, unit, sign, citation, or glossary-token edit. Short spans below `fuzzy_consensus_min_chars`, spans without Unicode letters or decimal digits, and spans with unknown or mixed source lineage route to judge/manual review rather than fuzzy acceptance. A blocked consensus writes a `material_conflict` audit record with the span class, compared candidate ids, lineage ids, comparator id, normalization profile id, opcode trace hash, similarity, conflict reason code, affected token classes, and selected next action.

   Low-risk normalization may repair whitespace, line wrapping, and obvious page-boundary joins. It must not change numbers, units, signs, Greek letters, equation syntax, code-like text, citations, headings, figure/table/equation labels, caption text, cross-references, or domain terms unless the exact normalized output is already supported by independent evidence. High-risk spans always go to judge or manual review unless an explicit per-span consensus rule covers that class. When the source manifest labels the embedded text layer for the relevant page or region as `poor`, `mixed`, or `unknown`, candidates that agree only through `embedded_text_layer` lineage cannot be auto-accepted; the span must go to judge or manual review with the rendered crop included.
3. Disputed spans are sent to the configured judge role with an evidence packet containing page-image or layout-region crops only when the role binding's `required_modalities` includes `image`, the backend's declared `input_modalities` includes `image`, the active content-handling approvals and network allowlists permit that image evidence to reach the backend, and the referenced crop evidence exists. If any of those checks fails, the runtime may route to a text-only judge packet only when the role binding permits text-only operation; otherwise the span routes to manual review or a tool-profile configuration error rather than silently dropping required pixels. The packet also contains candidate spans with `region_refs`, candidate lineage records, glossary context, nearby chapter context, and optional topic description. A text-only packet includes OCR text, embedded text, region geometry, page labels, crop ids, and lineage/confidence metadata; it never pretends that the backend inspected pixels.

   Each role/backend binding resolves an `evidence_packet_profile` before the first judge call. The profile records text-token budget, image-crop budget, required modality classes, maximum nearby-context windows, and whether crop thumbnails, full crop files, or crop ids are allowed for that backend. Defaults come from `evidence_packet_text_token_budget_default` and `evidence_packet_image_crop_budget_default`, then may be narrowed by backend context limits, run ceilings, or content-handling policy. When a packet would exceed the resolved budget, pruning is deterministic: keep the disputed span and its required crop first, then candidate spans that overlap the disputed region sorted by source-page index, region order, engine-family diversity, lineage confidence descending, and candidate id; then accepted neighboring spans needed to explain continuity; then glossary entries referenced by the span; then layout geometry and lineage summaries; then bounded nearby prose context; then optional topic description. Dropped material is not lost: the packet includes an `omitted_evidence_manifest` with omitted item ids, source page indices, reason codes, hashes, byte/token estimates, and the rule that omitted it. Required pixels, required candidate spans, protected-token evidence, and minimum lineage records are never silently pruned. If any required evidence still does not fit after deterministic pruning, the span routes to `MANUAL_REVIEW` with reason `evidence_packet_required_evidence_over_budget`. The `evidence_content_hash` and `rendered_prompt_hash` include the resolved packet profile id, profile hash, ordered included-evidence ids, and omitted-evidence manifest hash so two packets that differ only by pruning policy cannot share a local response-cache entry.
4. The backend returns schema-constrained records: chosen text, confidence, evidence references, reason, and review flag. For backends with native schema support, the response is still validated client-side after the call. For backends without native schema support, the pipeline validates client-side and retries with a tightened prompt on validation failure, subject to anti-spin limits. Schema validation failures count as backend failure signatures normalized to the response schema id and version and are not stored as replayable local response-cache entries.
5. Low-confidence spans are wrapped in searchable review markers. A span is low-confidence when the schema-valid adjudication response has `review_flag: accepted_with_flag` or its `confidence` is below `judge_low_confidence_review_threshold` after any role-specific threshold override in the active tool profile. Responses with `review_flag: manual_review` create a review marker and a Phase 11 manual-review input file rather than being treated as accepted text.

Role-specific confidence threshold overrides are valid only when the active `tool_profile.json` records an approved calibration for the exact judge role, backend id, model id, prompt id/version, response schema id/version, and `confidence_method`. The calibration compares pilot decisions against approved gold-set truth, buckets accepted and rejected decisions by reported confidence, and records false-acceptance rate, false-review rate, sample size, comparator id, gold-set id/version, suggested threshold, approved threshold, approver, and approval timestamp. Samples below `judge_confidence_calibration_min_gold_decisions` are stored as observations but do not lower the global threshold unless the reviewer explicitly records an override rationale at `PILOT_GLOSSARY_REVIEW`. If a backend omits numeric confidence, changes `confidence_method`, or lacks enough matched gold decisions, its role binding uses the global threshold plus `review_flag` until a mini-pilot refreshes calibration.

Review markers follow a strict grammar so that the chapter-QA tooling can extract them deterministically:

```text
review_open  = "<!-- REVIEW " key_value_list " -->"
review_close = "<!-- /REVIEW " "id=" quoted_id " -->"
key_value_list = key_value ( " " key_value )*
key_value    = key "=" value
key          = "id" | "page" | "reason" | "severity" | "category"
value        = quoted_string | bare_token
quoted_string = '"' ( escaped_char | not_quote_or_backslash )* '"'
escaped_char  = backslash ( backslash | quote | "n" | "r" | "t" )
quote         = %x22
backslash     = %x5C
not_quote_or_backslash = any character except quote or backslash
bare_token    = [A-Za-z0-9._:-]+
```

Required keys on the opening marker are `id`, `page`, and `reason`. `severity` defaults to `normal`; `category` is free-text. The closing marker carries the same `id` as its opening marker. Markers may span multiple lines of body text but must not nest. The grammar shown above is the emit form. The chapter-QA parser accepts a permissive form where any non-empty run of whitespace characters (`\s+`) is allowed between adjacent key-value pairs, and any non-empty run of whitespace is allowed between the leading `REVIEW` or `/REVIEW` token and the next token; the parser normalizes parsed markers to the strict single-space emit form when it rewrites them. This lets human reviewers add line breaks or extra spaces for readability without breaking deterministic parsing.

Opening marker keys must be unique. Machine-emitted markers use `severity` values from the closed enum `low`, `normal`, or `high`; human-authored marker severities outside that enum are parsed as invalid until the reviewer corrects them, while `category` remains free text. `page` is the decimal `source_page_index` for the marker's primary page; for multi-page spans it is the first affected source page and the full ordered list lives in the `review_marker.source_page_indices` sidecar record. Because markers are Markdown HTML comments, emitted marker comment text must also satisfy the HTML-comment constraints used by GitHub Flavored Markdown: the serialized comment body must not contain `--`, start with `>` or `->`, or end with `-`. When a full reason or category would violate that comment-safe subset, the inline marker uses a compact safe reason token and the full text is stored only in the `review_marker.reason` or `review_marker.category` field.

Markers may wrap body text blocks only. They are never emitted inside fenced or indented code blocks, inline code spans, raw HTML/table tags, link destinations, image paths, autogenerated anchors, equation markup, or other protected literal zones. When the issue applies to one of those zones, the marker is placed immediately before or after the affected block and the `review_marker_link` record references the affected `block_id` or asset record.

The review-marker parser is tested with Markdown-context fixtures before marker extraction is considered stable. Positive fixtures cover paragraph text, list items, ordinary table-adjacent body blocks, multiline wrapped body text, allowed whitespace permutations, quoted values with escaped characters, and markers that wrap a whole block while preserving the block's source offsets. Negative fixtures cover fenced code, indented code, inline code spans, raw HTML and sanitized table tags, link destinations, image destinations and alt-text ambiguity, autogenerated anchors, display and inline equations, duplicate opening keys, mismatched close ids, nested markers, unsafe HTML-comment text, missing required keys, unknown severity enum values, and markers split across protected-zone boundaries. The parser uses the same Markdown AST/context classifier as assembly validation plus the marker grammar above; a plain regex match is allowed only as a first-pass token locator and never as the authority for whether the marker is in a legal Markdown context. Fixtures live under `tests/fixtures/review_markers/`, include both expected `review_marker` / `review_marker_link` records and expected diagnostics, and are exercised by `tests/test_review_marker_parser.py`.

Example:

```markdown
<!-- REVIEW id="rv-0042" page=127 reason="all candidates disagree on technical term" severity="high" -->
candidate text here
<!-- /REVIEW id="rv-0042" -->
```

Prompts are structured as stable prefix, optional reusable middle blocks, and variable suffix so vendor prompt caching can apply. The stable prefix contains durable judge rules, system instructions, schema-output description, glossary version reference, and prompt metadata. The optional one-sentence topic description lives in a reusable middle block keyed by `topic_description_hash`, not in the stable prefix; this prevents a topic change from invalidating the prefix while still binding the topic into cache identity. Additional reusable middle blocks contain large context reused across multiple calls for the same chunk or chapter. The variable suffix contains page-specific layout regions, crop references, candidate spans, disputed spans, and other per-call evidence. Where the backend supports explicit cache controls, place cache breakpoints at the end of reusable blocks according to provider rules. Where the backend uses automatic caching, keep reusable prefixes byte-identical across calls.

Vendor prompt caching is an optimization, not a correctness dependency. Cache-control markers are enabled only for backend records whose provider profile declares support and whose content-handling approvals cover the provider's cache-retention semantics for the evidence being sent. Provider cache TTL and account behavior may make near-term repeated calls cheaper across process invocations or runs, but NoEasyMark treats those hits as opportunistic and never as replay authority. The runtime records provider cache usage fields when available and records `unavailable` when the provider does not expose them or the prompt is below that provider's cacheability requirements. Local response-cache identity remains governed by `cache_key`; provider cache hits or misses do not change the accepted adjudication result.

Before any judge call, the local response cache is consulted. A cache hit replays the prior adjudication record with its full provenance and records no spend. A cache miss verifies network-class approval and allowlist compliance, issues the call, stores the response with full provenance, and updates the spend and progress ledgers.

The backend must not invent missing captions, equation labels, units, cross-references, or bridge text. If evidence is insufficient, it must flag the span for review.

Definition of done: every disputed span has a schema-valid decision or review marker, every output block maps to source page evidence and at least one `layout_region`, and adjudication logs are complete.

## Markdown Assembly and Normalization

Markdown assembly is conservative and evidence-recorded. Deterministic normalization is allowed only for transformations that preserve source meaning and can be explained from layout evidence.

Chapter Markdown files are emitted as UTF-8 with LF line endings and exactly one final LF. `evidence_sidecar.block_entries[].markdown_offset_start` and `markdown_offset_end` are zero-based UTF-8 byte offsets into the exact emitted Markdown file after final normalization, Markdown escaping, and unresolved review-marker insertion; ranges are half-open `[start, end)`. When a review marker wraps a block, the block offset range includes the marker comments and `review_marker.marker_open_offset` / `marker_close_offset` record the comment-specific byte ranges.

The assembler emits only GFM constructs it intentionally creates from layout evidence. Source text that contains Markdown-significant punctuation is escaped as literal text unless the block classifier has accepted that punctuation as the intended Markdown structure for that block. Raw HTML from source text is escaped to literal text; the only raw HTML-like constructs emitted by the Markdown deliverable are Phase 8 review-marker comments and Phase 7 sanitized inert table markup that already carries a table record, sanitizer profile id, and output hash. Any unexpected raw HTML tag, autolink, task-list marker, heading marker, table delimiter, code fence, math delimiter, or link/image syntax introduced by source text becomes `markdown_escape` evidence or a review marker instead of silently changing the block kind.

Allowed low-risk transformations include:

- Removing repeated running headers, running footers, and standalone page numbers when repetition analysis and layout position show they are page furniture rather than body content.
- Repairing line wrapping inside a paragraph when adjacent lines belong to the same layout region and no list, table, caption, equation, heading, footnote, or sidebar boundary intervenes.
- Joining words split by soft hyphen or discretionary line-break hyphen only when the joined form is supported by engine agreement, glossary evidence, or nearby source evidence. Preserve the hyphen and flag for review when the term may be domain-specific.
- Normalizing Unicode to NFC while preserving visible symbols, math notation, Greek letters, units, superscripts/subscripts where represented, and source-significant punctuation. Do not apply compatibility normalization (`NFKC` or `NFKD`) to source-derived Markdown text.
- Escaping Markdown-significant punctuation, raw HTML-looking source text, or accidental GFM extension triggers when the intent is literal text rather than Markdown structure.
- Converting obvious list continuation lines into Markdown list items when indentation and region evidence agree.

Repeated page-furniture removal uses the closed `page_furniture_detector_v1` profile. A candidate is eligible for deterministic removal only when its NFC-normalized text recurs on at least `page_furniture_repetition_min_pages` distinct applicable pages and at least `page_furniture_repetition_min_fraction` of that chapter or repeated-section group, appears wholly within the top or bottom `page_furniture_vertical_band_fraction` of the normalized page geometry (or the corresponding outer margin band for standalone page numbers), and matches the median typography and geometry pattern within `page_furniture_typography_similarity_min` and `page_furniture_geometry_tolerance_points`. Page labels, chapter openers, section titles, table headers, figure/table/equation captions, footnotes, legal/code citations, form field labels, and any text whose region participates in body reading order are source-page exceptions and are not removed automatically even if repeated. When the detector is uncertain because the repetition count is borderline, a page-size family changes, OCR text is unstable, or the candidate overlaps a body block, the assembler preserves the text and emits a review marker plus `normalization_event: page_furniture_uncertain` instead of stripping it. Every removed item records the normalized text hash, original text, affected page indices, geometry band, typography summary, repetition counts, exception checks, and profile id in the evidence sidecar; chapter QA verifies that no source page becomes empty unless its `source_page_record.blank_page_status` is `intentionally_blank`.

List and table structure acceptance uses the closed `markdown_structure_classifier_v1` profile. A line that looks like a GFM bullet, ordered-list marker, task-list marker, or pipe table delimiter remains literal text unless layout evidence supports the corresponding block kind. Lists require compatible indentation within `list_classifier_indent_tolerance_points`, repeated item marker geometry or an accepted continuation relation, reading-order adjacency, and no intervening table, caption, equation, heading, footnote, sidebar, or protected-zone boundary. Ordered lists preserve the source ordinal when it is semantically meaningful and otherwise normalize only under `normalization_event: list_marker_normalize`. Task-list marker text such as `[ ]` or `[x]` is escaped unless the source evidence shows an actual checklist item rather than bracketed literal text. Tables require either an accepted `table_record` or row/column geometry with at least two stable columns, row baselines that align across adjacent rows, cell-boundary drift within `table_classifier_column_tolerance_points`, and evidence for wrapped cells when one source row occupies multiple text lines. The assembler creates GFM table delimiter rows only from accepted table structure, escapes literal pipes inside cell text, and routes tables that need merged cells, nested block content, or uncertain headers to the Phase 7 table representation rubric. After emission, the GFM parser round-trip must produce the accepted list/table block kinds and cell counts; otherwise the assembler escapes the punctuation or emits a review marker instead of trusting engine-emitted Markdown. Fixtures under `tests/fixtures/markdown_assembly/structure_classifier/` cover literal punctuation, accepted lists, nested/continued lists, task-list escapes, accepted pipe tables, escaped literal pipes, wrapped cells, and rejected ambiguous table-looking prose.

Soft-hyphen and discretionary line-break joins use the closed `soft_hyphen_join_v1` profile. The proof record distinguishes an explicit U+00AD soft hyphen, a nonprinting discretionary break inferred from adjacent line geometry, and a visible line-ending hyphen. A join is allowed only when the two pieces are adjacent in the same paragraph or accepted list/table cell, no protected-zone boundary intervenes, and the joined form is supported by at least one strong proof: direct agreement from two independent engine families, an approved glossary `surface_form`/alias/misreading that maps the joined form, or at least `soft_hyphen_join_unsplit_occurrence_min` independent unsplit occurrences in the source or approved reference material. Visible hyphen removal additionally requires either glossary approval or two strong proofs, and it is forbidden for part numbers, standard identifiers, legal/code references, ranges, units, all-caps abbreviations, and mixed digit-letter tokens unless the approved glossary explicitly marks the joined form. Unproven joins preserve the visible hyphen or soft-hyphen split as source text and add a low- or normal-severity review marker according to protected-token risk. Every join records original codepoints, source line ids, region refs, proof refs, joined text hash, and the profile id in `normalization_events`. Fixtures under `tests/fixtures/markdown_assembly/soft_hyphen_join/` cover explicit soft hyphen, visible discretionary hyphen, technical identifiers, glossary-approved joins, repeated unsplit proof, and protected-zone rejection.

`block_id` is deterministic, not a processing-order counter. For every source-derived block, the emitted `bk-<14-char>` value is the prefix of the SHA-256 of canonical JSON with `block_id_version`, `chapter_id`, deliverable-root-relative Markdown path, `block_kind`, `source_page_indices` in source order, ordered `region_refs`, ordered `candidate_refs`, ordered `adjudication_refs`, `reading_order_index`, `acceptance_status`, the post-normalization pre-review-marker emitted-text hash, and the hashes of normalization-event records that materially changed emitted text. Byte offsets, volatile timestamps, reviewer identity, and unresolved review-marker ids are excluded so marker insertion and report refreshes do not churn block ids. Generated index/front-matter blocks use the same tuple with empty source refs plus their generating record refs and `acceptance_status: generated`. If two blocks collide on the 14-character prefix inside one evidence sidecar, the runtime extends only the colliding ids by two lowercase hexadecimal characters at a time and records `block_id_collision_extension_chars`; the full SHA-256 remains in the sidecar for audit. `tests/test_block_id_derivation.py` covers stable reruns, marker insertion, offset changes, generated blocks, and synthetic collision extension.

Protected zones include equations, code blocks, file paths, figure IDs, table IDs, equation IDs, citations, quoted source text, page labels, legal/code references, numbers with units, and any span marked protected by schema or glossary. Normalization inside protected zones requires judge decision or manual review.

Every source-derived assembled Markdown block has an entry in the evidence sidecar and maps to at least one source page and one `layout_region`. The reserved generated block kinds `front_matter` and `index_link` are the only block kinds allowed to carry empty `source_page_indices` and `region_refs`; they use `acceptance_status: generated` and explain their generation source in `extra_notes`. Each sidecar entry records source pages, layout regions, candidate spans, deterministic normalizations applied, detailed `normalization_events`, judge/adjudication record ids where applicable, and review status. The Markdown itself should stay readable; evidence belongs in sidecars rather than inline comments except for unresolved review markers.

After writing a Markdown file, the assembler parses it with the same GFM/remark profile used by validation. Parsed block kinds for headings, lists, tables, code blocks, math, links/images, sanitized HTML tables, and review markers must match the block kinds and protected-zone decisions recorded in the evidence sidecar. A parser mismatch is a generation failure for deterministic assembly defects or a review marker for source ambiguity; it is not ignored.

## Phase 9 - Pilot, Glossary, and Tool Profile

The pilot, glossary build, backend selection, artifact strategy, and tool profile decision are one phase with one required post-pilot human gate: `PILOT_GLOSSARY_REVIEW`. This eliminates duplicate sampling work and ensures that the approved glossary is available before any full-run chunk is adjudicated. Operators who want to approve the pilot plan before source-derived pilot calls run may use the optional `PILOT_PLAN_REVIEW` gate.

Pilot scope: a stratified sample whose page count sits inside the `pilot_sample_pages_min`..`pilot_sample_pages_max` band (defaults 30..50). The sampler aims at the upper edge so the pilot exercises as many features as possible and only steps down toward the lower edge when stratification cells are exhausted. The sample covers chapter opening, dense technical prose, multi-column pages, diagrams and captions, equations and Greek symbols, tables, pages where the existing OCR is known to be poor, and at least one oversized chapter or section if the source has one.

Pilot selection is deterministic and persisted before any pilot conversion work starts. `noeasymark pilot` writes `.noeasymark-state/<run_id>/pilot/pilot-plan.json` as a schema-valid `pilot_plan` record with `pilot_plan_id`, sampler version, source-manifest hash, outline hash, run-config hash, operator-supplied pilot-page hints, sampler seed, feature-strata definitions, per-page detected stratum labels, candidate pages per stratum, selected pages in source order, tie-break decisions, and pages excluded with reasons. The default seed is the SHA-256 of canonical JSON containing `source_pdf_sha256`, source-manifest hash, outline hash, `pilot_sample_pages_min`, `pilot_sample_pages_max`, and operator page hints; if a future `run_config.pilot_seed` field is added, it must be recorded in the same plan and participate in the hash. Valid operator hints are forced into the candidate selection. If valid hints exceed `pilot_sample_pages_max`, the command does not run the pilot automatically: it emits the plan and pauses at `PILOT_PLAN_REVIEW` so the operator can trim the hints or approve an expanded pilot scope.

The default sampler uses the closed `pilot_feature_sampler_v1` profile. Required strata are `chapter_opening`, `dense_technical_prose`, `multi_column`, `visual_caption_pair`, `table`, `equation_or_symbol_heavy`, `poor_or_mixed_text_layer`, and `oversized_chapter_or_section`; optional future strata may be added only by versioning the sampler profile. For every stratum with candidates, the sampler first tries to select `pilot_feature_stratum_min_pages` pages, or `pilot_rare_feature_stratum_min_pages` when fewer candidates exist. A page may satisfy multiple strata and counts toward every stratum it covers. Candidate scoring is an ordered tuple, not an implementation-dependent random draw: highest number of still-unmet required strata covered; highest weighted feature score using `poor_or_mixed_text_layer=90`, `multi_column=80`, `table=70`, `visual_caption_pair=60`, `equation_or_symbol_heavy=60`, `dense_technical_prose=50`, `chapter_opening=40`, and `oversized_chapter_or_section=30`; highest rarity score, computed as the sum over still-unmet strata of `(total_source_pages - candidate_count_for_stratum)`; highest chapter-spread score for chapters not yet represented; operator-hinted pages before unhinted pages; then lowest `source_page_index`. After every present stratum meets its minimum or is exhausted, the sampler fills toward `pilot_sample_pages_max` with the same tuple recomputed against warning-level feature diversity, chapter spread, and poor-text-layer coverage. If the available corpus cannot reach `pilot_sample_pages_min`, the plan records exhausted strata and pauses at `PILOT_PLAN_REVIEW` unless the operator approves the smaller pilot or expands the source set. The persisted `pilot_plan` records the score tuple and selected/unselected reason for each candidate page so reviewer-visible plans can be reproduced without re-running detection.

Pilot-page hints from the operator are optional. If no known pilot pages are supplied, the tool auto-selects representative pages from source inspection and layout analysis. The proposed pilot set is recorded in the `pilot_plan` record. `noeasymark pilot --plan-only`, `noeasymark pilot --require-plan-review`, or `run_config.require_pilot_plan_review: true` emits the proposed pilot set, candidate backend plan, artifact strategy, and expected source-content movements, then pauses at `PILOT_PLAN_REVIEW` when the operator wants to review or edit the plan before source-derived pilot calls are issued. If the operator does not request a plan-only gate, the pilot may run from the schema-valid `init` approvals and the selected backend preferences.

Gold-set procedure:

1. The tool proposes a gold-set subset from the pilot pages. The subset must contain at least 5 pages. It must include one dense prose page, one multi-column page if present, one page with a visual/caption pair if present, one table page if present, one equation or symbol-heavy page if present, and one page with poor text-layer quality if detected. Features that the source document does not contain are not required and the corresponding gold-based metrics are marked `not_applicable` rather than `unavailable`. When the operator has placed reference materials at `corpus/<output-folder-name>/reference/` (operator-curated hand-converted Markdown, extracted images, and similar pre-existing material), `noeasymark pilot` surfaces those files alongside the review packet at `PILOT_GLOSSARY_REVIEW` so the reviewer can pre-fill `expected_text` or `expected_blocks` from them rather than transcribe from scratch. Reference materials are read-only inputs to the review packet; they are not auto-imported as gold-set entries, and the operator-reviewed gold-set gate remains the authority for what becomes approved gold truth.
2. The tool creates `gold_set` review packets containing rendered page views, layout-region crops, candidate text, and empty or prefilled expected-output fields. Prefilled fields are suggestions only.
3. A human reviewer approves or corrects the gold-set records before gold-based metrics are treated as passing. If no human-approved gold set exists, gold-based metrics are marked `unavailable`, full-run scale-up remains blocked, and any waiver must be explicit at `PILOT_GLOSSARY_REVIEW`.
4. Gold-set records are source-derived artifacts and follow the same repository, local-output, and artifact-archive rules as other review artifacts.

Gold-set review packets use a stable local layout under `<output_root>/<output-folder-name>/reviews/gold-set/<pilot_plan_id>/`. The directory contains `review.html` as the rendered reviewer surface, `gold-set.draft.json` as the editable schema-shaped draft, `gold-set.validation.json` with current validation diagnostics, `pages/page-<PPPP>.html` for per-page review, and `assets/` entries for page thumbnails and layout-region crops referenced by relative path. Each page view shows the rendered page thumbnail, crop links, detected feature strata, candidate Markdown/text by engine with lineage and confidence summaries, optional reference-material prefill snippets with path/hash provenance, and expected-field editors for `expected_form`, `expected_text`, `expected_blocks`, expected assets, acceptance criteria, confidence, and reviewer notes. The packet checklist records that every selected page has an expected form, every expected block maps to a source page and optional region, protected values were checked, prefill provenance was accepted or rejected, and schema validation passed. `review.html` never embeds absolute paths, raw prompts, raw model responses, credentials, or unredacted approval identities; it quotes source text only in the bounded fields needed for gold review. The reviewer-approved `gold_set` record is written only after the draft validates and the reviewer records `approval_status: approved` or an explicit waiver with approver and rationale.

Each `gold_set` record has at minimum: `gold_set_id`, `schema_id`, `schema_version`, `created_by`, `created_at`, `approval_status` (`pending`, `approved`, `waived`, `superseded`), nullable `approver`, nullable `approval_at`, `source_pages` (list of `source_page_index` covered), `feature_coverage` (mapping of optional feature flags to `present`, `absent`, or `unknown`), and an ordered list of `entries`. `approver` and `approval_at` are required when `approval_status` is `approved` or `waived` and null when it is `pending`; a superseded record preserves the prior approval fields plus the supersession link. Each entry has `entry_id`, `source_page_index`, optional `region_refs`, `expected_form` (`text` for prose-dominant pages or `blocks` for structured pages, set by the reviewer), `expected_text` (populated when `expected_form` is `text`), `expected_blocks` (populated when `expected_form` is `blocks`; an ordered list of typed block records covering `paragraph`, `heading`, `table`, `figure_ref`, `equation`, `list`, `code`), `expected_assets` for figures/tables/equations, `prefill_source_refs` (array of optional reference-material path/hash records used only as suggestions), optional `acceptance_criteria` (for example allowed normalizations), `confidence` of the gold-truth itself, and free-text `reviewer_notes`. Metric code chooses the matching comparator (`text_form` or `blocks_form`) based on `expected_form`; mixing forms within a single chapter's gold set is permitted because pages legitimately differ in structure. Prefilled reference material does not become gold truth until the reviewer approves the entry.

Each `glossary_entry` has at minimum: `entry_id`, `surface_form`, `normalized_form`, optional `aliases` and `misreadings`, `domain` (free-text tag), `protected_zone` flag (when true, the entry is never applied as a rewrite inside protected zones), `evidence_refs` (list of source pages and layout regions where the term was observed), `status` (`candidate`, `approved`, `rejected`, `deferred`), `decision_rationale`, nullable `approver`, nullable `approval_at`, optional `topic_description_hash` of the topic context that produced the candidate, and `confidence` in `[0.0, 1.0]`. `approver` and `approval_at` are required only for `approved` entries; `rejected` and `deferred` entries require a non-empty `decision_rationale`, while `candidate` entries may carry an empty rationale until review.

Quality-threshold procedure:

1. `quality_thresholds` are recorded in the tool profile before full-run scale-up. Each metric has a target, warning threshold, blocking threshold, measurement method, sample scope, waiver policy, comparator id, comparator implementation/version, transform profile id, transform profile hash, direction (`lower_is_better` or `higher_is_better`), numerator definition, denominator definition, and explicit `not_applicable` / `unavailable` behavior.
2. Default floor checks always apply unless explicitly waived: 100% source-page coverage, 100% rendered-page availability, no unresolved reading-order risk on pages used for final Markdown, no missing evidence sidecars for emitted Markdown blocks, no unresolved high-severity manual review items, no unwaived normal-severity manual review items at finalization, and no committed PDF outside Git LFS.
3. Gold-based pilot metrics must include character or word error rate for gold-set text, heading accuracy, figure-caption pairing accuracy when visual pages exist, table handling accuracy when table pages exist, equation/symbol handling accuracy when relevant pages exist, and review-marker density. The approved tool profile may choose character error rate, word error rate, or both, but must state why. The default text metric comparators are `jiwer.process_characters` for character error rate and `jiwer.process_words` for word error rate, using the NoEasyMark `gold_text_v1` transform profile on both reference and hypothesis: render final Markdown to plain text, remove review-marker comments, interpret Markdown escapes as their literal characters, normalize to NFC, normalize line endings to LF, and trim one trailing final newline. The default profile does not casefold, drop punctuation, normalize numbers or units, apply compatibility normalization, or strip protected-zone content. Review-marker density is `unresolved_review_marker_count / accepted_source_derived_markdown_block_count`, excluding generated `front_matter` and `index_link` blocks; waived markers are reported separately and are not silently removed from the numerator unless the approved waiver policy says so.
4. A full-run regression against pilot baselines beyond the approved warning threshold triggers a halt or targeted mini-pilot before more chapters are published.

Structured gold metrics use the closed `structured_block_comparators_v1` profile unless the approved tool profile records a newer comparator. Expected blocks are matched to output blocks by source page, optional `region_refs`, block kind, source-order index, accepted asset id when one exists, and normalized emitted-text hash where applicable. Heading accuracy counts a heading as correct only when text, source page, order, and heading level match the approved expected block; heading text uses the same `gold_text_v1` transform but does not casefold or normalize numbers. Figure-caption pairing accuracy uses expected visual/caption pairs as the denominator and counts success only when the visual asset or crop record matches the expected source region, the caption block is present, and the pair link references the same figure/table id; missing-caption records count as correct only when the expected block says the source lacks a caption. Table handling accuracy matches expected table blocks by source region or accepted table asset id, then requires row count, column count, header/body split, cell order, and protected cell text to match; prose-only cell text may use the approved text comparator, while numeric, unit, code, symbol, and legal/code-reference cells require exact post-normalization text. Equation/symbol accuracy matches expected equation blocks and symbol-heavy protected spans by source region/order and requires label, visible-symbol coverage, and protected-token order to match; accepted crop-only equations count only when the expected form allows crop-only output. Multi-column reading-order accuracy is pairwise order accuracy over expected source-derived blocks on pages labeled multi-column: the numerator is concordant block pairs and the denominator is all comparable expected block pairs after excluding generated blocks and waived manual-review replacements. Every structured comparator emits `matched_expected_ids`, `matched_block_ids`, numerator, denominator, exact-pass count, partial component subscores, tolerance class used, and examples of false positive/false negative matches in `pilot.json`; the headline metric uses exact-pass numerator/denominator unless the reviewer approves a partial-credit policy in `tool_profile.approved_metric_specs`. Fixtures under `tests/fixtures/quality_metrics/structured_blocks/` cover at least one passing and one failing case for every comparator, plus partial-credit examples, and `tests/test_structured_block_comparators.py` asserts stable numerator/denominator and diagnostic ids.

The tool ships conservative starter threshold profiles so the operator is not forced to invent metric policy from a blank page. A starter profile is not accepted silently: `PILOT_GLOSSARY_REVIEW` displays the proposed target, warning threshold, blocking threshold, measurement method, and waiver policy for each metric, and the reviewer must approve, amend, or waive each unavailable metric before full-run scale-up.

Starter operational-metric policies are explicit even when they are informational by default. Known-cost backends report cost per page with `cost_tracking.mode: native`, `estimated_by_model`, or `estimated_by_page`; the starter profile warns when pilot or full-run cost per page exceeds the approved forecast by 25% and blocks only when a configured spend or invocation ceiling is reached. `unknown` and `subscription_credit_pool` backends remain selectable only with the acknowledgments and mandatory invocation/runtime ceilings described in **Budgets, Cost Ceilings, and Caching**; their starter cost metric records `unavailable` for dollars, reports invocation/runtime exposure instead, and blocks full-run scale-up unless the reviewer approves the ceiling-backed policy at `PILOT_GLOSSARY_REVIEW`. Runtime per page uses median and median absolute deviation from the pilot; pages above `median + runtime_outlier_mad_multiplier * MAD` or above 2x the pilot median, whichever is larger, warn and are listed with engine/backend ids. Vendor-cache hit rate is an operational optimization metric: `native` and `inferred` modes report hit rate with source fields, while `unavailable` renders as `not_applicable` and does not fail quality. If a backend profile claims native provider-cache metrics but the pilot observes only unavailable fields, `doctor --backends` treats that as a backend-profile defect before scale-up. Local response-cache hit rate is reported for replay economics and determinism diagnostics; a low hit rate warns only when the approved run policy expected cache reuse for repeated calls.

Default pilot evaluation rubrics:

- Judge backend binding rubric: score each candidate binding on schema-output reliability, adjudication accuracy against the approved gold set, ability to use rendered crops and nearby context, failure-domain independence from the default judge, latency per disputed span, cost or invocation/runtime exposure, transcript-persistence risk, network-class fit with current approvals, and operational recoverability when credentials, quota, or local CLI state fail. The selected `default_judge`, `fallback_judge`, and `independent_auditor` bindings must record their scores, known weaknesses, and independence rationale in `tool_profile.json`.
- Judge confidence calibration rubric: for each role/backend pair exercised on gold-set decisions, compare the reported `confidence` and `confidence_method` against approved gold truth, then score candidate threshold policies on false acceptance of incorrect protected spans, unnecessary review load, sample size, backend/prompt stability, and explainability to the reviewer. Calibration may tighten thresholds automatically when the data shows risk; it may loosen thresholds only when the reviewer approves the specific role/backend override.
- Engine-set rubric: score each enabled engine on candidate quality against gold pages, layout-region coverage, multi-column reading-order behavior, figure/table/equation handling, lineage diversity, runtime per page, artifact growth, dependency/install reliability under the active Python/runtime, network-class fit with current approvals, and failure observability. Engines that do not improve quality, diversity, or coverage during the pilot remain disabled unless the operator records a reason to keep them.
- Artifact strategy rubric: score local deliverable retention and any external artifact archive on source-content exposure risk, review ergonomics, resumability across machines, storage cost, retention control, Git LFS requirements, path-length risk, and archive allowlist specificity. The approved artifact strategy records which artifact classes, if any, may move outside the local device.

Pilot procedure:

1. Run Phases 4-8 against the pilot pages with the candidate tool set and the candidate judge backends allowed by the current content-handling choices.
2. Build, import, or update the human-approved gold set for the selected subset. If the gold set is not yet approved, emit the review packet and remain blocked at `PILOT_GLOSSARY_REVIEW`.
3. Run `noeasymark glossary build` over pilot candidates and adjudicated output.
4. Candidate terms include high-frequency technical terms, non-ASCII symbols and terms, repeated OCR-confused spellings, cross-chunk spelling clusters, terms where engines frequently disagree, and terms suggested by the optional topic description.
5. Evaluate the backend bindings for `default_judge`, `fallback_judge`, and `independent_auditor` against the configured rubric. Decide whether to enable `escalation_judge`, `triage_model`, or an alternate auditor based on pilot evidence. Compare backends on per-page cost, latency, schema-output reliability, and adjudication quality against the gold set.
6. Compute judge-confidence calibration observations for every exercised role/backend/model/prompt/schema/`confidence_method` tuple that can be matched to approved gold truth. Record bucketed decision counts, false-acceptance examples, false-review examples, sample size, suggested threshold, whether the sample met `judge_confidence_calibration_min_gold_decisions`, and any reviewer-approved threshold override or waiver.
7. Evaluate artifact storage options using the configured rubric.
8. Emit `<output_root>/<output-folder-name>/reports/pilot.json`, `<output_root>/<output-folder-name>/glossary/candidates.yaml`, `.noeasymark-state/<run_id>/tool_profile.json`, and `<output_root>/<output-folder-name>/reports/tool-profile.json`.

`<output_root>/<output-folder-name>/reports/pilot.json` includes the `pilot_plan_id`, character or word error rate against gold, heading accuracy, figure-caption pairing accuracy, table handling accuracy, equation handling accuracy, multi-column reading-order accuracy, review marker density, glossary disagreement rate, judge-confidence calibration observations by role/backend, cost per page per backend, runtime per page per backend, vendor cache metric mode, vendor cache hit rate when available, and local response cache hit rate observed during the pilot.

`.noeasymark-state/<run_id>/tool_profile.json` records the `pilot_plan_id`, bound backend, model, model snapshot, and provider-native effort or thinking-budget setting for each enabled judge role, the independent auditor, approved role/backend confidence calibrations and threshold overrides, local GPU tools, artifact strategy, render-profile alias, expected review load, progress thresholds, budget forecast, content-handling choices including network-class approvals and allowlists, quality thresholds, approved metric specs, gold-set status, glossary match profile, and prompt ids and versions in use.

`<output_root>/<output-folder-name>/reports/tool-profile.json` is a redacted, schema-validated snapshot for reviewers (`tool_profile_public`). It includes backend names, backend kinds, network classes, model ids, prompt ids/versions, schema versions, cache settings, content-handling mode summary, thresholds, and hashes. It excludes credentials, absolute local paths, raw prompts if they contain source-derived content, and any secrets.

Always pause at `PILOT_GLOSSARY_REVIEW`. The reviewer approves or amends the pilot results, gold-set records or gold-metric waiver, glossary, tool profile including which judge roles are enabled and which backends they bind to, judge routing, artifact strategy, content-handling choices, quality thresholds, and budget/progress settings in a single cycle. If the reviewer amends the pilot page set at this gate, the current pilot report is marked superseded and the changed pilot scope must be run before full-run scale-up.

Any post-pilot change to the tool profile triggers a mini-pilot. The mini-pilot scope is the intersection of the change-affected pages with the existing pilot set, capped at 20 pages. If the intersection is empty (for example because the change affects an engine that the pilot did not exercise on the chosen pages), the mini-pilot falls back to a fresh stratified sample of equivalent size drawn from the same chapter sections that produced the original pilot. The operator may waive the mini-pilot only by recording an explicit waiver in the tool profile.

The mini-pilot impact map is deterministic. Engine roster, engine config, OCR policy, layout detector, table/visual/equation extractor, or resource-budget changes affect pilot pages where that engine participated plus pages whose strata match the engine's declared features. Render-profile or crop-padding changes affect every pilot page with rendered-page, crop, visual, table, equation, or text-layer-quality evidence. Judge backend, model, parameter, prompt, response-schema, evidence-packet, confidence-threshold, or provider-cache-profile changes affect pages whose adjudication records used the changed role/backend and at least one page from every pilot stratum that can produce disputed spans. Glossary or glossary-match-profile changes affect pilot pages with glossary candidates, approved glossary hits, OCR-confused spellings, or technical terms; if none exist, the fallback is dense technical prose. Quality-threshold, comparator, or waiver-policy changes affect the pages that contribute to the changed metric's numerator or denominator. Artifact strategy and content-handling changes affect pages whose evidence would move to or be shown by the changed artifact/backend class; pure local-retention wording changes refresh reports but do not require source-derived calls unless the changed policy alters available evidence or backend eligibility. Budget, runtime, and cache policy changes affect pages that used the changed backend/engine class and always include the highest-cost and slowest pilot pages. Each tool-profile revision records `mini_pilot_impact_reason`, candidate affected page ids, selected mini-pilot pages, fallback rule used, and whether the operator approved or waived the scope.

After approval, the glossary is used primarily as adjudication context for the full run. Deterministic rewrites are allowed only when reversible and protected-zone aware. The glossary apply step uses the schema-wide protected-zone registry from **Markdown Assembly and Normalization**: equations, code blocks, file paths, figure IDs, table IDs, equation IDs, citations, quoted source text, page labels, legal/code references, numbers with units, and any span marked protected by schema or glossary. It never rewrites inside protected zones.

`noeasymark glossary apply` uses one deterministic matching algorithm. It builds match patterns from each approved entry's `surface_form`, `aliases`, and `misreadings`, after NFC normalization and Python `str.casefold()` for matching only. The active `glossary_match_profile` records the matcher id, Python minor version, `unicodedata.unidata_version`, boundary classifier, and transform hash. Patterns are literal strings, not regular expressions. A match is eligible only when the character before and after the match is absent or is not a Unicode letter, Unicode decimal digit, or underscore; this prevents rewriting substrings inside identifiers or ordinary words while still allowing punctuation-adjacent terms. The scanner walks each unprotected Markdown text segment left to right. At a position with multiple eligible matches, the longest normalized match wins; ties break by lower `entry_id`, then by the pattern source order `surface_form`, `aliases`, `misreadings`, then by the pattern's position inside that source list. Replacements write `normalized_form` exactly as stored in the approved entry; they do not preserve source casing. The replacement is then passed through the Markdown assembly escape policy so a glossary replacement cannot introduce unintended active Markdown, raw HTML, links, images, math, or table syntax. A rewrite is permitted only when the original matched text is recorded in a reversible sidecar entry containing `chapter_id`, `chunk_id`, `source_text_hash`, `output_path`, `byte_start`, `byte_end`, `entry_id`, `matched_text`, and `replacement_text`; `byte_start` and `byte_end` use the same zero-based UTF-8 half-open offset convention as `evidence_sidecar.block_entries`. If a candidate replacement would overlap a protected zone, an earlier accepted replacement, a Markdown link destination, an image path, an HTML tag, a fenced code block, an inline code span, a footnote label, or an autogenerated anchor, it is skipped and recorded as `skipped_protected_or_overlap` in the glossary-apply report. The subcommand is idempotent: running it twice over unchanged input produces byte-identical Markdown and a report with zero new replacements on the second run.

`glossary/candidates.yaml` and `glossary/approved.yaml` are YAML documents with a top-level mapping of `schema_id`, `schema_version`, `glossary_id`, `glossary_version`, `created_by`, `created_at`, and an `entries:` list whose items each validate against the `glossary_entry` schema. The two files share the same shape; the invariant differences are: `candidates.yaml` permits `status` values `candidate`, `rejected`, and `deferred`, while `approved.yaml` permits `approved` only; `entry_id` values must be unique within each file; no two approved entries may have the same casefolded NFC `surface_form`, alias, or misreading unless they share the same `normalized_form`; `surface_form` and `normalized_form` must be non-empty after Unicode whitespace trimming; alias and misreading arrays are duplicate-free after NFC normalization and casefolding; `approved.yaml` must be sorted by `entry_id` ascending; and every approved entry must carry `approver`, `approval_at`, and a non-empty `decision_rationale`. `noeasymark glossary apply` reads `approved.yaml`; `noeasymark glossary build` writes `candidates.yaml`; the reviewer promotes entries during `PILOT_GLOSSARY_REVIEW`.

Do not scale to the full book until the pilot report meets the targets recorded in the approved tool profile.

## Phase 10 - Per-Chapter QA and Local Review Reports

For each chapter, run:

- Source page coverage checks
- No chunk over hard maximum without override
- Adjacent-chunk duplicate-content detection across overlap ranges
- Adjacent-chunk overlap divergence detection that emits `OVERLAP_DIVERGENCE` when any overlap page has normalized similarity below the configured `overlap_divergence_threshold`, with `per_page_scores`, `minimum_similarity`, `pages_below_threshold`, and the closed normalization profile recorded in the QA result
- Multi-column reading-order checks
- Full-page versus region-by-region extraction comparisons for multi-column-risk pages, including per-region similarity, missing/extra spans, reading-order inversions, and protected-block drop failures
- Layout-detector audit diagnostics, including audit-budget cap exhaustion and visual-detector preflight references when those detector classes were enabled
- Every image link resolves to a deliverable-root-relative path inside `<output_root>/<output-folder-name>/`; absolute paths, URLs, path traversal, and symlink escapes fail QA unless a future artifact policy explicitly permits that link class
- Every figure has a caption or explicit missing-caption record
- Every visual crop obeys crop-padding policy or is flagged
- Caption-pairing, equation-transcription, multi-page visual stitching, and final visual-asset QA records validate against their closed rubrics
- Material-conflict audit records exist for every consensus candidate blocked by an independent family or lineage
- Every evidence JSON file validates
- Every final Markdown block has a matching evidence-sidecar entry
- Deterministic assembly normalizations are allowed by policy and recorded in evidence sidecars
- `markdownlint` (using the template configuration) and `remark` lint
- Math delimiter and parse checks against the pinned GFM math delimiters, using the `remark-math` plugin wired into the template's existing `remark` configuration (`$$ … $$` for display, `$ … $` for inline); `markdownlint` covers structural Markdown lint and `remark-math` covers math-delimiter validity, escape handling inside code spans, and lists.
- No U+FFFD replacement characters (`\uFFFD`) in generated Markdown, and no escaped `\uFFFD` in generated Markdown or evidence JSON unless a test fixture intentionally contains it
- No unresolved high- or normal-severity review markers; low-severity unresolved markers must be at or below `chapter_max_unresolved_review_markers` unless waived by an approved policy
- Restricted-diversity flag enumeration when the relaxed diversity rule was used
- No empty source-derived pages unless every corresponding `source_page_record.blank_page_status` is `intentionally_blank`
- Candidate length and block-density smoke checks
- Deterministic rerun check for unchanged inputs, comparing canonical hashes of final Markdown, image assets, evidence sidecars, chapter reports, and manifests while excluding only schema-declared volatile audit fields such as `generated_at`, `created_at`, and access counters

Adjacent-chunk duplicate-content QA uses the closed `duplicate_content_comparator_v1` profile. It builds normalized text windows of `duplicate_content_window_chars` over source-derived paragraph/list/caption-adjacent prose in adjacent chunks after applying the `overlap_low_risk_v1` normalization profile and removing review markers, generated blocks, accepted page furniture, index links, and blocks whose `normalization_events` identify legitimate repeated headers or footers. It excludes tables, equations, figure/table/equation labels, captions that are linked to different assets, legal/code reference lists, bibliography/reference sections, and intentional repeated warnings when their block kind or section context records them as repeatable. Each window stores source page indices, block ids, region refs, SHA-256 exact hash, and normalized text length. QA first matches exact hashes, then runs `rapidfuzz.distance.Indel.normalized_similarity` and emits a duplicate candidate when similarity is at least `duplicate_content_similarity_threshold`. A duplicate is blocking only when the two windows are outside the declared overlap ownership range or both copies would be published as independent body content; legitimate overlap handoff text, cross-references, repeated captions for different assets, and approved boilerplate produce `not_applicable` or `warn` records with exclusion reason codes. The QA result records compared window ids, exact-hash hit status, similarity, opcode trace hash, excluded-block counts, source-page pairs, and the evidence-sidecar refs needed to inspect the duplicate.

Multi-column reading-order QA uses the closed `reading_order_comparator_v1` profile. Phase 5 writes expected-order records for every multi-column-risk page with `source_page_index`, layout-region id, column id, row band, block role, expected predecessor/successor ids, and exception class (`none`, `caption_near_visual`, `sidebar`, `footnote`, `table_internal`, or `continued_block`). Phase 10 maps final `evidence_sidecar.block_entries` back to those records by region refs and emitted Markdown offsets, then computes pairwise order accuracy over comparable source-derived blocks. Captions may appear immediately before or after their linked visual, sidebars may appear at the nearest anchor boundary recorded by Phase 5, footnotes may be grouped at the end of the page or chapter when the footnote link remains intact, and table cell ordering is delegated to the table record rather than flattened into prose order. Inversions involving headings, protected numeric/unit spans, equations, table rows, safety/code/legal requirements, or cross-references are blocking regardless of aggregate score. Other inversions warn when pairwise accuracy falls below `reading_order_pairwise_warn_min` and fail when the approved multi-column reading-order metric crosses its blocking threshold. Ambiguous source-order records produce review markers with affected region ids instead of being silently accepted.

Candidate length and block-density smoke checks use the closed `candidate_density_smoke_v1` profile. The denominator is nonblank source pages in the chapter after excluding pages whose `blank_page_status` is `intentionally_blank` and pages whose selected output is intentionally image-only, table-only, equation-only, or crop-only under approved records. The numerator for character density is accepted source-derived Markdown characters after removing review-marker comments, generated index/front-matter blocks, and escaped Markdown syntax that is not source text. The numerator for block density is accepted source-derived `evidence_sidecar.block_entries` with `block_kind` other than `front_matter` or `index_link`. The source-derived block ratio is accepted source-derived blocks divided by all non-generated final blocks, with manual-review placeholders counted as source-derived only when they carry source refs. Starter warnings fire when character density, block density, or source-derived block ratio falls below the corresponding constants, or below `candidate_density_pilot_deviation_warn_ratio` times the matching pilot stratum baseline. The check records feature-specific exceptions for image-heavy, table-heavy, equation-heavy, intentionally blank, poor-text-layer, and reference-only pages; exceptions require source-page ids and accepted record refs. A warning becomes blocking only when paired with missing source-page coverage, missing evidence sidecars, unresolved normal/high review markers, or an approved quality-threshold policy that treats density regression as blocking.

Each check emits one `chapter_report.qa_check_results[]` record. A `fail` result with `blocking: true` blocks chapter publication and updates `chapter_manifest.manual_review_blocking_items` or the appropriate gate/link record. A `warn` result is included in the report and may proceed only when the approved quality-threshold policy permits it. `not_applicable` is used when the source lacks the relevant feature; `unavailable` is used when the feature exists but the measurement could not be computed.

Emit `<output_root>/<output-folder-name>/reports/chapter-NN.json` as a schema-valid `chapter_report` with agreement rates, multi-column risk pages, review marker count, restricted-diversity span count and redacted evidence matrices, overlap-divergence pages, glossary hits, unresolved risks, runtime, per-backend cost, normalized document-service usage counters, vendor cache hit rate, local response cache hit rate, and the full QA-check result list.

Each chapter is written into the deliverable folder under `output_root` (Markdown, final assets intended for review, evidence summaries, and the chapter report), and its review surface is the local `reports/chapter-NN-review.html`. The orchestrator does not wait for human review before advancing: it emits each chapter's deliverable and review report and proceeds with the next independent chapter unit in the work queue. The operator reviews chapters from the local reports in any order; cross-chapter finalization (Phase 12) requires every chapter to have reached its terminal `published` state in the deliverable.

The runtime refreshes the chapter-review HTML report at `<output_root>/<output-folder-name>/reports/chapter-NN-review.html` and updates the local run-status file with the relative path plus metrics.

The chapter-review HTML report is a static, local-only document with no remote scripts, no remote stylesheets, and no absolute host paths. It starts with a skip link and a landmarked summary containing source page range, chunk ids, publication state, blocking/warning count, and a link to the authoritative `reports/chapter-NN.json`. The main navigation groups anchors by summary, blocking checks, warnings, per-page review, unresolved manual-review items, visual/table/equation assets, evidence links, and generated metadata. Every crop preview has alt text derived from non-sensitive ids and source page numbers, not raw source text; keyboard focus order follows the same order as the visual layout; anchors are deterministic from ids and remain stable across report refreshes. QA result cards show the JSON authority fields (`check_id`, result, blocking flag, comparator id/version, affected block/page ids, waiver id, and evidence refs) before prose summaries. Unresolved risks are grouped by severity and source page, with relative links to manual-review files and evidence sidecars. The report footer records `run_id`, `chapter_id`, `tool_profile_hash_compact`, `render_profile_id`, schema versions, generation timestamp, and the hash of the JSON report it rendered from.

Quality-check failure response is type-differentiated and is performed exclusively by the shipped `noeasymark` CLI. The CLI **classifies** the failure and **surfaces** the next action; it does not invoke any judge backend or CLI-agent backend to auto-fix ambiguous failures, and it does not edit source files in response to ambiguous check signals. The supervising coding agent (separate from the shipped `noeasymark` runtime; this is the implementing agent or any coding agent the operator is running outside the CLI) is what executes the review workflow when one is requested.

- Deterministic generated-output formatter or lint failures: the CLI applies only deterministic local rewrites to the deliverable files it just produced, refreshes the affected report, and reruns the check up to 2 attempts. It never creates commits. Repository pre-commit and CI failures during the separate build-time implementation workflow are handled by the supervising coding agent outside the shipped runtime.
- Test failures, logic failures, smoke-check failures, reading-order failures, crop-policy failures, schema-design failures, or ambiguous failures: the CLI surfaces a coding-agent review request and pauses with exit code `90` (`SUPERVISING_AGENT_REVIEW_PENDING`). The request is written to `<output_root>/<output-folder-name>/reports/supervising-agent-review/<chapter_id_or_unit_id>.md` and the local run-status file is updated with a relative-path link. The supervising coding agent observes the file and performs the review workflow described below. During the separate build-time process for constructing `noeasymark`, the equivalent review request may be posted to the implementation phase PR through the MCP connector; that PR comment is build-time behavior, not runtime behavior.

The deterministic formatter/lint rewrite allowlist is closed. The CLI may normalize line endings to LF, ensure exactly one final newline, remove trailing spaces outside code/math/protected literal zones, apply formatter-stable indentation to generated list/table syntax already accepted by `markdown_structure_classifier_v1`, renumber ordered-list markers only when the source ordinal is not semantically meaningful and the sidecar already records `list_marker_normalize`, refresh generated report timestamps/hashes, and rewrite generated link fragments to the deterministic heading-anchor form when the target heading and sidecar id are unchanged. It must not alter source-derived words, punctuation, numbers, units, code/equation text, links/images, raw HTML escaping decisions, table cell content, glossary replacements, review-marker spans, or any protected-zone bytes. Every applied rewrite writes a `formatter_rewrite_event` with file path, byte range, rewrite kind, before/after hashes, protected-zone check result, and rerun attempt index. Ambiguous lint failures, parser kind changes, math parse changes, unsafe raw HTML, broken links, table shape changes, or fixes requiring source-text edits pause with `SUPERVISING_AGENT_REVIEW_PENDING`.

Every quality-check failure writes or updates a `normalized_failure_event` whose recurrence signature is derived from `check_id`, `failure_signature`, affected chapter/unit id, relevant schema version, and the active `tool_profile_hash_full`. This signature is the key used for the two-attempt recurrence rule below; free-text messages and absolute paths do not participate in recurrence identity.

Coding-agent review workflow prompt (literal text the CLI posts):

```text
Please double-check the code reviewer's recommendation or failing check. If the gap or concern is valid, think hard about possible ways to resolve the problem or address the feedback. List the options. Then, develop an evaluation rubric to score the options and determine which is best. Apply the evaluation rubric to the options and display the results/scores in a table. Then, use the table to select the best option. Finally, implement the necessary changes corresponding to the selected option.
```

If, after the supervising coding agent has acted and the quality check is re-run, the same failure signature recurs across 2 implementation attempts on the same unit, the CLI escalates by updating the same review file to add the failure history and pauses again with exit code `90`. The CLI never proceeds past a `SUPERVISING_AGENT_REVIEW_PENDING` halt without a fresh successful quality-check run or an explicit operator action.

## Phase 11 - Manual Review Workflow

When a chunk or span is marked `MANUAL_REVIEW`, the agent:

1. Records the item in the local run-status file and in the manual-review file (below), linking the chunk ID, source page range, candidate Markdown files, page image crops, layout-region crops, evidence sidecars, review markers, and adjudication ids explaining what could not be resolved. Full adjudication records remain in internal run state and are referenced by id only from the deliverable unless a future artifact policy explicitly promotes them; the deliverable surfaces redacted reason summaries through evidence sidecars and chapter reports. These are local files inside the deliverable; the runtime posts nothing to any remote surface.
2. Creates or refreshes one schema-validated file at `<output_root>/<output-folder-name>/reviews/manual/<chunk_id>.md` whose frontmatter conforms to the `manual_review_input` schema. The file aggregates all currently open manual-review items for that chunk, carries the item ids and body scope, and starts with `status: pending`. The body is the canonical location for the reviewer-supplied resolved Markdown for the declared scope.
3. Continues processing independent chunks. The blocked chunk's chapter is not finalized in the deliverable until the required manual-review files for that chapter are filled in or explicitly waived.

The initial release intentionally keeps the manual-review file as schema frontmatter plus reviewer body only. It does not insert non-schema instructional comments, editor hints, local preview command snippets, or duplicated evidence ordering into the review file. Reviewer guidance, evidence ordering, preview links, validation status, and report navigation live in the chapter-review report, run-status output, and manual-review validation diagnostics. Future template ergonomics require a new schema/profile version so added reviewer affordances remain deterministic, parser-safe, and auditable.

When a reviewer fills in the body, updates the frontmatter to `status: submitted`, and saves the file, `noeasymark status` treats it as ready only if the frontmatter validates, required reviewer fields are present, the body is non-empty for the declared scope, `body_sha256` matches the normalized body bytes, every referenced path passes its containment rule, and the body parses under the configured Markdown profile. If the file changes between two reads in the same status invocation, it is reported as `pending_write` and is not consumed. Invalid submitted files leave the work item blocked, record validation errors in the local run-status report, and do not trigger re-adjudication.

Submitted manual-review records must carry `reviewer_identity` and `reviewer_role`; waiver records must carry `approver` and `approver_role`. The starter role enum is `operator`, `maintainer`, `domain_reviewer`, `technical_reviewer`, `delegated_reviewer`, and `external_reviewer`, with profile-specific extensions allowed only through schema versioning. Identity source precedence is: explicit frontmatter or CLI flag, imported review metadata, then interactive defaults from run configuration. Git config user fields or OS usernames may be displayed only as local prefill suggestions and must be confirmed by the reviewer before submission; non-interactive ingestion requires explicit flags or imported metadata and must not infer identity from ambient machine state. Delegated review requires an append-only `delegated_reviewer_authorization` record in run state with delegator identity, delegate identity, delegated role, scope, rationale, creation timestamp, and optional expiry or revisit timestamp. Imported reviews preserve origin system, imported identity, imported role when present, and import timestamp; they are not silently converted to a local maintainer identity. Public reports redact direct identities unless the active public report schema explicitly allows role-only display, salted identity hash display, or another approved disclosure mode.

The next session re-runs adjudication for that chunk with the reviewer's text or validated structured patches supplied as an additional, highest-priority evidence source. Reviewer-supplied Markdown is still validated against the configured GFM profile, the Markdown assembly safety rules, raw-HTML policy, link/image containment rules, math delimiter policy, and the declared `body_scope`; it is never spliced into final output opaquely. The same code path produces the final merged output, records `decision_source: manual_review` in the adjudication log, links the resolving `manual_review_id` to resolved review markers, and updates the chunk/chapter status. The resolved review file remains as a permanent audit record. The runtime does not overwrite a submitted review file; if source inputs, candidates, or tool profile state change enough to invalidate the request, it creates a superseding review input with a new `manual_review_id` and marks the previous record `superseded`.

Manual-review conflict handling is conservative. A submitted body is identified by `manual_review_id`, `body_sha256`, `submitted_at`, `reviewer_identity`, `reviewer_role`, and `submission_format`. The first schema-valid submitted body that is successfully ingested for a `manual_review_id` becomes the accepted body snapshot and records `accepted_body_sha256`; later edits to the same review file do not alter published output and are reported as `amended_after_acceptance` until the operator creates a superseding manual-review input or waiver. If two schema-valid submitted bodies for the same unresolved review scope have different body hashes before acceptance, or if a body hash changes while the same status invocation is preparing to ingest it, the validation result records `manual_review_conflict`, the affected chapter remains blocked, and the run-status/chapter report lists the competing `manual_review_id`, body hashes, reviewer roles, and timestamps. Resolving the conflict requires an explicit superseding `manual_review_input` or `manual_review_waiver`; the runtime never merges reviewer bodies automatically and never uses last-write-wins semantics.

Manual review blocking policy:

- High severity: blocks affected chapter and final publication. Use high severity for missing or disputed technical meaning, numbers, units, formulas, safety/code/legal requirements, headings that affect structure, table values, figure/table/equation labels, captions that affect interpretation, cross-references, or any span where a wrong answer could materially change the document.
- Normal severity: blocks affected chapter and final publication unless explicitly waived. Use normal severity for uncertain prose, formatting that may affect meaning, noncritical caption wording, ambiguous glossary terms, and local assembly issues that should be resolved before publication.
- Low severity: may become a tracked follow-up only if the owner explicitly allows that class of follow-up. Use low severity for cosmetic Markdown formatting, optional alt text, nonblocking index polish, and other issues that do not change source meaning or traceability.
- Independent chapters may continue while a different chapter is blocked.

Waivers are schema-validated `manual_review_waiver` records written under `reviews/waivers/`. A waiver records the item id, severity, affected pages, blocks, and spans, rationale, approver, approver role, timestamp, scope, whether final publication is still allowed, expiry or revisit timing when applicable, and any follow-up owner. Waivers do not delete the original review marker, manual-review file, or adjudication record; they explain why the run may proceed despite it and are linked through append-only `review_marker_link` records. A waiver whose `final_publication_allowed` is false may unblock local processing for independent units but does not satisfy Phase 12 finalization.

## Phase 12 - Cross-Chapter Finalization

After every chapter has been written into the deliverable folder under `output_root` and reached its terminal `published` state:

- `finalize` first builds the complete expected chapter set from the non-superseded chapter manifests reachable from the approved outline and work queue, then verifies that those chapters cover the source page ranges declared for publication without gaps or duplicate ownership, that every expected chapter has terminal state `published`, and that no unresolved normal/high manual-review marker remains without a schema-valid waiver whose `final_publication_allowed` is true. If any chapter is missing, superseded, blocked, coverage-incomplete, or carrying a disallowed marker, `finalize` writes a `gate_record` with `gate_name: FINALIZATION_BLOCKED`, lists the exact chapter ids and the reason each is not finalization-ready in the gate record and in `aggregate_quality_report.finalization_preflight`, and exits with the `FINALIZATION_BLOCKED` exit code defined in **Exit Codes**. The operator clears the gate after publishing or waiving the listed chapters by running `noeasymark gate retry <gate_id>`; retry re-runs the finalization preflight and only resolves the gate when every chapter is finalization-ready. `finalize` never writes aggregate indexes while the gate is open.
- Per-chapter steps never write the aggregate indexes; finalization owns them. Specifically, `finalize` is the sole writer of `<output_root>/<output-folder-name>/README.md`, `<output_root>/<output-folder-name>/markdown/README.md`, and any chapter-spanning figure/table/equation indexes, so those shared navigation files have a single writer.
- Generate `<output_root>/<output-folder-name>/README.md` as the deliverable overview (document title, summary, content-handling mode summary, and a one-line pointer into `markdown/`).
- Generate `<output_root>/<output-folder-name>/markdown/README.md` as the Markdown navigation index (ordered chapter list with slugs and page ranges, generation metadata, tool-profile public snapshot reference, and glossary version in effect).
- Emit `markdown/00-front-matter.md` only when the source PDF has front matter (cover, copyright, dedications, table-of-contents preface, foreword); when the source has no such material, the file is omitted and the README navigation skips it.
- Generate `markdown/figures.md`, `markdown/tables.md`, and `markdown/equations.md` when the corresponding accepted record count is nonzero; omit the file when the count is zero and record the omission in `aggregate_quality_report.indexes_emitted`. Each emitted index entry includes the stable figure/table/equation id, chapter id, source page indices, caption or label text when present, deliverable asset path when applicable, and evidence-sidecar reference.

Final README and index templates are closed for the initial release. The root `README.md` uses this section order: H1 document title, `Generated by NoEasyMark` metadata line, source PDF summary, content-handling mode summary, Markdown navigation link, review-status summary, and file inventory. `markdown/README.md` uses this section order: H1 `Markdown`, generation metadata, glossary version, tool-profile public snapshot link, and a chapter table sorted by `chapter_index` ascending with front matter first when present. The chapter table columns are `Order`, `Chapter`, `Source pages`, `Markdown`, `Evidence`, and `Review report`; each link is deliverable-root-relative and every generated row has an `index_link` evidence block.

The generated `figures.md`, `tables.md`, and `equations.md` files use this section order: H1 plural index name, generation metadata, count summary, and one Markdown table. Figure rows sort by primary `source_page_index`, `chapter_index`, per-page visual ordinal, then `figure_id`. Table rows sort by primary `source_page_index`, `chapter_index`, table ordinal, then `table_id`. Equation rows sort by primary `source_page_index`, `chapter_index`, equation ordinal, then `equation_id`. Ties never depend on filesystem order. Generated block ids for README/index rows use the existing `bk-` prefix with a preimage of canonical JSON containing `index_kind`, target record id, destination Markdown path, row ordinal, and schema version. Each generated row's evidence sidecar block has `block_kind: index_link`, `acceptance_status: generated`, source page ids from the target record, and `candidate_refs` or `region_refs` pointing to the underlying figure/table/equation evidence rather than to generated prose.

Example figure index row shape:

```markdown
| Figure ID | Chapter | Source pages | Caption | Asset | Evidence |
| --- | --- | --- | --- | --- | --- |
| `fg-3f7a1e9b2c4d80` | `ch01` | 12 | Example caption | [image](../images/ch01-page-0012-fig-01.png) | [evidence](../evidence/figures.evidence.json#bk-3f7a1e9b2c4d80) |
```

- Re-run approved glossary consistency checks and record match counts, skipped protected-zone rewrites, glossary version, and any blocking inconsistencies in `aggregate_quality_report.glossary_summary`.
- Validate internal links and cross-references by parsing every final Markdown file and generated report link. Internal relative paths must resolve inside `<output_root>/<output-folder-name>/`; absolute paths, path traversal, and symlink escapes fail. Markdown heading fragments are checked with the GitHub-compatible heading-anchor algorithm, including duplicate-heading suffixes. Explicit custom anchors are allowed only when they appear in `manifests/custom-anchors.json` and satisfy the custom-anchor registry collision checks; arbitrary raw HTML anchors remain forbidden. External URLs are allowed only when they match `manifests/external-links.json`, carry an approved purpose/domain/source/approval record, and are inventoried in `aggregate_quality_report.link_validation_results`; finalization parses external URLs locally and does not fetch them. Duplicate generated anchors, broken fragments, custom-anchor collisions, or unapproved external URLs fail finalization unless an explicit artifact policy records a safe degradation such as preserving visible text without the link target.
- Detect orphan assets by building an `asset_inventory` from final images, chapter records, evidence sidecars, and generated indexes. Every file under `images/` must be referenced by at least one final Markdown file, emitted index, chapter report, or evidence sidecar, and every asset reference must resolve to an existing file. Unreferenced files and missing targets both block finalization unless an approved artifact policy classifies the file as retained-but-unlinked evidence.
- Populate `aggregate_quality_report.manual_review_summary` from the final set of non-superseded manual-review inputs, waivers, review markers, and chapter manifests. The summary includes unresolved counts by severity and chapter, aging buckets, median time to submission, median time to acceptance or waiver, waiver counts by severity, conflict counts, and per-chapter open review load. It omits direct identities and per-reviewer productivity metrics from public reports; direct reviewer identity remains available only in private manual-review or waiver records governed by the redaction policy.
- Produce `<output_root>/<output-folder-name>/reports/aggregate-quality.json` as the schema-valid `aggregate_quality_report`, the machine-readable aggregate quality and progress report.
- Write `<output_root>/<output-folder-name>/reports/aggregate-quality.html` as the final aggregate-quality local review report rendered from `aggregate-quality.json`.

The aggregate-quality HTML report is a static, local-only document rendered only from `aggregate-quality.json`, with no remote scripts, no remote stylesheets, no inline event handlers, and no absolute host paths. It starts with a skip link and landmarked summary showing run id, source page coverage, chapter count, publication readiness, finalization gate id when present, and a link to the JSON authority. Its navigation order is summary, finalization blockers, chapter table, quality metrics, manual-review summary, link and anchor validation, asset/orphan validation, glossary/content-handling summary, artifact/storage summary, and generation metadata. Blockers are grouped first by gate/finalization reason, then severity, then chapter/source page. Tables use deterministic anchors from record ids, visible focus states, scoped table headers, and text alternatives for any asset thumbnail. The footer records schema versions, tool-profile hash, source/outline/report hashes, generation timestamp, and redaction mode.

The initial release intentionally uses classic branch protection settings for this post-development handoff and does not require repository rulesets. If the operator chooses to implement equivalent rules through repository rulesets after development, that is an owner-side repository administration decision and should be recorded outside this generated TODO; it is not a shipped NoEasyMark runtime requirement.

The very last action the implementing agent performs at the conclusion of its end-to-end development of NoEasyMark is to write a `_TODO_AFTER-BUILD-MANUAL-STEPS.md` file at the repository root that enumerates the post-development manual GitHub setup steps the operator deferred so they would not impede initial development. The file is committed as part of the agent's final development PR (separate from any conversion-run final PR) and is removed by the operator once the actions inside it have been completed. The shipped `_TODO_AFTER-BUILD-MANUAL-STEPS.md` content is:

````markdown
# NoEasyMark Post-Development Manual Setup

Development is complete. The following manual GitHub repository settings were
deliberately deferred so they would not impede initial development. Apply them
before relying on the `main` branch for production-quality output, then delete
this file.

## 1. Configure branch protection on `main`

Path in the GitHub web UI:

> Settings → Branches → Branch protection rules → Add branch protection rule

Field-by-field settings:

- **Branch name pattern:** `main`
- **Require a pull request before merging:** ON
  - **Require approvals:** 1
  - **Dismiss stale pull request approvals when new commits are pushed:** ON
  - **Require review from Code Owners:** ON
  - **Require approval of the most recent reviewable push:** ON
- **Require status checks to pass before merging:** ON
  - **Require branches to be up to date before merging:** ON
  - **Status checks that are required** (select the actual check-run names that GitHub shows after the retained workflows have completed successfully at least once within the past seven days; do not type workflow filenames as placeholders, and regenerate this list from the retained workflow job names if Phase 0 changed them):
    - `Pre-commit CI / Pre-commit`
    - `Data file linting / Data file linting`
    - `Markdown Lint / markdownlint`
    - every retained `Python CI / Test (...)` matrix check
    - `Python CI / Type Check (mypy)` after Phase 0 has converted it from the template's advisory posture to an enforcing check
    - any additional optional-stack workflow/job checks the repository retained; leave these unselected if the corresponding workflow was removed during Phase 0.
  - If GitHub offers an **expected source** selector for a required check, choose the GitHub App or integration that actually owns that check (for example GitHub Actions for Actions workflow jobs or the Pre-commit CI app for Pre-commit CI) rather than "any source"; record any source that cannot be constrained in the deferred-items section below.
- **Require conversation resolution before merging:** ON
- **Require signed commits:** OFF unless the operator already uses signed commits everywhere
- **Require linear history:** ON
- **Require deployments to succeed before merging:** OFF (no deployments are configured)
- **Lock branch:** OFF
- **Do not allow bypassing the above settings:** ON
- **Restrict who can push to matching branches:** ON; allow only the maintainer. Do not add `github-actions[bot]` to the `main` push allowlist merely because `auto-fix-precommit.yml` is retained; that workflow pushes only to scoped Copilot branches and does not need direct push rights to `main`.
- **Allow force pushes:** OFF
- **Allow deletions:** OFF

Click **Create** (or **Save changes**) at the bottom.

## 2. Verify the rule applies

Open any pull request that targets `main` (for example the development handoff PR). The PR's
status panel should now show the required status checks and the
"Review required from Code Owners" pill. If any check is shown as "missing,"
either the check name in the rule is incorrect (re-check spelling) or that
workflow has never run on the repository yet — push a trivial commit on a
feature branch to trigger every retained workflow at least once, then re-edit
the protection rule to pick the checks up from the now-populated list.

## 3. Remove this file

After branch protection is in place and verified, delete `_TODO_AFTER-BUILD-MANUAL-STEPS.md` and commit.

On PowerShell:

```powershell
Remove-Item _TODO_AFTER-BUILD-MANUAL-STEPS.md
git add _TODO_AFTER-BUILD-MANUAL-STEPS.md
git commit -m "Remove post-development TODO after applying branch protection"
git push
```

On bash:

```bash
rm _TODO_AFTER-BUILD-MANUAL-STEPS.md
git add _TODO_AFTER-BUILD-MANUAL-STEPS.md
git commit -m "Remove post-development TODO after applying branch protection"
git push
```

## Additional Items Deferred During Implementation

When the implementing agent encounters a deferred item during Phase 0 or any later
phase (model-identifier verification deferred because vendor docs were unreachable,
CUDA image-tag verification deferred per the registry-failure clause, upstream
template visibility still private at handoff time, or any similar deferral the
spec explicitly authorizes), the agent appends the deferred item here as a new
numbered section with its own operator checkbox. The file as a whole is removed
only when every checkbox above is checked off and every item below this heading
has been resolved. The agent does not paraphrase the spec language that authorized
the deferral; it cites the spec section by name so the operator can find the
governing rule.
````

## Artifact Storage Strategy

Default strategy:

- The runtime writes the deliverable (final Markdown, final images, README files, redacted manifest copies under `<output_root>/<output-folder-name>/manifests/`, chapter reports, aggregate reports, completed manual-review files, the redacted tool-profile snapshot, and lightweight evidence summaries) to `output_root` as a commit-ready folder; it never commits. NoEasyMark's own pipeline code, schemas, prompts, and backend/engine/archive configurations live in the tool repository and are committed by the build-time process, not by a conversion run.
- Place the original PDF in `<output_root>/<output-folder-name>/original/` with Git LFS declared in the deliverable's `.gitattributes` (page-split above `lfs_max_file_bytes`), so a later operator commit stores it via LFS. The runtime records the source copy and any split parts in `artifact_usage_ledger` with `retention_class: commit_ready`.
- Keep renders, temporary crops, candidate outputs, OCR sidecars, raw model logs, full adjudication scratch files, per-call agent run directories, the response cache, and tool caches under the resolved artifact root (`.noeasymark-artifacts/<run_id>/` for run-scoped artifacts and `.noeasymark-artifacts/_shared/<output-folder-name>/` for folder-scoped caches) or an operator-configured mounted volume that is recorded as the artifact root. These paths stay gitignored and are never copied into the deliverable unless a later artifact policy explicitly promotes a narrow class. Every created, archived, pruned, or superseded artifact writes an `artifact_usage_ledger` event with content sensitivity, retention class, path root, relative path, size, hash, and producer unit.
- Store hashes and relative artifact references in manifests, evidence sidecars, and `artifact_usage_ledger` so a run can prove what it used without exposing absolute local paths.
- Optionally archive bulky run artifacts to object storage or shared storage after the pilot if portability across machines or CI runners becomes important, external artifact archive approval is granted at both folder and run scope, the artifact archive's network class is approved, the archive target is present in the configured allowlist, and the artifact class is explicitly eligible for that archive.

Each artifact archive configuration records `archive_id`, `network_class`, endpoint identifiers, credentials environment variable names, retention policy, encryption-at-rest expectation, provider verification method, whether raw source-derived content may be stored, and which artifact classes are eligible. It never stores credential values, signed URLs, or provider console export blobs. Before the first archive write in a run, `doctor --artifacts` verifies that the selected archive config, folder-scope approval, run-scope approval, allowlist entry, credential environment variables, and encryption/retention expectation are all present. Each archive operation writes an archive manifest with source artifact ids, source hashes, destination object refs, destination hashes or provider ETags when available, encryption mode observed or requested, retention class, expiration or legal-hold metadata when applicable, and the `artifact_usage_ledger` events that prove the copy. A failed archive verification leaves the source artifact local and records a blocking `normalized_failure_event`; it never deletes the local source.

The default `local-only` archive is not a remote archive. It stores artifacts only under the gitignored local artifact root, writes normal `artifact_usage_ledger` events, and has `network_class: device`.

Artifact-root paths are sharded deterministically. Run-scoped artifacts use `.noeasymark-artifacts/<run_id>/<artifact_class>/<aa>/<bb>/<artifact_id>-<safe-stem>.<ext>`, where `aa/bb` are the first four lowercase SHA-256 characters selected by `artifact_hash_shard_prefix_chars` from the artifact content hash when known and from the artifact id otherwise. Folder-scoped shared caches use `.noeasymark-artifacts/_shared/<output-folder-name>/<artifact_class>/<aa>/<bb>/...` with the same sharding rule. The `safe-stem` is ASCII slug text truncated to `artifact_filename_component_max_chars`, with a 6-character hash suffix appended before the extension when truncation occurs or a sibling collision exists. `init` and every artifact-writing command compute the fully resolved path and emit `PATH_LENGTH_WARNING` when it exceeds `artifact_path_warning_chars`; the warning names the longest predicted path and suggests a shorter `output_root` or mounted artifact root. The writer still rejects path traversal, absolute injected segments, and symlink escapes before creating the file.

Default retention and pruning are class-based and state-aware:

| Artifact class | Default retention trigger | Prune eligibility | Archive-before-prune default | Operator override field |
| --- | --- | --- | --- | --- |
| `deliverable` and original PDF copy | Retained outside prune control as commit-ready output. | Not pruned by `noeasymark cache prune`. | Not applicable. | Operator deletes deliverable manually. |
| `private_working_copy`, `render`, `temporary_crop` | Retain until owning chapter is `published`, then 14 days. | Eligible after the trigger when no unresolved normal/high review marker references it. | Optional when eligible archive exists. | `retention_policy.working_artifact_grace_days`. |
| `candidate_output`, `ocr_sidecar`, `adjudication_scratch` | Retain until run finalization, then 30 days. | Eligible after finalization unless referenced by unresolved review, waiver, evidence sidecar, or cache provenance. | Required when the artifact is source-derived raw and the approved archive declares it eligible; otherwise manual confirmation is required. | `retention_policy.audit_artifact_grace_days`. |
| `raw_model_log`, failed `agent_run` diagnostics | Retain until manual prune, or 90 days after finalization with `--include-failed-diagnostics`. | Eligible only when the operator passes the explicit failed-diagnostics prune flag. | Required when eligible archive exists; otherwise the prune command records the operator confirmation. | `retention_policy.failed_diagnostic_grace_days`. |
| Successful `agent_run` diagnostics | Retain until owning chapter is `published`. | Eligible immediately after publication when the unit had no failed attempt. | Optional. | `agent_runs_retention`. |
| `response_cache` and `tool_cache` | Cache policy by last access, byte ceiling, and tool-profile compatibility. | Eligible by cache policy only after any relied-on provenance has a `cache_provenance_archive` or still-resolvable cache entry. | Provenance archive required; raw response payload archive is optional and content-handling-gated. | `retention_policy.cache_ttl_days` and cache byte ceilings. |
| `superseded_chapter_artifact` | Retain 30 days after supersession and finalization of the replacing chapter. | Eligible when all review links point to the replacement or to retained evidence summaries. | Optional when eligible archive exists. | `retention_policy.superseded_artifact_grace_days`. |
| `external_archive_copy` | Governed by the archive provider policy and approval record. | Local prune never deletes the remote object directly. | Not applicable. | Archive provider lifecycle policy. |

Prune never deletes the minimum audit set required to understand a published deliverable: manifests, evidence sidecars, chapter and aggregate reports, manual-review inputs, waivers, review markers, ledgers, cache provenance snapshots that are still referenced, and hash/path records for pruned private artifacts. If an artifact is pruned, the ledger keeps the original artifact id, relative path, size, SHA-256 when known, prune timestamp, prune actor, retention rule id, and any archive manifest hash.

Archive verification adapters share a canonical result shape: `verification_method`, `provider_kind`, `object_ref`, `observed_size_bytes`, `observed_hashes`, `provider_etag` (nullable), `provider_version_id` (nullable), `encryption_mode_observed`, `key_or_scope_ref_hash` (nullable), `retention_or_immutability_observed`, `checked_at`, `attempt_count`, `consistency_status`, and `unavailable_reason` (nullable). `device_path_check` reads the local file under an approved artifact root, rejects symlink escapes, and verifies SHA-256 bytes directly. `head_object` retrieves provider object properties and metadata, verifies object identity, size, declared NoEasyMark SHA-256 metadata when present, provider checksum fields when they use a declared full-object algorithm, and observed encryption metadata; provider ETags are recorded but are not accepted as full-object SHA-256 unless the adapter proves that provider/object mode makes that true. `object_inventory` verifies only from a provider inventory report whose generation timestamp is after the archive write and whose object version or checksum fields match the archive manifest; stale or incomplete inventories produce `unavailable`, not success. `provider_api` is reserved for adapters that can prove a stronger provider-specific checksum or retention/encryption state than a generic object head call, and its adapter config must map every provider field into the canonical result shape. `manual_attestation` records the attester identity, role, observed object ref, observed size/hash method, encryption evidence description, timestamp, and rationale; it is acceptable only when the active artifact policy permits manual evidence for that artifact sensitivity class.

Provider-specific adapters must treat missing checksum fields, size mismatches, missing encryption metadata when encryption is required, unavailable inventory freshness, endpoint allowlist mismatch, credential failure, and raw-source-derived archive attempts against an archive with `raw_source_derived_allowed: false` as verification failures. The source artifact remains local until verification succeeds or the operator explicitly records a manual prune decision allowed by the retention policy.

The starter encryption policy accepts provider-default encryption at rest for external archives when the archive configuration sets `encryption_at_rest_required: true` and the verification adapter observes an allowed provider-managed mode. The initial release does not impose a stronger sensitivity-to-encryption matrix, customer-managed key requirement, client-side encryption requirement, or double-encryption requirement by content sensitivity. `secret_bearing` artifacts are never eligible for archive writes, because NoEasyMark must not create or persist secrets; source-derived private or raw artifacts may be archived only when external artifact archive approval, network-class approval, allowlist matching, `raw_source_derived_allowed`, and the archive verification policy all pass. A future security-policy version may strengthen encryption by sensitivity class only through a schema/profile bump.

Because committed PDFs must use Git LFS, the deliverable's `.gitattributes` declares the canonical binary-asset LFS block from **Phase 0**, and `doctor` verifies that the deliverable's `original/` PDF (or its split parts) is within `lfs_max_file_bytes`, that the LFS rules are present, and, when the deliverable lives inside a Git worktree with Git LFS installed, that `git check-attr filter -- <path>` resolves to `lfs` for every PDF and generated image asset. If the deliverable is outside a Git worktree or Git LFS is unavailable, `doctor` reports `lfs_pointer_unverified` with remediation steps rather than claiming the later commit is safe. The runtime performs no remote GitHub LFS quota or repository-setting check. A `.gitattributes` `binary` rule alone is insufficient because it prevents text conversion but does not move the file into Git LFS.

## Named Pipeline Signals

The canonical signal registry is `config/signals/signal-registry.yaml`, validated by the `signal_registry` schema. The registry is the source used to generate or validate this **Named Pipeline Signals** section, the conversion-runtime `progress.json` exit-signal enum, and the **Exit Codes** table. Each registry entry carries `signal_name`, `signal_scope`, `record_kind`, `exit_code`, `progress_enum_value` (nullable for development-time-only signals), `gate_name` (nullable), `recoverability` (`retryable`, `manual_input`, `configuration_change`, `informational`, or `terminal`), `default_resume_command` (nullable), `status_report_group`, `documentation_section`, and `recovery_example_id` (nullable). The registry also carries a top-level `recovery_examples` map keyed by those ids, with command examples, required prior edits, success summaries, and failed retry diagnostics. `noeasymark dev validate-signal-registry --check` fails if the prose list, progress enum, status template allowed signal set, or exit-code rows contain a signal not in the registry, omit a registry signal, assign a mismatched code/scope/record kind, or use an undocumented recovery command. Informational markers have exit code 0 in the registry. Development-time reserved codes remain in the registry with `signal_scope: development_time` but are excluded from the conversion-runtime enum.

Every named signal records `signal_name`, `signal_scope` (`conversion_runtime`, `development_time`, `doctor_finding`, or `informational`), `record_kind` (`gate_record`, `manual_review_input`, `work_queue_event`, `progress_ledger`, `doctor_finding`, or `review_marker`), `emitting_unit_id` when a work unit exists, `exit_code`, `opened_at`, `resume_command` (nullable), and `clears_when`. Conversion-runtime signals are emitted by the shipped `noeasymark` CLI only; development-time signals are emitted only by the implementing agent while building NoEasyMark and are never produced by ordinary conversion runs. When a conversion-runtime signal opens a `gate_record`, the record lives under `.noeasymark-state/<run_id>/gates/` and follows the local gate workflow from **Output and Review Surface**.

### Blocking Gates and Resumable Halts

1. `CHAPTER_MAP_REVIEW` - required when `outline_status` is `OUTLINE_PARTIAL`, `OUTLINE_LOW_CONFIDENCE`, or `OUTLINE_MISSING`, or when the operator passed `--require-chapter-map-review`. The gate record's `review_packet_paths` point to the chapter-map packet with proposed chapters, boundary evidence, confidence, chunk plan, unresolved items, coverage proof, and approve/request-changes instructions.
2. `PILOT_PLAN_REVIEW` - optional pre-pilot gate emitted by `noeasymark pilot --plan-only` when the operator wants to approve or edit the pilot page set, candidate backend plan, artifact strategy, and source-content movement plan before pilot calls run.
3. `PILOT_GLOSSARY_REVIEW` - always required after pilot execution; covers pilot metrics, glossary candidates, tool profile including judge backend bindings, artifact strategy, content-handling choices, quality thresholds, and budget/progress settings in one cycle. If the pilot page set is amended here, the pilot report is superseded and the changed scope must be run.
4. `BUDGET_THRESHOLD_HIT` - automatic halt when any configured hard limit (dollar spend, invocation count, runtime, wall-clock, artifact bytes, rendered-page bytes, or minimum free space) is reached. The producing unit records `limit_subreason: configured_ceiling_exceeded` and writes a `gate_record` containing the limit kind, configured ceiling, observed value, ledger refs, and remediation choices; `noeasymark gate retry <gate_id>` resumes only after the operator raises the ceiling, prunes artifacts/cache where applicable, or explicitly removes the hard limit in configuration.
5. `NO_PROGRESS_HALT_CALLS`, `NO_PROGRESS_HALT_TIME`, `NO_PROGRESS_HALT_UNITS`, `NO_PROGRESS_HALT_CHUNK`, `NO_PROGRESS_HALT_SIGNATURE` - independent named anti-spin halts; whichever fires first halts the run, and the halt name identifies the limit that tripped. The gate record includes the limit, observed count or duration, last schema-valid state transition, recent failure signatures, and the blocked unit id. Retry is allowed only after the operator changes the relevant limit/configuration or resolves the underlying repeated failure.
6. `JUDGE_BACKEND_BLOCKED` - automatic pause when judge backend failures require correction, such as outage, credentials, quota, connectivity, budget or credit exhaustion, repeated schema-output failure, missing CLI agent on `PATH`, missing network-class approval, backend not present in the configured allowlist, or undeclared transcript persistence. Provider-side quota, billing, or subscription-credit exhaustion records `limit_subreason: provider_quota_or_credit_exhausted` on the backend failure event. The producing unit writes a `gate_record`; after the operator corrects the cause, `noeasymark gate retry <gate_id>` performs the backend preflight and marks the gate resolved only if the preflight passes.
7. `ENGINE_BLOCKED` - automatic pause or skip-path decision when an engine cannot run because of credentials, network-class approval, allowlist mismatch, repeated failures, missing local dependencies, storage budget, or provider-side quota, billing, or subscription-credit exhaustion. Provider-side quota or credit exhaustion records `limit_subreason: provider_quota_or_credit_exhausted` on the engine failure event. The producing unit writes a `gate_record`; after the operator corrects the cause or edits the engine roster, `noeasymark gate retry <gate_id>` performs the engine preflight and marks the gate resolved only if the preflight passes.
8. `ENGINES_EXHAUSTED` - all engines failed for a chunk. The failing unit records failed `candidate_output` records, creates or refreshes the corresponding manual-review input, and exits with `ENGINES_EXHAUSTED` for the invocation that discovered exhaustion. Later invocations that are merely waiting for the reviewer return `MANUAL_REVIEW`.
9. `MANUAL_REVIEW` - per-chunk or per-span review tracked by `manual_review_input`, `review_marker`, and `work_queue_event` records rather than by a gate record; independent chunks may continue. The restricted-diversity rule routes here with exit code 30 when `restricted_diversity_acknowledgment` is not granted and only one engine family is available.
10. `PILOT_REGRESSION_HALT` - automatic halt when an approved full-run quality metric crosses its blocking threshold. The gate record names the metric id, comparator id/version, measured value, threshold, affected chapters, baseline pilot report, and permitted remediation path; it resolves only after an approved mini-pilot updates the tool profile or an explicit schema-valid waiver is linked.
11. `CACHE_CAP_HIT` - automatic halt when the response cache reaches a nonzero `response_cache_size_cap` and the eviction policy is `none`; resumes after `noeasymark cache prune` or a raised cap.
12. `SCHEMA_MIGRATION_BLOCKED` - automatic halt when a state file's schema version is older than the installed schema and no registered migration chain covers the gap. The gate record names the file, current schema id/version, installed schema id/version, missing migration edge, and whether read-only status reporting can continue.
13. `STALE_WORK_UNIT_BLOCKED` - automatic halt when stale work units block a command and the runtime cannot safely choose rerun, migration, waiver, or supersession. The gate record names affected unit ids, stale dependency hashes, publication impact, and permitted resolution paths; `noeasymark gate retry <gate_id>` rechecks hashes and resolves only after every affected unit is runnable, succeeded, superseded, migrated, or waived with valid refs.
14. `SUPERVISING_AGENT_REVIEW_PENDING` - automatic halt when a runtime quality check surfaces an ambiguous failure that requires supervising-coding-agent review; the CLI has written or refreshed the local review prompt and pauses until a fresh successful quality-check run or an explicit operator action.
15. `FINALIZATION_BLOCKED` - automatic halt when `noeasymark finalize` finds chapters that are missing, superseded, coverage-incomplete, duplicate-owned, blocked, or carrying unresolved normal/high manual-review markers without a schema-valid waiver whose `final_publication_allowed` is true. The gate record enumerates the chapter ids and the reason each is not ready. The operator publishes or waives the listed chapters, then runs `noeasymark gate retry <gate_id>`; retry re-runs the finalization preflight and resolves the gate only when every chapter is finalization-ready.
16. `OCRMYPDF_NOT_AVAILABLE` - automatic halt when `noeasymark preprocess` is about to issue an OCR call but OCRmyPDF cannot be invoked through the active profile: the module is not importable, `python -m ocrmypdf --version` or the configured host-native executable fails, or `doctor --quick` reports missing requested Tesseract language data or OCRmyPDF rasterizer/PDF-output dependencies. The gate record names the resolved OCR policy, resolved OCR languages, pages that would have been OCR-redone or force-OCRed, the generated OCRmyPDF `--mode` arguments and page ranges, and the two remediations: install OCR support via `uv sync --extra ocr` and re-run, or set `ocr_policy_default: none` in `pipeline.toml` (repository scope) or `ocr_policy_override: none` in `run_config.json` (per-run scope) and re-run. The gate is resolved automatically on the next `noeasymark run` once the operator has applied either remediation; no `gate retry` is needed because the resumption replays the same preflight.
17. `MANUAL_SETUP_INCOMPLETE` - development-time halt emitted by the Phase 0 manual-setup precheck (a build-time check the implementing agent runs through its GitHub MCP connector) when one or more GitHub-side configuration items required by the frontmatter `manual_setup_steps` block are missing and are not authorized for agent-side mutation. The development-time gate record under `.implementation-state/gates/` enumerates the missing items with the exact GitHub MCP action or web-UI path to fix each. The agent never mutates Discussions, vulnerability reporting, Dependabot, secret scanning, branch protection, or workflow permissions on the operator's behalf; the only agent-side mutation the precheck performs is creating the named labels (via the MCP connector) using the fallback colors recorded in the frontmatter, because the frontmatter explicitly authorizes that mutation.
18. `STACKED_REBASE_CONFLICT` - development-time halt emitted when the agent attempts to rebase a dependent Phase 0 sub-phase branch onto a newly merged parent and the rebase produces conflicts. The development-time gate record names the dependent branch, the parent branch, and the conflicting file paths. The maintainer resolves the conflict manually (rebase locally, force-push, or merge); the agent does not edit commit content as part of a rebase. After the maintainer pushes the resolved branch, the agent resumes normally in the active implementation session.
19. `PROTECTED_FILE_SCOPE_EXCEEDED` - development-time halt emitted when the implementing agent detects, during a protected-file edit, that the change it is about to make does not match one of the six authorized change kinds enumerated under the **Surgical-edit constraint on protected files** paragraph of Phase 0. The development-time gate record names the file, the proposed change, the closest matching authorized kind, and the reason the change exceeds that kind's scope. The agent does not edit the file; the maintainer either narrows the change to fit an authorized kind, authorizes a new change kind by amending `docs/specification.md` and `.template-sync/marker.yml`, or instructs the agent to skip the change.
20. `TOOLCHAIN_DRIFT` - doctor finding, not a conversion-run gate, emitted by `noeasymark doctor --devcontainer-fingerprint` or the nightly devcontainer byte-identity job when the observed base-image digest or active toolchain diverges from `.devcontainer/toolchain-fingerprint.json`. The process exits with doctor finding code 3 when this is emitted by `doctor`; the nightly job reports drift and waits for a deliberate fingerprint/expected-output refresh.

Recoverable signal examples are generated from the signal registry's `recovery_examples` block and must stay fixture-tested:

| Signal(s) | Required prior action | Command example | Success summary | Failed retry diagnostic |
| --- | --- | --- | --- | --- |
| `CHAPTER_MAP_REVIEW`, `PILOT_PLAN_REVIEW`, `PILOT_GLOSSARY_REVIEW` | Approve or edit the local review packet named in the gate record. | `noeasymark gate retry <gate_id>` | Prints `gate resolved`, updated tool/chapter state, and next runnable unit. | Prints remaining packet validation errors and leaves the gate open. |
| `BUDGET_THRESHOLD_HIT` | Raise the relevant ceiling, prune/cache where allowed, or remove the configured hard limit. | `noeasymark gate retry <gate_id>` | Prints observed value below new ceiling and resumes the blocked unit. | Prints the still-exceeded limit kind and current ledger value. |
| `NO_PROGRESS_HALT_*` | Change the failing backend/engine/configuration or raise the matching anti-spin limit with rationale. | `noeasymark gate retry <gate_id>` | Prints the reset signature/attempt counters and next runnable unit. | Prints the repeated signature and the unchanged limit. |
| `JUDGE_BACKEND_BLOCKED`, `ENGINE_BLOCKED`, `OCRMYPDF_NOT_AVAILABLE` | Install missing dependency, fix credentials, approve network class, or disable/replace the component. | `noeasymark gate retry <gate_id>` or rerun the blocked command for OCR. | Prints the preflight that passed and the component now selected. | Prints exact failed probe, platform, command path, and remediation hint. |
| `ENGINES_EXHAUSTED`, `MANUAL_REVIEW` | Fill and submit the manual-review file or create a valid waiver. | `noeasymark status` then rerun the blocked command. | Reports submitted/accepted review ids and unblocks the chapter. | Reports frontmatter/body validation errors and keeps the item blocked. |
| `PILOT_REGRESSION_HALT` | Run an approved mini-pilot or attach a schema-valid waiver. | `noeasymark gate retry <gate_id>` | Prints new baseline/waiver id and metric status. | Prints metric value, threshold, and missing approval or waiver refs. |
| `SCHEMA_MIGRATION_BLOCKED`, `STALE_WORK_UNIT_BLOCKED` | Install a migration, choose rerun/migration/waiver/supersession, or requeue stale work. | `noeasymark gate retry <gate_id>` | Prints migrated/requeued/superseded unit ids. | Prints unresolved schema edge or stale dependency hashes. |
| `CACHE_CAP_HIT` | Run cache prune or raise the cap. | `noeasymark cache prune --run-id <run_id>` then rerun. | Prints freed bytes and retained provenance count. | Prints cache entries still protected by referenced provenance. |
| `FINALIZATION_BLOCKED` | Publish missing chapters or attach valid final-publication waivers. | `noeasymark gate retry <gate_id>` | Prints all chapters finalization-ready and writes aggregate indexes. | Prints the exact chapters/reasons still blocking finalization. |
| `SUPERVISING_AGENT_REVIEW_PENDING` | Supervising coding agent applies the fix and the quality check passes. | Rerun the failed command. | Prints the cleared failure signature and refreshed report path. | Prints the repeated signature history and keeps the review prompt active. |
| Lock contention exit `80` | Wait for or clear the owning process/lock after operator confirmation. | `noeasymark status` or `noeasymark unlock <run_id>` | Prints active run state without mutation, or confirms lock cleanup only. | Prints lock holder metadata and refuses unsafe state edits. |

OCR dependency diagnostics use the closed `ocrmypdf_dependency_probe_v1` matrix. All platforms probe the Python module (`python -m ocrmypdf --version`), the configured host-native executable when one is set, Tesseract version, requested Tesseract language data via `tesseract --list-langs`, Ghostscript version, qpdf version, and the selected rasterizer/PDF-output helper profile. Linux and devcontainer probes also record package-manager family when detectable and whether the selected devcontainer variant declares OCR support. macOS probes record whether Homebrew paths or explicit operator paths satisfied Tesseract, Ghostscript, and qpdf. Windows probes check `PATH`, the configured explicit paths, and the standard OCRmyPDF discovery locations for Tesseract and Ghostscript; WSL is reported as Linux with the Windows host path redacted. Each failed probe emits a stable diagnostic id, platform id, command attempted, redacted executable path, missing language codes, expected remediation (`uv sync --extra ocr`, install host packages, select a no-OCR policy, or adjust `ocr_languages`), and a fixture id. Fixtures cover Linux/devcontainer, macOS, Windows, missing Tesseract, missing Ghostscript, missing qpdf, missing language pack, invalid explicit path, and OCR policy `none` bypass.

Per-chapter review walkthroughs of the local chapter-review report and the final walkthrough of the local aggregate-quality report are reviewer checklist items embedded in those reports. They do not create `gate_record` files, do not have `gate_name` values, and do not reserve exit codes.

### Informational Markers

These signals do not halt the run and resolve to exit code 0. They appear in `progress.json`, chapter reports, and the local run-status report so reviewers can spot conditions that merit attention.

1. `OVERLAP_DIVERGENCE` - chapter-QA warning when adjacent chunks disagree materially on any overlap page; QA records per-page scores, minimum similarity, pages below threshold, and the closed normalization profile, and the operator may record a per-chapter override.
2. `PATH_LENGTH_WARNING` - emitted on `init` when worst-case asset paths exceed 240 characters; informational only.
3. `RESTRICTED_DIVERSITY` - per-span review flag attached to spans accepted under the relaxed diversity rule after the operator granted `restricted_diversity_acknowledgment`; enumerated in the chapter report.

## Exit Codes

Every shipped `noeasymark` subcommand returns one of the conversion-runtime process exit codes below so CI orchestration and shell scripts can react deterministically. Codes 44-47 are reserved for the implementing agent's development-time workflow and are never produced by ordinary conversion-runtime subcommands. Multiple conditions in a single conversion-runtime invocation resolve by this priority order: usage parse errors (`1`) first; schema/configuration validation failures (`2`) second; doctor actionable findings (`3`) when the invoked command is `doctor`; lock contention (`80`) before any state mutation; then the lowest numeric halt code among simultaneously detected conversion-runtime halts; informational markers stay at `0`; unhandled internal errors use `99` only when no more specific code was produced.

| Code | Meaning |
| --- | --- |
| 0 | Success or informational marker only (includes `OVERLAP_DIVERGENCE`, `PATH_LENGTH_WARNING`, and `RESTRICTED_DIVERSITY` flagged spans). |
| 1 | Usage error (bad flag, missing required argument, no active run). |
| 2 | Schema validation failure on operator-supplied input, malformed state that cannot be read far enough to classify, or a state file whose installed schema id is unknown. A valid older state file with a known schema id but no registered migration chain uses `SCHEMA_MIGRATION_BLOCKED` (`60`) instead. |
| 3 | `noeasymark doctor` reports actionable findings: missing or unauthenticated CLI agent, missing required GPU for a selected engine, missing Docling model-cache prefetch, missing visual-detector preflight approval or local model artifact, missing credential environment variable, network-class or allowlist mismatch, undeclared transcript persistence, `delegates_to` chain too deep or self-referential, `unknown_cost_acknowledgment` missing for a configured unknown-cost backend, mandatory `invocation_ceiling` or `runtime_ceiling_seconds` missing for a `subscription_credit_pool` backend, `env_url_host` endpoint identifier whose named environment variable is unset or does not resolve to a URL with an extractable host, artifact archive approval/configuration/encryption verification failure from `doctor --artifacts`, `lfs_pointer_unverified` when deliverable LFS readiness cannot be proven locally, or `TOOLCHAIN_DRIFT` from `doctor --devcontainer-fingerprint`. Distinct from runtime halts so CI can shell-test `if noeasymark doctor; then ...`. |
| 10 | `BUDGET_THRESHOLD_HIT`. |
| 11 | `NO_PROGRESS_HALT_CALLS`. |
| 12 | `NO_PROGRESS_HALT_TIME`. |
| 13 | `NO_PROGRESS_HALT_UNITS`. |
| 14 | `NO_PROGRESS_HALT_CHUNK`. |
| 15 | `NO_PROGRESS_HALT_SIGNATURE`. |
| 20 | `JUDGE_BACKEND_BLOCKED`. |
| 21 | `ENGINE_BLOCKED`. |
| 22 | `ENGINES_EXHAUSTED`. |
| 23 | `OCRMYPDF_NOT_AVAILABLE`. |
| 30 | `MANUAL_REVIEW` pending (unit halted awaiting reviewer input). Also returned when restricted diversity routes a chunk to manual review because `restricted_diversity_acknowledgment` was not granted. |
| 40 | `CHAPTER_MAP_REVIEW` pending. |
| 41 | `PILOT_PLAN_REVIEW` pending. |
| 42 | `PILOT_GLOSSARY_REVIEW` pending. |
| 43 | `FINALIZATION_BLOCKED` - `noeasymark finalize` found chapters that are missing, superseded, coverage-incomplete, duplicate-owned, blocked, or carrying unresolved normal/high manual-review markers without schema-valid waivers whose `final_publication_allowed` is true. |
| 44 | `SCHEMA_DESIGN_REVIEW` pending - development-time only; the implementing agent has opened the milestone schema PR, written `.implementation-state/gates/schema-design-review.json` with `status: pending`, and returned control to the operator while awaiting operator PR review. Headless runtimes that support process status use exit code 44; interactive runtimes use the gate file plus PR review state as the durable signal and may not expose a literal shell status. Re-launching the agent after the PR review is recorded resumes Phase 0. This exit code is never produced by a `noeasymark` subcommand invoked during normal conversion runs. |
| 45 | `MANUAL_SETUP_INCOMPLETE` - development-time only; the Phase 0 manual-setup precheck found missing operator-side GitHub setup. |
| 46 | `STACKED_REBASE_CONFLICT` - development-time only; a stacked phase branch rebase produced conflicts that the maintainer must resolve. |
| 47 | `PROTECTED_FILE_SCOPE_EXCEEDED` - development-time only; the implementing agent detected a protected-file edit outside the spec-authorized scope. |
| 50 | `PILOT_REGRESSION_HALT`. |
| 60 | `SCHEMA_MIGRATION_BLOCKED`. |
| 61 | `STALE_WORK_UNIT_BLOCKED`. |
| 70 | `CACHE_CAP_HIT`. |
| 80 | Single-writer lock already held by another `noeasymark` invocation on the same run, folder-approval scope, or shared cache scope. |
| 90 | `SUPERVISING_AGENT_REVIEW_PENDING` - ambiguous runtime quality-check failure surfaced for supervising-coding-agent review; the CLI has written or refreshed the local review prompt and is awaiting a fresh successful quality-check signal or an explicit operator action. |
| 99 | Unhandled internal error. |

The exit-code rows are generated or validated from `config/signals/signal-registry.yaml`. The registry owns the numeric code, signal name, signal scope, record kind, whether the code is conversion-runtime or development-time-only, and the short meaning rendered above. `noeasymark dev validate-signal-registry --check` fails when this table contains a row not present in the registry, omits a registry code, or assigns a mismatched meaning/scope. The conversion-runtime subset of the same enum is recorded in `progress.json` so a resumed run can correlate the exit reason with the next runnable unit. Development-time reserved codes are recorded in `.implementation-state/` gate records instead.

Common shell handling examples:

```bash
noeasymark run
code=$?

case "$code" in
  0)
    echo "NoEasyMark completed or reported informational markers only."
    ;;
  3)
    noeasymark doctor
    ;;
  30)
    noeasymark status
    echo "Fill the listed reviews/manual/<chunk_id>.md file or create a waiver."
    ;;
  43)
    noeasymark status
    echo "Publish or waive the listed chapters, then run: noeasymark gate retry <gate_id>"
    ;;
  80)
    noeasymark status
    echo "Wait for the active writer or run: noeasymark unlock <run_id>"
    ;;
  90)
    noeasymark status
    echo "Open the supervising-agent review prompt listed in the status report."
    ;;
  *)
    echo "Unhandled NoEasyMark exit code: $code" >&2
    ;;
esac

exit "$code"
```

```powershell
noeasymark run
$code = $LASTEXITCODE

switch ($code) {
    0 {
        Write-Output "NoEasyMark completed or reported informational markers only."
    }
    3 {
        noeasymark doctor
    }
    30 {
        noeasymark status
        Write-Output "Fill the listed reviews/manual/<chunk_id>.md file or create a waiver."
    }
    43 {
        noeasymark status
        Write-Output "Publish or waive the listed chapters, then run: noeasymark gate retry <gate_id>"
    }
    80 {
        noeasymark status
        Write-Output "Wait for the active writer or run: noeasymark unlock <run_id>"
    }
    90 {
        noeasymark status
        Write-Output "Open the supervising-agent review prompt listed in the status report."
    }
    default {
        Write-Error "Unhandled NoEasyMark exit code: $code"
    }
}

exit $code
```

## Run Status File

Maintain one local run-status file per active run at `<output_root>/<output-folder-name>/reports/status-<run_id>.md`, titled `Conversion run: <document title> (<run_id>)`. The `<run_id>` segment disambiguates concurrent or successive runs of the same document. The per-document overview is served by the final aggregate-quality report. The conversion runtime opens no GitHub issue and performs no remote automation; this local file is the canonical human-readable run-status surface for the run. It is a derived Markdown report, not an independent state authority: every value comes from schema-valid run state, ledgers, gate records, chapter reports, manual-review records, or redacted public snapshots.

Sections:

- Current state
- Last exit signal and exit code
- Next action, including exact local command or review file path when one exists
- Next runnable or blocked work unit
- Spend and runtime per backend
- Progress rate
- Vendor cache metrics mode and hit rate, local response cache hit rate
- Tool profile including bound judge backends, models, network classes, and prompt versions
- Content-handling mode summary: which classes are approved and allowlist scope, without source-derived content
- Doctor health summary, including artifact archive and LFS-readiness findings
- Blocked gates with `gate_id`, signal name, opened timestamp, scope, relative review-packet paths, and resume command
- Judge backend or engine blocked state, if any
- Pilot metrics
- Relative chapter-review report paths
- Manual review items with ids, severity, affected pages/blocks/spans, relative review-file path, validation status, and waiver id when present
- Artifact and storage summary: deliverable size, private artifact size, cache size, artifact budget status, and archive status
- Known risks
- Finalization checklist

The exact Markdown template uses the heading order above. The H1 is `# Conversion run: <document title> (<run_id>)`. `Current state`, `Last exit signal and exit code`, `Next action`, `Spend and runtime per backend`, `Progress rate`, `Vendor cache metrics`, `Content-handling mode summary`, `Doctor health summary`, `Artifact and storage summary`, `Known risks`, and `Finalization checklist` render as two-column tables with columns `Field` and `Value`. `Next runnable or blocked work unit` uses columns `Unit ID`, `Type`, `Scope`, `Status`, `Attempts`, and `Next command`. `Blocked gates` uses `Gate ID`, `Signal`, `Scope`, `Opened`, `Resume command`, and `Status`. `Pilot metrics` and chapter metrics use `Metric`, `Value`, `Threshold`, `Status`, and `Evidence`. `Relative chapter-review report paths` uses `Chapter`, `State`, `Report`, and `Blocking items`. `Manual review items` uses `Review ID`, `Severity`, `Scope`, `Pages`, `Review file`, `Validation`, and `Waiver`. Empty sections render one row whose first cell is `No items.` and whose remaining cells are `n/a`. `not_applicable` values render as `Not applicable (<reason_code>)`; `unavailable` values render as `Unavailable (<reason_code>)`; missing optional paths render as `n/a`, never as an empty string.

Example blocked-gate row:

```markdown
| Gate ID | Signal | Scope | Opened | Resume command | Status |
| --- | --- | --- | --- | --- | --- |
| `gt-3f7a1e9b2c4d80` | `FINALIZATION_BLOCKED` | `run` | `2026-06-07T14:00:00.000Z` | `noeasymark gate retry gt-3f7a1e9b2c4d80` | `open` |
```

The renderer consumes a closed `run_status_report` template context with only these allowed field groups: run metadata (`run_id`, document title, output folder, generated timestamp), current state and next action, exit signal, work-unit ids and statuses, gate summaries, spend/runtime/cache metrics, tool-profile public summary, content-handling summary, doctor findings, pilot and chapter metrics, manual-review item summaries, artifact/storage totals, known-risk summaries, and finalization checklist items. Field values may contain ids, enum values, timestamps, counts, metric numbers, hashes, source page indices, severity labels, redacted reason summaries, and deliverable-root-relative paths. They must not contain source text excerpts, reviewer-resolved Markdown bodies, raw prompts, raw responses, raw adjudication reasons, absolute local paths, unredacted approval identities, or secret-bearing values. If an implementation needs a new status field, it adds that field to this template contract before rendering it.

The runtime rewrites this file using the same atomic text-output protocol as other generated reports after each phase, halt, resumed gate result, manual-review status change, or other material state change. If a crash occurs before the status file is refreshed, `noeasymark status` regenerates it from state before printing the console summary. Like every other file in the deliverable, it uses paths relative to the deliverable root and contains no absolute or host-local paths, so it remains safe to include if the operator commits the deliverable. It carries run metadata, metrics, and status only. It must not duplicate source-derived text snippets, reviewer-supplied resolved Markdown, raw prompts, raw model responses, raw adjudication reasons, credential names or values beyond approved public env-var names, signed URLs, full local artifact paths, or unredacted approval identities. When a status item needs to point at source-derived material, it links to the existing deliverable artifact and records ids, hashes, page numbers, counts, and redacted reason summaries rather than copying the text.

`run_status_snapshot` required fields: state_record mixin, `run_id`, `report_markdown_path`, `generated_at`, `current_state`, `last_exit`, `next_action`, `work_units`, `gate_summaries`, `spend_runtime_summary`, `cache_summary`, `tool_profile_summary`, `content_handling_summary`, `doctor_findings`, `pilot_metrics`, `chapter_reports`, `manual_review_items`, `artifact_storage_summary`, `known_risks`, `finalization_checklist`, and `generation_metadata`.

`noeasymark status --json` emits a schema-valid `run_status_snapshot` containing the same field groups and redaction boundaries as the Markdown status report. The JSON snapshot is the render input for the Markdown report: every Markdown table row must be derivable from the JSON without reading private state again, except for the report generation timestamp. Field names are snake_case, enum values match the signal registry and schema enums exactly, paths are deliverable-root-relative, and unavailable/not-applicable states use `{ "status": "unavailable"|"not_applicable", "reason_code": "..." }` rather than prose-only strings. `status --json` never dumps raw internal state files, raw ledgers, source text, reviewer bodies, prompts, responses, credentials, signed URLs, absolute paths, or unredacted identities.

The initial release intentionally keeps the status surface current-state only. It does not append history to `status-<run_id>.md` and does not ship `status --history` or `status --since`; operators use the append-only ledgers and gate records for forensic history. Future observability work may add history views only if it derives them from ledgers without bloating the current-state report.

The status file is emitted as GitHub-Flavored Markdown under the same Markdown safety profile as generated reports: UTF-8, LF line endings, one final LF, no raw HTML, no remote links unless an approved artifact policy allows them, and no link target outside `<output_root>/<output-folder-name>/`. It may be regenerated byte-identically for unchanged state except for explicitly volatile timestamps declared in the report generation metadata.

## Quality Expectation

Do not rely on a generic percentage estimate, a single aggregate score, or a model's self-reported confidence.

Quality is measured from the pilot and reported throughout the run using the approved metric specs stored in `tool_profile.approved_metric_specs`. Each metric uses its recorded comparator id/version, transform profile id/hash, direction, numerator/denominator definitions, sample scope, waiver policy, and `not_applicable` / `unavailable` behavior. Required quality metrics include error rate against the gold set, review marker density, figure-caption accuracy, heading accuracy, multi-column reading-order accuracy, terminology consistency, unresolved manual-review count, cost per page per backend, runtime per page per backend, vendor cache hit rate, local response cache hit rate, and progress per session. Operational metrics such as cost, runtime, and cache hit rate are reported alongside quality metrics but do not satisfy source-fidelity quality requirements; they halt only when the approved threshold policy gives them a blocking threshold.

When any approved full-run metric crosses its blocking threshold, the pipeline halts with `PILOT_REGRESSION_HALT`, writes or refreshes the gate record named in **Named Pipeline Signals**, updates the local run-status file, and records the metric id, measured value, threshold, comparator, affected chapters, and baseline pilot report. It resumes only after a targeted mini-pilot establishes a new approved baseline or the operator records an explicit schema-valid waiver. Warning thresholds are reported in chapter and aggregate reports; they do not halt unless the approved quality-threshold policy says a warning class is blocking for that run.

The approved tool profile must record the metric target, warning threshold, blocking threshold, and measurement method for each quality metric. If a metric is unavailable because the document lacks that feature, the report records `not_applicable`. If a metric is unavailable because no gold set, comparator, tool support, or source evidence exists, the report records `unavailable` and full-run scale-up remains blocked unless the operator explicitly waives that metric at the appropriate gate.

Terminology consistency uses the closed `terminology_consistency_v1` comparator. The denominator is every eligible source-derived text span that matches an approved glossary entry's `surface_form`, `aliases`, or `misreadings`, plus every human-reviewed terminology decision recorded in gold-set entries or manual-review resolutions. Protected zones, code/equation text, link destinations, image paths, raw HTML tags, generated index text, page furniture, and spans whose source text is unavailable are excluded. A numerator success requires the final Markdown to use the approved `normalized_form` or an explicitly approved alias for that entry while preserving surrounding source meaning. False positives include glossary terms inserted where no eligible source span or reviewer decision supports them, replacements inside excluded zones, and conflicting normalized forms for the same approved entry within the same source scope. Each result records glossary version, matcher profile, eligible span count, success count, false-positive count, false-negative count, skipped-protected count, and example refs capped by the evidence-packet redaction policy.

Chapter-to-run rollups are defined per metric in `tool_profile.approved_metric_specs` and are reported with numerator, denominator, weighting policy, chapter count, and worst-chapter summary. Gold text error rates roll up as micro-averages by summing edit-distance numerators and character/word denominators across approved gold entries. Heading, figure-caption, table, equation, multi-column reading-order, and terminology metrics roll up by summing their comparator numerator/denominator pairs, with a separate worst-chapter row for reviewer attention. Review-marker density is total unresolved non-waived markers divided by total accepted source-derived blocks. Unresolved manual review count and waiver count roll up as sums by severity. Cost per page is total cost divided by published source pages, with per-backend subtotals. Runtime per page reports median, p95, and worst chapter from per-page runtime observations rather than only a mean. Vendor and local cache hit rates are total hits divided by total eligible lookups by cache metric mode. Any metric whose chapters mix incompatible comparator versions is `unavailable` until the operator approves a migration or waiver.

## Operator Inputs Still Needed at Runtime

These are operational inputs, not unresolved design decisions. They are not all collected in one prompt: `init` collects run-configuration inputs, `doctor` verifies local prerequisites, and pilot or review gates collect correction and approval inputs only when the run reaches those gates. Every approval or declined approval is persisted as schema-valid state rather than inferred from omitted flags.

The runtime must never persist credential values. For backend setup it may record backend ids, role preferences, credential-source names such as environment-variable names, CLI authentication status booleans, and redacted diagnostics. It must not write API keys, access tokens, auth-cache contents, raw secret environment-variable values, signed URLs, or provider credential files into `run_config.json`, `tool_profile.json`, reports, logs, status files, public snapshots, or deliverables.

Interactive `noeasymark init` prompts in this fixed order, skipping any field already supplied by a CLI flag or imported `init_bundle` value:

1. Required identity: source PDF path, document title, local processing acknowledgment, and optional topic description.
2. Output and storage: output folder name preview/override, `output_root`, `lfs_max_file_bytes`, artifact/free-space ceilings, default diagram text policy, render-profile alias, OCR policy, and OCR languages.
3. Content handling approvals: perimeter, internet, external artifact archive identity, per-run archive mode, allowlists, restricted-diversity acknowledgment, unknown-cost acknowledgment, and approval audit identity/role/rationale.
4. Backend and engine hints: optional pre-pilot backend preferences by role, credential-source names, cost-tracking mode acknowledgments, and required invocation/runtime ceilings for unknown or subscription-credit-pool backends.
5. Budgets and run limits: spend, invocation, runtime, wall-clock, rendered-page bytes, and minimum free-space hard ceilings.
6. Pilot and glossary hints: pilot page hints, `require_pilot_plan_review`, and seed glossary terms.

Each prompt displays the resolved default, the owning schema field, whether the value will be stored in run state or folder state, and the validation rule that will apply. A prompt whose value remains omitted must either use the schema default or fail validation immediately; `init` does not continue with an implicit unknown approval, empty audit identity, or missing local-processing acknowledgment.

Backend credential preflight diagnostics are keyed by backend kind and cost-tracking mode:

| Backend/cost class | Preflight checks | Redaction-safe finding | Remediation hint |
| --- | --- | --- | --- |
| `cli_agent` backends | Executable on `PATH`, version probe, adapter-declared auth probe, schema-output smoke against synthetic prompt, transcript-persistence declaration. | `cli_agent_auth_unavailable:<backend_id>` or `cli_agent_missing:<backend_id>` with executable name only. | Install the CLI, run the backend's documented login command outside NoEasyMark, then rerun `noeasymark doctor --backends`. |
| `litellm` or API-env backends | Required credential env var names are set, endpoint identifiers/hosts match allowlist, synthetic schema-output smoke passes, model id is configured. | `credential_env_missing:<env_var_name>` or `endpoint_allowlist_mismatch:<backend_id>` without env values or raw URLs. | Set the named env vars or fix the endpoint allowlist; never paste credential values into NoEasyMark config. |
| `device` local model backends | Local model artifact exists, model id/version matches config, no network class approval needed, synthetic smoke passes. | `local_model_unavailable:<backend_id>:<model_ref>` with redacted path. | Install or prefetch the local model, or disable/replace the backend before pilot. |
| `cost_tracking.mode: unknown` | `unknown_cost_acknowledgment`, `invocation_ceiling`, and `runtime_ceiling_seconds` are present. | `unknown_cost_acknowledgment_missing` or `unknown_cost_ceiling_missing`. | Re-run init/import with the acknowledgment and both ceilings, or choose a known-cost backend. |
| `cost_tracking.mode: subscription_credit_pool` | `invocation_ceiling` and `runtime_ceiling_seconds` are present regardless of unknown-cost acknowledgment. | `subscription_credit_pool_ceiling_missing`. | Add both ceilings so a credit-pool backend cannot run unbounded. |
| `zero_subscription` or `runtime_only` | Optional ceilings are displayed but not mandatory; runtime still records invocations and elapsed time. | Informational `cost_ceiling_not_configured:<backend_id>`. | Add ceilings if the operator wants stricter operational bounds. |

Run initialization and preflight inputs:

- Source PDF path.
- Document title.
- Local processing acknowledgment confirming local or devcontainer processing, subject to the operator's rights, licenses, and source-document terms.
- Optional one-sentence topic description.
- Optional output folder name override.
- Optional output location (`output_root`, default `./output/`). The runtime writes the commit-ready deliverable there and never commits it.
- Optional maximum committed-file size (`lfs_max_file_bytes`, default 2 GiB). This is the operator's declared per-file ceiling for later Git LFS commits; it drives source-PDF page splitting when needed and does not prove the remote host's current plan, quota, or storage availability.
- Optional hard minimum-free-space floor.
- Optional default diagram text policy (`crop_only` or `transcribe`).
- Optional render-profile alias override. If omitted, the run uses `default-300dpi`; a 400 DPI alias must pass artifact-budget preflight before rendering.
- Optional per-run OCR policy override. If omitted, the run uses `pipeline.toml`'s `ocr_policy_default`.
- Optional per-run OCR languages. If omitted, the run uses `pipeline.toml`'s `ocr_languages_default`; values are Tesseract language identifiers validated during preprocessing against installed language data.
- Optional pre-pilot backend preferences by role. Init records preferred backend ids as soft hints; pilot validation records the actual tool-profile bindings. When a `cli_agent` backend such as Codex CLI or Claude Code is preferred or bound, the operator must install and authenticate that CLI outside NoEasyMark (`codex login`, `claude auth login`, or equivalent) and set any required environment variables. When a `litellm` backend is preferred or bound, the operator must set the provider-required environment variables outside NoEasyMark.
- Perimeter processing approval with explicit `mode` (`all` or structured allowlist) covering perimeter backends, engines, and artifact archives.
- Internet processing approval with explicit `mode` (`all` or structured allowlist) covering internet backends, engines, and artifact archives.
- External artifact archive decisions, split into the folder-scope archive identity and retention approval and the per-run archive network-class `mode` (`all`, structured allowlist, or deny). A run may use an external archive only when both the folder-scope archive approval and this run's network-class approval/allowlist permit it.
- Optional restricted-diversity acknowledgment when content-handling restrictions or local engine availability prevent assembling two engine families.
- Optional unknown-cost acknowledgment when any enabled or tool-profile-bound backend declares `cost_tracking.mode: unknown`. When granted, the operator must also supply both an `invocation_ceiling` and a `runtime_ceiling_seconds`.
- Mandatory `invocation_ceiling` and `runtime_ceiling_seconds` for any enabled or tool-profile-bound backend that declares `cost_tracking.mode: subscription_credit_pool`, regardless of the unknown-cost acknowledgment.
- Optional hard spend ceiling, enforced only for dollar-denominated backends.
- Optional invocation ceiling or runtime ceiling for `zero_subscription` and `runtime_only` backends. These are recommended but not required.
- Optional hard wall-clock ceiling.
- Approval audit identity, role, and rationale for approval or denial decisions that will be persisted. Interactive prompts collect these fields when they are missing; non-interactive invocations must supply them through flags or an imported init bundle.

Minimal `init_bundle` example:

```json
{
  "schema_id": "init_bundle",
  "schema_version": "v0001",
  "source_pdf_path": "corpus/comed-blue-book-2025/source.pdf",
  "source_pdf_path_kind": "repo_relative",
  "document_title": "ComEd Blue Book 2025",
  "local_processing_acknowledgment": true
}
```

Full non-interactive `init_bundle` example shape:

```json
{
  "schema_id": "init_bundle",
  "schema_version": "v0001",
  "source_pdf_path": "corpus/comed-blue-book-2025/source.pdf",
  "source_pdf_path_kind": "repo_relative",
  "document_title": "ComEd Blue Book 2025",
  "local_processing_acknowledgment": true,
  "topic_description": "Electric utility service requirements and distributed energy interconnection guidance.",
  "output_folder_name": "comed-blue-book-2025",
  "output_root": "./output",
  "lfs_max_file_bytes": 2147483648,
  "pilot_pages": [1, 5, 12, 24],
  "require_pilot_plan_review": true,
  "seed_glossary_terms": ["service drop", "meter socket", "point of interconnection"],
  "diagram_text_policy_default": "crop_only",
  "ocr_policy_override": "conditional_redo_then_force",
  "ocr_languages": ["eng"],
  "render_profile_alias_override": "default-300dpi",
  "spend_ceiling_usd": 25.0,
  "invocation_ceiling": 200,
  "runtime_ceiling_seconds": 14400,
  "wall_clock_ceiling_seconds": 28800,
  "artifact_bytes_ceiling": 53687091200,
  "rendered_page_bytes_ceiling": 10737418240,
  "minimum_free_space_bytes": 21474836480,
  "restricted_diversity_acknowledgment": false,
  "unknown_cost_acknowledgment": false,
  "pre_pilot_backend_preferences": {
    "default_judge": "codex-cli",
    "fallback_judge": "litellm-openai"
  },
  "approval_audit_default": {
    "decided_by": "operator@example.invalid",
    "decided_by_role": "maintainer",
    "rationale": "Initial local conversion; only the listed perimeter backend may receive source-derived evidence."
  },
  "perimeter_processing_approval": {
    "decision": "approve",
    "mode": "allowlist",
    "allowlist": [
      {
        "kind": "litellm_provider",
        "identifier": "azure"
      }
    ]
  },
  "internet_processing_approval": {
    "decision": "decline"
  },
  "external_artifact_archive_identity": {
    "decision": "decline"
  },
  "external_artifact_archive": {
    "decision": "decline"
  }
}
```

Pilot, glossary, and tool-profile inputs:

- Any known pages that should be included in the pilot set, supplied as `source_page_index` values in 1-based physical PDF page order. If omitted, the tool auto-selects a representative pilot set. Page hints are validated after preprocessing knows the source page count; invalid indices fail validation rather than being silently clipped or interpreted as printed page labels.
- Optional approval or edits to the pilot plan before source-derived pilot calls, when the operator requests `PILOT_PLAN_REVIEW`.
- Gold-set corrections or approval for the pilot subset before gold-based quality metrics can pass.
- Any known terminology, symbols, or jargon that should seed the glossary.
- Approval, amendment, or waiver decisions at `PILOT_GLOSSARY_REVIEW` for the gold set, glossary, tool profile, judge bindings, engine set, artifact strategy, quality thresholds, budget settings, and any unavailable quality metric.

Later gate or as-needed inputs:

- Manual-review corrections, waivers, and reviewer identity for chapter review files.
- Tool-profile amendments after pilot approval, plus the resulting targeted mini-pilot approval or explicit schema-valid mini-pilot waiver.
- Finalization waivers for unresolved normal or high manual-review items only when the waiver schema permits final publication.

Repeated-run source-path examples intentionally remain in the `noeasymark init` CLI contract earlier in this specification, where `--fresh`, `--reuse`, `--import`, and explicit `--source` replacement behavior are defined together with active-run detection. This runtime-input checklist does not duplicate those examples; `init` usage errors and status output should link back to that canonical source-path section rather than maintaining a second copy.

## Operating System Support

NoEasyMark targets three first-class operating-system families: mainstream supported distributions of Linux, supported versions of macOS, and supported versions of Windows. The detailed support contract is the one in **Cross-platform Toolchain Installation (Host-Native)**: supported means the upstream OS release remains in its vendor support window, the required native tools can be installed through the documented path or a vendor-equivalent path, and `noeasymark doctor` passes the host capability checks.

All three OS families are exercised by the cross-platform CI matrix described in **Phase 0**, run host-native using the same `uv`-based toolchain documented in **Cross-platform Toolchain Installation (Host-Native)**, and produce the schema-equivalent, evidence-comparable conversion outputs that the rest of this specification depends on. "Identical inputs" for cross-platform output comparison purposes means the same source bytes, run configuration, committed lockfile, render profile, tool profile, source manifest, cache policy and cache inputs, and exact external-tool version set (qpdf, mupdf-tools or `mutool`, ghostscript, tesseract-ocr plus language data, poppler-utils, pandoc, PyMuPDF, and any active conversion-engine binary). When those inputs and tool versions match, generated text outputs are expected to be byte-identical; when host-native external tool versions or runner images diverge, the comparison standard is schema-equivalent and evidence-comparable. The host-native cross-platform CI matrix (`cross-platform-check.yml` on `ubuntu-latest`, `macos-latest`, `windows-latest`) asserts only the schema-equivalent standard and refuses byte-identity assertions, because the `-latest` runner labels are moving GitHub-hosted images and do not guarantee tool-version parity across OS families. Each matrix job records the resolved runner image metadata exposed by GitHub's setup logs or environment into the CI summary and uploads a schema-valid `runner-image-metadata.json` artifact containing runner label, OS family/version, image version fields exposed by the runner, tool versions, workflow/job ids, generated timestamp, and toolchain-fingerprint comparison result. The CPU devcontainer variant is the byte-reproducibility target only when its active `.devcontainer/toolchain-fingerprint.json` matches the committed fingerprint: a nightly job in `cross-platform-check.yml` builds the CPU variant from its floating base tag, verifies that the observed digest and active toolchain match the committed fingerprint, and asserts byte-identity against a committed expected-output set only after that fingerprint check passes. A fingerprint mismatch is reported as toolchain drift and requires a deliberate fingerprint/expected-output refresh rather than being misclassified as a conversion regression.

| OS family | Supported versions | Host-native execution | Devcontainer execution | CUDA / GPU engines | Notes |
| --- | --- | --- | --- | --- | --- |
| Linux | Ubuntu 24.04 LTS or later Ubuntu releases still in Canonical standard support; Debian 12 or later while under Debian security or LTS support on the target architecture; currently maintained Fedora releases; other mainstream distributions providing comparable kernel and glibc versions plus the required native packages | Fully supported | CPU and CUDA variants both supported | Supported on x86_64 with NVIDIA Container Toolkit | Primary CI substrate; doctor reports EOL or missing package-manager support as an actionable finding |
| macOS | Current macOS major release and the two prior major release lines, as reflected by Apple's current-version and compatibility documentation, on Apple Silicon or Intel hardware that Apple officially supports for the chosen release | Fully supported | CPU variant supported through Docker Desktop or OrbStack; CUDA variant unavailable (no NVIDIA hardware) | Not supported; CPU-only engines remain fully usable; Metal-accelerated engines may be added later but the initial engine set ships none | Apple Silicon and supported Intel combinations are validated; future macOS release lines that drop Intel support do not extend Intel host support |
| Windows | Windows 11 24H2 and later while in support; Windows Server 2022 and later while in support; Windows 10 22H2 only with active Microsoft Extended Security Updates enrollment; Windows 10 LTSC/LTSB editions only while their Microsoft lifecycle remains active | Fully supported when the release is still supported | CPU variant supported through Docker Desktop with WSL2 backend; CUDA variant requires Docker Desktop with WSL2 backend and an NVIDIA driver supporting WSL2 GPU pass-through | Supported through WSL2 GPU pass-through | Windows 10 22H2 without ESU/LTSC coverage is unsupported; long-path support is automatically bypassed by the runtime's `\\?\` prefix usage, while doctor still warns when `LongPathsEnabled` is `0` |

Capabilities that are not portable across all three families are surfaced by `noeasymark doctor` rather than silently disabled. The CUDA-dependent engines are the only initial-engine-set features unavailable on macOS; the CPU engine set provides full conversion coverage on every supported OS. Cross-platform contributors and reviewers can confirm the host's full status by running `noeasymark doctor`, which reports OS family, lock primitive, process-supervision primitive, atomic-write primitive, long-path support, and native-tool availability with actionable per-OS install commands.

The runtime relies on no Linux-specific, macOS-specific, or Windows-specific behavior beyond the OS-native primitives the spec explicitly enumerates: POSIX process groups (Linux, macOS) or Windows job objects for child cleanup; `os.fsync` plus `os.replace` for atomic write on all three; `fcntl.flock` (Linux, macOS) or `msvcrt.locking` (Windows) for single-writer locking; and the platform-native temp directory for per-call CLI-agent scratch. Where a spec phase depends on optional external tools, the per-OS install commands in **Cross-platform Toolchain Installation (Host-Native)** are the canonical reference.

OS lifecycle classification uses committed data, not live vendor calls during ordinary `doctor` runs. The file `config/os-lifecycle/facts.json` records vendor, product family, version, support channel, architecture scope when relevant, support status, support end date when known, source URL, retrieved timestamp, and notes for each supported or explicitly unsupported edge case. `noeasymark dev refresh-os-lifecycle --write` refreshes the file from vendor sources for maintainer review; `--check` fails when the file is older than the configured freshness window or when required rows are missing. Runtime `doctor` reads only the committed file, warns when lifecycle facts are stale, and reports unsupported/EOL hosts as actionable findings without depending on network availability.

Mocked OS-support tests cover the lifecycle and hardware edge cases without requiring CI to own every real host. Fixtures under `tests/fixtures/os_support/` include Windows 10 22H2 without ESU, Windows 10 22H2 with ESU, Windows LTSC/LTSB in and out of support, supported and EOL Fedora releases, Debian LTS architecture limits, supported and unsupported macOS release/hardware pairs, and stale lifecycle fact data. Tests inject mocked platform facts plus `config/os-lifecycle/facts.json` rows into `noeasymark doctor` and assert the emitted status, diagnostic id, remediation text, and exit-code behavior.

## Reference Notes

Reference links are useful starting points, not normative requirements, not an implementation allowlist, and not content-handling approval. Vendor behavior, CLI flags, billing rules, runner images, lifecycle status, model ids, and model availability can change. The implementation should verify current docs when building adapters, follow the canonical destination when a vendor URL redirects, and rely on runtime capability checks from `noeasymark doctor` / `noeasymark doctor --backends` plus pilot evidence to determine what is actually supported.

Reference-link checking is redirect-aware and CI-owned. A docs CI job extracts this section's URLs, performs bounded HEAD/GET checks with redirects enabled, records final URL, redirect chain, HTTP status, content type when available, checked timestamp, and failure reason into a JSON report artifact, and fails only on hard failures, TLS/DNS failures after retry, or unexpected cross-domain redirects not present in the committed allowlist. Same-domain redirects and approved canonicalization changes are reported for maintainer review without breaking CI. The initial release does not add inline `last_verified` dates to every bibliography bullet; verification history lives in the generated CI report and review artifacts so the prose remains readable and non-normative.

Project and schema references:

- Upstream template providing CI, pre-commit, markdownlint, and agent-instruction baselines: <https://github.com/franklesniak/copilot-repo-template>
- JSON Schema enum reference: <https://json-schema.org/understanding-json-schema/reference/enum>
- JSON Schema object, required-property, and additional-property reference: <https://json-schema.org/understanding-json-schema/reference/object>
- JiWER usage documentation: <https://jitsi.github.io/jiwer/usage/>
- JiWER measures reference: <https://jitsi.github.io/jiwer/reference/measures/>

Backend, model, and prompt references:

- LiteLLM main docs: <https://docs.litellm.ai/docs/>
- LiteLLM structured outputs (JSON Schema): <https://docs.litellm.ai/docs/completion/json_mode>
- LiteLLM Router (fallbacks, retries): <https://docs.litellm.ai/docs/routing>
- OpenAI structured outputs: <https://developers.openai.com/api/docs/guides/structured-outputs>
- OpenAI models documentation: <https://developers.openai.com/api/docs/models>
- OpenAI prompt caching guide: <https://developers.openai.com/api/docs/guides/prompt-caching>
- Anthropic models overview: <https://platform.claude.com/docs/en/about-claude/models/overview>
- Anthropic prompt caching: <https://platform.claude.com/docs/en/build-with-claude/prompt-caching>
- Azure AI Foundry models sold by Azure: <https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure>
- Google Gemini models documentation: <https://ai.google.dev/gemini-api/docs/models>
- Mistral models documentation: <https://docs.mistral.ai/models>
- Ollama model library: <https://ollama.com/library>

CLI-agent references:

- OpenAI Codex CLI repository: <https://github.com/openai/codex>
- Codex authentication: <https://developers.openai.com/codex/auth>
- Codex CLI non-interactive mode (`codex exec`, `--output-schema`): <https://developers.openai.com/codex/noninteractive>
- Codex CLI features: <https://developers.openai.com/codex/cli/features>
- Codex environment variables: <https://developers.openai.com/codex/environment-variables>
- Claude Code CLI reference: <https://code.claude.com/docs/en/cli-reference>
- Claude Code programmatic usage (`claude -p`, `--output-format`, `--json-schema`, `--bare`): <https://code.claude.com/docs/en/headless>
- Claude Code environment variables: <https://code.claude.com/docs/en/env-vars>

Storage, OS, and toolchain references:

- GitHub Git LFS documentation: <https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage>
- GitHub-hosted runners reference: <https://docs.github.com/en/actions/reference/runners/github-hosted-runners>
- GitHub Actions runner images: <https://github.com/actions/runner-images>
- `uv` installation documentation: <https://docs.astral.sh/uv/getting-started/installation/>
- Windows 10 ESU program: <https://learn.microsoft.com/en/windows/whats-new/extended-security-updates>
- Apple macOS current-version and compatibility index: <https://support.apple.com/en-us/109033>
- Docker Desktop GPU support on Windows: <https://docs.docker.com/desktop/features/gpu/>
- NVIDIA Container Toolkit install guide: <https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html>
- Python `os.replace` documentation: <https://docs.python.org/3/library/os.html#os.replace>

PDF and conversion-tool references:

The PDF and conversion-tool reference subsection is generated or checked from `config/engines/*.yaml` `documentation_urls` entries plus the small fixed set of shared toolchain references that are not owned by one engine YAML. `noeasymark dev validate-engine-doc-links --check` fails when a shipped engine lacks required upstream docs links, a listed engine reference below is absent from every shipped engine YAML, or a YAML documentation URL is missing from the generated reference report. Disabled shipped examples still provide docs links because they guide operator enablement; operator-added engines keep their docs in their own YAML and appear in generated reports only when included in the active configuration set.

- qpdf documentation: <https://qpdf.readthedocs.io/en/stable/>
- MuPDF tools documentation: <https://mupdf.readthedocs.io/en/1.27.0/tools/index.html>
- Ghostscript documentation: <https://ghostscript.readthedocs.io/en/latest/>
- Tesseract OCR documentation: <https://tesseract-ocr.github.io/>
- Poppler project: <https://poppler.freedesktop.org/>
- Pandoc user guide: <https://pandoc.org/MANUAL.html>
- PyMuPDF documentation: <https://pymupdf.readthedocs.io/en/latest/>
- OCRmyPDF documentation: <https://ocrmypdf.readthedocs.io/en/latest/>
- Marker repository: <https://github.com/datalab-to/marker>
- Docling documentation: <https://docling-project.github.io/docling/>
- MarkItDown repository: <https://github.com/microsoft/markitdown>
