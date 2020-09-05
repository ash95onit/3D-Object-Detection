## 3D Object tracking

### Match 3D points
The function matchBoundingboxes takes in the matches, current and previous frame as input. Firstly, the best matches are stored by iterating through all the keypoint matches and if the previous and current frame contains the respective keypoints then the box ids are paired. Secondly, the number of pair points per bounding box match between current and previous frame are evaluated. And lastly, the highest number of points per bounding box in the current and previous frame is determined and the bounding box with maximum count is choosen for each object.

### Compute Lidar-based TTC
The Time-to-collision calculation based on lidar is implemented in the computeTTCLidar function. The first step is to remove all the outliers and this is achieved with the help of isOutlier function. The outliers are removed according to a threshold distance around each point belonging to a cluster. Then the minimum distance between the current and previous lidar point is calculated to find the closest point. lastly, the TTC between both frames is computed in seconds.

### Associate Keypoint correspondences with bounding boxes
The algorithm is implemented in clusterKptMatchesWithROI method. Firstly for the keypoints belonging to the bounding box, mean distance between previous and current frame is computed. Based on a certain threshold the keypoints are stored. 

### Compute Camera-based TTC
The Time-to-collision calculation based on camera is implemented in the computeTTCCamera function. The distance ratio is calculated between all the keypoint matches. The median of distance ratios are then computed to make the algorithm more robust to outliers. And then the TTC for camera is computed.

### Performance evaluation 1
The TTC estimate for lidar where the value does not seem plausible has been identified. Below are 3 examples, the reason for incorrect TTC can be because of the outliers. These outliers cause the overestimation of TTC on all 3 examples.

![FAST ORB](/images/FAST_ORB_Lidar1.png)

![FAST FREAK](/images/FAST_FREAK_lidar2.png)

![SIFT FREAK](/images/SIFT_FREAK_lidar3.png)

### Performance evaluation 2
The TTC estimate for camera where the value does not seem plausible has been identified. Below are 2 examples where TTC estimation is way off. 

![BRISK FREAK](/images/BRISK_FREAK_camera1.png)

![BRISK BRIEF]((/images/BRISK_BRIEF_camera2.png)

The reason behind TTC being overestimated could be due to the overlapping of the bounding boxes as shown in the example below.

![Overlapping Bounding boxes](/images/multiplebox.png)

Here is another example where TTC estimation of camera is way off. The TTC is negative infinite because the median distance ratio is equal to 1. This will lead to division by zero.

![ORB ORB]((/images/ORB_ORB_camera3.png)

The top three best performing combination of detectors/descriptors are:
1. SHITOMASI/BRISK
2. FAST/FREAK
3. SHITOMASI/BRIEF

The plots for all the combinations of detectors and descriptors can be found in the plots folder.
