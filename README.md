# Diff-BGM: A Diffusion Model for Video Background Music Generation

Implementation

## 1. Installation

``` shell
pip install -r requirements.txt
pip install -e diffbgm
pip install -e diffbgm/mir_eval
```
Note : mir-eval file is not present in this repo, this has to be downloaded from [here](https://github.com/aik2mlj/polyffusion).
## 2. Training

### Preparations
// Please download this files, as they are very big to be uploaded in repository.
1. The extracted features of the dataset POP909 can be accessed [here](https://yukisaki-my.sharepoint.com/:u:/g/personal/aik2_yukisaki_io/EdUovlRZvExJrGatAR8BlTsBDC8udJiuhnIimPuD2PQ3FQ?e=WwD7Dl). Please put it under `/data/` after extraction.

2. The extracted features of the dataset BGM909 can be accessed [here](https://drive.google.com/drive/folders/1zRNROuTxVNhJfqeyqRzPoIY60z5zLaHK?usp=sharing). Please put them under `/data/bgm909/` after extraction. We use [VideoCLIP](https://github.com/CryhanFang/CLIP2Video) to extract the video feature, use [BLIP](https://github.com/salesforce/BLIP) to gain the video caption then use [Bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased) as the language encoder and use [TransNetV2](https://github.com/soCzech/TransNetV2) to capture the shot.   
We also provide the original captions [here](https://drive.google.com/drive/folders/1q2F7jOfJ6Y0eD-hM_pbZRuP7Jnk-1r7u?usp=sharing).

3. The needed pre-trained models for training can be accessed [here](https://yukisaki-my.sharepoint.com/:u:/g/personal/aik2_yukisaki_io/Eca406YwV1tMgwHdoepC7G8B5l-4GRBGv7TzrI9OOg3eIA?e=uecJdU). Please put them under `/pretrained/` after extraction. The split of the dataset can be find [here](https://drive.google.com/file/d/1IK0H4_pm85oGE7Dm9DXwEKhD2G6WY6J0/view?usp=sharing).

### Commands

```shell
python diffbgm/main.py --model ldm_chd8bar --output_dir [output_dir]
```
This code is not working in the original repo, I have resolved multiple errors in my local system and local environment. I made the code work but in the first epoch it is throwing an error related to ran out of input (while loading the pickel file, which is again not present in original repo, I have added it).

## 3. Inference

Please use the following message to generate music for videos in BGM909.

```shell
python diffbgm/inference_sdf.py --model_dir=[model_dir] --uncond_scale=5.
```
This code is working but it is running over multiple folders, so it is giving inference for only files present in repo (code is set also for other folders but it is not present in repo or any other drive of the author), but for present folders, I am getting the inference result in diffbgm/exp folder.

## 4. Test

To reproduce the metrics in our original paper, please refer to `/diffbgm/test.ipynb`.

| Backbone | PCHE | GPS | SI | P@20 | Weights|
| -------- | ---- | --- | -- | ---- | ------ | 
| Diff-BGM (original) | 2.840 | 0.601 | 0.521 | 44.10 | [weights](https://drive.google.com/file/d/1QzmJjNsSDQKpAEATD3XbSZalI1AULx1O/view?usp=sharing) |  // this is weight.pt file
| Diff-BGM (only visual) | 2.835 | 0.514 | 0.396 | 43.20 | [weights](https://drive.google.com/file/d/1mtX24RLViblmSBbwx1WPqzQnSLnat5i3/view?usp=sharing) |
| Diff-BGM (w/o SAC-Att) | 2.721 | 0.789 | 0.523 | 38.47 | [weights](https://drive.google.com/file/d/1q39Azhty0lznhfdVMWxplUkYN7CE0VmA/view?usp=sharing) |

We provide our generation results [here](https://drive.google.com/drive/folders/1kYQLAmw8-zyBx43RW7aUSE8VXcFDxkez?usp=sharing).

See our [demo](./video.mp4)!

<video width="320" height="240" controls>
  <source src="./video.mp4" type="video/mp4">
  <img src="video.jpg">
</video>
