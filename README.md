### Настроенный devcontainer для VS Code для решения задач ML/NLP/CV на GPU sm30

На основе:
- [VS Code Remote / GitHub Codespaces Container Definitions](https://github.com/microsoft/vscode-dev-containers)
- [VSCode Development Container Template](https://github.com/tadejsv/dev-container-repo)
- [Pytorch Dockerfile for graphics cards capability 3.0](https://github.com/dizcza/pytorch-sm30-docker)

Проверялось на Astra Linux CE:
- подключенные репозитории debian stretch
- nvidia driver 460.91.03
- CUDA 11.2
- [libnvidia-container](https://nvidia.github.io/libnvidia-container/)
- docker.io 18.09
- docker-compose 1.21.0

В `Dockerfile` прописать `UID` (если не 1000)
requirements.txt - пакеты python
sys_requirements.txt - системные бибилиотеки, которые должны быть в контейнере, и которые могут понадобиться отдельным пакетам python
