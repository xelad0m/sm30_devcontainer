FROM dizcza/pytorch-sm30

# Prepare shell and file system
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 SHELL=/bin/bash
ENV PATH /opt/conda/bin:$PATH
SHELL ["/bin/bash", "-c"]

# Install all system stuff, including node
COPY sys_requirements.txt /tmp
ARG DEBIAN_FRONTEND="noninteractive"
RUN apt -o Acquire::AllowInsecureRepositories=true \
    -o Acquire::AllowDowngradeToInsecureRepositories=true \
    update && apt install -y --no-install-recommends \
    $(cat /tmp/sys_requirements.txt) && \
    apt clean && rm -rf /var/lib/apt/lists/*

# Create the sudo user
ARG USERNAME
# ARG UID # some issues with exporting/passing UID as ARG ??? passed directly
ENV UID=1000
RUN echo $USERNAME $UID
RUN useradd $USERNAME -u $UID -G sudo -s /bin/bash -m && \
    echo $USERNAME' ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
USER $USERNAME
ENV HOME=/home/$USERNAME
RUN sudo mkdir /workspace && sudo chown -R $USERNAME /workspace

# Copy over conda and bashrc, install environment
RUN sudo ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

# RUN conda install -n base ipykernel --update-deps --force-reinstall
COPY requirements.txt /tmp
RUN pip install -r /tmp/requirements.txt

# Jupyter config
ENV PATH ${HOME}/.local/bin:$PATH
COPY --chown=$USERNAME .devcontainer/jupyter_lab_config.py $HOME/.jupyter/ 

# Prepare entrypoint and mount folder
WORKDIR /workspace