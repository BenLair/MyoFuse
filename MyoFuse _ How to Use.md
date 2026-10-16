# MyoFuse: How to Use (v1.1.0-rc.5)

This guide describes the Linux and Windows notebooks for two-channel microscopy images. The v1.0.0 notebooks remain in `Codes/` for comparison.

## 1. Install and launch

Install [Conda](https://docs.conda.io/) and create the environment from the repository root:

```bash
# Linux
conda env create -f Setup/myofuse_linux.yml
conda activate myofuse_env
voila Codes/MyoFuse_linux_v1.1.0-rc.5.ipynb
```

On Windows, use `Setup/myofuse_win.yml` and launch `Codes/MyoFuse_win_v1.1.0-rc.5.ipynb`. Run these commands from the folder where you downloaded the repository. Voilà opens the interface in a browser. Keep the terminal running during analysis.

Download the `MyoNuclei` Cellpose model from the [releases](https://github.com/BenLair/MyoFuse/releases). The classification model `MyoFuse.pth` and `Config.json` are in `Models/Svetlana/`. These two files must remain in the same folder. You can select the models from any location in the interface.

## 2. Prepare the image folder

Put original two-channel `.tif`, `.tiff`, or `.czi` files in one folder. OME-TIFF is supported. Channel indices start at **0**. The notebook uses the first time point and first Z plane. Confirm that these are the planes you intend to measure. The CZI reader is opened without mosaic reconstruction.

Use distinct names before the extension: `sample.tif` and `sample.tiff` cannot be processed together because they would produce the same output name.

You can begin at a later stage with prepared files:

| Available files | Folder in the selected image directory | Interface setting |
| --- | --- | --- |
| Myotube channel images | `Images/` | Turn off **Split original channels** |
| Nuclei channel images | `Nuclei/` | Turn off **Split original channels** |
| Labeled nuclei masks | `Masks/` | Turn off **Run nuclei segmentation** |

Corresponding images and masks must have matching filenames, including the extension (for example, `sample.tif` in each folder). Each mask is a two-dimensional label image with background `0` and a distinct positive integer for each nucleus. To classify existing masks, provide the corresponding `Images/` files, select the classifier, and keep classification enabled.

## 3. Set parameters

The interface is arranged in six sections: files, image channels, segmentation, classification, outputs, and run controls.

| Setting | Meaning |
| --- | --- |
| **Myotube channel / Nuclei channel** | Channel indices in the original image. |
| **Split original channels** | Read original images and write separate `Images/` and `Nuclei/` TIFFs. Turn off if those folders already contain prepared channels. |
| **Run nuclei segmentation** | Create labeled TIFF masks in `Masks/`. Turn off when using existing masks. |
| **Cellpose diameter** | Expected nuclear diameter in pixels; `0` requests Cellpose automatic diameter estimation. |
| **Segmentation device / Classifier device** | Select GPU or CPU. Classification falls back to CPU if CUDA is unavailable. |
| **Run classification and quantify FI** | Classify nuclei and create `Fusion_index.xlsx`. Normalized images are generated during this stage, which is required if **Save normalized images** is selected. |
| **Upper percentile** | Upper intensity threshold used during myotube image normalization. Default: `99.7`. |
| **Half patch size** | Half of the classifier crop width, in pixels. Default `100` gives a `200 × 200` patch. Match training. |
| **Batch size** | Nuclei per classifier batch. Lower it if GPU memory is insufficient. |
| **Save annotated prediction images** | Write overlays to `Predictions/`. |
| **Save normalized images** | Write normalized images to `Normalized_images/` during classification. |
| **Preview downsampling** | Downsample annotated prediction images; `1` keeps full resolution. |

For very large images, saving full-resolution prediction overlays can consume substantial memory. Consider disabling annotated predictions or increasing the preview downsampling factor. This setting does not reduce the memory needed for segmentation or classification.

## 4. Launch or resume

- **Launch new analysis** backs up an existing `Fusion_index.xlsx` into `Log/`, resets checkpoints and the per-image result store in the selected folder, and processes selected inputs afresh. It does not delete your `Images/`, `Nuclei/`, or `Masks/` folders.
- **Resume analysis** checks saved per-image results, input and model file paths, sizes and modification times, processing parameters, and expected output files. It skips valid completed stages and processes missing or changed items. Excel is rebuilt from all valid image-level results, retaining earlier completed rows.

Old v1.0.0 checkpoints lack the parameter signatures needed for validation. The first **Resume analysis** with this candidate therefore reprocesses those images once and backs up the previous workbook in `Log/` before rebuilding it. Subsequent resumes can skip validated images. Keep original settings and models available until you have checked the new outputs.

If an image cannot be processed, the other images continue. Correct the problematic file and use **Resume analysis**. Review the final count shown in Voilà: a partial run reports fewer valid image rows than eligible input images.

## 5. Inspect the outputs

The selected image folder contains:

```text
your_image_folder/
├── Images/                  # Split myotube channel
├── Nuclei/                  # Split nuclei channel
├── Masks/                   # Labeled nuclei masks
├── Predictions/             # Optional annotated prediction TIFFs
├── Normalized_images/       # Optional normalized TIFFs
├── Log/                     # Checkpoints, result store, backups
└── Fusion_index.xlsx        # Image-level summary
```

The workbook columns are `Image Name`, `Total Number of Nuclei`, `Nuclei In`, `Nuclei Out`, `Fusion Index (%)`, and `Status`. FI is `100 × Nuclei In / Total Number of Nuclei`. Failed or skipped images have no completed row.

`Log/classification_results.json` holds image-level results used to rebuild the workbook. Keep `Log/` when moving a dataset if you intend to use **Resume analysis**.
