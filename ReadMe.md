
## Environment
1. pip install -r requirements.txt
2. confirm file ```/usr/share/fonts/Fonts/simsun.ttf``` is exists.
## filter error map 
### generate error map
```shell
python disp_tof_2_erroeMap.py --height 360 --width 576 --output_dir /work/data/Parker/DEPTH/CREStereo_100_tof/TRAIN/data_2023_08_23/scale_tof_crop_disp --disp_dir /work/data/Parker/DEPTH/CREStereo_100_tof/TRAIN/data_2023_08_23/scale_tof_crop_disp --tof_dir /work/data/Parker/TOF/TRAIN/data_2023_08_23
```
### generate error map with error ratio
```shell
python errorMap_ratio.py --output_dir ./resilt_0823 --error_map_dir /work/data/Parker/DEPTH/CREStereo_100_tof/TRAIN/data_2023_08_23/scale_tof_crop_disp/diff
```
### generate error map list with error ratio(wrote in demo error points' value > 10 with above 25% will be remove)
```shell
python errorMap_ratio.py --save_dir ./result_desk_half_list --error_map_dir /work/data/Parker/DEPTH/TRAIN/img_tree_crop_scale/diff
```
## new test example
### KITTI
 - no disp(输入左右目、尺寸、onnx模型，输出测试结果)
```angular2html
python main.py --data_dir /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/test_images/kitti/temp/ --height 375 --width 1242 --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/D10.2.13_ --output_dir ./result/D10.2.13/kitti_2000_no
```
 - with disp(输入左右目、尺寸、--disp_dir KITTI类型的标签disp、onnx模型，输出测试结果+errormap)
```angular2html
python main.py --data_dir /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/test_images/kitti_train/REMAP --height 375 --width 1242 --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/D10.6.0_epoch_900_1242_375/epoch-0900.onnx --disp_dir /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/test_images/kitti_train/disp --output_dir ./result/D10.6.0/kitti_1242
```
### Parker
 - use tof as label(输入左右目、尺寸、--disp_dir 三通道tof值、bf、onnx模型，，输出测试结果+errormap)
```angular2html
python main.py --data_dir /data/Parker/REMAP/TEST/data_2023_08_23_pick --height 360 --width 576 --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/D10.6.0_epoch_900_576_360/epoch-0900.onnx --disp_dir /data/Parker/test_result/NPU/depth_data/data_2023_08_23_pick/result_of_tof --bf 3424 --output_dir ./tt1
```
 - use depth as labels(输入左右目、尺寸、--disp_dir 图像深度 ？？？？、bf、onnx模型，，输出测试结果+errormap)
```angular2html
python main.py --data_dir /data/Parker/REMAP/TEST/pick_mask_REMAP  --disp_dir /data/Parker/test_result/NPU/depth_data/pick_mask/result_depth --height 360 --width 576 --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/D10.2.13_576_360/epoch-2000.onnx --bf 3424 --without_tof --output_dir ttt1 
```

### Rubby
```angular2html
 python main.py --data_dir /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/test_images/rubby --height 360 --width 576 --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/D10.2.14_576_360/epoch-1700.onnx   --bf 1420 --center_crop 0.9 --output_dir ./result/D10.2.14/rubby-1700
```
 - use depth as labels
```angular2html
python main.py --data_dir /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/test_images/rubby  --disp_dir /data/rubby --height 360 --width 576 --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/D10.2.13_576_360/epoch-2000.onnx --bf 3424 --center_crop 0.9 --without_tof --output_dir ttt1 
```
## image different 输出报告的对比数据
```angular2html
python main.py --data_dir /data/Parker/REMAP/TEST/pick_mask_REMAP --output_dir result/D10.2.13/parker_compare_psl_new --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/D10.2.13_576_360/epoch-2000.onnx --bf 3424 --tof_dir /data/Parker/test_result/NPU/depth_data/pick_mask/result_tof --tof_selected /data/Parker/test_result/NPU/depth_data/pick_mask/result_image_tof --center_crop 0.9
```
### 对比立体匹配和模型输出
```angular2html
python main.py --data_dir /data/Parker/REMAP/TEST/pick_mask_REMAP --output_dir result/D10.2.13/parker_compare_psl_slam --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/ONNX_MODLE_SAVE/D10.2.13_576_360/epoch-2000.onnx --bf 3424 --tof_dir /data/Parker/test_result/depth_slam/pick   --width 576 --height 360
```

## old version test example demo
### 1. only output depth(disp) images
```angular2html
python main.py --data_dir test_images --img_height 400 --img_width 640 --output_dir result --onnx_file *.onnx --bf 3424
```
input ```--data_dir``` is a dir containing left(cam0) and right(cam0) dirs for test image  
output 
    dir ```result/color、result/depth、result/gray``` is disp result for show
    dir ```depth_psl``` is used for tof calculation

### 2.output depth(disp) images, and compare it with tof and tof with selected
```angular2html
python main.py --data_dir image_dir --img_height 400 --img_width 640 --output_dir result_parker --onnx_file *.onnx --bf 3424 --tof_dir result_tof --tof_selected result_image_with_tof
```
```--data_dir``` 、```--tof_dir ``` 、 ```--tof_selected``` in cam0 should contain same images, if all selected point's tof value * depth == zero, continue next image.  
output  
    dir ```result/color、result/depth、result/gray``` is disp result for show
    dir ```depth_psl``` is used for tof calculation
    dir ```tof_error``` contain result
### 3. output kitti disp diff
```angular2html
python main.py --datra_dir /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/test
_images/kitti_train/REMAP --img_height 375  --img_width 1242 --disp_dir /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/test_images/kitti_train/disp --onnx_file /home/indemind/Code/PycharmProjects/Depth_Estimation/Stereo/madnet/kitti.onnx/kitti.onnx
```