### Настроенный devcontainer для VS Code для решения задач ML/NLP/CV на GPU sm30

На основе:
- [VSCode Development Container Template](https://github.com/tadejsv/dev-container-repo)
- [Pytorch Dockerfile for graphics cards capability 3.0](https://github.com/dizcza/pytorch-sm30-docker)

### Сборка контейнера отдельно 

Из директории `sm30_devcontainer/.devcontainer`:

`docker compose build`

Проверим работоспособность:

`docker run -it --rm --gpus all sm30`
`-> python -c "import torch; print(torch.rand(5).cuda())"`

### Детали 

В VS Code в настройках расширения `Dev Containers` убедиться, что команда для `docker?compose` соответствует той, что используется на хосте (`docker-compose` VS `docker compose`).

В конфигурации докера убедиться, что [добавлена](https://stackoverflow.com/questions/52865988/nvidia-docker-unknown-runtime-specified-nvidia) среда исполнения `nvidia`.

Если нужно, редактируются 
- `requirements.txt` - пакеты python, которые будут установлены в контейнер
- `sys_requirements.txt` - системные бибилиотеки, которые должны быть в контейнере, и которые могут понадобиться отдельным пакетам python в контейнере

Открыть локальную директорию в контейнере
- папка `.devcontainer` должна находиться в корне целевой директории
- `Ctrl+Shift+P` -> Dev Containers: Open Folder in Container и т.п. команды

Открыть папку локально
- `Ctrl+Shift+P` -> Dev Containers: Reopen Folder Locally

Проверялось на: 

- [x] Astra Linux CE:
    - подключенные репозитории debian stretch
    - nvidia driver 460.91.03
    - CUDA 11.2
    - [libnvidia-container](https://nvidia.github.io/libnvidia-container/)
    - docker 18.09
    - docker-compose 1.21.0

- [x] Альт Рабочая станция 10
    - nvidia driver 470.103.01
    - CUDA 10.2
    - [libnvidia-container](https://nvidia.github.io/libnvidia-container/)
    - docker 20.10
    - docker compose v2 2.12.0