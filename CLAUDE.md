# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a personal technical reference/knowledge base. Each `.md` file is a collection of commands, snippets, and notes organized by technology. There is no build system, test suite, or application code.

## Structure

Files are organized into category subfolders:

| Folder | Contents |
|--------|----------|
| `cloud/` | AWS, Terraform |
| `containers/` | Kubernetes (kubectl, Helm) |
| `databases/` | ElasticSearch, MSK Kafka, Oracle, SQL Server |
| `devops/` | Git, GitLab, SSL |
| `languages/` | .NET, iOS/Swift, Java, JavaScript/Angular, Python |
| `legacy/` | AS400, Genexus, SAP |
| `monitoring/` | K6, Prometheus |
| `os/` | IIS, Linux, Terminal/Shell, Windows |
| `tools/` | Development setup, Email |

`README.md` and `CLAUDE.md` live at the root and serve as the entry point.

## Document conventions

- `#` heading = technology/file title
- `##` headings = individual topic or use-case
- Fenced code blocks with the correct language tag (`sh`, `yaml`, `json`, `powershell`, `sql`, `http`, `promql`, etc.)
- Links formatted as `[text](url)` — never bare URLs

## Contributing Notes

When adding or editing content:
- Place the file in the appropriate category subfolder.
- Follow the existing heading and code block style.
- Add a link to new files in `README.md` under the correct category section.
- Keep entries concise and command-focused — this is a quick-reference resource, not a tutorial.
