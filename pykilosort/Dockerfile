FROM drewyangdev/matlab:R2021a-GUI

USER root
# system level dependencies
RUN apt-get update
COPY ./apt_requirements.txt /tmp/apt_requirements.txt
RUN xargs apt-get install -y < /tmp/apt_requirements.txt

# NVIDIA driver is managed by nvidia-container-toolkit and nvidia-docker-2
# https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#setting-up-nvidia-container-toolkit

# CUDA Toolkit
RUN wget -P /tmp/ http://developer.download.nvidia.com/compute/cuda/11.0.2/local_installers/cuda_11.0.2_450.51.05_linux.run
RUN cd /tmp && bash cuda*.run --silent --toolkit
ENV PATH /usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/local/cuda-11.0/lib64:${LD_LIBRARY_PATH}

# MATLAB Python API
RUN cd /home/muser/.MATLAB/extern/engines/python && python setup.py install
RUN apt-get install -y tk
ENV PATH /home/muser/.MATLAB/bin:${PATH}

# Mounted Data Volume Permission
RUN groupadd ubuntu --gid 1000
RUN usermod -aG ubuntu muser

# mkdir for everything
USER muser
RUN mkdir /home/muser/neuropixel
WORKDIR /home/muser/neuropixel

# CatGT
RUN wget -P /tmp/ http://billkarsh.github.io/SpikeGLX/Support/CatGTLnxApp.zip
RUN unzip /tmp/CatGTLnxApp.zip
RUN cd ./CatGT-linux && bash install.sh

# TPrime
RUN wget -P /tmp/ http://billkarsh.github.io/SpikeGLX/Support/TPrimeLnxApp.zip
RUN unzip /tmp/TPrimeLnxApp.zip
RUN cd ./TPrime-linux && bash install.sh

# C_Waves
RUN wget -P /tmp/ http://billkarsh.github.io/SpikeGLX/Support/C_WavesLnxApp.zip
RUN unzip /tmp/C_WavesLnxApp.zip
RUN cd ./C_Waves-linux && bash install.sh

# KiloSort
# 3.0
RUN git clone https://github.com/MouseLand/Kilosort.git Kilosort-3.0
# 2.5
RUN wget -P /tmp/ https://github.com/MouseLand/Kilosort/archive/refs/tags/v2.5.zip
RUN unzip /tmp/v2.5.zip
# 2.0
RUN wget -P /tmp/ https://github.com/MouseLand/Kilosort/archive/refs/tags/v2.0.zip
RUN unzip /tmp/v2.0.zip
# mexGPU build require cuda and matlab in the temp container
# cuda can use --gpu(docker run) or runtime: nvidia(docker-compose)
# matlab has to make sure this temp container has the matlab license and the mac address registered on the license
# the only difficulty is to set mac_address for the temp container
# TODO - find out how to set mac_address while docker-compose build

WORKDIR /home/muser/neuropixel

# pykilosort
RUN git clone https://github.com/MouseLand/pykilosort.git
RUN pip install cupy-cuda110
# pip install packages that requires cuda build, will be enable in docker run --gpu or docker-compose.yaml runtime: nvidia
RUN sed -i 's/cupy/cupy-cuda110/' /home/muser/neuropixel/pykilosort/requirements.txt && \
        pip install -r /home/muser/neuropixel/pykilosort/requirements.txt && \
        pip install phylib pyqtgraph pyqt5 && \
        pip install /home/muser/neuropixel/pykilosort/

# npy_matlab
RUN git clone https://github.com/kwikteam/npy-matlab.git

# ecephys_spike_sorting
RUN git clone https://github.com/ttngu207/ecephys_spike_sorting.git
#RUN pip install ./ecephys_spike_sorting/
#RUN pip install argschema==1.* marshmallow==2.*

# Workflow Array Ephys
RUN git clone https://github.com/ttngu207/sciops-demo-workflow-1.git
RUN pip install ./sciops-demo-workflow-1/
