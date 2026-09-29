# Vmake Skills

Seven Agent Skills for image enhancement, video enhancement, removal, dynamic captions, and smart montage using Vmake Labs.

## Install skills

Install all seven skills without interactive prompts:

```bash
npx skills add meitu/vmake-skills -y
```

Optional: list available skills or install only one:

```bash
npx skills add meitu/vmake-skills --list
npx skills add meitu/vmake-skills --skill vmake-image-repair
```

## Prepare the runtime

Installing a Skill does not install its CLI. Requires Node.js 20.3 or newer, npm, and network access. Local video validation uses `ffprobe` installed with the CLI through npm dependencies; no separate FFmpeg installation is required.

```bash
npm install -g --include=optional vmake-labs-cli@0.1.10
vmake --version
vmake auth login
vmake auth status --check
```

The distributed Skills are version 1.0.7, target CLI 0.1.10, and declare compatibility with `>=0.1.10 <0.2.0`. Use Vmake credentials and `CLI_*` configuration; the default configuration root is `~/.vmake`.

## Available skills

| Skill | Capability |
| --- | --- |
| [vmake-dynamic-caption](skills/vmake-dynamic-caption/SKILL.md) | Create dynamic captions from video speech or subtitle timelines. |
| [vmake-smart-montage](skills/vmake-smart-montage/SKILL.md) | Analyze and edit image, video, or mixed media into a montage. |
| [vmake-image-repair](skills/vmake-image-repair/SKILL.md) | Enhance image clarity and detail. |
| [vmake-image-remove-watermark](skills/vmake-image-remove-watermark/SKILL.md) | Remove image text and text watermarks. |
| [vmake-video-repair](skills/vmake-video-repair/SKILL.md) | Enhance video quality. |
| [vmake-video-remove-subtitle](skills/vmake-video-remove-subtitle/SKILL.md) | Remove burned-in video subtitles. |
| [vmake-video-remove-watermark](skills/vmake-video-remove-watermark/SKILL.md) | Remove video watermarks. |

## Use

Ask your agent to enhance an attached image, remove subtitles from a video, or add dynamic captions. Provide the actual source files and the desired result.

For example: "Use vmake-image-repair to improve the clarity of this attached photo."

Smart montage first analyzes the inputs and presents a plan; generation follows confirmation of the actual ready plan. Each Skill includes its own setup and recovery references. Keep the complete Skill directory when copying it manually.

Skill instructions are in English. Vmake defaults its client language to `en`; an explicit `--lang` or parameter-file `lang` takes precedence. Client language does not translate source audio or replace montage content `language`.

## Validation

Release verification covers Skill discovery, metadata and local references, build checks, CLI contracts, and offline validation of the documented command examples. Offline validation does not establish completion of real media-processing jobs or compatibility with every agent host.
