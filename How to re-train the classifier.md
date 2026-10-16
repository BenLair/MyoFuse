# How to Re-Train the MyoFuse Classifier

Use images from the cell type and acquisition conditions you plan to analyze. Keep training and independent evaluation images separate. For the complete workflow and folder structure, see the [usage guide](MyoFuse%20%3A%20How%20to%20Use.md).

## Prepare images and masks

Use `MyoFuse_linux_v1.1.0-rc.4.ipynb` or `MyoFuse_win_v1.1.0-rc.4.ipynb` in Voilà to split channels and segment nuclei. Outputs are stored in `Images/`, `Nuclei/`, and `Masks/` inside the selected image folder. Check masks visually before annotation.

**Normalization in the candidate notebook happens during classification.** If you need the `Normalized_images/` files for training, enable both **Run classification and quantify FI** and **Save normalized images**, with a valid classifier and `Config.json`. Running splitting and segmentation alone does not create them. You can alternatively normalize training images separately with a documented method.

For Svetlana annotation, prepare a folder with myotube images and their labeled masks. If using the notebook's normalized images, use files from `Normalized_images/` as image inputs and the matching files from `Masks/` as masks. Preserve matching filenames.

## Annotate in Svetlana

1. Open Svetlana in Napari and load the prepared images and masks.
2. Use **200 × 200 pixels** as the patch size when retraining a model intended to run with the notebook's default half patch size of `100`.
3. Label a nucleus outside a myotube with `1` and a nucleus inside a myotube with `2`, following the criteria in the [MyoFuse paper](https://doi.org/10.1038/s41598-026-40047-y).
4. Save annotation progress and retain Svetlana's project files. Add source images before starting an annotation project if your Svetlana workflow does not permit adding them later.

## Train and evaluate

Load the labeled dataset in Svetlana and select `MyoFuse.pth` as the custom network if you want to fine-tune it. Use normalization and class weighting appropriate to your prepared images; record augmentation settings with the trained model. The published classifier used class weights plus rotations and flips. Applying every available augmentation is not a requirement.

Evaluate on independent images. Inspect both nucleus-level classification and image-level FI, including mistakes near myotube boundaries and in dense nuclei clusters. Save the selected `.pth` file with its matching `Config.json` before using it in the complete Voilà workflow. Keep the notebook half patch size consistent with the training patch size.

## Quantify FI from Svetlana predictions

For predictions generated directly by Svetlana, `Codes/Fusion Index.ipynb` is a separate legacy converter for a `prediction_regionprops.xlsx` export. Edit its input path before running. It assumes image paths in the first spreadsheet column, labels in the eighth, and `1` means outside / `2` means inside. Check those columns and conventions before using its output. The complete MyoFuse notebooks calculate FI directly and do not require this converter.
