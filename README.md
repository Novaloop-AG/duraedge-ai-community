# DuraEdgeAI Community Profiles

Community-maintained extraction profiles for [DuraEdgeAI](https://duraedge.ai) — the local-first AI clinical documentation tool.

Extraction profiles define the section structure and instructions used by the AI to organize clinical transcriptions. Different specialties need different documentation formats — this repo makes them shareable.

## How It Works

DuraEdgeAI syncs profiles from this repo automatically on startup, and can be triggered manually from **Preferences > Integrations > Extraction Profiles > Sync Now**.

Only new or updated profiles are downloaded. Your locally cached profiles are never deleted by a sync — custom profiles you create stay untouched.

## Available Profiles

| Profile | Specialty | Sections | Description |
|---------|-----------|----------|-------------|
| `soap` | General Medicine | 4 | Standard SOAP format (Subjective, Objective, Assessment, Plan) |
| `ortho` | Orthopedics | 7 | Diagnosis, History, Clinical Exam, Radiology, Treatment Plan, Inconsistencies, To-Dos |

## Contributing a Profile

1. Fork this repo
2. Create a YAML file in `profiles/` (use an existing profile as a template)
3. Add an entry to `index.json`
4. Open a pull request

### Profile YAML Schema

```yaml
id: my-specialty           # Unique identifier (lowercase, hyphens)
name: "My Specialty"       # Display name in the UI
description: "Short description of what this profile covers"
version: "1.0.0"           # Semver — bump when updating
author: "Your Name"        # Optional
specialty: "my-specialty"  # Optional, for filtering
tags:                      # Optional
  - tag1
  - tag2

sections:
  - id: 1                  # Unique integer per section
    title: "Section Name"  # Displayed in the UI
    color: "color-blue"    # One of: color-blue, color-green, color-orange,
                           #         color-pink, color-purple, color-cyan,
                           #         color-red, color-yellow
    placeholder: "##SECTION_NAME##"  # Used for DOCX template replacement
    content_extraction_instructions: >  # What belongs in this section (optional)
      Description of what content to extract for this section.
    format: prose           # Format hint: prose, bullets, bullets_if_multiple,
                           #              numbered, summary (optional)

prompts:
  writing_style: >          # Cross-cutting style rules for all sections (optional)
    Style instructions that apply globally, e.g. person, tone, etc.

```

### `index.json` Manifest

When adding a profile, append an entry:

```json
{
  "version": 1,
  "profiles": [
    { "id": "my-specialty", "path": "profiles/my-specialty.yaml", "version": "1.0.0" }
  ]
}
```

The `version` field in the manifest must match the profile's YAML `version`. DuraEdgeAI compares this to the locally cached version to decide whether to re-download.

### Guidelines

- **Test your profile** before submitting — load the YAML in DuraEdgeAI (place it in the app's `profiles/` data directory) and run a few extractions
- **Keep section count reasonable** — 3 to 10 sections works well
- **Write clear developer instructions** — the AI follows them literally
- **Include `{lang_name}`** in the reminder block so extractions work in any language
- **Use descriptive IDs** — `dermatology`, `emergency-medicine`, not `profile1`

## Using a Custom Repo

You can point DuraEdgeAI to any repo with the same structure (e.g. your fork). Go to **Preferences > Integrations > Extraction Profiles** and change the Repository URL. This is useful for organizations that maintain private specialty profiles or to test changes before submitting a PR.

## License

[MIT](LICENSE) — Novaloop AG
