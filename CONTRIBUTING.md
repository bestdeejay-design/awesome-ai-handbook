[![English](https://img.shields.io/badge/English-Contributing-blue.svg)](CONTRIBUTING.md)
[![Русский](https://img.shields.io/badge/Русский-Contributing-blue.svg)](CONTRIBUTING.ru.md)

# Contributing to Awesome AI Handbook

Hello! 👋

Thank you for considering contributing to this handbook. Our mission is to create the most practical AI reference for Russian‑speaking developers, but we welcome contributions from everyone. We appreciate any improvements – from fixing typos to adding new guides and benchmarks.

## Who can help?

- **Developers** – add new tutorials, code examples, or automation scripts.
- **Testers** – run benchmarks on your hardware and share the results.
- **Translators** – help with the English version or other languages.
- **Editors** – fix typos, improve wording, make the text clearer.

## What you need to know before you start

- Basic Markdown – [cheatsheet](https://www.markdownguide.org/basic-syntax/).
- Git and GitHub – you know how to fork, branch, and create pull requests.
- To test commands, you’ll need [Ollama](https://ollama.com/) installed (as described in our Quick Start).

## How to contribute (step by step)

1. **Open an Issue** – describe what you want to work on, so we don’t duplicate efforts.
2. **Fork** the repository to your own account.
3. **Create a branch** with a descriptive name: `feature/add-ollama-guide` or `fix/benchmark-typo`.
4. **Make your changes** in the relevant files (see table below).
5. **Test your changes**:
   - Make sure any commands work.
   - If you add benchmark results, include the hardware specs (RAM, CPU, OS) and the model version.
6. **Open a Pull Request** (PR) against the `main` branch.
7. In the PR description, explain what you changed and why.

## Where to add content

| Content type | File |
|--------------|------|
| New model / catalog update | `local-models/catalog.md` |
| Task-based recommendations | `local-models/models.md` |
| Inference tool | `local-models/tools.md` |
| Apple Silicon benchmark | `local-models/benchmarks/apple-silicon.md` |
| Problem and fix | `local-models/troubleshooting.md` |
| Agent framework | `agents/frameworks.md` |
| Agent tutorial | `agents/tutorials/` |
| Practical use case | `use-cases/` |
| RU/EN resource | `resources/ru.md` or `resources/en.md` |

## Style guidelines

- **Language**: Use only Russian in `README.ru.md` and only English in `README.md` – please don't mix languages in the same file.
- **Tone**: Friendly but professional. Avoid excessive jargon – explain things simply (we promised no deep math).
- **Headings**: Use `##` for sections, `###` for subsections.
- **Code blocks**: Always specify the language (```bash, ```python, ```yaml).
- **Benchmarks**: Always include the hardware configuration and the model version.
- **Links**: Add `last-verified: YYYY-MM-DD` when citing external resources.
- **No duplication**: Prefer linking to a detailed doc over copying large blocks.

## Review criteria

We check the following when reviewing PRs:

- Does the change align with the project’s goals?
- Are there any spelling, grammar, or factual errors?
- Does any code work correctly?
- Is the text clear and well‑structured?

## Code of Conduct

We follow the [Code of Conduct](CODE_OF_CONDUCT.md). Please read it before contributing.

## Questions?

Open an **Issue** or start a **Discussion**. We’re here to help!

---

Thanks again! Your contribution makes the AI community stronger. 🚀
