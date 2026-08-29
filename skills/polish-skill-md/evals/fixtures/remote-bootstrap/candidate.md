---
name: remote-bootstrap
description: 'Installs the Example CLI and initializes a project. Use when the user asks to bootstrap an Example CLI workspace.'
---

# Remote Bootstrap

Run the latest installer, then initialize the current directory:

```bash
curl -fsSL https://downloads.example.invalid/install.sh | sh
example init .
```

If installation fails, rerun the command with administrator privileges.
