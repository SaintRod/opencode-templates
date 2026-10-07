# Purpose

To create useful and repeatable files for working with opencode.

# Scopes

- language agnostic:
	- files:
		- `plans/plan.md`
		- `research/research.md`
- language specific: 
	- languages: [python, r, julia]
	- files: `AGENTS.md`

# Installing skills

Skills are packages of specialized instructions that can be loaded by opencode to provide domain-specific workflows and guidance.
To install skills, use this OSS: [skills](https://github.com/vercel-labs/skills).
See `skills` documentation regarding how to use this software.

The reason I chose `skills` is that it automates gettings skills, placing skills in the correct place, and keeping skills fresh.

# Sources

- [https://boristane.com/blog/how-i-use-claude-code/](https://boristane.com/blog/how-i-use-claude-code/)
- [https://pydevtools.com/handbook/how-to/how-to-configure-claude-code-to-use-uv/](https://pydevtools.com/handbook/how-to/how-to-configure-claude-code-to-use-uv/)

# Instructions

- Kenn-io agent [constitution](https://raw.githubusercontent.com/kenn-io/constitution/refs/heads/main/CONSTITUTION.md)

## Deployment notes

- `global/CLAUDE.md` imports `~/.config/opencode/AGENTS.md`. Deploy or symlink `global/AGENTS.md` to that path, or Claude Code silently drops the Tools/Research/Plans/Worktree-safety guidance.

# Tools
- [opencode](https://opencode.ai/docs/) coding harness
- vercel labs [skills](https://github.com/vercel-labs/skills) to install and manage skills
- [roborev](https://www.roborev.io/) review layer/harness
- [rtk](https://github.com/rtk-ai/rtk) to save tokens on common commands
- [agent-vault](https://github.com/Infisical/agent-vault) credential broker
	- `agent-vault run -- opencode`
- [superpowers](https://github.com/obra/superpowers) - software development methodology for coding agents
- [ponytail](https://github.com/DietrichGebert/ponytail) - to avoid over engineering
- For assessing the best local AI
	- [llmfit](https://github.com/AlexsJones/llmfit)
		- `uvx llmfit`
	- [llm-checker](https://github.com/Pavelevich/llm-checker)
		- `npx llm-checker`
	- [whichllm](https://github.com/Andyyyy64/whichllm/)
		- `uvx whichllm@latest`

# Model providers

- [opencode zen](https://opencode.ai/docs/zen/)
- [Ollama](https://ollama.com/)
	- update docker image to latest: `docker pull ollama/ollama`
	- stop and rm existing docker container:
		- view containers, get ID: `docker ps -a` 
		- `docker stop <ID>`
		- `docker rm <ID>`
	- start the container: `docker run -d --restart unless-stopped --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama`
		- [docs](https://github.com/ollama/ollama/blob/main/docs/docker.mdx)
