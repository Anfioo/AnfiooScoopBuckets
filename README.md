# Scoop Bucket Template

<!-- Uncomment the following line after replacing placeholders -->
<!-- [![Tests](https://github.com/<username>/<bucketname>/actions/workflows/ci.yml/badge.svg)](https://github.com/<username>/<bucketname>/actions/workflows/ci.yml) [![Excavator](https://github.com/<username>/<bucketname>/actions/workflows/excavator.yml/badge.svg)](https://github.com/<username>/<bucketname>/actions/workflows/excavator.yml) -->

Template bucket for [Scoop](https://scoop.sh), the Windows command-line installer.

## WingHarness plugins

This bucket publishes **WingHarness plugins**: Scoop apps that carry a
`plugin.json` at the root, which the app reads to render their settings, run
them, and embed the page they serve.

**A manifest here must be named `wingharness-<name>.json`.** The file name *is*
the Scoop app name — it becomes the app's folder under `apps/`, its persist
folder, and its id in the app — so the `wingharness-` prefix is the only marker
that says "this app is a WingHarness plugin". The app relies on it: Settings →
Plugins refuses to install an app without it, and the Scoop page badges a
prefixed app as a plugin.

A manifest for this bucket also says where it came from, in the standard fields
a user sees in `scoop info`:

| Field | Convention |
| --- | --- |
| file name | `wingharness-<name>.json` |
| `description` | starts with `WingHarness plugin: ` |
| `notes` | includes `Installed from WingHarness (Settings > Plugins).` |

Structured metadata — the app version a plugin was written against, its
category — does **not** go here. Scoop's `schema.json` forbids unknown keys, so a
custom field in a manifest installs fine but fails this bucket's tests. It lives
in the zip's own `plugin.json` instead:

```json
"wingharness": { "minAppVersion": "0.0.12", "category": "proxy" }
```

## How do I use this template?

1. Generate your own copy of this repository with the "Use this template"
   button.
2. Allow all GitHub Actions:
   - Navigate to `Settings` - `Actions` - `General` - `Actions permissions`.
   - Select `Allow all actions and reusable workflows`.
   - Then `Save`.
3. Workflow permissions:
   - Navigate to `Settings` - `Actions` - `General` - `Workflow permissions`.
   - Ensure `Read repository contents and packages permissions` is selected.
   - Then `Save`.
4. Document the bucket in `README.md`.
5. Replace the placeholder repository string in `bin/auto-pr.ps1`.
6. Create new manifests by copying `bucket/app-name.json.template` to
   `bucket/wingharness-<name>.json` (see [WingHarness plugins](#wingharness-plugins)).
7. Commit and push changes.
8. If you'd like your bucket to be indexed on `https://scoop.sh`, add the
   topic `scoop-bucket` to your repository.

## How do I install these manifests?

After manifests have been committed and pushed, run the following:

```pwsh
scoop bucket add <bucketname> https://github.com/<username>/<bucketname>
scoop install <bucketname>/<manifestname>
```

## How do I contribute new manifests?

To make a new manifest contribution, please read the [Contributing
Guide](https://github.com/ScoopInstaller/.github/blob/main/.github/CONTRIBUTING.md)
and [App Manifests](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests)
wiki page.
