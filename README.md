# Multimodal-Sensor-Fusion
Fusing data from a LiDAR and a Camera


## Early Fusion
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
  - Filter the outliers using $\text-\sigma$
  - Retrieve the best distance using the average


## Late Fusion
