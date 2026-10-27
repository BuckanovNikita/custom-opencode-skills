# Custom training augmentation

Custom JSON augmentation is available in
[clearml-yolo v0.39.0](https://github.com/BuckanovNikita/clearml-yolo/tree/v0.39.0).
Check the installed command schema before using it. The
[verification record](https://github.com/BuckanovNikita/clearml-yolo/blob/v0.39.0/specs/004-custom-augmentation/evidence/verification.md)
records native coverage and the remaining pretrained checks.

Use this reference for supplied Albumentations pipelines, Mosaic, or conflicts with
native preprocessing. Supported commands are `cy-train` and training inside `cy`.
Prediction, validation and export keep their checkpoint preprocessing.

## Select and save a pipeline

`transforms=default` retains existing backend augmentation. Save an ordinary
Albumentations `Compose` with `A.save()` for custom mode:

```python
import albumentations as A

pipeline = A.Compose(
    [A.HorizontalFlip(p=0.5), A.RandomBrightnessContrast(p=0.2)],
    bbox_params=A.BboxParams(format="pascal_voc", label_fields=["class_labels"]),
)
A.save(pipeline, "training.json")
```

```bash
cy-train model=rfdetr/nano ground_truth=ground_truth.csv \
  transforms=albumentations transforms.pipeline_file=training.json \
  model.train.epochs=1 model.train.device=0 \
  clearml.project_name=experiments clearml.tags=[custom-augmentation]
```

For YOLO select `model=ultralytics` and use `ultralytics.epochs` and
`ultralytics.device`. The transform selector also works for DEIMv2 and Faster R-CNN.
CY 0.41.0 adds the same interface to all six FCOS presets; full native FCOS
GPU acceptance remains pending.
Generate editable profiles with `cy-init-config configs --command cy-train
--model rfdetr/nano`; preserve generated defaults and specialization groups. Fresh
exports from CY 0.41.0 include local command bases and all referenced application YAML.

## Ownership and conflicts

Custom mode replaces native random augmentation, including native Mosaic, MixUp,
color jitter, flips and random scaling. Composed profiles expose disabled values.
Conflicting explicit native settings are reset with a warning before execution;
inspect the resulting canonical `run` configuration in ClearML.

The pipeline receives original RGB `uint8` images and matching boxes. Supported
serialized bbox formats are `pascal_voc`, `coco`, `yolo` and `albumentations`, with
one class-label field. The adapter keeps instance/crowd metadata aligned and accepts
empty targets, including crops that remove all objects.

Each backend then performs its required final sizing and tensor conversion once:
YOLO letterboxing and division by 255; DEIMv2 resize/normalization and box conversion;
RF-DETR deterministic sizing/normalization; Faster R-CNN/FCOS native normalization and
resizing. A user `Resize` does not replace these model input contracts. Omit it unless
it is part of the intended augmentation geometry.

Do not add `Normalize`, tensor conversion, `Lambda`, custom executable classes, or
unsupported extra targets. Those inputs fail validation. Standard nested composition
is supported subject to these constraints. Backend-owned normalization avoids
applying it twice.

Saved batch estimates describe native augmentation profiles. Custom JSON requires an
explicit numeric training batch; do not reuse a native estimate through `batch=null`.
See [batch estimation](usage.md#training-batch-estimation) for strict lookup behavior.

## Mosaic across models

Put one `A.Mosaic(...)` first in the top-level `Compose`. Nested, repeated or later
Mosaic transforms are rejected. For example, with the imports above:

```python
pipeline = A.Compose(
    [A.Mosaic(grid_yx=(2, 2), target_size=(640, 640), p=0.5),
     A.HorizontalFlip(p=0.5)],
    bbox_params=A.BboxParams(format="pascal_voc", label_fields=["class_labels"]),
)
A.save(pipeline, "mosaic.json")
```

The adapter supplies raw training donors automatically: grid cells minus one slots,
distinct other samples when available, replacement for small datasets, and the primary
sample only when it is the sole training image. Validation/test images are never donors.
Do not additionally enable backend-native Mosaic.

## Replay and verification

Before queue submission or execution, validated JSON content is frozen with its SHA-256
in canonical configuration. Agent/DDP replay uses that snapshot even if the source file
is absent. The snapshot is internal replay state, not a public inline alternative.
Selecting a different file replaces it; malformed or modified content fails validation.

Inspect effective native settings and frozen JSON separately. Composition proves
neither optimization nor checkpoint/publication success. Native verification observes
actual parameter updates, finite losses, and downloaded checkpoint reload.
The existing Severstal matrix covers YOLO26n, DEIMv2, RF-DETR and Faster R-CNN in
default, JSON and JSON-with-Mosaic modes; it does not establish FCOS acceptance.
Full dataset training is a separate workload requiring its own authorization.
