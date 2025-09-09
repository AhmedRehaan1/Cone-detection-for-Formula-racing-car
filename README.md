# Formula Car Cone Detection

This repository contains a Python-based solution for real-time cone detection on a formula car, utilizing computer vision techniques without relying on machine learning or deep learning models.

https://github.com/user-attachments/assets/ede5f649-2ba2-4dd0-8a68-1cbb4006cb8c

## Table of Contents
- [Project Description](#project-description)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [How it Works](#how-it-works)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Project Description

Developed for the Curt Autonomous team, this project addresses the critical need for accurate and real-time detection of cones in a formula car racing environment. The solution is designed to operate on live camera feeds from a camera mounted on top of the vehicle. Unlike traditional approaches that often leverage complex machine learning or deep learning models, this system employs a purely computer vision-based methodology, making it lightweight, efficient, and suitable for embedded systems with limited computational resources.

The primary goal is to identify and differentiate between yellow and blue cones, which are standard markers in formula car tracks. The system processes video frames to isolate these cones, providing essential spatial information for autonomous navigation and race strategy.

## Features

- **Real-time Processing**: Designed for live video streams from a formula car camera.
- **Color-based Detection**: Utilizes HSV color space to accurately identify yellow and blue cones.
- **Masking for Noise Reduction**: Implements a masking technique to exclude the car body from detection, preventing false positives.
- **Morphological Operations**: Employs closing morphology (dilation followed by erosion) to reduce noise and improve the integrity of detected cone shapes.
- **Contour Filtering**: Filters detected contours based on area to eliminate small artifacts and overly large, irrelevant objects, ensuring only valid cones are considered.
- **Bounding Box and Labeling**: Draws bounding boxes around detected cones and labels them with their respective colors (Yellow Cone, Blue Cone) for clear visualization.
- **Lightweight**: Achieves cone detection without the need for computationally intensive machine learning or deep learning frameworks.

## Installation

To set up the project locally, follow these steps:

### Prerequisites

Ensure you have Python 3.x installed on your system. This project relies on the following Python libraries:

- `opencv-python`
- `numpy`

### Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/formula-car-cone-detection.git
cd formula-car-cone-detection
```

### Install Dependencies

It is recommended to use a virtual environment to manage project dependencies.

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
pip install opencv-python numpy
```

## Usage

To run the cone detection script, execute the `Cones_detection_curt.py` file. Ensure that the `Car.mp4` video file is present in the same directory as the script, as it is used as the input video stream.

```bash
python Cones_detection_curt.py
```

The script will open a window displaying the video feed with detected cones highlighted by bounding boxes and labels. Press the 'd' key to exit the application.

## How it Works

The `Cones_detection_curt.py` script processes each frame of the input video (`Car.mp4`) through a series of computer vision operations:

1.  **Frame Acquisition**: Reads frames sequentially from the `Car.mp4` video file.
2.  **Car Body Masking**: Applies a mask to each frame to black out the area occupied by the car's body. This prevents the system from detecting parts of the car as cones, which would lead to false positives. The `apply_mask` function creates a black mask over a specified rectangular region (`BLOCK = [800, 400, 400, 800]`) and applies it to the image.
3.  **Color Space Conversion**: Converts the masked BGR image to the HSV (Hue, Saturation, Value) color space. HSV is preferred for color-based object detection as it separates color information (Hue) from intensity (Value) and purity (Saturation), making it more robust to lighting variations.
4.  **Color Thresholding**: Defines specific HSV ranges for yellow and blue colors. The `cv2.inRange` function is then used to create binary masks for each color, where pixels falling within the defined range are white (255) and others are black (0).
    -   **Yellow Cones**: `yellow_lower = [20, 100, 100]`, `yellow_upper = [30, 255, 255]`
    -   **Blue Cones**: `blue_lower = [90, 50, 150]`, `blue_upper = [130, 255, 255]`
5.  **Morphological Closing**: Applies a morphological closing operation (`cv2.MORPH_CLOSE`) to both yellow and blue masks. This operation (dilation followed by erosion) helps to close small holes within the detected cone regions and smooth their boundaries, reducing noise and improving the shape of the detected objects.
6.  **Contour Detection**: Finds contours in the processed binary masks using `cv2.findContours`. Contours are continuous lines joining all points along the boundary of an image region that have the same color or intensity.
7.  **Contour Filtering**: The `filter_contours` function is applied to the detected contours. This function filters out contours whose areas fall outside a specified range (`min_area=200`, `max_area=5000`), ensuring that only objects resembling cones in size are considered.
8.  **Drawing Bounding Boxes and Labels**: For each filtered contour, a bounding rectangle is calculated using `cv2.boundingRect`. This rectangle is then drawn on the original image, and a text label (


"Yellow Cone" or "Blue Cone") is placed above it. This provides a clear visual indication of the detected cones.

## Contributing

We welcome contributions to improve this cone detection system. Please feel free to fork the repository, make your changes, and submit a pull request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the `LICENSE` file for details (if applicable, otherwise state 'No specific license is provided for this project at this time.').

## Contact

For any inquiries or further information, please contact:

- **Ahmed Mostafa Rehaan** (Original Author)
  - *Curt Autonomous Team*

---

*This README was generated by Manus AI.*

