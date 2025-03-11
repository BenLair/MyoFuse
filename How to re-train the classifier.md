# How to Re-Train the Classifier

To train the model to perform better for your specific use case, begin by selecting relevant images of the same cell type and acquisition conditions as those you plan to analyze.

---

## Pre-Processing
Before annotating the images, the following steps are necessary. They can all be performed using the **MyoFuse.ipynb** notebook, as described in the [MyoFuse: How to Use](#) guide, without performing the classification step.

1. **Channel Splitting:** Separate the channels of your original images to create individual files for nuclei and myotubes.
2. **Nuclei Segmentation:** Perform segmentation of the nuclei using the custom Cellpose model.
3. **Image Normalization:** Normalize myotube images to prepare them for training.
4. **File Preparation:** Transfer the resulting `Images` and `Masks` folders into a new folder with a name of your choice to prepare them for annotation.

---

## Annotation
1. **Loading Images:** Open the **Svetlana annotation plugin** and load your images by selecting the folder created in the pre-processing step.
2. **Parameters:** Keep the parameters unchanged:
   - **Patch size:** 200 pixels
   - **Number of labels:** Unchanged
3. **Annotation Process:**
   - Use your keyboard for annotation:
     - Press `1` if the nucleus is located **outside** a myotube.
     - Press `2` if the nucleus is located **inside** a myotube.
   - Follow the method described in the article, using the local loss of myotube-specific fluorescence as the basis for annotations.
4. **Saving Progress:** Save your annotations regularly. If needed, you can resume labeling later by using the `Svetlana` folder generated when first saving your annotations.

> **Important:** Insert additional images into the initial folder **before** starting the annotation process. While you are not required to annotate all the images, you won’t be able to add new ones later if you want to improve the results.

---

## Training
1. **Loading Data:** Load your data by selecting the `label` file from the `Svetlana` folder.
2. **Custom Network:** Use the **Load custom network** option and select the `MyoFuse.pth` file.
3. **Training Parameters:**
   - **Data Normalization:** Use "Min-Max normalization."
   - **Class Weights:** Keep the "Apply Class Weights" option activated.
   - **Data Augmentation:** Activate **all** options in the "Data Augmentation Type" section for optimal results.
4. **Evaluation:** After training, assess the model’s accuracy using the prediction module with new images that were **not used** for training. These new images must be pre-processed like the training images (**channel splitting, segmentation, normalization**). Test different models saved during the training process (found in the `Svetlana` folder).

---

## Prediction
Once the model achieves satisfactory accuracy, analyze your images using the **prediction module**. You can use the `Fusion Index.ipynb` notebook provided in the `Codes` section to convert the output file from Svetlana into Fusion index quantification. Alternatively, use your new model in the complete `MyoFuse.ipynb` code.

For more details, refer to the `MyoFuse: How to Use` guide.
