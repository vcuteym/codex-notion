下面给你一份\*\*在 macOS 上、给 Codex CLI 配好这些 MCP 服务器（filesystem / Git / Shell/Bash / pandoc / sympy / GitHub / Docker）\*\*的完整实操手册与可直接粘贴的 `config.toml` 模板。

> ✅ Codex CLI 原生支持 MCP。只要在 `~/.codex/config.toml` 里新增 `[mcp_servers]` 区块即可让 Codex 以 **stdio** 方式拉起本地/容器化的 MCP 服务器。([GitHub][1])

---

## 0) 前置条件（macOS）

用 Homebrew 安装常用依赖（如已装可跳过）：

```bash
# Node（给 npx 用）
brew install node

# Python 包运行器 uv（很多 Python 系 MCP 用它的 uvx 一键拉起）
brew install uv

# Git
brew install git

# Pandoc（pandoc MCP 必需），做 PDF 还建议装 TeX Live
brew install pandoc
brew install texlive   # 仅当需要输出 PDF 时

# Docker Desktop（给 Docker MCP / GitHub MCP 的 docker 运行方式用）
brew install --cask docker
```

> *mcp‑pandoc* 要求已安装 `pandoc`，输出 PDF 时还需要 TeX Live；推荐用 `uvx mcp-pandoc` 直接运行。([GitHub][2])

---

## 1) 新建/编辑 Codex 配置文件

```bash
mkdir -p ~/.codex
nano ~/.codex/config.toml
```

Codex 的 MCP 配置就是一个 TOML 文件，区块名为 `[mcp_servers.<名字>]`，每个服务器指定 `command/args`（必要时还可加 `env`）。这在多家集成文档与示例中是一致的。([Snyk User Docs][3])

---

## 2) 可直接粘贴的 `~/.codex/config.toml` 模板

> \*\*把下面所有 `<...>` 占位符替换成你的实际路径或 Token。\*\*每个区块都可单独保留/删除。

```toml
# =============== Filesystem（文件系统）================
# 允许 LLM 只访问你显式列出的目录（建议最小权限）
[mcp_servers.filesystem]
command = "npx"
args = ["-y", "@modelcontextprotocol/server-filesystem", "/Users/<you>/work", "/Users/<you>/projects"]
# 说明：该官方服务器通过传入「允许访问的绝对目录」来做白名单。需要哪个目录就追加一个参数。 
# 文档：modelcontextprotocol.io 的“Connect to local MCP servers”。 

# ================== Git（本地 Git 操作）==================
# 官方 Python 版 Git MCP，推荐用 uvx 直接拉起
[mcp_servers.git]
command = "uvx"
args = ["mcp-server-git", "--repository", "/path/to/your/repo"]
# 也可省略 --repository，由工具调用时再传；或换成 Docker 版本，见其 PyPI 说明。

# ================= Shell / Bash =========================
# 选一个简单的 Node 版 Shell MCP：hdresearch/mcp-shell（npx 一键）
[mcp_servers.shell]
command = "npx"
args = ["-y", "mcp-shell"]
# 备注：该服务器默认带“黑名单”防护，阻止危险命令（如 rm/chmod/sudo 等）。

# ================= Pandoc（文档格式互转）================
[mcp_servers.pandoc]
command = "uvx"
args = ["mcp-pandoc"]
# 若要 PDF 输出，请确保本机已装 texlive（见前置条件）。

# ================= SymPy（符号计算）=====================
# 该服务器需要先 clone 仓库；路径替换为你本地的 server.py 绝对路径
[mcp_servers.sympy]
command = "uv"
args = [
  "run",
  "--with", "mcp[cli]",
  "--with", "sympy",
  # （可选）做广义相对论相关还需：
  # "--with", "einsteinpy",
  "mcp", "run", "/ABSOLUTE/PATH/TO/sympy-mcp/server.py"
]

# ================= GitHub（API 操作：Issues/PRs/Repo 等）=
# 使用 GitHub 官方 MCP 的 Docker 运行方式（最简、跨环境稳定）
[mcp_servers.github]
command = "docker"
args = ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "ghcr.io/github/github-mcp-server"]
env = { GITHUB_PERSONAL_ACCESS_TOKEN = "<your_github_pat_token>" }
# （可选）只读模式避免写操作：
# env = { GITHUB_PERSONAL_ACCESS_TOKEN = "<token>", GITHUB_READ_ONLY = "1" }

# ================ Docker（管理容器/镜像/卷/网络）=========
# 方案 A：用 Python 版 mcp-server-docker（uvx 一键）
[mcp_servers.docker]
command = "uvx"
args = ["mcp-server-docker"]

# 方案 B：用容器方式运行 Docker MCP，自身再挂载本机 docker.sock（等价于“本机 Docker 控制权”）
# [mcp_servers.docker]
# command = "docker"
# args = ["run", "-i", "--rm",
#         "-v", "/var/run/docker.sock:/var/run/docker.sock",
#         "mcp-server-docker:latest"]
```

**出处与对应要点：**

* Filesystem 官方服务器示例（通过参数白名单授权目录，适配各 MCP 客户端），以及本地连接说明。([Model Context Protocol][4])
* Git MCP（`uvx mcp-server-git`、可选 Docker 方式与 `--repository` 参数）。([PyPI][5])
* Shell MCP（`npx mcp-shell` 一键，带默认黑名单防护）。([GitHub][6])
* Pandoc MCP（`uvx mcp-pandoc`，并要求已装 pandoc/TeX Live）。([GitHub][2])
* SymPy MCP（`uv run mcp run server.py` 的典型配置；可选 `--with einsteinpy`）。([GitHub][7])
* GitHub 官方 MCP（本地用 Docker 运行，`GITHUB_PERSONAL_ACCESS_TOKEN` 与只读模式环境变量）。([GitHub][8])
* Docker MCP（`uvx mcp-server-docker`；容器方式需挂载 `/var/run/docker.sock`；支持远端 `DOCKER_HOST=ssh://...`）。([GitHub][9])

---

## 3) 启动与验证

1. 先启动 Docker Desktop（若用了任何 `command = "docker"` 的方式）。
2. 重启 Codex CLI：

   ```bash
   codex
   ```
3. 直接让 Codex 试用这些工具，比如：

   * Filesystem：*“在 `./notes` 里新建一个 `todo.md` 并写入两行文本”*
   * Git：*“对当前仓库执行 `git status` 并把输出贴出来”*
   * Pandoc：*“把 `README.md` 转成 `README.html` 并保存到项目根目录”*
   * SymPy：*“对 $x^3 - 1 = 0$ 求根；再给出 LaTeX 表达”*
   * GitHub：*“在 `<owner/repo>` 新建一个标题为 XXX 的 issue（如只读模式会提示不可写）”*
   * Docker：*“拉取 `nginx:alpine` 并以 8080 端口运行一个容器，随后返回容器列表”*

> 注：一些第三方文档指出 **Codex 目前没有专门的“列出 MCP 连接情况”的命令**，通常通过启动日志或直接调用工具来验证是否连通。([Bright Data][10])

---

## 4) 常见坑与安全建议

* **最小权限**：
  Filesystem 只白名单到项目根或子目录，别把整个人目录 `~` 暴露给 LLM。([Model Context Protocol][4])
* **GitHub 令牌**：
  使用最小必要权限的 PAT；如无写需求，启用只读（`GITHUB_READ_ONLY=1` 或 `--read-only`）。([GitHub][8])
* **Docker 安全**：
  将 `/var/run/docker.sock` 暴露给容器意味着赋予容器**管理宿主 Docker 的能力**，务必知情同意、注意审查 LLM 生成的操作。该 MCP 项目也明确给出风险提示。([GitHub][9])
* **nvm/fnm 等多版本 Node 管理器**：
  某些环境下 `npx` 或全局包路径可能不可见，需在 `command` 写入 Node/可执行文件的**绝对路径**（第三方指引中也给出过此建议）。([Snyk User Docs][3])

---

## 5) 参考与更多

* Codex CLI：支持在 `~/.codex/config.toml` 中以 `mcp_servers` 声明 MCP 服务器。([GitHub][1])
* Filesystem MCP（官方）：如何连接本地 MCP 服务器。([Model Context Protocol][4])
* Git MCP（Python/uvx/Docker 用法示例）。([PyPI][5])
* Shell MCP（`npx mcp-shell` 一键 & 黑名单防护）。([GitHub][6])
* Pandoc MCP（`uvx mcp-pandoc`，依赖 pandoc/texlive）。([GitHub][2])
* SymPy MCP（`uv ... mcp run server.py` 配置示例）。([GitHub][7])
* GitHub MCP（官方，Docker 运行、只读模式等）。([GitHub][8])
* Docker MCP（`uvx mcp-server-docker`；Docker 方式需挂 sock；支持 `DOCKER_HOST=ssh://...`）。([GitHub][9])

---

### 需要我把你机器上的实际路径和 GitHub PAT 权限范围一并“定制到位”的话，我可以直接基于你的目录结构和需求给出替换后的最终版 `config.toml`。

[1]: https://github.com/openai/codex?utm_source=chatgpt.com "openai/codex: Lightweight coding agent that runs in your ..."
[2]: https://github.com/vivekVells/mcp-pandoc "GitHub - vivekVells/mcp-pandoc: MCP server for document format conversion using pandoc."
[3]: https://docs.snyk.io/integrations/developer-guardrails-for-agentic-workflows/quickstart-guides-for-mcp/codex-cli-guide?utm_source=chatgpt.com "Codex CLI guide | Snyk User Docs"
[4]: https://modelcontextprotocol.io/docs/develop/connect-local-servers?utm_source=chatgpt.com "Connect to local MCP servers"
[5]: https://pypi.org/project/mcp-server-git/ "mcp-server-git · PyPI"
[6]: https://github.com/hdresearch/mcp-shell "GitHub - hdresearch/mcp-shell: Execute a secure shell in Claude Desktop using the Model Context Protocol."
[7]: https://github.com/vivekVells/mcp-pandoc/blob/main/README.md "mcp-pandoc/README.md at main · vivekVells/mcp-pandoc · GitHub"
[8]: https://github.com/github/github-mcp-server "GitHub - github/github-mcp-server: GitHub's official MCP Server"
[9]: https://github.com/ckreiling/mcp-server-docker "GitHub - ckreiling/mcp-server-docker: MCP server for Docker"
[10]: https://brightdata.com/blog/ai/codex-cli-with-web-mcp?utm_source=chatgpt.com "OpenAI Codex CLI with the Bright Data Web MCP Server"
