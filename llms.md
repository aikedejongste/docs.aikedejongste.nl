---
layout: default
title: LLM's and AI
has_children: false
---

# LLM's and AI

## Links

[Chatbot leaderboard](https://chat.lmsys.org/?leaderboard)
[WhisperFusion](https://github.com/collabora/WhisperFusion) - for realtime conversations with AI.

## Using a GPU at Vultr

Check if you have a GPU:

```bash
root@ttsgpu1:~# lspci | grep NVIDIA
06:00.0 VGA compatible controller: NVIDIA Corporation GA107GL [A2 / A16] (rev a1)
```

Check GPU info:

```bash
root@ttsgpu1:~# nvidia-smi

+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.125.06   Driver Version: 525.125.06   CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA A16-2Q       On   | 00000000:06:00.0 Off |                    0 |
| N/A   N/A    P8    N/A /  N/A |   1581MiB /  2048MiB |      0%      Default |
|                               |                      |             Disabled |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      8909      C   python3                          1581MiB |
+-----------------------------------------------------------------------------+
```

## Compose file

```
version: '3.8'
services:
  tts-gpu:
    image: ghcr.io/coqui-ai/tts
    container_name: tts-gpu
    ports:
      - "0.0.0.0:5002:5002"
    entrypoint: sleep infinity
    volumes:
      - /root/models:/root/.local
      - /root/inputs:/root/inputs
      - /root/outputs:/root/outputs
      - /root/scripts:/root/scripts
    deploy:
      resources:
        reservations:
          devices:
          - capabilities: ["gpu"]
```
