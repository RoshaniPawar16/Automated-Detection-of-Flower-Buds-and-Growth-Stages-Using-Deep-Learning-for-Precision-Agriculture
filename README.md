# Automated Detection of Flower Buds and Growth Stages

## 1. Project Goal

This project aims to automate the detection of flower buds and their different growth stages using deep learning. By leveraging computer vision, specifically the YOLO (You Only Look Once) object detection model, we can analyze images of plants to identify and classify flower buds. This capability is a step towards precision agriculture, where automated monitoring can lead to more efficient crop management, timely interventions, and potentially higher yields.

The primary objective is to build and train a model that can accurately locate flower buds in an image and determine their stage of development.

## 2. The Dataset

The images used for this project are from the **PlantVillage dataset**. This is a public dataset containing thousands of images of various plants under different conditions, both healthy and diseased.

For this project, we are focusing on tomato plants. The dataset is structured as follows:

```
datasets/
└── PlantVillage/
    ├── train/
    │   ├── Tomato_healthy/
    │   └── ... (other classes)
    └── valid/
        ├── Tomato_healthy/
        └── ... (other classes)
```

- **`train/`**: Contains the images used for training the model.
- **`valid/`**: Contains the images used for validating the model's performance during training.

Each sub-directory within `train` and `valid` represents a different class of plant or disease, allowing the model to learn from a diverse set of examples.

## 3. The Model: YOLOv8

We are using **YOLOv8**, the latest version of the You Only Look Once family of models. YOLO is known for its high speed and accuracy in real-time object detection, making it a suitable choice for agricultural applications where efficiency can be important.

We started with a pre-trained `yolov8n.pt` (YOLOv8 Nano) model, which has been trained on the large-scale COCO dataset. This technique, known as **transfer learning**, allows us to fine-tune the model on our specific dataset of flower buds, saving significant training time and computational resources.

## 4. Project Structure

Here is an overview of the key files and directories in this project:

```
├── datasets/
│   └── PlantVillage/       # Image data for training and validation.
│
├── runs/
│   └── detect/             # Output directory for all training runs.
│       ├── train/          # Each 'train' folder contains results from a session.
│       │   ├── args.yaml   # Configuration for the training run.
│       │   └── weights/
│       │       ├── best.pt # The model with the best validation performance.
│       │       └── last.pt # The model from the final training epoch.
│       └── ...
│
├── temp.ipynb              # A Jupyter Notebook for experiments and analysis.
│
├── yolov8n.pt              # The base pre-trained YOLOv8 model.
│
└── README.md               # This file.
```

- **`datasets/`**: Holds all the image data.
- **`runs/detect/`**: This is where YOLOv8 saves the results of each training session. Inside, you will find multiple `train` folders (e.g., `train`, `train2`, etc.), each corresponding to a different experiment. The most important files in these folders are the trained model weights (`best.pt`).
- **`temp.ipynb`**: A notebook for testing code, visualizing data, or running inference on the trained models.
- **`yolov8n.pt`**: The starting point for our model training.

## 5. How to Use

### Prerequisites
- Python 3.8 or later
- Pip package manager

### Installation
1.  Clone this repository:
    ```bash
    git clone https://github.com/RoshaniPawar16/Automated-Detection-of-Flower-Buds-and-Growth-Stages-Using-Deep-Learning-for-Precision-Agriculture.git
    cd Automated-Detection-of-Flower-Buds-and-Growth-Stages-Using-Deep-Learning-for-Precision-Agriculture
    ```

2.  Install the required libraries. The primary dependency is the `ultralytics` package which contains YOLOv8.
    ```bash
    pip install ultralytics
    ```

### Training
To train the model on the provided dataset, you can use the YOLO command-line interface. You would typically create a `.yaml` file to define the dataset paths and classes, and then start the training.

Example of a training command:
```bash
yolo task=detect mode=train model=yolov8n.pt data=your_dataset_config.yaml epochs=100 imgsz=640
```
The results, including trained models and performance metrics, will be saved in the `runs/detect/` directory.

### Inference
To use a trained model to make predictions on new images:
```bash
yolo task=detect mode=predict model=runs/detect/train/weights/best.pt source='path/to/your/image.jpg'
```
This will run the detection and save the annotated image with bounding boxes around the detected flower buds.