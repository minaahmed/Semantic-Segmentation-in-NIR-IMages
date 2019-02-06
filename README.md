
# DeepLab_V3 Image Semantic Segmentation Network

Implementation of the Semantic Segmentation DeepLab_V3 CNN as described at [Rethinking Atrous Convolution for Semantic Image Segmentation](https://arxiv.org/pdf/1706.05587.pdf).

For a complete documentation of this implementation, check out the [blog post](https://sthalles.github.io/deep_segmentation_network/).

## Dependencies

root folder contains environment setup file - environment.yml

## Downloads

### Evaluation

Pre-trained model.

- [checkpoints](https://www.dropbox.com/sh/s7sx69pqjhrk0s4/AACXWCRd9JJ0zvcvDES9G3sba?dl=0) - folder name 16645

Place the checkpoints folder inside `./tboard_logs`. If the folder **does not** exist, create it.

### Retraining

Original datasets used for training.

- Dataset
  * [Option 1](https://mega.nz/#F!LlFCSaBB!1L_EoepUwhrHw4lHv1HRaA)
  * [Option 2](http://www.mediafire.com/?wx7h526chc4ar)

Place the tfrecords files inside ```./dataset/tfrecords```. Create the folder if it **does not** exist.

## Training and Eval

Once you have the training and validation *TfRefords* files, just run the command bellow. Before running Deeplab_v3, the code will look for the proper `ResNets` checkpoints inside ```./resnet/checkpoints```, if the folder does not exist, it will first be **downloaded**.

```
python train.py --starting_learning_rate=0.00001 --batch_norm_decay=0.997 --crop_size=513 --gpu_id=0 --resnet_model=resnet_v2_50
```

Check out the *train.py* file for more input argument options. Each run produces a folder inside the *tboard_logs* directory (create it if not there).

To evaluate the model, run the *test.py* file passing to it the *model_id* parameter (the name of the folder created inside *tboard_logs* during training).

Note: Make sure the `test.tfrecords` is downloaded and placed inside `./dataset/tfrecords`.

```
python test.py --model_id=16645
```

## Retraining

To use a different dataset, you just need to modify the ```CreateTfRecord.ipynb``` notebook inside the ```dataset/``` folder, to suit your needs.

Also, be aware that originally Deeplab_v3 performs random crops of size **513x513** on the input images. This **crop_size** parameter can be configured by changing the ```crop_size``` hyper-parameter in **train.py**.

## Datasets

Dataset used is https://ivrl.epfl.ch/research-2/research-downloads/supplementary_material-cvpr11-index-html/

This dataset consists of 477 images in 9 categories captured in RGB and Near-infrared (NIR). The images were captured using separate exposures from modified SLR cameras, using visible and NIR filters. For more info on NIR photography, see the references below. The scene categories are: country, field, forest, indoor, mountain, oldbuilding, street, urban, water.

**Note: You do not need both datasets.**
 - If you just want to test the code with one of the datasets (say the SBD), run the notebook normally, and it should work.

After, head to ```dataset/``` and run the ```CreateTest_TfRecord.ipynb``` notebook.

The ```custom_test.txt``` file contains the name of the images selected for training. This file is designed to use the Pascal VOC 2012 set as a **TESTING** set. Therefore, it doesn't contain any images from the VOC 2012 val dataset. For more info, see the **Training** section of [Deeplab Image Semantic Segmentation Network](https://sthalles.github.io/deep_segmentation_network/).

Obs. You can skip that part and direct download the datasets used in this experiment - See the **Downloads** section


## Results

- Pixel accuracy: ~91%
- Mean Accuracy: ~82%
- Mean Intersection over Union (mIoU): ~74%
- Frequency weighed Intersection over Union: ~86

![Results](https://github.com/sthalles/sthalles.github.io/blob/master/assets/deep_segmentation_network/results1.png)
