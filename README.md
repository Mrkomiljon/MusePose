# MusePose

MusePose: a Pose-Driven Image-to-Video Framework for Virtual Human Generation. 


https://github.com/Mrkomiljon/MusePose/assets/92161283/7b75b776-3eff-4eb4-8ba2-1bc7bf6d6e68









## Demos
<table class="center">
  
<tr>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/Mrkomiljon/MusePose/assets/92161283/7b75b776-3eff-4eb4-8ba2-1bc7bf6d6e68" muted="false"></video>
    </td>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/TMElyralab/MusePose/assets/47803475/6667c9ae-8417-49a1-bbbb-fe1695404c23" muted="false"></video>
    </td>
</tr>

<tr>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/TMElyralab/MusePose/assets/47803475/7f7a3aaf-2720-4b50-8bca-3257acce4733" muted="false"></video>
    </td>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/Mrkomiljon/MusePose/assets/92161283/a1c4622b-409a-4f66-b830-b7ad45ac494e" muted="false"></video>
    </td>
</tr>


<tr>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/TMElyralab/MusePose/assets/47803475/00a9faec-2453-4834-ad1f-44eb0ec8247d" muted="false"></video>
    </td>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/TMElyralab/MusePose/assets/47803475/41ad26b3-d477-4975-bf29-73a3c9ed0380" muted="false"></video>
    </td>
</tr>

<tr>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/TMElyralab/MusePose/assets/47803475/2bbebf98-6805-4f1b-b769-537f69cc0e4b" muted="false"></video>
    </td>
    <td width=50% style="border: none">
        <video controls autoplay loop src="https://github.com/TMElyralab/MusePose/assets/47803475/1b2b97d0-0ae9-49a6-83ba-b3024ae64f08" muted="false"></video>
    </td>
</tr>

</table>


## News
- [05/27/2024] Release `MusePose` and pretrained models.


## Todo:
- [x] release our trained models and inference codes of MusePose.
- [x] release pose align algorithm.
- [ ] training guidelines.
- [ ] Huggingface Gradio demo.
- [ ] a improved architecture and model (may take longer).


# Getting Started
We provide a detailed tutorial about the installation and the basic usage of MusePose for new users:

## Installation
To prepare the Python environment and install additional packages such as opencv, diffusers, mmcv, etc., please follow the steps below:

### Build environment

We recommend a python version >=3.10 and cuda version =11.7. Then build environment as follows:

```shell
pip install -r requirements.txt
```

### mmlab packages
```bash
pip install --no-cache-dir -U openmim 
mim install mmengine 
mim install "mmcv>=2.0.1" 
mim install "mmdet>=3.1.0" 
mim install "mmpose>=1.1.0" 
```


### Download weights
You can download weights manually as follows:

1. Download our trained [weights](https://huggingface.co/TMElyralab/MusePose).

2. Download the weights of other components:
   - [sd-image-variations-diffusers](https://huggingface.co/lambdalabs/sd-image-variations-diffusers/tree/main/unet)
   - [sd-vae-ft-mse](https://huggingface.co/stabilityai/sd-vae-ft-mse)
   - [dwpose](https://huggingface.co/yzd-v/DWPose/tree/main)
   - [yolox](https://download.openmmlab.com/mmdetection/v2.0/yolox/yolox_l_8x8_300e_coco/yolox_l_8x8_300e_coco_20211126_140236-d3bd2b23.pth)
   - [image_encoder](https://huggingface.co/lambdalabs/sd-image-variations-diffusers/tree/main/image_encoder)

Finally, these weights should be organized in `pretrained_weights` as follows:
```
./pretrained_weights/
|-- MusePose
|   |-- denoising_unet.pth
|   |-- motion_module.pth
|   |-- pose_guider.pth
|   └── reference_unet.pth
|-- dwpose
|   |-- dw-ll_ucoco_384.pth
|   └── yolox_l_8x8_300e_coco.pth
|-- sd-image-variations-diffusers
|   └── unet
|       |-- config.json
|       └── diffusion_pytorch_model.bin
|-- image_encoder
|   |-- config.json
|   └── pytorch_model.bin
└── sd-vae-ft-mse
    |-- config.json
    └── diffusion_pytorch_model.bin

```
## Quickstart
### Inference
#### Preparation
Prepare your referemce images and dance videos in the folder ```./asserts``` and organnized as the example: 
```
./asserts/
|-- images
|   └── ref.png
└── videos
    └── dance.mp4
```

#### Pose Alignment
Get the aligned dwpose of the reference image:
```
python pose_align.py --imgfn_refer ./assets/images/ref.png --vidfn ./assets/videos/dance.mp4
```
After this, you can see the pose align results in ```./assets/poses```, where ```./assets/poses/align/img_ref_video_dance.mp4``` is the aligned dwpose and the ```./assets/poses/align_demo/img_ref_video_dance.mp4``` is for debug.

#### Inferring MusePose
Add the path of the reference image and the aligned dwpose to the test config file ```./configs/test_stage_2.yaml``` as the example:
```
test_cases:
  "./assets/images/ref.png":
    - "./assets/poses/align/img_ref_video_dance.mp4"
```

Then, simply run
```
python test_stage_2.py --config ./configs/test_stage_2.yaml
```
```./configs/test_stage_2.yaml``` is the path to the inference configuration file.

Finally, you can see the output results in ```./output/```

#### Face Enhancement

If you want to enhance the face region to have a better consistency of the face, you could use [FaceFusion](https://github.com/facefusion/facefusion). You could use the `face-swap` function to swap the face in the reference image to the generated video.

