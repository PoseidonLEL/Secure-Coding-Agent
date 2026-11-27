# Secure Web Application Developer Assistant v2.0 — Installation

This package contains nine Markdown configuration files (one custom agent, two instruction files, six skills) plus this guide. It modernizes the original `secure-coding.agent.md` while keeping the same web application development scope. No scripts, additional agents, MCP servers, or external scanning tools are installed.

## Recommended: install per repository (Windows / VS Code)

1. Extract the ZIP to a temporary folder. Inside it is a `.github` directory and this guide. Enable hidden items/file extensions in File Explorer if needed.
2. Open the root of your web application repository in VS Code (the folder containing your `.git` directory or project root). For example `C:\Projects\MyWebApp`.
3. Copy the supplied `.github` directory into that root, merging it with any existing `.github` directory. Do not replace the entire existing `.github` directory or overwrite workflows or other customizations. Back up the old `secure-coding.agent.md` outside recognized agent folders before replacing it. Review any existing matching filenames before overwriting them.
4. The final layout should be exactly as below. The guide is optional and may remain outside the repository. Keep all six `SKILL.md` filenames unchanged; their parent directory names distinguish them.

```text
C:\Projects\MyWebApp\
└── .github\
    ├── agents\
    │   └── secure-coding.agent.md
    ├── instructions\
    │   ├── secure-coding.instructions.md
    │   └── web-application-security.instructions.md
    └── skills\
        ├── repository-discovery\SKILL.md
        ├── threat-modeling\SKILL.md
        ├── secure-implementation\SKILL.md
        ├── security-code-review\SKILL.md
        ├── security-testing\SKILL.md
        └── security-verification\SKILL.md
```

5. In an up-to-date VS Code with GitHub Copilot available, open a new Copilot chat. Select the Copilot harness if your version offers harness selection. Use the agent picker to choose **Secure Web Application Developer Assistant**. If it does not appear, run `Chat: Open Customizations` from the Command Palette (`Ctrl+Shift+P`) and inspect Agents, Instructions, and Skills. Reload the window if needed and start a fresh chat.

## Alternative: install once for all projects (personal Copilot configuration)

For the current Copilot harness, copy the **contents** of the supplied `.github` directory (the `agents`, `instructions`, and `skills` directories) into `%USERPROFILE%\.copilot\`. Do not copy the `.github` wrapper into `.copilot`.

```text
%USERPROFILE%\.copilot\
├── agents\secure-coding.agent.md
├── instructions\secure-coding.instructions.md
├── instructions\web-application-security.instructions.md
└── skills\
    ├── repository-discovery\SKILL.md
    ├── threat-modeling\SKILL.md
    ├── secure-implementation\SKILL.md
    ├── security-code-review\SKILL.md
    ├── security-testing\SKILL.md
    └── security-verification\SKILL.md
```

Paste `%USERPROFILE%\.copilot` into File Explorer's address bar; create the folder if absent. Use one installation method initially to avoid duplicate agent or skill names. If you use an older VS Code Local harness or another IDE, user-level locations and tools may differ; repository installation is the recommended starting point. On remote/WSL/container sessions, use the home/workspace of the machine where the harness runs.

## Scope and tool configuration

- The agent uses the documented `read`, `search`, `edit`, and `execute` aliases for current Copilot. Actual tool availability and approval policies are controlled by the harness. The agent does not request delegation, web browsing, or MCP tools. Official references in the policies may be consulted via an authorized available tool or provided documentation; if unavailable, report the limitation rather than inventing facts. Do not use terminal/network workarounds to bypass tool restrictions. Review tool availability in your agent editor if an alias is unsupported.
- Both instruction files intentionally omit `applyTo` and `description` to avoid deliberately creating repository-wide rules. The agent explicitly reads their relative links or exact installation paths. Different harnesses may handle discovery differently; test actual behavior. They can also be attached manually if necessary. No `copilot-instructions.md` is installed or changed.
- Skills use standard discovery metadata and may also be available to other agents or invoked from the `/` menu. They are not exclusive to this agent. The custom agent explicitly selects the appropriate procedure and falls back to reading its file. Mandatory safeguards remain in the central agent even if skill discovery fails.
- The package is configuration guidance, not a technical security enforcement mechanism. Keep existing CI checks, tool permission boundaries, secret scanning, and tests enabled.

## Verify installation

1. Confirm the agent appears in the agent picker and the six skill names appear in the customizations/skills view (or slash menu where supported).
2. Select the custom agent and issue this read-only smoke test:

   > Do not modify files or run commands. Identify your two policy files and read them. Identify which skills would be applicable to adding an authenticated invoice download endpoint. Read the threat-modeling skill and summarize the trust boundaries, security controls, and verification plan. Report which files you actually accessed and any unavailable files.

3. Inspect the tool activity, not just the response. Confirm both policies and the relevant skill were accessed (or actually loaded into context); the response should address ownership, tenant boundaries, safe download behavior, and denied-path tests without modifying the repository.
4. Test a small real development change in a disposable branch or test repository. Confirm it uses existing conventions, adds appropriate negative tests, runs available checks, and reports unverified controls honestly. Test a review-only request to confirm it does not edit files.
5. If discovery fails, confirm `.github` is at the repository root, filenames have not acquired `.txt` extensions, skills retain their parent names, and the selected harness supports these formats. Inspect `Chat: Open Customizations`, reload, and retry. For personal installation verify the active harness's home location and read permissions.

## Reference documentation

- [VS Code custom agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)
- [VS Code custom instructions reference](https://code.visualstudio.com/docs/agents/reference/custom-instructions-reference)
- [VS Code agent skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [GitHub Copilot custom agents configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [OWASP Top 10:2025](https://top10.owasp.org/2025/)
- [OWASP ASVS](https://asvs.owasp.org/)

Configuration locations and aliases were checked against official documentation on 2026-11-26. This package was statically validated for layout, YAML parsing, and local links; actual Copilot activation and repository behavior have not been exercised here.
