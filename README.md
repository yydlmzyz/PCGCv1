# Learned Point Cloud Geometry Compression
___
Jianqiang Wang, Hao Zhu, Haojie Liu, Zhan Ma.  **[[arXiv]](https://arxiv.org/abs/1909.12037)**

<font color=red size=5> PCGCv2: Multiscale Point CLoud Geometry Compression </font> **[[arXiv]](https://arxiv.org/abs/2011.03799)**  **[[github]](https://github.com/NJUVISION/PCGCv2)**

<p align="center">
  <img src="figs/intro.png?raw=true" alt="introduction" width="800"/> (a)

  <img src="figs/framework.png?raw=true" alt="framework" width="800"/> (b)

 An illustrative overview in (a) and detailed diagram in (b) for point cloud geometry compression consisting of a pre-processing for PCG voxelization, scaling & partition, a compression network for compact PCG representation, and metadata signaling, and post-processing for PCG reconstruction and rendering. “Q” stands for “Quantization”, “AE” and “AD” are Arithmetic Encoder and Decoder respectively. “Conv” denotes the convolution layer with the number of the output, channels, and kernel size.
</p>

## Requirements
- ubuntu16.04
-  python3
- tensorflow-gpu=1.13.1
- Pretrained models: http://yun.nju.edu.cn/f/2a54abc9e8/
- ShapeNet Dataset: http://yun.nju.edu.cn/f/6d39b9cba0/
- Test data: http://yun.nju.edu.cn/f/e200741271/

## Usage 

### **Encoding**

```shell
python test.py compress "testdata/8iVFB/longdress_vox10_1300.ply" \
        --ckpt_dir="checkpoints/hyper/a6b3/"
```

### **Decoding**

```shell
python test.py decompress "compressed/longdress_vox10_1300" \
        --ckpt_dir="checkpoints/hyper/a6b3/"
```

Other default options: **--scale=1, --cube_size=64, --rho=1.0, --mode='hyper', --modelname='models.model_voxception'**

#### Examples
Please refer to `demo.ipynb` for each step.

---
### **Evaluation**
```shell
 python eval.py --input "testdata/8iVFB/longdress_vox10_1300.ply" \
                --rootdir="results/hyper/longdress/" \
                --cfgdir="results/hyper/8iVFB_vox10.ini" \
                --res=1024
```

The results can be downloaded in http://yun.nju.edu.cn/f/b413edb458/

---
### Training
#### **Generating training dataset**
sampling points from meshes, here we use **pyntcloud** (pip install pyntcloud)

```shell
cd dataprocess
python mesh2pc.py
```
The output point clouds can be download in http://yun.nju.edu.cn/d/227493a5bd/
```shell
python generate_dataset.py
```
the output training dataset can be download in http://yun.nju.edu.cn/d/604927e275/

--- 
#### **Training**
```shell
python train_hyper.py --alpha=0.75 \
        --prefix='hyper_' --batch_size=8 --init_ckpt_dir='checkpoints/hyper/a0.75b3' --reset_optimizer=1
```
or
```shell
python train_factorized.py --alpha=2  \
        --prefix='voxception_' --batch_size=8 --init_ckpt_dir='./checkpoints/factorized/a2b3' --reset_optimizer=1
```

## Comparison
### Objective  Comparison
`results.ipynb`
### Qualitative Evaluation
<center class="half">
  <img src="figs/redandblack.png?raw=true" height="260" alt="redandblack"/> <img src="figs/phil.png?raw=true" height="260" alt="phil"/>
</center>

## Update
- 2019.10.09 initial smbmission.
- 2019.10.22 submit demos, several pretrained models and training datasets.
- 2019.10.27 submit all pretrained models and evaluate on 8i voxelized full bodies.
- 2019.11.14 check bug and start testing on AVS PCC Cat3.
- 2019.11.15 test point cloud sequences using avs metric tool. (thanks for the help from Wei Yan)
- 2019.11.19 finish testing AVS Cat3.
- 2019.11.20 test AVS Cat2.
- 2019.11.26 test AVS single frame.
- 2019.11.31 doc.
- 2020.06.27 python3 & clean up.
- 2020.10.03 open source.
- 2020.12.09 ablation studies & experiment configuration.

## Todo
- pytorch version & tensorflow2.0 version.
- training again.

### Authors
These files are provided by Nanjing University [Vision Lab](http://vision.nju.edu.cn/). And thanks for the help from SJTU Cooperative Medianet Innovation Center. Please contact us (wangjq@smail.nju.edu.cn) if you have any questions. 
