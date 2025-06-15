# local-ai

This project is a collection of personal docker images to run local generative AI, as well as a docker-compose file to easily start the containers.
Please note that these images are fairly heavy, at up to 20Gb per image due to all the requirements needed. It may be possible to lower the image size further, but this is not my current priority

This project is only for setting up the tools, not actually using them. Some of them may initialize themselves, others may require additional downloads (like models).
The way the images are built are up to the level of my current use in tools, and may not be adapted to every user (ex: I may not be using extensions and my setup may not be compatible with extension installation as of now, since I never encountered the situation)

This project expects you to have a NVIDIA GPU for inference, and either Docker desktop on Windows with GPU support, or the Nvidia container toolkit installed.

Finally, this project aims to test and compare the different projects. I am only running one container at a time at this moment, and port/resource conflict is likely to happen in the project's current state if you try to run more than one container at a time.

Additional documentation will be added to this README at a later date

## Starting guide

### Build
build all images:
```sh
docker compose build
```

build a single image:
```sh
docker compose build {service name, as listed in the docker-compose.yml file}
```

example: 
```sh
docker compose build forge
```

### Run
run a single container (in attached mode):
```sh
docker compose up {service name, as listed in the docker-compose.yml file}
```

example: 
```sh
docker compose up forge
```
To stop a container in attached mode, simply press Ctrl+c. 

Alternatively, run the up command in detached mode with `-d`, and shut it down with `docker compose down`


## Text generation

### text-generation-webui

https://github.com/oobabooga/text-generation-webui

Will optimize the image later

### ollama

https://github.com/ollama/ollama

### KoboldCPP

https://github.com/LostRuins/koboldcpp

TODO

## Text generation frontend

### open-webui

https://github.com/open-webui/open-webui

Note: i'm only currently running it as a frontend for ollama

## Image Generation

### stable-diffusion-webui

https://github.com/AUTOMATIC1111/stable-diffusion-webui

### forge

https://github.com/lllyasviel/stable-diffusion-webui-forge

### reforge

https://github.com/Panchovix/stable-diffusion-webui-reForge

TODO

### ComfyUI

https://github.com/comfyanonymous/ComfyUI

TODO

## Music

### ACE-Step

https://github.com/ace-step/ACE-Step

## Video

TODO