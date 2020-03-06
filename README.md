# SFND 2D Feature Tracking

<img src="images/keypoints.png" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, you will now build the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

* First, you will focus on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load. 
* Then, you will integrate several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed. 
* In the next part, you will then focus on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson. 
* In the last part, once the code framework is complete, you will test the various algorithms in different combinations and compare them with regard to some performance measures. 

See the classroom instruction and code comments for more details on each of these parts. Once you are finished with this project, the keypoint matching part will be set up and you can proceed to the next lesson, where the focus is on integrating Lidar points and on object detection using deep-learning. 

## Dependencies for Running Locally
* cmake >= 2.8
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* OpenCV >= 4.1
  * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors.
  * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.

# Benchmark

I have implemented code in main function to loop through all the possible combination of detector and descriptor pairs.

The result is output as table by markdown format.

I used KNN match selection (k=2) and performed descriptor distance ratio filtering with t=0.8 in file `matching2D_Student.cpp`.

|No. | Detector + Descriptor |Total Keypoints |Total Matches |Total Detection Time (ms) |Total Extraction Time (ms) |Ratio (matches/time) |
|:---:|:----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| 0| SHITOMASI + BRISK |13423 |767 |107.218|15.2741 |6.26164 |
| 1| SHITOMASI + BRIEF |13423 |944 |110.949|9.21848 |7.85571 |
| 2| SHITOMASI + ORB |13423 |907 |143.661|31.0107 |5.1926 |
| 3| SHITOMASI + FREAK |13423 |768 |87.1917|333.428 |1.82588 |
| 4| SHITOMASI + AKAZE |N/A |N/A |N/A|N/A |N/A |
| 5| SHITOMASI + SIFT |13423 |927 |84.2384|78.6206 |5.69204 |
| 6| HARRIS + BRISK |1859 |143 |102.464|4.61738 |1.33543 |
| 7| HARRIS + BRIEF |1859 |178 |114.137|4.04793 |1.50611 |
| 8| HARRIS + ORB |1859 |163 |103.92|27.1239 |1.24386 |
| 9| HARRIS + FREAK |1859 |145 |96.5279|316.615 |0.350968 |
| 10| HARRIS + AKAZE |N/A |N/A |N/A|N/A |N/A |
| 11| HARRIS + SIFT |1859 |165 |98.6979|73.7608 |0.956751 |
| 12| FAST + BRISK |49204 |2183 |12.6312|31.9452 |48.9722 |
| 13| FAST + BRIEF |49204 |2831 |12.5097|9.75824 |127.134 |
| 14| FAST + ORB |49204 |2762 |12.4732|29.9829 |65.0555 |
| 15| FAST + FREAK |49204 |2233 |13.2568|342.946 |6.26891 |
| 16| FAST + AKAZE |N/A |N/A |N/A|N/A |N/A |
| 17| FAST + SIFT |49204 |2782 |12.8714|147.335 |17.3651 |
| 18| BRISK + BRISK |27116 |1570 |2922.86|23.5199 |0.532858 |
| 19| BRISK + BRIEF |27116 |1704 |2891.05|7.15239 |0.58795 |
| 20| BRISK + ORB |27116 |1510 |2954.15|86.5792 |0.496591 |
| 21| BRISK + FREAK |27116 |1524 |2947.09|331.3 |0.464862 |
| 22| BRISK + AKAZE |N/A |N/A |N/A|N/A |N/A |
| 23| BRISK + SIFT |27116 |1646 |2885.61|213.13 |0.531184 |
| 24| ORB + BRISK |5000 |751 |48.8404|11.1446 |12.5198 |
| 25| ORB + BRIEF |5000 |545 |45.3318|3.70297 |11.1146 |
| 26| ORB + ORB |5000 |761 |46.1667|91.9059 |5.51159 |
| 27| ORB + FREAK |5000 |420 |45.4186|327.522 |1.12619 |
| 28| ORB + AKAZE |N/A |N/A |N/A|N/A |N/A |
| 29| ORB + SIFT |5000 |763 |45.0498|215.254 |2.93119 |
| 30| AKAZE + BRISK |13430 |1215 |396.628|17.524 |2.9337 |
| 31| AKAZE + BRIEF |13430 |1266 |384.164|7.36915 |3.23344 |
| 32| AKAZE + ORB |13430 |1186 |400.363|66.3193 |2.54134 |
| 33| AKAZE + FREAK |13430 |1187 |368.059|336.897 |1.68379 |
| 34| AKAZE + AKAZE |13430 |1259 |341.29|267.742 |2.06721 |
| 35| AKAZE + SIFT |13430 |1270 |335.565|122.856 |2.77038 |
| 36| SIFT + BRISK |13861 |592 |721.238|11.9756 |0.807405 |
| 37| SIFT + BRIEF |13861 |702 |757.33|5.02038 |0.920836 |
| 38| SIFT + ORB |OOM |OOM |OOM|OOM |OOM |
| 39| SIFT + FREAK |13861 |593 |733.597|329.674 |0.557712 |
| 40| SIFT + AKAZE |N/A |N/A |N/A|N/A |N/A |
| 41| SIFT + SIFT |13861 |800 |619.202|384.874 |0.796753 |

## Writeup, Task MP.0

### MP.1 Data buffer optimization
`std::vector::push_back` is used to push new element on end
`std::vector::erase(vector.begin())` is used to remove element on head

### MP.2 Keypoint detection
Loop through `detectorTypes` to test all detectors.
```cpp
std::vector<std::string> detectorTypes = {"SHITOMASI", "HARRIS", "FAST", "BRISK", "ORB", "AKAZE", "SIFT"};
```
### MP.3 Keypoint removal
```cpp
bool bFocusOnVehicle = true;
cv::Rect vehicleRect(535, 180, 180, 150);
if (bFocusOnVehicle)
{
    vector<cv::KeyPoint> filteredKeypoints;
    for (auto kp : keypoints) {
        if (vehicleRect.contains(kp.pt)) filteredKeypoints.push_back(kp);
    }
    keypoints = filteredKeypoints;
}
```
as shown above

### MP.4 Keypoint descriptors
Loop through `descriptorTypes` to test all descriptors.
```cpp
std::vector<std::string> descriptorTypes = {"BRISK", "BRIEF", "ORB", "FREAK", "AKAZE", "SIFT"};
```

### MP.5 Descriptor matching
The function `matchDescriptors` in `matching2D_Student.cpp` contains a kind of decision tree structure, based on the settings of these string parameters:
- `descriptorSourceType` either: `DES_BINARY` (binary), `DES_HOG` (histogram of gradients)
- `matcherType` either: `MAT_FLANN` (cv::FlannBasedMatcher), `MAT_BF` (brute force)
- `selectorType` either: `SEL_NN` (nearest neighbors), `SEL_KNN` (k nearest neighbors)

To make matching work for SIFT descriptor, DES_HOG should be selected.
```cpp
std::string descriptorSourceType = descriptorType == "SIFT" ? "DES_HOG" : "DES_BINARY"; // DES_BINARY, DES_HOG
```

For the performance benchmarks (MP.7-9) below, `matcherType` was set to `MAT_BF` and `selectorType` was set to `SEL_KNN`, which implements match filtering based on the descriptor distance ratio.

### MP.6 Descriptor distance ratio

This distance ratio filter compares the distance (SSD) between two candidate matched keypoint descriptors. A threshold of `0.8` is applied and the stronger candidate (minimum distance) is selected as the correct match. This method eliminates many false-positive keypoint matches.

### MP.7 Performance evaluation 1

as shown in **Benchmark** table

### MP.8 Performance evaluation 2

as shown in **Benchmark** table

### MP.9 Performance evaluation 3

#### Top 3 detector/ descriptor pairs

As you can see in the table above the efficiency ratio i.e. matches/time is highest for following three detector descriptor pair.

total time = total detection time + total descriptor extraction time

|No. | Detector + Descriptor |Total Keypoints |Total Matches |Total Detection Time (ms) |Total Extraction Time (ms) |Ratio (matches/time) |
|:---:|:----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| 13| FAST + BRIEF |49204 |2831 |12.5097|9.75824 |127.134 |
| 14| FAST + ORB |49204 |2762 |12.4732|29.9829 |65.0555 |
| 12| FAST + BRISK |49204 |2183 |12.6312|31.9452 |48.9722 |

