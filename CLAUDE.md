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

# Debug mode - explore sandbox environment
./bclaude --debug [project-path]

# Offline mode - disable network access
./bclaude --offline /path/to/project

# Read-only access to additional paths
./bclaude --read /path/to/data /path/to/project

# Multiple read-only paths
./bclaude --read /data --read /models /path/to/project
```

## Usage Modes

**Project mode** (with path argument):
- Write access to project directory + Claude config files
- Standard workflow for working on code

**Administration mode** (no arguments):
- Write access only to `~/.claude` and `~/.claude.json`
- For installing skills, MCPs, editing user CLAUDE.md, etc.

**Debug mode** (`--debug` flag):
- Launches bash shell instead of claude
- Allows exploring what the sandbox provides
- Useful for verifying file access before running claude

**Offline mode** (`--offline` flag):
- Disables network access using `--unshare-net`
- Can be combined with other modes (e.g., `--debug --offline`)

**Read-only mounts** (`--read <path>` flag):
- Adds additional paths as read-only mounts in the sandbox
- Can be specified multiple times
- Useful for accessing data or dependencies outside the project

## Sandbox Configuration

The bwrap configuration provides:
- Read-only access: `/usr`, `/lib`, `/lib64`, `/bin`, SSL certs, passwd/group files, `.gitconfig`
- Read-write access: Project directory, `~/.claude`, `~/.claude.json`
- Network access by default (`--share-net`), disabled with `--offline`
- PID namespace isolation (`--unshare-pid`)
- tmpfs at `/tmp`

Known limitations:
- Network enabled by default (use `--offline` to disable)
- No seccomp filters
- No user namespace isolation
