# MyoFuse v1.1.0-rc.5 — Patch Notes

**Status:** Release candidate with new interface for review and validation. The v1.0.0 notebooks remain in `Codes/`.
## Changes since v1.0.0

### Results and resume

- Image-level classification results are saved in `Log/classification_results.json` before a classification checkpoint is marked complete. `Fusion_index.xlsx` is rebuilt from all valid results, preserving earlier rows when **Resume analysis** processes additional images.
- **Launch new analysis** resets checkpoints in the selected image folder rather than the terminal's working directory. It backs up an existing workbook in `Log/` before starting again.
- **Resume analysis** checks input/model paths, sizes and modification times, relevant parameters, and expected output files. A missing mask triggers segmentation again when that stage is enabled; otherwise the affected image is skipped. A changed model triggers the corresponding stage again.
- Checkpoint JSON and the final workbook are written through temporary files and replaced after a successful write.

### Image handling and FI

- Channel extraction uses explicit microscopy axes for TIFF/OME-TIFF/CZI input, with a fallback for plain TIFFs storing channels as the last axis. Both selected channels must form matching 2D images.
- A segmentation with no detected nuclei saves an all-zero mask. The workbook records `No nuclei detected` in a new `Status` column and leaves FI blank instead of reporting 0%.
- Source names differing only by extension are rejected before splitting because they would collide in output folders.

## Output compatibility

Existing workbook columns retain their names; `Status` is appended. Check scripts that require an exact five-column schema or use positional column selection. The output filename is `Fusion_index.xlsx`.

The candidate creates a new result-store format in `Log/`. Old v1.0.0 checkpoints lack parameter signatures: the first **Resume analysis** reprocesses previously completed images once. It saves the previous workbook as `Log/Fusion_index_before_migration_*.xlsx` before rebuilding it. **Launch new analysis** saves a previous workbook as `Log/Fusion_index_before_launch_*.xlsx`.
