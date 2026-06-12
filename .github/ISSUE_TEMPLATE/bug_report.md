---
name: Bug report
about: Report a bug with bird CLI
title: '[BUG] '
labels: bug
assignees: ''
---

**Describe the bug**
A clear description of what the bug is.

**Commands run**
```bash
# What command did you run?
```

**Expected behavior**
What you expected to happen.

**Actual behavior**
What actually happened. Include full error output.

**Environment**
- OS: [e.g. Ubuntu 24.04]
- Node version: [e.g. v22.22.3]
- bird version: [e.g. v3.0.0]

**Env file check**
```bash
# Run this and confirm auth_token is 40 chars:
python3 -c "print(len(open(os.path.expanduser('~/.config/bird/env')).read().split('\"')[1]))"
```
