# modify-ssd
Take down some notes about how to modify the code of [SSD](https://arxiv.org/abs/1512.02325) to train my own dataset. The original code is [here](https://github.com/lufficc/SSD).

## Prepare datasets
Create datasets with [LabelImg](https://github.com/tzutalin/labelImg) or [Labelme](https://github.com/wkentaro/labelme). This procedure is hideous but quite simple. Remember to check the format of dataset is switched to `VOC`.
Put RGB images in `datasets/VOC2007/JPEGImages` and annotation files in `datasets/VOC2007/Annotations`. In `datasets/VOC2007/ImageSets/Main` create `test.txt`, `train.txt` and `trainval.txt`.
In `datasets/VOC2007` run `python split.py` to divide dataset into train data and test data.
## Modify code
### Add class names
In `ssd/data/voc_dataset.py` line6 `class_names=`，write down the class names in your dataset, but do not delete `__background__`.
### Create a new config file
In `configs` create a new config file. Copy the content of `ssd300_voc0712.yaml` and paste into the new yaml file. `NUM_CLASSES`, `DATASETS`, `BATCH_SIZE`... Modify the content according to your needs.
### Training
```bash
python train_ssd.py --config-file configs/vgg_ssd300_voc0712.yaml
```
Modify the config file path and name. Also there is some argument we can specify such as `--log_step`, `--save_step`and `--eval_step`. See `train_ssd.py` for more details.
### Run demo
```bash
python demo.py --config-file configs/vgg_ssd300_voc0712.yaml --images_dir demo --ckpt https://github.com/lufficc/SSD/releases/download/1.2/vgg_ssd300_voc0712.pth
```
When running `demo.py`, remeber to check line37 to make sure your data format is `jpg` or `png`.
