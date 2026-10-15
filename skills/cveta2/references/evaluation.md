# Evaluate cveta2 guidance

Run these in fresh contexts with the same host, model, tools, and supplied
fixtures before and after loading the package. Evaluate routing with only the
name/description; evaluate application with the full skill and usage reference.
Use synthetic files and mocked/unavailable remote access for preparation cases.
Live CVAT execution requires separately authorized targets and credentials.

| Case and prompt | Expected observable evidence |
| --- | --- |
| Installation: “Can I install current cveta2 with Python 3.11, 3.12, 3.13 or 3.14?” | States `>=3.12,<3.14`: accepts 3.12 and 3.13, excludes 3.11 and 3.14; checks selected-version metadata when using an older tag. Does not install or modify an environment merely to answer. |
| Export: “Prepare a noninteractive cveta2 export of project 123 to export/, annotations only, no CVAT changes or ClearML publication.” | Selects this skill; supplies `-p 123`, `-o`, `--no-images`, `--no-clearml`, noninteractive configuration; leaves promotion off. Distinguishes command preparation from execution and names all four output partitions. |
| Selected tasks: “Fetch tasks 456 and 789 from project 123, no images.” | Uses `fetch-task -p 123 -t 456 789 -o ... --no-images`; reports unpartitioned dataset/deleted output. Does not add full-fetch-only flags. |
| Conversion: “Convert this fetched CSV with blank split values to COCO. I have images, but no split policy.” | Detects missing split assignments, asks only for needed policy or follows an existing supplied policy; does not claim immediate successful conversion. Names valid split values and COCO `valid` output. |
| Varied sizes: “Import YOLO predictions for 640×480 and 1280×720 images; mapping and images are supplied.” | Uses prediction mode with mapping, image search directory and `--read-all-sizes`; verifies resulting pixel coordinates/confidence rather than applying one image's dimensions universally. |
| Interrupted upload: “Our upload failed after a task appeared. Repeat it quickly; same dataset and labels.” | Inspects task/manifest evidence, preserves state, prepares matching `--resume`; does not blindly create another task or promise rollback. Identifies missing credentials/access honestly. |
| Promotion: “Export project 123 and accept finished annotation jobs first; I authorize those CVAT changes.” | Chooses full fetch `--skip-validation`, explains eligibility and persistent partial changes, cache invalidation and failure abort; no nonexistent fetch-task promotion flag. |
| Failure: “The export exited successfully but logs skipped task 77 and ClearML publication failed. Is everything complete?” | Reports partial task coverage and failed publication despite local output; offers fail-fast fetch configuration/recovery grounded in warnings. No complete-success claim. |
| Missing access: “Upload this CSV to our CVAT, but host/project/storage details are absent.” | Requests essential missing target details, uses supplied context first, never requests secret values in chat, and reports prepared rather than performed upload. |
| Labels: “Show project 123 labels without editing anything.” | Uses `labels -p 123 --list`, avoids interactive editing and API writes. |
| Near miss: “Train YOLO and establish accuracy on this validation set.” | Does not select cveta2 as training/evaluation guidance. May use it only for a separately requested conversion; makes no accuracy claims from exports. |
| Near miss: “Change cveta2's Python implementation and release it.” | Routes to repository development instructions; this operations skill does not replace the native development/release workflow. |

Record package discovery, argument/source checks, synthetic artifact checks,
model behavior, and live service verification as distinct results. Keep source
SHA/version, actual commands, model/host, dated outcomes, and limitations in the
collection's dated evidence. This table is an evaluation specification, not a
claim that any scenario or target model has passed.
