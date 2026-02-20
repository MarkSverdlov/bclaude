# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

`bclaude` is a bash wrapper that runs Claude Code inside a bubblewrap (bwrap) sandbox. It provides filesystem isolation by mounting system directories read-only while allowing write access only to the project directory and Claude's configuration files.

## Key Files

- `bclaude` - Main launcher script that invokes `bwrap` with sandbox configuration

## Running

Launch Claude Code in a sandboxed environment:
```bash
./bclaude /path/to/project
```

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
