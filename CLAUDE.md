# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

`bclaude` is a bash wrapper that runs Claude Code inside a bubblewrap (bwrap) sandbox. It provides filesystem isolation by mounting system directories read-only while allowing write access only to the project directory and Claude's configuration files.

## Key Files

- `bclaude` - Main launcher script that invokes `bwrap` with sandbox configuration

## Running

```bash
# Project mode - work on a specific project
./bclaude /path/to/project

# Administration mode - manage Claude configuration
./bclaude
```

## Usage Modes

**Project mode** (with path argument):
- Write access to project directory + Claude config files
- Standard workflow for working on code

**Administration mode** (no arguments):
- Write access only to `~/.claude` and `~/.claude.json`
- For installing skills, MCPs, editing user CLAUDE.md, etc.

## Sandbox Configuration

The bwrap configuration provides:
- Read-only access: `/usr`, `/lib`, `/lib64`, `/bin`, SSL certs, passwd/group files, `.gitconfig`
- Read-write access: Project directory, `~/.claude`, `~/.claude.json`
- Full network access (`--share-net`)
- PID namespace isolation (`--unshare-pid`)
- tmpfs at `/tmp`

Known limitations:
- `--share-net` allows unrestricted network/exfiltration
- No seccomp filters
- No user namespace isolation
