Pole Analysis AI System – Complete Project Overview
1. Project Objective

The goal of this project is to build an AI-based pole analysis system that can analyze poles from a user-provided image and provide detailed infrastructure information.

The system should be able to:

Detect poles in an image

Identify the type of pole

Detect components attached to the pole

Identify the material of the pole

Present the results clearly in a structured format

The system should work even when multiple poles are present in the same image.

Height estimation will be added later as a future extension, so it is not part of the current implementation.

2. Expected Input

The system accepts:

An image containing one or more poles

Examples:

Street images

Infrastructure inspection images

Telecom tower images

Utility pole images

Rooftop tower images

Example input:

input_image.jpg
3. Expected Output

For every detected pole, the system should provide:

Pole ID
Pole Type
Pole Material
Detected Components

Example result:

Detected Poles: 3

Pole 1
Type: Utility Pole
Material: Concrete
Components:
 - Transformer
 - Crossarm
 - Insulator

Pole 2
Type: Telecom Pole
Material: Steel
Components:
 - Antenna

Pole 3
Type: Rooftop Tower
Material: Steel
Components:
 - Antenna
4. Pole Types to Identify

The system must classify poles into three categories:

1. Utility Pole

Used for electrical power distribution.

Common characteristics:

Transformers

Crossarms

Insulators

Electrical lines

Possible materials:

Wood
Concrete
Steel
2. Telecom Pole

Used for telecommunications.

Common characteristics:

Antennas

Telecom equipment

Material:

Steel (assumed default)
3. Rooftop Tower

Telecom towers installed on top of buildings.

Common characteristics:

Antennas

Mounted on buildings

Material:

Steel (assumed default)
5. Components to Detect

The system must detect important components attached to poles.

Components include:

Transformer
Insulator
Crossarm
Lamp
Antenna

These components help:

Provide infrastructure details

Assist in determining pole type

Example:

Transformer + Crossarm → Utility Pole
Antenna → Telecom Pole
6. Pole Material Detection

Pole material must be identified.

Materials include:

Wood
Concrete
Steel

Rules:

Telecom Pole → Steel
Rooftop Tower → Steel
Utility Pole → Detect material (wood/concrete/steel)

Material detection is therefore required only for utility poles.

7. System Architecture

The project uses a multi-model AI pipeline.

Instead of training one model for everything, the system is divided into specialized models.

Pipeline overview:

User Input Image
        ↓
Pole Detection Model
        ↓
Crop Each Detected Pole
        ↓
Pole Type Classification Model
        ↓
Component Detection Model
        ↓
Material Classification Model
        ↓
Display Final Results
8. Stage 1 – Pole Detection
Purpose

Detect all poles present in the image.

Model

Object Detection Model

Recommended:

YOLOv8
Detection Class
pole
Output Example
Pole 1 → Bounding Box
Pole 2 → Bounding Box
Pole 3 → Bounding Box

Each detected pole will be assigned a unique ID.

9. Pole Cropping

After detection:

Each pole is cropped from the original image.

Example:

pole_1_crop.jpg
pole_2_crop.jpg
pole_3_crop.jpg

These cropped images are used for further analysis.

10. Stage 2 – Pole Type Classification

Each cropped pole image is passed to a classification model.

Classes
Utility Pole
Telecom Pole
Rooftop Tower
Recommended Models
ResNet50
EfficientNet
MobileNetV3
Output Example
Pole 1 → Utility Pole
Pole 2 → Telecom Pole
Pole 3 → Rooftop Tower
11. Stage 3 – Component Detection

Each pole crop is analyzed for components.

Model

Object detection model

Recommended:

YOLOv8
Classes
Transformer
Insulator
Crossarm
Lamp
Antenna
Example Output
Pole 1
 - Transformer
 - Crossarm
 - Insulator
Pole 2
 - Antenna
12. Stage 4 – Material Detection

Material detection is only required for utility poles.

Material Classes
Wood
Concrete
Steel
Logic
If pole type = telecom → steel
If pole type = rooftop → steel
If pole type = utility → run material classifier
Model

Image classification model.

Recommended:

ResNet
EfficientNet
MobileNet
13. Dataset Requirements

The dataset must contain diverse images.

Target dataset size:

1000 images

Initial images available:

500 existing images

Additional images required:

500 new images
14. Dataset Annotation (CVAT)

Annotation tool:

CVAT
Bounding Box Labels
pole
transformer
insulator
crossarm
lamp
antenna

Optional labels:

human
car
building

These optional labels may help future improvements.

15. Dataset Split

Recommended split:

70% Training
20% Validation
10% Testing
16. User Interface Workflow
Step 1

User uploads an image.

Upload Image
Step 2

System detects poles.

Detected Poles: 3

Bounding boxes appear on the image.

Step 3

User clicks:

Analyze Poles
Step 4

System analyzes each pole and shows:

Pole ID
Pole Type
Material
Components
17. Example Final Result

Example output:

Detected Poles: 3

Pole 1
Type: Utility Pole
Material: Concrete
Components:
 - Transformer
 - Crossarm
 - Insulator

Pole 2
Type: Telecom Pole
Material: Steel
Components:
 - Antenna

Pole 3
Type: Rooftop Tower
Material: Steel
Components:
 - Antenna
18. Future Extension (Phase 2)

A future extension of this system will include:

AI-based pole height estimation

Height will be estimated only when:

Pole Type = Utility Pole

Possible reference objects:

Crossarm
Human
Car

This feature will be implemented after the base system is completed.

19. Technologies Used

Suggested technology stack:

AI Models
YOLOv8
ResNet / EfficientNet
Programming
Python
Libraries
PyTorch
OpenCV
Ultralytics YOLO
NumPy
Annotation Tool
CVAT
Interface (optional)
Streamlit
Flask
Gradio
