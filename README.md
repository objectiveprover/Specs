# Specs
Specifications, guidelines, and conventions related to my projects

## Autoconfigurable Services

A service provides information about itself using the `whodis` subcommand, so it can be automatically configured by other programs. A service is any program that can run in the background.

The `whodis` command response must contain at least the following fields:

```
type: "whodis-info"
exec: ""
version: "1.0"
description: ""
service_args: ""
port: ""
health_check: ""
config_path: ""
env_path: ""
data_path: ""
```

- The `whodis` subcommand prints the description above as a plain string on stdout and exits 0.
- The format version must be ≥1.0. If it's missing or 0, the binary counts as not autoconfigurable.
- The literal string whodis-info must appear somewhere in the binary. The program asking scans the file for it before running anything. This means a program never gets called with an argument it doesn't know.
- It must answer within 3 seconds. A timeout isn't saved, so the binary is asked again next time. Any other failure ("unknown command", bad formatting) counts as unsupported.

## Directories

- Binaries: `~/.local/bin`
- App config: `~/.config/<binary>/config.<ext>`
- Secrets: `~/.secrets/<binary>.env`
  - Kept outside `~/.config` so dotfile repos and sync can leave them out. The directory should be `0700` and the file `0600`.
  - An env file that other users can read is flagged with a chmod 600 hint.
  
