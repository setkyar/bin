# `find-usage` - File Usage Search Tool

`find-usage` is a command-line utility located in `~/bin/find-usage` designed to search for references to a specified term (e.g., a file path, function, or module) across a codebase. It’s optimized with defaults for JavaScript/TypeScript projects and offers flexible options for broader applicability.

## Overview

- **Purpose**: Quickly identify where a term is used in source files, excluding common build and dependency directories.
- **Location**: Pre-installed at `~/bin/find-usage`.
- **Compatibility**: Works on macOS and other Unix-like systems.

## Usage

Run `find-usage` from any terminal to search files for a given term.

### Syntax

```
find-usage <search-term> [directory] [--patterns "pattern1 pattern2"]
```

- `<search-term>`: The string to search for (e.g., a file path, function name). **Required**.
- `[directory]`: Optional directory to search. Defaults to the current directory (`.`).
- `[--patterns "pattern1 pattern2"]`: Optional space-separated file patterns to override defaults.

### Examples

1. **Search in Current Directory**:

   ```
   find-usage "actions/companies/companies"
   ```

   Searches for `"actions/companies/companies"` in `.ts`, `.tsx`, `.js`, `.jsx` files, excluding `node_modules`, `.next`, etc.

2. **Search in a Specific Directory**:

   ```
   find-usage "utils/helpers" ~/code/my-project
   ```

   Searches within `~/code/my-project`.

3. **Custom File Patterns**:

   ```
   find-usage "some_function" ~/code/python-app --patterns "*.py *.pyi"
   ```

   Searches Python files instead of the default JS/TS patterns.

4. **Using Environment Variable**:

   ```
   export FILE_PATTERNS="*.go *.templ"
   find-usage "main"
   ```

   Overrides default patterns for the current session.

5. **Check Usage**:
   ```
   find-usage
   ```
   Displays:
   ```
   Usage: find-usage <search-term> [directory] [--patterns "pattern1 pattern2"]
   Example: find-usage "actions/companies/companies" ./my-project
            find-usage "utils/helpers" --patterns "*.py *.sh"
   Set FILE_PATTERNS env var to override defaults: *.ts *.tsx *.js *.jsx
   ```

## Features

- **Default File Patterns**: Targets `.ts`, `.tsx`, `.js`, `.jsx` files, ideal for JavaScript/TypeScript projects like Next.js or React.
- **Automatic Exclusions**: Ignores `node_modules`, `.next`, `dist`, `build`, and `.git` directories.
- **Customizable Patterns**: Override defaults with `--patterns` or the `FILE_PATTERNS` environment variable.
- **Clean Output**: Suppresses irrelevant errors (e.g., “file not found”) for a focused result list.
- **Global Access**: Available from any directory via `~/bin`, assuming it’s in your PATH.

## Configuration

- **File Patterns**:
  - Default: `.ts *.tsx *.js *.jsx`.
  - Override temporarily with `--patterns "pattern1 pattern2"`.
  - Set permanently via `export FILE_PATTERNS="pattern1 pattern2"` in `~/.zshrc`.
- **Exclusions**: Pre-configured to skip common build/dependency directories. Edit the script to adjust if needed.
- **PATH**: Ensure `~/bin` is in your PATH (`export PATH="$HOME/bin:$PATH"` in `~/.zshrc`).

## Notes

- **macOS Compatibility**: Built for Bash but runs fine under Zsh (macOS default since Catalina). Adjust shebang to `#!/bin/zsh` if preferred.
- **Performance**: Efficient for small-to-medium projects. For large repositories, specify a narrower `[directory]` to speed up searches.
- **Search Scope**: Matches exact strings. For regex or line context, pipe results to `grep` (e.g., `find-usage "term" | xargs grep -H "term"`).

## Troubleshooting

- **“command not found”**: Confirm `~/bin` is in your PATH (`echo $PATH`) and reload your shell (`source ~/.zshrc`).
- **No Results**: Verify the `<search-term>` matches how it’s referenced (e.g., `"utils/helpers"` not `"utils/helpers.js"`) and the directory contains relevant files.
- **Permission Issues**: Ensure the script is executable (`ls -l ~/bin/find-usage` should show `-rwxr-xr-x`). Fix with `chmod +x ~/bin/find-usage`.
