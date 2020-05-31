# Overview
Forked from [here](https://github.com/weixu000/libtorch-yolov3-deepsort).

## Libraries Used

1. libtorch - 1.5.0 GPU enabled version.
2. OpenCV - 3.4.7
3. wxWidgets - 3.0.0. Simply install using apt.
4. CMake
5. CUDA 10.1 with CUDNN 7.6.5.32 allows you to successfully compile. My driver
   was not compatible with the latest CUDA. I upgraded my driver to 435.21. The
   build could run.

## Build

1. `mkdir build`
2. `cd build`
3. `CMAKE_PREFIX_PATH=/home/$USER/cpplibs/libtorch-1.5.0-gpu cmake ../`
4. `make -j8`

## Usage

1. After building, there will be a `build/bin/GUI` and `build/bin/processing`
   file. The GUI is used to visualize the results and processing is used to run
   a video.
2. A sample test video can be downloaded from [TownCentreXVID.avi](http://www.robots.ox.ac.uk/ActiveVision/Research/Projects/2009bbenfold_headpose/Datasets/TownCentreXVID.avi).
3. In the root directory of the project, create a `weights` folder and put
   `yolov3.weights`, which can be downloaded from
   [Darknet](https://pjreddie.com/darknet/yolo/). You should download
   YOLOv3-416, as this is what this repository uses.
4. Run `./build/bin/processing /path/to/TownCentreXVID.avi scale_factor`. A
   scale factor of 1 means original sized images are used. 4 means you're
   scaling the image down by 4. I need to check how the scaling is done in the
   code.
5. The results will be written to a `results` folder. 
6. `./build/bin/GUI` and simply select the `results` folder to visualize the
   results. It's a pretty neat tool.

## Statistics

Builds were run on a GTX 1060 with 6GB of RAM.

1. Scale: 1. FPS: 2.6.
2. Scale: 4. FPS: 17.0.

## Other Notes

1. A build using CUDA 8.0 and CUDNN 5.0 probably will not work against PyTorch
   1.5.0. The error message said that you needed at least CUDA 9.0.

# Original Comments from weixu000

There are four modules in the project:

- Detection: YOLOv3
- Tracking: SORT and DeepSORT
- Processing: Run detection and tracking, then display and save the results (a compressed video, a few snapshots for each target)
- GUI: Display the results

# YOLOv3
A Libtorch implementation of the YOLO v3 object detection algorithm, written with modern C++.

The code is based on the [walktree](https://github.com/walktree/libtorch-yolov3).

The config file in .\models can be found at [Darknet](https://github.com/pjreddie/darknet/tree/master/cfg).

# SORT
I also merged [SORT](https://github.com/mcximing/sort-cpp) to do tracking.

A similar software in Python is [here](https://github.com/weixu000/pytorch-yolov3), which also rewrite form [the most starred version](https://github.com/ayooshkathuria/pytorch-yolo-v3) and [SORT](https://github.com/abewley/sort)

## DeepSORT
Recently I reimplement [DeepSORT](https://github.com/nwojke/deep_sort) which employs another CNN for re-id.
It seems it gives better result but also slows the program a bit.
Also, a PyTorch version is available at [ZQPei](https://github.com/ZQPei/deep_sort_pytorch), thanks!

# Performance
Currently on a GTX 1060 6G it consumes about 1G RAM and have 37 FPS.

The video I test is [TownCentreXVID.avi](http://www.robots.ox.ac.uk/ActiveVision/Research/Projects/2009bbenfold_headpose/Datasets/TownCentreXVID.avi).

# GUI
With [wxWidgets](https://www.wxwidgets.org/), I developed the GUI module for visualization of results.

Previously I used [Dear ImGui](https://github.com/ocornut/imgui).
However, I do not think it suits my purpose.

# Pre-trained network
This project uses pre-trained network weights from others
- [YOLOv3](https://pjreddie.com/media/files/yolov3.weights)
- [YOLOv3-tiny](https://pjreddie.com/media/files/yolov3-tiny.weights)
- [DeepSORT](https://drive.google.com/drive/folders/1xhG0kRH1EX5B9_Iz8gQJb7UNnn_riXi6)

# How to build
This project requires [LibTorch](https://pytorch.org/), [OpenCV](https://opencv.org/), [wxWidgets](https://www.wxwidgets.org/) and [CMake](https://cmake.org/) to build.

LibTorch can be easily integrated with CMake, but there are a lot of strange things...

On Ubuntu 16.04, I use `apt install` to install the others. Everything is fine.
On Windows 10 + Visual Studio 2017, I use the latest stable version of the others from their official websites.

# Snapshots
Here are some intermediate output from detection and tracking module:
![Detection](https://github.com/weixu000/libtorch-yolov3-deepsort/blob/master/snapshots/detection.png)
![Tracking](https://github.com/weixu000/libtorch-yolov3-deepsort/blob/master/snapshots/tracking.png)

Here is the snapshot of processing module:
![Processing](https://github.com/weixu000/libtorch-yolov3-deepsort/blob/master/snapshots/UI-online.png)

Here is the snapshot of GUI module:
![GUI](https://github.com/weixu000/libtorch-yolov3-deepsort/blob/master/snapshots/UI-offline.png)
