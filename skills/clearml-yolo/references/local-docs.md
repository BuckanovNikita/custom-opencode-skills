# Local Russian reference and configuration preview

Use this reference when the user wants field explanations, Russian tutorials,
editable configuration examples or composition inspection through `cy-docs`.
The interface requires CY 0.43.0 or later. Confirm the installed command exists
before claiming it is available in an older environment. Do not upgrade the
installation merely to answer a usage question.

## Open or verify the documentation

With a compatible CY installation:

```bash
cy-docs
cy-docs --no-browser
cy-docs --port 8765 --no-browser
cy-docs --check
```

The server prints a `http://127.0.0.1:PORT/` URL and attempts to open a browser unless
disabled. The default selects an available port; explicit `--port 0` does the same.
Use the printed address. `Ctrl+C` stops the server and cleans up its preview workers.
`--check` checks catalog completeness, packaged assets, tutorial links and composition
examples without starting a server or opening a browser.

HTML/CSS/JavaScript and rendered Russian tutorials ship with the Python installation
and work offline. No Node.js setup, internet font or hosted asset is required. This
does not make the normal training/evaluation commands offline: they retain their
ClearML, dataset, model and runtime requirements.

## Explain and edit configuration

Search by a field path or Russian description, then filter by command and model.
Stable links use the printed URL followed by `#field=ground_truth`, for example.
Inspect the selected command/model/profile defaults alongside inherited expressions;
the catalog derives them from the installed declarations and native-default adapter.

Use named YAML documents and ordered Hydra overrides in the editor. Copy/download
the resulting command and YAML for later terminal execution. Keep defaults/group
selection and distinguish wrapper inputs from native parameters. Generated snapshots
and provenance are separate from editable inputs. Refer the user to the field's
shape, constraint, applicability and owning source when a nullable or dynamic input
is unclear; do not infer that every `null` has the same meaning.

The tutorials cover installation/data, every registered family, training, evaluation,
complete experiments, Hydra/DDP, Albumentations/Mosaic, typed Python dependencies,
Agent/queue operations, cache/weight preparation and supported export. Native
capability limitations remain applicable. Queue examples use existing `cy-queue`
interfaces; the site adds neither queue schemas nor a new plugin API.

## Respect the preview boundary

Preview composes in a fresh Python worker using built-in groups. It returns composed
YAML, schema errors, unresolved values and deferred runtime checks. It never launches
the application, instantiates Python targets, loads augmentation JSON or accesses
datasets/checkpoints. Environment and custom resolver expressions remain unresolved.

Submitted `hydra` runtime blocks/overrides, external search paths and
plugin/launcher/sweeper replacement are rejected before composition. Apply Hydra
runtime controls in the terminal. Named documents use `.yaml`, not `.yml`, matching
Hydra's file source. Requests are limited to 1 MiB and 32 named documents, sweep expansion
to 100 jobs, and each worker to 15 seconds. Split a larger experiment into smaller
previews rather than presenting a timeout or rejection as a successful check.

Do not claim preview has verified file contents, credentials, model loading,
GPU capacity, augmentation operators, native execution, uploads or deployment quality.
`???` is an unfilled required input. Environment/custom expressions require resolution
in the actual execution context. Schema feedback helps edit inputs; runtime validation
remains authoritative. Training is terminal-driven and queue submission stays explicit.

## Diagnose failures without expanding scope

- Missing `cy-docs`: inspect the selected environment/version and report the mismatch;
  use the existing source/help/configuration reference where available.
- Explicit port unavailable: choose an available port or use automatic selection.
- Missing/stale assets or tutorial failure: report the documentation check failure;
  this does not by itself establish that ordinary CY commands are broken.
- Preview schema error: explain the offending field using its actual declaration.
- Unsupported search path/plugin: include needed YAML as named documents and retain
  the built-in composition boundary; do not suggest executing arbitrary targets.
- Worker failure or timeout: report the error, reduce the request when appropriate,
  and distinguish an incomplete preview from an invalid runtime workload.

For typed dependency customization, follow the packaged Python tutorial and existing
`WorkflowDependencies` ports. Use `dataclasses.replace` within a caller-owned lifecycle
and preserve complete input recording. Do not imply arbitrary pipeline stages can be
replaced by Hydra `_target_` overrides.

## Verification evidence

Report help/check output and the preview result actually observed. A passing
`cy-docs --check` establishes documentation consistency, not browser interaction or
native execution. Browser acceptance is a separate Python development suite; runtime
users do not need that tooling. Follow the tool repository's current
[local documentation contract](https://github.com/BuckanovNikita/clearml-yolo/blob/master/docs/local-documentation.md)
for maintenance details and compare its revision with the installed source.
