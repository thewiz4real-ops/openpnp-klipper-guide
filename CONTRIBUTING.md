# Contributing

Thanks for helping!

## How to propose a change
1. Fork the repo
2. Create a branch: `feat/...` or `fix/...`
3. Commit small, testable changes with clear messages
4. Open a Pull Request and fill out the template

## Style
- Prefer Markdown fenced code blocks
- Always label commands by OS: **Windows PowerShell** vs **Ubuntu (WSL)**
- For fixes, include: what broke → why → the tested solution
- Regex: paste with a one‑line explanation and a sample line that it matches

## Testing
- For Klipper: show a console log that ends in `ok`
- For OpenPnP: include the driver snippet and a screenshot if UI‑related
- For Windows↔WSL bridge: show the `M105` round‑trip test output
