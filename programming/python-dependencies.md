---
layout: default
title: Python dependencies
has_children: false
parent: Programming
---

# Python Dependencies

## In general

###  Use a Virtual Environment
This is the recommended approach. Create and use a Python virtual environment inside the Docker container. This isolates your Python environment from the system Python and allows you to use pip freely.

```bash
COPY requirements.txt .
RUN python -m venv /venv
ENV PATH="/venv/bin:$PATH"
RUN pip install --upgrade pip && pip install -r requirements.txt
```

## Use --break-system-packages
This is less recommended as it might lead to a broken Python installation or OS issues.

```bash
RUN pip install --break-system-packages -r requirements.txt
```

## Install with APT:
If the packages you need are available as Debian packages, you can install them using apt.
However, this approach is less flexible than using pip and might not have the latest versions of the packages.

```bash
apt-get update && apt-get install -y python3-numpy
```

