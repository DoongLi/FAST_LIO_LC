# FAST_LIO_LC

The code in this repo only modifies the parameters based on FAST_LIO_LC to make it suitable for mid-360, and added the file **fastlio_livox.launch**.

## Tips

#### 1.gtsam and Ceres Version

FAST-LIO does not use gtsam and Ceres, but the backend optimization library used by this Project relies on GTSAM and Ceres; 

Recommended **gtsam 4.0.2(or 4.0.0-alpha2)** and **Ceres 1.14.0**, I tried gtsam4.0.3, but still got an error.

When installing gtsam, use `cmake -DGTSAM_BUILD_WITH_MARCH_NATIVE=OFF ..` to compile.

In long-term use, if an error occurs when recompiling FAST_LIO_LC, you can recompile gtsam and Ceres.

#### 2.livox_ros_driver Problem

- Put livox_ros_driver and FAST_LIO_LC under the same src;
- Or Before compiling FAST_LIO_LC, source the workspace of livox_ros_driver first, like this:

![1](/home/doongli/Github-Workspace/DoongLi/FAST_LIO_LC/IMG/1.png)

#### 3.Release Mode

- Add to FAST-LIO/CMakeLists.txt: `SET(CMAKE_BUILD_TYPE "Release")`
- Or Use directly: `catkin_make -DCMAKE_BUILD_TYPE=Release` 

#### 4.Correction Parameters 

4.1 Before using FAST_LIO_LC, you need to modify the parameters of the radar like FAST_LIO, for example, I am using mid-360 and I need to modify `FAST_LIO_LC/FAST-LIO/config/avia.yaml`.

Before:

![2](/home/doongli/Github-Workspace/DoongLi/FAST_LIO_LC/IMG/2.png)

After:

![3](/home/doongli/Github-Workspace/DoongLi/FAST_LIO_LC/IMG/3.png)

4.2 Set up saved map

You need to change `<param name="pcd_save_enable" type="bool" value="0" />` to `<param name="pcd_save_enable" type="bool" value="1" />` in **mapping_avia.launch** before it can be saved. The final PCD file of the map.

#### 5.The trajectory of dynamic objects on the map

Reference：https://github.com/yanliang-wang/FAST_LIO_LC/issues/22

I posted an issue on the FAST_LIO_LC repo, but I haven’t found the specific reason yet.

## RUN

```bash
source devel/setup.bash 
roslaunch fast_lio mapping_avia.launch
source devel/setup.bash 
roslaunch aloam_velodyne fastlio_livox.launch
# rosbag play xxx.rosbag or roslaunch xxx_driver
```

The following error is normal: 

![5](/home/doongli/Github-Workspace/DoongLi/FAST_LIO_LC/IMG/5.png)

If a loop is detected and passed successfully, the following will be displayed:

![4](/home/doongli/Github-Workspace/DoongLi/FAST_LIO_LC/IMG/4.png)

## Reference

- https://github.com/hku-mars/FAST_LIO

- https://github.com/yanliang-wang/FAST_LIO_LC



