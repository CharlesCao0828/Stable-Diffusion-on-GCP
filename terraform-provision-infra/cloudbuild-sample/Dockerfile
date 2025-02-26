FROM nvidia/cuda:11.8.0-runtime-ubuntu22.04

RUN set -ex && \
    apt update && \
    apt install -y wget git python3 python3-venv python3-pip xdg-utils && \
    rm -rf /var/lib/apt/lists/*

ENV LD_LIBRARY_PATH $LD_LIBRARY_PATH:/usr/local/cuda/lib64
RUN ln -s /usr/local/cuda/lib64/libcudart.so.11.0 /usr/local/cuda/lib64/libcudart.so

RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git

RUN pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 --extra-index-url https://download.pytorch.org/whl/cu117

RUN set -ex && cd stable-diffusion-webui \
    && mkdir repositories \
    && git clone https://github.com/CompVis/stable-diffusion.git repositories/stable-diffusion \
    && git clone https://github.com/CompVis/taming-transformers.git repositories/taming-transformers\
    && git clone https://github.com/sczhou/CodeFormer.git repositories/CodeFormer \
    && git clone https://github.com/salesforce/BLIP.git repositories/BLIP \
    && git clone https://github.com/crowsonkb/k-diffusion.git repositories/k-diffusion \
    && git clone https://github.com/Stability-AI/stablediffusion repositories/stable-diffusion-stability-ai \
    && pip install transformers diffusers invisible-watermark --prefer-binary \
    && pip install git+https://github.com/crowsonkb/k-diffusion.git --prefer-binary \
    && pip install git+https://github.com/TencentARC/GFPGAN.git --prefer-binary \
    && pip install git+https://github.com/mlfoundations/open_clip.git --prefer-binary \
    && pip install -r repositories/CodeFormer/requirements.txt --prefer-binary \
    && pip install -r requirements.txt --prefer-binary

RUN pip install opencv-contrib-python-headless opencv-python-headless xformers
RUN pip install --upgrade fastapi==0.90.1
RUN cp /stable-diffusion-webui/repositories/CodeFormer/basicsr/utils/misc.py \
    /usr/local/lib/python3.10/dist-packages/basicsr/utils/misc.py

EXPOSE 7860

WORKDIR /stable-diffusion-webui/
CMD ["python3", "webui.py", "--listen", "--xformers",  "--medvram"]