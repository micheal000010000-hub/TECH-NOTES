# pip install -U -q

## What
`-U` upgrades packages to the latest compatible version.  
`-q` runs pip in quiet mode with minimal output.

## Why
When installing dependencies—especially from `requirements.txt`—pip can
produce a large amount of log output. Quiet mode keeps the terminal
readable while still performing the installation.

## Example
```bash
pip install -U -q -r requirements.txt
