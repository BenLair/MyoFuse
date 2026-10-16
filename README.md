# MyoFuse

MyoFuse quantifies the fusion index (FI) in two-channel fluorescence images of skeletal muscle cell cultures. It segments nuclei with Cellpose, classifies each detected nucleus as inside or outside a myotube, and reports the percentage classified as inside.

**Current release candidate: v1.1.0-rc.5.** The published v1.0.0 notebooks remain in `Codes/`. The candidate notebooks are separate so existing workflows can be compared before adopting the update. This candidate has passed synthetic workflow checks; image reading and inference still need validation on representative microscopy files and the released models.

## Quick start

1. Clone or download this repository and create the environment from the repository root:

   ```bash
   conda env create -f Setup/myofuse_linux.yml
   conda activate myofuse_env
   ```

   On Windows, use `Setup/myofuse_win.yml` instead.

2. Download the `MyoNuclei` Cellpose segmentation model from the [MyoFuse releases](https://github.com/BenLair/MyoFuse/releases). The classifier `MyoFuse.pth` and its `Config.json` are in [`Models/Svetlana`](Models/Svetlana/); keep those two files together.

3. Launch the notebook with Voilà from the repository root:

   ```bash
   voila Codes/MyoFuse_linux_v1.1.0-rc.5.ipynb
   ```

   On Windows, run `voila Codes/MyoFuse_win_v1.1.0-rc.5.ipynb`. Select your image folder and model files in the browser interface.

4. Use **Launch new analysis** for a fresh run or **Resume analysis** to continue an interrupted run. Image-level results are exported to `Fusion_index.xlsx` in the selected image folder.

See the [usage guide](MyoFuse%20%3A%20How%20to%20Use.md) for folder layouts, parameters, outputs, and upgrade behavior. The [classifier retraining guide](How%20to%20re-train%20the%20classifier.md) explains how to adapt the model. Changes in this candidate are summarized in the [patch notes](PATCH_NOTES_v1.1.0-rc.4.md).

## Inputs and models

- Original images: two-channel `.tif`, `.tiff`, or `.czi` files, including OME-TIFF. The candidate selects the first time point and first Z plane. Check the channel assignments and first output images before processing a large dataset.
- Prepared images: matching myotube images in `Images/`, nuclear images in `Nuclei/`, and labeled nuclei masks in `Masks/`. You can turn off stages that are already complete.
- Segmentation model: `MyoNuclei`, distributed through the releases.
- Classification model: `Models/Svetlana/MyoFuse.pth`, with `Config.json` beside it.

The classifier training images are available on [Zenodo](https://zenodo.org/records/14731491). Test images are available from the [project releases](https://github.com/BenLair/MyoFuse/releases).

## Results

`Fusion_index.xlsx` contains one row per successfully processed image: total nuclei, nuclei inside and outside myotubes, FI in percent, and a `Status` column. Images with no detected nuclei have a blank FI and `No nuclei detected` status. Failed or skipped images are not reported as completed; review the messages in Voilà and resume after correcting the input.

The selected image folder also holds intermediate `Images/`, `Nuclei/`, `Masks/`, optional `Predictions/` and `Normalized_images/`, and a `Log/` folder with checkpoints, per-image results, and workbook backups.

## Citation

If you use MyoFuse, please cite: Lair B. *et al.* “MyoFuse is a fully AI-based workflow for automated quantification of skeletal muscle cell fusion in vitro.” *Scientific Reports* **16**, 9387 (2026). [https://doi.org/10.1038/s41598-026-40047-y](https://doi.org/10.1038/s41598-026-40047-y).

## License and contact

MyoFuse is released under the [MIT License](LICENSE). For questions, contact lair.b1jamin@gmail.com.
