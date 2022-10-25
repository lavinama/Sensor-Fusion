# Sensor-Fusion
Fusing data from a LiDAR and a Camera. This is an example of the output of the early fusion algorithm:

https://user-images.githubusercontent.com/70475094/197755891-984b8950-6943-49a9-9738-f308cc33b35d.mp4

## Early Fusion: First we fuse, then we detect (`Early_Fusion.ipynb`)
Steps:
1. Project the Point Clouds (3D) to the Image (2D):
  - Convert the points from the LiDAR frame to the Image frame.
  $Y = P \times R_0 \times R|t \times X$\
  where:
    - $X$ 3D coordinates
    - $R|t$ Rotation and translation matrix from the LiDAR to the Camera
    - $R_0$ Stereo vision rectifier
    - $P$ Intrinsic and extrinsic camera calibration parameters 
  - Select the points that are visible in the image.
2. Detect Obstacles in 2D (Camera)
  - Used a YOLOv4 network
3. Fuse the results
  - Shrink the bounding boxes
  - Filter the outliers using one-sigma
  - Retrieve the best distance using the average


## Late Fusion: First we detect, then we fuse (`Late_Fusion.ipynb`)
Steps:
1. Detect obstacles in 2D with the camera
  - Used a YOLOv4 network
2. Detect obstacles in 3D with the LiDAR
  - Not implemented: assumed to have the position $(X, Y, Z)$, the 3D bounding box $(W, H, L)$, orientation of 3D bounding box $R_y$
3. Project the 3D obstacles in the image with 3D bounding boxes
  - Project points from 3D to 2D (conver to homo, multiply by $P$, convert to cartesian)
  - Compute the 3D bounding box in the camera frame using $(X, Y, Z, W, H, L , R_y)$
  - Project the 3D bounding box in the camera frame to the image
  - Convert the 3D bounding box to a 2D bounding box and draw it
4. Fuse the 3D bounding boxes with the 2D bounding boxes
  - Show the LiDAR and Camera boxes on the same image
  - Compute the IOU (Intersection Over Union) metrics for each box
  - Use the Hungarian Algorithm to output the matches given the IOU matrix
5. Create an ultimate fused objects

## Downlaoding the Dataset
**Videos:**
1. Download the videos using the following links:
  * [video_1](https://drive.google.com/drive/folders/1pjYqlStUbtcI46mKhakv0aAyZWxgNnmT)
  * [video_2](https://drive.google.com/drive/folders/1Fg5lW9eC61Fyk-y62EDHmIr4ezBHEfQk?usp=sharing)
  * [video_3](https://drive.google.com/drive/folders/1s4n_ukH7Ujp1V6u8GaYUZrLhNNDzZlmO?usp=sharing)
  * [video_4](https://drive.google.com/drive/folders/1MrRHWnuO2uZnmXCC38t3vq2d1lD4x2HJ?usp=sharing)
2. Store them under `data/videos/*`

**Yolov4:**
1. Create a folder in the main directory called `Yolov4/*`
2. Download the following files:
  * [classes.txt](https://drive.google.com/file/d/10O9H8syamaFgrBVekxOxH7RBwiTKYBFM/view?usp=drive_web)
  * [coco.names](https://drive.google.com/file/d/1pj4tuL9xockS0LO90zyy21xPKWTvx-xF/view?usp=drive_web)
  * [yolov4-tiny.cfg](https://drive.google.com/file/d/1B0mTKm_ugLUBUeabwvMpdY2lhz4XYWjZ/view?usp=drive_web)
  * [yolov4.cfg](https://drive.google.com/file/d/1KXySj9KOSJXewowAbBp5mMnf14A0BPvk/view?usp=drive_web)
  * [yolov4.weights](https://drive.google.com/file/d/1G7NrrJnOZU6TcKONc0B5ZuGvxbhoVsyM/view?usp=drive_web)
3. Store these files in the directory `Yolov4/*`

**Output**
1. Create a folder in the main directory called `output/*`. Here all the fused videos will be stored.

## Extra useful files
* `point_clouds/*`: directory containing all the point clouds
* `bin_to_pcd.py`: if the LiDAR point clouds are in `bin` format, this Python script converts them to `pcd` format.
* `calc_rotation_matrix.ipynb`: calculates the rotation matrix given the angles of rotation

