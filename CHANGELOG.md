# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] - 2026-02-05

### Added
- `allowed-tools` constraint in command frontmatter for better tool scoping
- More specific framework detection (React, Vue, Next.js, Svelte, Angular, Nuxt)
- Structured AskUserQuestion format with headers and options
- CHANGELOG.md for version tracking
- marketplace.json for Claude Code marketplace publishing

### Changed
- Improved task type detection with categorized indicators (UI elements, styling, frameworks)
- Better structured AskUserQuestion prompts with explicit headers

### Fixed
- Version number in plugin.json now reflects actual version

## [1.1.0] - 2026-02-05

### Added
- RFC 2119 conventions (MUST, SHOULD, MAY) throughout command and skill
- RFC 2119 Keywords section in both command and skill files

### Changed
- Moved plugin.json to correct location (.claude-plugin/plugin.json)
- Updated all instructions to use RFC 2119 language

### Fixed
- Plugin structure now follows Claude Code conventions

## [1.0.0] - 2026-02-05

### Added
- Initial release
- `/ifd` command for generating implementation workflows
- `ifd:workflow` skill documenting IFD philosophy
- Support for Linear and JIRA via issue-dev delegation
- Three-pass planning protocol
- Task type inference (Frontend, Backend, Full-stack, Design/Exploration)
- Positive and negative criteria framework
- Worktree-based development workflow
- Integration with superpowers and issue-dev plugins
