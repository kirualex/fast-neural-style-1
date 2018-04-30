# torch-fns


### Create Dataset (needed once)

```
python scripts/make_style_dataset.py \
--train_dir ~/Documents/data/train2014 \
--val_dir ~/Documents/data/val2014 \
--output_file ~/Documents/data/datasets/fast_dataset.h5 \
--width 460 \
--max_images 20000
```

### Train

```
th train.lua \
-h5_file ~/Documents/data/datasets/fast_dataset.h5 \
-style_image ~/Documents/data/images/styles/filter_pablo3.jpg \
-checkpoint_name ~/Documents/data/models/fast_checkpoint \
-loss_network ~/Documents/data/models/vgg16.t7 \
-arch c9s1-16,d32,d64,R64,R64,R64,R64,R64,U2,c3s1-32,U2,c9s1-3 \
-checkpoint_every 250 \
-style_image_size 256 \
-content_weights 1.0 \
-style_weights 5.0 \
-upsample_factor 2 \
-learning_rate 7e-4 \
-batch_size 2 \
-num_iterations 2000 \
-resume_from_checkpoint ~/Documents/data/models/fast_checkpoint.t7
```	

Options
```
-arch c21s1-32,d64,d128,R128,R128,R128,R128,R128,u64,u32,c9s1-3
```

### Stylize test
```
th fast_neural_style.lua \
-image_size 0 \
-median_filter 0 \
-model ~/Documents/data/models/fast_checkpoint.t7 \
-input_image ~/Documents/data/images/test.jpg \
-output_image ~/Documents/data/images/stylized-test.jpg
```

### Convert to CoreML

```
th prepare_model.lua \
-input ~/Documents/data/models/fast_checkpoint.t7 \
-output ~/Documents/data/models/prepared.t7

python torch_to_coreml.py \
-input ~/Documents/data/models/prepared.t7 \
-output ~/Documents/data/models/model.mlmodel
```
