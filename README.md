# Multimodal-Sensor-Fusion
Fusing data from a LiDAR and a Camera


## Early Fusion: First we fuse, then we detect
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


## Late Fusion: First we detect, then we fuse
Steps:
1. Detect obstacles in 2D with the camera
  - Used a YOLOv4 network
2. Detect obstacles in 3D with the LiDAR
  - Not implemented: assumed to have the position $(X, Y, Z)$, the 3D bounding box $(W, H, L)$, orientation of 3D bounding box $R_y$
3. Project the 3D obstacles in the image with 3D bounding boxes
  - Project points from 3D to 2D (conver to homo, multiply by $P$, convert to cartesian)
  - Compute the 3D bounding box in the camera frame using $(X, Y, Z, W, H, L , R_y)$
  - Draw the 3D bounding box in the image
  - Convert the 3D bounding box to a 2D bounding box and draw it
4. Fuse the 3D bounding boxes with the 2D bounding boxes
  - Show the LiDAR and Camera boxes on the same image
  - Compute the IOU (Intersection Over Union) metrics for each box
  - Use the Hungarian Algorithm to output the matches given the IOU matrix
5. Create an ultimate fused objects
