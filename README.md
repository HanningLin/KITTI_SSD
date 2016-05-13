# SSD detection system for KITTI

1. 准备数据

在data/KITTI下写了一个将KITTI的标签格式转化为VOC的xml格式的工具，首先在data/KITTI下解压KITTI的训练数据，图片存储在data/KITTI/image_2中，对应的标签存储在label_2中，本工程检测三个类别Pedestrian,Car,Cyclist,如果要检测其他类别就在KITTI_xml.py中类别索引处添加其他类别，该程序在data/KITTI中生成train.txt用来记录image path和对应的xml的label path. test_name_size.txt记录检测图像以及它的长宽。在labelmap_KITTI.prototxt中对应了类别和编号索引。

然后在root目录下运行create_data.sh,注意路径修改，则能在data/KITTI中生成lmdb的KITTI_train_lmdb数据，同理生成KITTI_test_lmdb数据。para_show.sh是为了方便显示create_data.sh的参数。create_data.sh调用了scripts/create_annoset.py,create_annoset_debug.py为对应的调试版本。生成lmdb并软连接到examples/KITTI中。（注意绝对路径修改）

2. 训练和检测

运行examples/ssd下的ssd_KITTI.py在models里生成VGGNet的KITTI网络模型文件，在该文件夹内同时存放VGG_ILSVRC_16_layers_fc_reduced.caffemodel, 同时在jobs/VGGNet中生成测试和训练命令,VGG_KITTI_SSD_600x150.sh用来训练和测试网络，VGG_KITTI_SSD_600x150_TEST.sh用来测试网络。网络训练好以后利用jobs/VGGNet/KITTI/SSD_600x150_webcam/VGG_KITTI_SSD_600x150.sh可以调取USB摄像头做实时检测啦。这里我修改了caffe.proto里的VideoData层使其支持本地视频的检测，详细步骤将在我的博客中更新。

另外源程序完全在C++下实现，并没有python接口程序，我在ssd_caffe/jobs实现了检测过程检测图片，检测图片序列压缩成视频，以及读本地视频实时检测的三个py程序detection.py,detection_video.py和demo_video_for_video.py.

注意，所有绝对路径都需要修改

If you have any problems or find some bugs, please contact with manutdzou@126.com and feel free. Thank you again for your observation.



