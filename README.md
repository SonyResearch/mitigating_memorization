# [Classifier-Free Guidance inside the Attraction Basin May Cause Memorization](https://arxiv.org/abs/2411.16738)

[Anubhav Jain](http://anubhav1997.github.io/), [Yuya Kobayashi](https://scholar.google.co.jp/citations?hl=ja&user=BhKHVqQAAAAJ&view_op=list_works&sortby=pubdate), [Takashi Shibuya](https://scholar.google.com/citations?user=XCRO260AAAAJ), [Yuhta Takida](https://scholar.google.co.jp/citations?user=ahqdEYUAAAAJ&hl=ja), [Nasir Memon](https://engineering.nyu.edu/faculty/nasir-memon), [Julian Togelius](https://engineering.nyu.edu/faculty/julian-togelius), [Yuki Mitsufuji](https://www.yukimitsufuji.com/)

New York University, Sony AI, and Sony Group Corporation

CVPR 2025


This repository contains the official codebase for the paper "Classifier-Free Guidance inside the Attraction Basin May Cause Memorization". We provide an approach to mitigate memorization in conditional diffusion models during inference. 

## Getting Started

### Data and Pre-trained Models
You can download the LAION-10K dataset from this [link](https://drive.google.com/drive/folders/1TT1x1yT2B-mZNXuQPg7gqAhxN_fWCD__?usp=sharing). 

SDv2.1 finetuned on the LAION-10K dataset is available on this [link](https://drive.google.com/file/d/1sNBcLASudpz09lvOghdMlKXTqwFLEh37/view?usp=share_link).

You can download the finetuned SDv1.4 on 200 memorized samples from this [link](https://drive.google.com/drive/folders/1XiYtYySpTUmS_9OwojNo4rsPbkfCQKBl) and the training images from this [link](https://drive.google.com/drive/folders/1oQ49pO9gwwMNurxxVw7jwlqHswzj6Xbd). 


## Mitigating Memorization using Static Transition Point Method

To mitigate memorization using the static transition point method for SDv2.1 finetuned on the LAION-10K dataset split, run the following command. Output images will be saved in ```sd-21-finetuned_LAION10K```. You will need the download the LAION-10K split to be able to compute the metrics. 

```
$ python3 generate_test_dataset_stp.py --pretrained_model_name_or_path ./sd-21-finetuned_LAION10K/ --outdir LAION_10k_SDv21_0_500_7_5 --guidance_change_step 500 --guidance_scale 0.0 --guidance_scale_later 7.5
```

## Mitigating Memorization with Opposite Guidance and Static Transition Point Method

To run the static transition point method with opposite guidance, run the following command. 

```
$ python3 generate_test_dataset_stp.py --pretrained_model_name_or_path ./sd-21-finetuned_LAION10K_og/ --outdir LAION_10k_SDv21_-2_650_7_5 --guidance_change_step 650 --guidance_scale -2.0 --guidance_scale_later 7.5
```


## Mitigating Memorization using Dynamic Transition Point Method

To run the experiments on the pretrained SDv1.4 (scenario 4 in the paper) set ```--unet_path CompVis/stable-diffusion-v1-4``` and to run the experiments on the SDv1.4 finetuned on 200 duplicated prompts (scenario 3) set ```--unet_path checkpoint-20000/unet```. Similarly, ```--end_iter``` is 500 for the former and 200 for the latter given the sizes of the datasets. And ```--exp``` should be set to ```sdv1_pretrained``` or ```200_memorized``` based on the scenario you want to experiment on. 

```
$ python3 generate_test_dataset_dtp.py --unet_path /path/to/unet/model --end_iter <200 or 500> --exp <sdv1_pretrained or 200_memorized>
```


## Mitigating Memorization using Opposite Guidance and Dynamic Transition Point Method

You can run the experiments on opposite guidance and DTP method using the following commands. The arguments remains the same as earlier, based on the configuration you are interested in. 

```
$ python3 generate_test_dataset_opposite_guidance.py --unet_path /path/to/unet/model --end_iter <200 or 500> --exp <sdv1_pretrained or 200_memorized>
```

## Mitigating Memorization on the ImageNette Dataset

To run the experiments on the Imagenetter dataset (scenario 2 in the paper), you can use the following command. To replicate results from the paper the transition point (```--guidance_change_step```) needs to be set at 600 or 700. 

```
$ python3 generate_test_dataset_stp.py --pretrained_model_name_or_path ./sd-21-finetuned_Imagenette/ --outdir Imagenette_SDv21_0_500_7_5 --guidance_change_step <600 or 700> --guidance_scale 0.0 --guidance_scale_later 7.5
```




If you use our work, please consider citing it. 
```
@article{jain2024classifier,
  title={Classifier-Free Guidance inside the Attraction Basin May Cause Memorization},
  author={Jain, Anubhav and Kobayashi, Yuya and Shibuya, Takashi and Takida, Yuhta and Memon, Nasir and Togelius, Julian and Mitsufuji, Yuki},
  journal={arXiv preprint arXiv:2411.16738},
  year={2024}
}
```

