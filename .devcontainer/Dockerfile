FROM dizcza/pytorch-sm30

# Prepare shell and file system
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 SHELL=/bin/bash
ENV PATH /opt/conda/bin:$PATH
SHELL ["/bin/bash", "-c"]

# Install all system stuff
COPY sys_requirements.txt /tmp
RUN apt -o Acquire::AllowInsecureRepositories=true \
    -o Acquire::AllowDowngradeToInsecureRepositories=true \
    update && apt install -y --no-install-recommends \
    $(cat /tmp/sys_requirements.txt) && \
    apt clean && rm -rf /var/lib/apt/lists/*

# Make shared dir
RUN mkdir /workspace

# Copy over conda and bashrc, install environment
RUN ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrpc && \
    echo "conda activate base" >> ~/.bashrc

# RUN conda install -n base ipykernel --update-deps --force-reinstall
COPY requirements.txt /tmp
RUN pip install -r /tmp/requirements.txt

# Prepare entrypoint and mount folder
WORKDIR /workspace