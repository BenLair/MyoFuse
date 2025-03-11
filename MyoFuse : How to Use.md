# MyoFuse: How to Use

Perform nuclei segmentation and classification with a single pipeline.

## Set Up MyoFuse

### Install Python with Anaconda
The easiest way to get started is by installing the [Anaconda](https://www.anaconda.com/) distribution, which includes both Python and many useful libraries.

### Set up your environment
Download the `requirements.txt` file from the `Setup` section.
Create a Python 3.9 environment and install the required dependencies by running the following commands in your shell:

```bash
# Example commands:
conda create -n MyoFuse_env python=3.9
conda activate MyoFuse_env
pip install -r requirements.txt
# Make sure to be in the directory containing your requirements.txt file
```

### Download necessary files
Download the models and code required to run MyoFuse.
- The notebook `MyoFuse.ipynb` can be found in the `Codes` section of the repository.
- The nuclei segmentation model `MyoNuclei` is provided in the first release of the repository.
- The classification model `MyoFuse.pth` and its configuration file `Config.json` are available in the `Svetlana` folder under `Models/Svetlana`.

---

## Process Your Images
MyoFuse is designed to process two-channel images, containing:

- A **myotube** channel
- A **nuclei** channel

You can also start with pre-processed images, as described later in this guide.

### 1. Prepare your folder
Create a folder in the directory of your choice to hold the images you want to process.

Optionally, you can place the following files in the same folder for convenience:

- `MyoFuse.ipynb`
- `MyoNuclei`
- `MyoFuse.pth`
- `Config.json`

> **Note:** This is not mandatory, as you can specify different paths for these files when running the workflow. However, ensure that `MyoFuse.pth` and `Config.json` are located in the same directory.

### 2. Launch the notebook with Voila
In your shell, navigate (`cd`) to the folder that contains `MyoFuse.ipynb`.
Activate your MyoFuse environment, then run the notebook:

```bash
cd Desktop/Images  # Example directory
conda activate MyoFuse_env
voila MyoFuse.ipynb
```

This will open a new tab in your web browser with the MyoFuse graphical interface, allowing you to process your images.

### 3. Analyze Your Images
Once the interface is open, you can select paths and adjust parameters:

- **Select the folder** containing your images.
- **Select the file** containing the segmentation model (`MyoNuclei`).
- **Select the file** containing the classification model (`MyoFuse.pth`).
- **Myotube channel:** Enter the channel number containing the myotube image. Use `0` for the first channel, `1` for the second, etc.
- **Nuclei channel:** Enter the channel number for the nuclei channel.
- **Cellpose diameter:** Adjust if segmentation results are unsatisfactory. Set to `0` for automatic detection.
- **Normalization max percentile:** Lowering this value will push pixels above this percentile to the maximum intensity (`1.0`).
- **Half patch size:** Change only if a different patch size was used in training.
- **Batch size:** Adjust based on your GPU capacity.
- **Split channels:** Check this box if you need to split multi-channel images before processing.
- **Run segmentation:** Check to include segmentation (needed if no pre-existing nuclei masks).
- **Device for segmentation:** Choose `GPU` if available. If memory issues arise, switch to `CPU`.
- **Run classification:** Check to include classification (needed for normalized images generation).
- **Device for classification:** `GPU` is recommended.
- **Save normalization:** Check to save normalized images.
- **Save prediction:** Check to save prediction images.
- **Downsampling factor:** Increase to reduce resolution and file size of prediction images.

Click **Launch** to start the analysis. A progress bar will indicate the status for each step. Results are exported to a `Fusion Index.xlsx` file in the folder containing your original images.

> **Tip:** If some files cannot be processed, the code will continue with the remaining ones. You can correct or replace problematic images, then click **Resume** to re-run the analysis on those files without reprocessing everything.

---

## Important Note on Multi-Channel Images and Pre-Existing Masks

- If you **manually split** your multi-channel images:
  - Place the **myotube** images in an `Images` subfolder.
  - Place the **nuclei** images in a `Nuclei` subfolder.
- If you **already have nuclei segmentation masks**, place them in a `Masks` subfolder and proceed directly to classification.

> In these cases, make sure to **uncheck any steps** in the interface that are not needed (e.g., segmentation).
