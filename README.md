### Disclaimer

The official Faster R-CNN code (written in MATLAB) is available [here](https://github.com/ShaoqingRen/faster_rcnn).


This repository contains a Python3 *reimplementation* of the MATLAB code.
This Python3 implementation is built on a fork of [Faster R-CNN](https://github.com/rbgirshick/py-faster-rcnn) which is written with python2

# *Faster* R-CNN: Towards Real-Time Object Detection with Region Proposal Networks

By Shaoqing Ren, Kaiming He, Ross Girshick, Jian Sun (Microsoft Research)

This Python implementation contains contributions from Sean Bell (Cornell) written during an MSR internship.

Please see the official [README.md](https://github.com/ShaoqingRen/faster_rcnn/blob/master/README.md) for more details.

Faster R-CNN was initially described in an [arXiv tech report](http://arxiv.org/abs/1506.01497) and was subsequently published in NIPS 2015.

### License

Faster R-CNN is released under the MIT License (refer to the LICENSE file for details).

### Citing Faster R-CNN

If you find Faster R-CNN useful in your research, please consider citing:

    @inproceedings{renNIPS15fasterrcnn,
        Author = {Shaoqing Ren and Kaiming He and Ross Girshick and Jian Sun},
        Title = {Faster {R-CNN}: Towards Real-Time Object Detection
                 with Region Proposal Networks},
        Booktitle = {Advances in Neural Information Processing Systems ({NIPS})},
        Year = {2015}
    }

### Contents
1. [Requirements: software](#requirements-software)
2. [Requirements: hardware](#requirements-hardware)
3. [Basic installation](#installation-sufficient-for-the-demo)
4. [Demo](#demo)
5. [Beyond the demo: training and testing](#beyond-the-demo-installation-for-training-and-testing-models)
6. [Usage](#usage)

### Requirements: software

1. Requirements for `Caffe` and `pycaffe` (see: [Caffe installation instructions](http://caffe.berkeleyvision.org/installation.html))

  **Note:** Caffe *must* be built with support for Python layers!

  ```make
  # In your Makefile.config, make sure to have this line uncommented
  WITH_PYTHON_LAYER := 1
  # Unrelatedly, it's also recommended that you use CUDNN
  USE_CUDNN := 1
  ```

  You can download my [Makefile.config](http://www.cs.berkeley.edu/~rbg/fast-rcnn-data/Makefile.config) for reference.
2. Python packages you might not have: `cython`, `python-opencv`, `easydict`
3. [Optional] MATLAB is required for **official** PASCAL VOC evaluation only. The code now includes unofficial Python evaluation code.

### Requirements: hardware

1. For training smaller networks (ZF, VGG_CNN_M_1024) a good GPU (e.g., Titan, K20, K40, ...) with at least 3G of memory suffices
2. For training Fast R-CNN with VGG16, you'll need a K40 (~11G of memory)
3. For training the end-to-end version of Faster R-CNN with VGG16, 3G of GPU memory is sufficient (using CUDNN)

### Installation (sufficient for the demo)

1. Clone the repository
  ```Shell
  # Make sure to clone with --recursive
  git clone --recursive https://github.com/surajitsaikia27/Faster_RCNN_COCO_py3
  ```

2. 
    **Note 1:** The `caffe-fast-rcnn` submodule needs to be on the `root` branch (or equivalent detached state). You need to clone it inside from the repository (https://github.com/rbgirshick/py-faster-rcnn). 
	If you clone the normal Caffe then the module wont work, because it doesnt not contain the RPN.
    **Note 2:** This version supports python3. But the original version is written in python2, so you need to install python3.
3. Build the Cython modules
    ```Shell
    cd $Faster_RCNN_COCO_py3/lib
    make
    ```

4. Build Caffe and pycaffe
    ```Shell
    cd $Faster_RCNN_COCO_py3/caffe-fast-rcnn
    # Now follow the Caffe installation instructions here:
    #   http://caffe.berkeleyvision.org/installation.html

    # If you're experienced with Caffe and have all of the requirements installed
    # and your Makefile.config in place, then simply do:
    make -j8 && make pycaffe
    ```

5. Download pre-computed Faster R-CNN detectors
    ```Shell
    cd $Faster_RCNN_COCO_py3
    ./data/scripts/fetch_faster_rcnn_models.sh
    ```

    This will populate the `$Faster_RCNN_COCO_py3/data` folder with `faster_rcnn_models`. See `data/README.md` for details.
    These models were trained on VOC 2007 trainval.

### Demo

*After successfully completing [basic installation](#installation-sufficient-for-the-demo)*, you'll be ready to run the demo.

To run the demo
```Shell
cd $Faster_RCNN_COCO_py3
./model/Detect_Objs.py
```
The code performs detection using a VGG16 network trained with MS-COCO dataset.
Inside the 'main' of the code you need to set path to the prototxt file and the pre-trained model as follows:
 
 prototxt = 'path/to/prototxt'
 caffemodel = 'path_to_model/vgg16_faster_rcnn_iter_80000.caffemodel'

 The prototxt file has been provided, but you need to download the caffemodel.

