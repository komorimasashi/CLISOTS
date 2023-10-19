FROM pytorch/pytorch:1.13.1-cuda11.6-cudnn8-devel

WORKDIR /
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y curl wget git cmake ffmpeg
RUN pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu116
RUN pip install -U pip && pip install svgwrite svgpathtools cssutils numba torch-tools visdom svgwrite imageio-ffmpeg torchfile wandb ftfy imageio==2.4.1 regex tqdm jupyterlab ipywidgets moviepy scikit-image mediapipe dlib imutils openai
RUN git clone https://github.com/yael-vinker/CLIPasso.git
RUN git clone https://github.com/BachiLi/diffvg.git --recursive

WORKDIR /diffvg
# RUN sed -i "s/find_package(TensorFlow)/# find_package(TensorFlow)/g" CMakeLists.txt
# RUN sed -i "s/if(TensorFlow_FOUND)/if(0)/g" CMakeLists.txt
RUN DIFFVG_CUDA=1 python setup.py install
RUN pip install git+https://github.com/openai/CLIP.git --no-deps

WORKDIR /CLIPasso
RUN sed -i 's/pad = F._pad/pad = F.pad/g' CLIP_/clip/auxilary.py
RUN sed -i "s/sketches.append(sketch)/sketches.append(np.asarray(sketch))/g" display_results.py
RUN wget https://huggingface.co/akhaliq/CLIPasso/resolve/main/u2net.pth -O ./U2Net_/saved_models/u2net.pth

