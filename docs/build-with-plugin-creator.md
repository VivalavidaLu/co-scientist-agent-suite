# Build as a Codex Plugin with plugin-creator

This repository is currently a portable agent-suite source tree. To turn it into a Codex plugin, use Codex's `plugin-creator` skill to wrap the suite with the required plugin manifest and, optionally, a local marketplace entry.

论文来源：<https://www.nature.com/articles/s41586-026-10644-y>

## Recommended Codex Prompt

在 Codex 里可以这样说：

```text
调用 $plugin-creator，帮我把 https://github.com/VivalavidaLu/co-scientist-agent-suite 构建成一个 Codex 插件。

要求：
- 插件名：co-scientist-agent-suite
- 保留 AGENTS.md、subagents/、workflows/、docs/、codex-agents/、平台适配文件
- 生成必需的 .codex-plugin/plugin.json
- 如果需要在 Codex 插件界面安装，生成或更新 marketplace entry
- 构建完成后运行 validate_plugin.py 验证插件结构
- 不要把 prompt-level suite 描述成原始 Nature / Google / Gemini 官方系统
```

If you want to explicitly point Codex to your local `plugin-creator` skill, use a generic local path placeholder instead of committing a personal username path:

```text
Call <path-to-plugin-creator>/SKILL.md and build this plugin:
https://github.com/VivalavidaLu/co-scientist-agent-suite
```

For example, on Windows this path may look like `%USERPROFILE%\.codex\skills\.system\plugin-creator\SKILL.md`.

## What plugin-creator Should Produce

Minimum plugin shape:

```text
co-scientist-agent-suite/
  .codex-plugin/
    plugin.json
  AGENTS.md
  README.md
  README.zh-CN.md
  subagents/
  workflows/
  docs/
  codex-agents/
  .cursor/
  .github/
```

The generated `.codex-plugin/plugin.json` should use the same normalized plugin name as the folder:

```json
{
  "name": "co-scientist-agent-suite",
  "version": "0.1.0",
  "displayName": "Co-Scientist Agent Suite",
  "description": "Portable Co-Scientist-style scientific discovery agent suite inspired by the Nature paper.",
  "author": {
    "name": "VivalavidaLu"
  }
}
```

Keep unsupported manifest fields out of `plugin.json`. Do not list apps or MCP servers unless you actually add companion `.app.json` or `.mcp.json` files.

## Manual Build Outline

If you want to do the process manually after invoking `plugin-creator`, use this sequence:

1. Scaffold the plugin folder:

```powershell
python <path-to-plugin-creator>\scripts\create_basic_plugin.py co-scientist-agent-suite --with-marketplace
```

2. Copy this repository's agent-suite files into the scaffolded plugin root.

3. Edit `.codex-plugin/plugin.json` so the name, version, display name, description, and author match this suite.

4. Validate the plugin:

```powershell
python <path-to-plugin-creator>\scripts\validate_plugin.py <plugin-path>
```

5. If a marketplace entry was generated, restart or refresh Codex and install/view the plugin from the local marketplace.

## Important Scope Notes

- This plugin is still an instruction-level / prompt-level suite. It does not reproduce Google's original Gemini infrastructure.
- Parallel or asynchronous execution depends on the host platform. Platforms without native subagents should use the sequential discovery loop.
- Database skills such as PubMed, bioRxiv, ClinicalTrials.gov, GEO, AlphaFold, or OpenTargets should be treated as optional tool contracts, not mandatory bundled dependencies.
- Validation outputs are planning artifacts. They are not wet-lab or clinical validation results.
