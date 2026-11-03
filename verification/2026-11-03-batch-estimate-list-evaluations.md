# Saved coverage usage evaluations, 2026-11-03

Two native Codex subagents ran in separate fresh contexts with no inherited
conversation. Both were explicitly instructed to read the source skill and relevant
references, answer the exact explanation-only prompt, perform no product commands,
write nothing and not delegate. This evaluates explicit skill reading; automatic
routing, installed discovery and live OpenCode/DeepSeek performance were not tested.
Parent-selected model/effort inheritance was requested; realized runtime settings
and host version were not exposed by the collaboration result. No provider benchmark
claim follows. Agent identifiers are `eval_coverage` and `eval_missing`.

Source under test: collection base `42a201b` plus the proposed 0.7.2 usage changes.
The parent compared observed responses against the two new evaluation rubrics and
the implemented CY source. Both passed the scoped explanation requirements. Exact
responses are retained in the task transcript. The transcriptions below normalize
resource paths to package-relative form, line wrapping and inline Markdown formatting;
no substantive answer text was changed.

## Coverage prompt

“Explain only. My cy-batch-estimate help exposes --list. I have no GPU available
and want to see which registered models still lack a saved estimate. Show commands
for the whole catalogue and only yolo11n plus rfdetr/small. An old 960-resolution
numerically_limited result exists for one model, but a newer 640-resolution attempt
failed. Does coverage mean batch=null will work now?”

### Observed coverage response

````text
Use these read-only commands:

```bash
# Whole registered catalogue
cy-batch-estimate --list

# Only these two models
cy-batch-estimate --list --models yolo11n,rfdetr/small
```

They need no GPU or resolution, run no CUDA trials, and do not change the catalogue.
Do not combine --list with measurement controls such as --imgsz, --devices or --refresh.

The report lists only models lacking any usable saved estimate, plus
selected/covered/missing totals and the catalogue path. Missing entries include
recorded failures and diagnostics; “No saved attempt” means no stored evidence.

Your old 960-resolution numerically_limited result counts as covered, even though
a newer 640-resolution attempt failed. Coverage means usable saved evidence exists
somewhere; it does not mean batch=null will work for the current run.

Automatic batch selection requires evidence compatible with the actual resolution,
hardware, model, source/dependency versions and training settings, with dataset
classes and maximum object density within the measured bounds. The 960 result does
not establish compatibility at 640, and a failed attempt supplies no recommendation.
A numerical limitation remains attached to any compatible recommendation; it is not
proof of maximum capacity. Such a result requires a fresh successful single-GPU
confirmation of the last good batch.

For YOLO, the setting is ultralytics.batch=null; native backends use
model.train.batch=null. Without compatible evidence, training stops and requires
measurement or an explicit numeric batch. Coverage also does not make GPU estimates
usable for CPU training.

Resources read: skills/clearml-yolo/SKILL.md and references/usage.md,
training batch estimation section. No CY commands or workloads were executed.
````

Assessment: passed. It supplied both correct commands, offline/read-only semantics,
coverage statuses, diagnostics/totals/path and compatibility limitations without
inventing a new recommendation. Fresh confirmation describes the evidence needed
for a usable numerical result, not a command executed by this evaluation.

## Missing-evidence prompt

“Explain only. --list shows no saved attempt for one model and a failed
confirmation-pending record for another. Was the first skipped because it cannot
fit, and did the second definitely crash? Can I combine --list --imgsz 960
--refresh, or replace a corrupt catalogue with an empty file to get a report?”

### Observed missing-evidence response

````text
Neither conclusion is established:

- “No saved attempt” means no stored evidence. It does not prove the model was
  skipped because it cannot fit.
- A failed confirmation-pending record can describe an attempt still running or
  interrupted. It does not prove a crash; retain its diagnostics and recorded context.

--list --imgsz 960 --refresh is rejected: --imgsz and --refresh are measurement
controls. Keep listing separate:

```bash
cy-batch-estimate --list
```

A separate cy-batch-estimate --imgsz 960 --refresh would execute local CUDA
measurements; none were run here.

Do not replace a corrupt catalogue with an empty file. An unreadable or corrupt
catalogue is an error; preserve it and its diagnostics for recovery. A genuinely
missing catalogue means empty coverage.

Listing reports saved coverage across recorded configurations, not capacity or
compatibility at your current resolution, GPU or training settings.

Resources read: skills/clearml-yolo/SKILL.md and references/usage.md,
including the complete Inspect saved coverage section and relevant measurement guidance.
````

Assessment: passed. It preserved uncertainty, rejected mixed-mode controls and
corrupt-file replacement, distinguished missing from invalid evidence, and performed
no workload or catalogue recovery. The answer did not repeat every output column
or the successful missing-estimate exit code; it made no contradictory claim.
