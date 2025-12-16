# PyPI package vs Python package

## Core idea
A **PyPI package** and a **Python package** are related but represent
different concepts and serve different purposes.

## What is a PyPI package
A PyPI package is a **distribution unit**:
- Published to the Python Package Index (PyPI)
- Installed using `pip`
- May bundle one or more Python packages
- Has its own name, version, and metadata

Example:
```bash
pip install python-dotenv
