---
title: "Local development"
description: "Run KubeVoIP local development tasks, including uv dependency setup, Ruff linting, pytest, Helm rendering, and operator testing."
url: "/contributing/local-development/"
---

# Local development

In the platform repository:

```bash
uv sync --extra dev
uv run ruff check .
uv run pytest
helm lint charts/kubevoip
helm template kubevoip charts/kubevoip
```
