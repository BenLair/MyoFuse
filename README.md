# MyoFuse

**MyoFuse** is a fully AI-based workflow to quantify the fusion index in skeletal muscle cell 2D culture in vitro. The workflow is currently at version **v1.0.0**.  
In this repository, you will find all the necessary models, code, and instructions for use. The images used to train the classifier are available on [Zenodo](https://zenodo.org/records/14731491).

---

## How to Use MyoFuse

Detailed instructions to use the workflow are provided in the `MyoFuse: How to Use`pdf file. Instructions for retraining the classifier are also available. Here's a quick overview of the setup process:

1. **Setup Environment**:  
   In the `Setup` folder, you will find a `requirements.txt` file that lists the necessary dependencies. Use it to set up your Python environment:  
   ```bash
   pip install -r requirements.txt

2. **Download Models**:
   The classifier model, `MyoFuse.pth`, is available in the `Models` section of this repository. Download it and place it in the appropriate folder as described in the instructions. If you are using the Svetlana plugin for classification, add the `Svetlana` folder to your project directory, as explained in the instructions. The segmentation model for Cellpose is included in the first release of this repository.
   
3. **Process Your Images**:
    Use the Python notebooks provided in the `Codes` section to process your images and quantify the fusion index. Follow the step-by-step guide `MyoFuse: How to Use` to ensure accurate results.

4. **Test Images**:
    If you want to make sure that your configuration is working, you can download test images from the `Test Images` release to run the MyoFuse.ipynb code. A large image is also provided to test potential RAM and GPU memory issues.

## Licence
This project is licensed under the MIT License.

## Citation
If you use MyoFuse, please cite ...

## Contact
If you have any questions or suggestions, feel free to reach out:

Email: lair.b1jamin@gmail.com
