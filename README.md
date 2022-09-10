# CogVideo

This is the official repo for the paper: [CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers](http://arxiv.org/abs/2205.15868).


**News!** The [demo](https://wudao.aminer.cn/cogvideo/) for CogVideo is available! 

It's also integrated into [Huggingface Spaces 🤗](https://huggingface.co/spaces) using [Gradio](https://github.com/gradio-app/gradio). Try out the Web Demo [![Hugging Face Spaces](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue)](https://huggingface.co/spaces/THUDM/CogVideo)


**News!** The code and model for text-to-video generation is now available! Currently we only supports *simplified Chinese input*. 

https://user-images.githubusercontent.com/48993524/170857367-2033c514-3c9f-4297-876f-2468592a254b.mp4

* **Read** our paper [CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers](https://arxiv.org/abs/2205.15868) on ArXiv for a formal introduction. 
* **Try** our demo at [https://wudao.aminer.cn/cogvideo/](https://wudao.aminer.cn/cogvideo/)
* **Run** our pretrained models for text-to-video generation. Please use A100 GPU.
* **Cite** our paper if you find our work helpful

```
@article{hong2022cogvideo,
  title={CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers},
  author={Hong, Wenyi and Ding, Ming and Zheng, Wendi and Liu, Xinghan and Tang, Jie},
  journal={arXiv preprint arXiv:2205.15868},
  year={2022}
}
```

## Web Demo

The demo for CogVideo is at [https://wudao.aminer.cn/cogvideo/](https://wudao.aminer.cn/cogvideo/), where you can get hands-on practice on text-to-video generation. *The original input is in Chinese.*


## Generated Samples

**Video samples generated by CogVideo**. The actual text inputs are in Chinese. Each sample is a 4-second clip of 32 frames, and here we sample 9 frames uniformly for display purposes.

![Intro images](assets/intro-image.png)

![More samples](assets/appendix-moresamples.png)



**CogVideo is able to generate relatively high-frame-rate videos.**
A 4-second clip of 32 frames is shown below. 

![High-frame-rate sample](assets/appendix-sample-highframerate.png)

## Getting Started

### Setup

* Hardware: Linux servers with Nvidia A100s are recommended, but it is also okay to run the pretrained models with smaller `--max-inference-batch-size` and `--batch-size` or training smaller models on less powerful GPUs.
* Environment: install dependencies via `pip install -r requirements.txt`. 
* LocalAttention: Make sure you have CUDA installed and compile the local attention kernel.

```shell
pip install git+https://github.com/Sleepychord/Image-Local-Attention
```

## Docker
Alternatively you can use Docker to handle all dependencies.

1. Run ```./build_image.sh```
2. Run ```./run_image.sh```
3. Run ```./install_image_local_attention```

Optionally, after that you can recommit the image to avoid having to install image local attention again.


### Download

Our code will automatically download or detect the models into the path defined by environment variable `SAT_HOME`. You can also manually download [CogVideo-Stage1](https://lfs.aminer.cn/misc/cogvideo/cogvideo-stage1.zip) , [CogVideo-Stage2](https://lfs.aminer.cn/misc/cogvideo/cogvideo-stage2.zip) and [CogView2-dsr](https://model.baai.ac.cn/model-detail/100041) place them under SAT_HOME (with folders named `cogvideo-stage1` , `cogvideo-stage2` and `cogview2-dsr`)

### Text-to-Video Generation

```
./script/inference_cogvideo_pipeline.sh
```

Arguments useful in inference are mainly:

* `--input-source [path or "interactive"]`. The path of the input file with one query per line. A CLI would be launched when using "interactive".
* `--output-path [path]`. The folder containing the results.
* `--batch-size [int]`. The number of samples will be generated per query.
* `--max-inference-batch-size [int]`. Maximum batch size per forward. Reduce it if OOM. 
* `--stage1-max-inference-batch-size [int]` Maximum batch size per forward in Stage 1. Reduce it if OOM. 
* `--both-stages`. Run both stage1 and stage2 sequentially. 
* `--use-guidance-stage1` Use classifier-free guidance in stage1, which is strongly suggested to get better results. 

You'd better specify an environment variable `SAT_HOME` to specify the path to store the downloaded model.

*Currently only Chinese input is supported.*
