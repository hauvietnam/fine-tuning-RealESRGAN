# fine-tuning-RealESRGAN
Hướng dẫn fine-tuning model Real-ESRGAN với bộ ảnh biển số xe Việt Nam
- Tham khảo repo gốc tại đây https://github.com/xinntao/Real-ESRGAN/



## 🔧 Cài đặt các thư viện

- Python >= 3.7 (Recommend to use [Anaconda](https://www.anaconda.com/download/#linux) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html))
- [PyTorch >= 1.7](https://pytorch.org/)

### Cài đặt

1. Clone repo

    ```bash
    git clone [https://github.com/hauvietnam/fine-tuning-RealESRGAN.git]
    cd fine-tuning-RealESRGAN
    ```

1. Cài đặt thư viện
- Vì model gốc đã được tác giả đóng gói thành framework nên 1 số lỗi có thể phát sinh trong thư viện, mình sẽ tổng hợp 1 số lỗi mình đã tìm ra.
    ```bash
    # Install basicsr - https://github.com/xinntao/BasicSR
    pip install basicsr  
    # facexlib and gfpgan are for face enhancement
    pip install facexlib
    pip install gfpgan
    pip install -r requirements.txt
    python setup.py develop
    ```
- Sau khi cài xong, nếu gặp lỗi
  
  ```bash
  ModuleNotFoundError: No module named 'torchvision.transforms.functional_tensor'
  ```
  thì các bạn vào thư viện basicsr/data/degradations.py và sửa dòng
  
  ```bash 
  from torchvision.transforms.functional_tensor import rgb_to_grayscale
  ```
  thành
  
  ```bash
  from torchvision.transforms._functional_tensor import rgb_to_grayscale
  ```
  

| Option             | Description                                                                                                            |
|--------------------|------------------------------------------------------------------------------------------------------------------------|
| `-h`              | Show this help                                                                                                        |
| `-i input-path`   | Input image path (jpg/png/webp) or directory                                                                           |
| `-o output-path`  | Output image path (jpg/png/webp) or directory                                                                          |
| `-s scale`        | Upscale ratio (can be 2, 3, 4. Default=4)                                                                              |
| `-t tile-size`    | Tile size (>=32/0=auto, default=0). Can be `0,0,0` for multi-GPU                                                       |
| `-m model-path`   | Folder path to the pre-trained models (default=models)                                                                |
| `-n model-name`   | Model name (default=`realesr-animevideov3`, options: `realesr-animevideov3` | `realesrgan-x4plus` | `realesrgan-x4plus-anime` | `realesrnet-x4plus`) |
| `-g gpu-id`       | GPU device to use (default=auto). Can be `0,1,2` for multi-GPU                                                        |
| `-j load:proc:save`| Thread count for load/proc/save (default=`1:2:2`). Can be `1:2,2,2:2` for multi-GPU                                   |
| `-x`              | Enable TTA mode                                                                                                       |
| `-f format`       | Output image format (jpg/png/webp, default=`ext/png`)                                                                 |
| `-v`              | Verbose output                                                                                                        |



### Python script

#### Usage of python script

1. You can use X4 model for **arbitrary output size** with the argument `outscale`. The program will further perform cheap resize operation after the Real-ESRGAN output.

```console
Usage: python inference_realesrgan.py -n RealESRGAN_x4plus -i infile -o outfile [options]...

A common command: python inference_realesrgan.py -n RealESRGAN_x4plus -i infile --outscale 3.5 --face_enhance

  -h                   show this help
  -i --input           Input image or folder. Default: inputs
  -o --output          Output folder. Default: results
  -n --model_name      Model name. Default: RealESRGAN_x4plus
  -s, --outscale       The final upsampling scale of the image. Default: 4
  --suffix             Suffix of the restored image. Default: out
  -t, --tile           Tile size, 0 for no tile during testing. Default: 0
  --face_enhance       Whether to use GFPGAN to enhance face. Default: False
  --fp32               Use fp32 precision during inference. Default: fp16 (half precision).
  --ext                Image extension. Options: auto | jpg | png, auto means using the same extension as inputs. Default: auto
```

#### Inference general images

Download pre-trained models: [RealESRGAN_x4plus.pth](https://github.com/xinntao/Real-ESRGAN/releases/download/v0.1.0/RealESRGAN_x4plus.pth)

```bash
wget https://github.com/xinntao/Real-ESRGAN/releases/download/v0.1.0/RealESRGAN_x4plus.pth -P weights
```

Inference!

```bash
python inference_realesrgan.py -n RealESRGAN_x4plus -i inputs --face_enhance
```

Results are in the `results` folder


