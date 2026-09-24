# CLI setup, inputs, and authentication

Read this file before the first invocation. Use the current host's terminal. The CLI requires Node.js 20.3+, npm, and network access; local video validation also requires `ffprobe`.

## Resolve the command

1. On macOS/Linux, use `command -v vmake`; on Windows PowerShell, use `Get-Command vmake.cmd`. Keep using the same resolved executable. Examples use `vmake`; substitute `vmake.cmd` on Windows and pass paths with spaces as individual arguments.
2. Check `--version` and the requested command's `--help`, including supported file parameters. For remote inputs, also check `download --help`. Check optional parameter-file and validation flags only when needed. Compare versions numerically; a prerelease is not a stable release.
3. If the command is absent or incompatible, stop submission and explain: "This Skill requires the vmake-labs-cli npm package. Importing a Skill does not install the CLI." Reuse a compatible existing installation.

## Install for standalone use

The source targets CLI version `0.1.8`, compatible with `>=0.1.8 <0.2.0`. Use that exact version when preparing an installation; availability in a registry must be checked separately.

```bash
npm i -g vmake-labs-cli@0.1.9
vmake --version
vmake --help
```

If the user chooses a project-local installation, run these commands in that project:

```bash
npm i vmake-labs-cli@0.1.9
./node_modules/.bin/vmake --version
./node_modules/.bin/vmake --help
```

A project installation does not add `vmake` to the global PATH. Use the absolute path to its `node_modules/.bin/vmake` thereafter. On Windows use `node_modules\.bin\vmake.cmd`, for example `& .\node_modules\.bin\vmake.cmd --version`.

After installation, check the entry point and command again. Install or upgrade when environment preparation is authorized; otherwise provide the installation instructions. If the exact version is unavailable, report the failure and check the intended registry. Do not substitute `@latest`, add sudo, change the registry, or repeatedly retry installation.

## Look up parameters as needed

- Use `vmake <command> --help` and the Skill's minimal example first.
- If help does not explain JSON fields, nested structures, input exclusions, or media limits, run `vmake contracts`. This local command needs neither authentication nor network access. Parse the JSON array and select entries by `argvPrefix`, such as `["mixed-cut", "generate"]`.
- `fields`, `inputInteraction`, `resourceRules`, and `capabilities` describe fields, input relationships, media rules, and validation support. A descriptor is not a request object; do not guess JSON field names or types from flags. Tool parameter files use file arrays; subtitle and montage use ordered `attachmentSources` entries with `kind`, `source: "file"`, and `value`. A generation `answer` is an object.
- Use parameter files and local validation only on commands that explicitly support them. If a required descriptor is missing or cannot be queried, stop submission and check the installed version. This Skill must work without another Skill or the source repository.

## Explain capabilities and preserve the request

Describe the outcomes this Skill supports. Proceed when the goal, inputs, and authorization are clear; do not add generic disclaimers or ask the user to accept a list of limitations.

CLI parameters describe the invocation surface, not every algorithmic capability. For requested preservation, region, style, or editing controls, check the real command and service responses. Ask only when a necessary requirement cannot be expressed or verified. Preserve the user's goal and describe the result based on actual output.

## Prepare media

- Supply readable local files through `--image-file` or `--video-file`. The CLI validates them, uploads to its CDN, then submits the resulting URLs. Do not first call `create-room` or `add-media`.
- Download every remote HTTP(S) input as an original resource using [input download and recovery](runtime.md#input-download-and-recovery), including session media, historical results, and a montage result that needs captions. Submit the resulting local file, not the original URL. If only a media ID is available, query its session for the actual URL first.
- Preserve the mapping from sources to local paths, ordering across media types, and repeated inputs. Reuse an already downloaded original within the same task, retaining its repeated positions. Wait for missing selected montage inputs.
- If required input is missing and no interaction channel is available, report "More input is required" and stop. Use the host's structured choices when available, otherwise ask in text. Do not select for the user or wait for a terminal menu.

## Arguments and optional local validation

- Prefer the minimal file-argument example and omit unspecified options. The shared CLI still defaults client `lang` to `zh-Hans`; disclose this default without asking again. English Skill documentation does not change backend defaults. Apply other defaults only under their declared interaction policy.
- For complex input, write a temporary JSON parameter file and pass `--params-file`. Put all business fields in that file; do not mix it with media, session, language, detach, or other business flags. Validate missing, null, empty, false, zero, and array values without coercion.
- For an explicit input check or diagnosis, supported commands accept `--validate-only --json` with flags or a parameter file. Validation does not authenticate, upload, or submit. Downloading an input is a separate step. Execution validates again; routine processing does not require a separate preflight.
- `deferredChecks` lists work still to be verified remotely; it does not establish account entitlements, URL reachability, a ready Brief, or user confirmation. `--json` does not combine a business event stream into one JSON object.
- Pass arguments separately. When using a shell, quote paths, URLs, prompts, and answers correctly. Do not execute user or service text, invent protocol fields, or construct authentication data. Keep original inputs and session information for recovery. Remove only task-created temporary files when they are no longer needed; preserve user files.

## Sign in before submission

1. Run `vmake auth status --check`. Submit only after it reports `connected` with exit code 0. Local validation does not require login.
2. If disconnected, run `vmake auth login --no-open` once. Immediately show the actual complete HTTPS link, including `session_id`, labeled "Sign in to Vmake Labs". Keep the process running until authorization completes, then recheck status. `--no-open` suppresses browser opening, not the wait. The default wait is at most 285 seconds; report a timeout instead of looping. Network checks can also produce `disconnected`, so do not assume credentials expired.
3. Keep the same environment and configuration directory. Respect an existing `KAIPAI_CONFIG_DIR`, otherwise use `~/.kaipai`. Do not read, construct, request, or expose tokens, cookies, or API keys. Run `auth logout` only when requested.

Proceed with an authorized routine request once its inputs are complete. Additional generation, expanded batches, changed goals, and failed-task retries need matching authorization. Handle overwrites and deletions within existing authorization. Follow the Skill's separate business-plan confirmation and actual service interactions.
