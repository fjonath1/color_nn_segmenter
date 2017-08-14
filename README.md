# Color Nearest Neighboor Segmenter

Author: Felix Jonathan (fjonath1@jhu.edu)

This rosnode will perform a color based segmentation for objects with flat color. The color training is performed using k-means clustering of each object color category from their color point clouds. Then the model is used for perfoming nearest neighboor search on input color point cloud.

# Requirements
This package requires these dependencies:
 - PCL 1.7.2+
 - OpenCV 2.4.x
 - ROS Indigo
 - [object_on_table_segmenter](https://github.com/jhu-lcsr/object_on_table_segmenter)

This package has been tested on 14.04 using PCL 1.7.2, OpenCV 2.4.8, and ROS Indigo.

# Usage
Run this node using roslaunch with:
```
roslaunch color_nn_segmenter segmenter.launch
```

## Roslaunch Parameters
The default ros parameter use this following layout for training and saving data
$(catkin source directory)/color_nn_segmenter
├── include
├── launch
├── models
│   ├── model.dat
│   ├── model.pcd
│   └── ...
├── src
└── training_data
    ├── color_category1
    │   ├── file1.pcd
    │   ├── file2.pcd
    │   └── ...
    ├── color_category2
    │   ├── file1.pcd
    │   ├── file2.pcd
    │   └── ...
    └── ...


These are the required parameters for loading the model:
 - load_existing_model: Loads the color model saved specified in the model path. If set to false, model training will be done before color segmentation is performed;
 - model_directory: Directory that contains the model data;
 - model_name: The name of the model to be loaded/saved.

These are the required parameters for saving the model:
 - save_new_model
 - training_data_directory: Location of the training data that contains color category folders
 - kmeans_point_per_model: The number of kmeans points per color category
 - save_directory: Target directory for saving the new model

To specify the point cloud input/output topic, these parameters can be set:
 - cloud_input_topic: Name of the point cloud topic to be segmented
 - segmented_cloud_topic: Name of the topic to publish segmented cloud that contains PointXYZL
 - visualized_cloud_topic: Name of the topic to publish segmented cloud in rviz-compatible point cloud

This program also include a way to perform segmentation on a volume above a convex hull/table. This convex hull is generated by searching largest region around a tf frame based on [object_on_table_segmenter](https://github.com/jhu-lcsr/object_on_table_segmenter).
