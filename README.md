# SciOps Demo Workflow Docker

***Drew Yang @ DataJoint*** | ***2021.10.05***

---

## Prerequisite
Build the base [MATLAB image](https://github.com/Yambottle/matlab) at first.
- Installer has to be an offline installer that includes all the toolboxes
- Wasn't ablt to activate licenses using the activation binary, but was able to directly mount license file in `~/MATLAB/licenses/` and skip the activation.

> The required image is referenced from [MATLAB image - Raphael Guzman](https://github.com/guzman-raphael/matlab)

## .env file exmaple
```
# MATLAB
MATLAB_HOSTID=06:a4:03:05:48:0c # MAC Address supplied to Mathworks as HostID associated with license.
MATLAB_USER=muser # System/ComputerLogin user associated with license. Default is muser.
MATLAB_UID=1000 # UID of your docker host. This is utilized to resolve permissions on volume data.
MATLAB_GID=1000 # GID of your docker host. This is utilized to resolve permissions on volume data.
MATLAB_IMAGE_TYPE=GUI # Available MATLAB Image types are: GUI, MIN
# MATLAB_LICENSE=$(base64 ./licenses/R2021a_joseph_license.lic -w 0) # base64 encode
MATLAB_VERSION=R2021a # Matlab version to be built. Must supply COMPLETE matlab_RXXXXX_glnxa64.zip for install in an installers dir.
MATLAB_FILE_KEY=<FILEKEY> # Mathworks provided file key associated with installation.
PY_VERSION=3.7 # >= 3.0. Necessary to complete install of Jupyter Notebook with MATLAB kernel (GUI).
MATLAB_INSTALLED_ROOT=/home/muser/.MATLAB # Directory where MATLAB root path is located.

# Datajoint
DJ_HOST=tutorial-db.datajoint.io
DJ_USER=<USERNAME>
DJ_PASS=<PASSWORD>
DATABASE_PREFIX=<USERNAME>_

# Data Input/Output -> Local Mapping
ROOT_DATA_DIR=/mnt/efs-incf/incf2021
PROCESSED_DATA_DIR=/home/ubuntu/workflow_processed_data

# In-container Module Directories
ecephys_directory=/home/muser/neuropixel/ecephys_spike_sorting/ecephys_spike_sorting
kilosort_repository=/home/muser/neuropixel/Kilosort-2.5
npy_matlab_repository=/home/muser/neuropixel/npy-matlab
catGTPath=/home/muser/neuropixel/CatGT-linux
tPrime_path=/home/muser/neuropixel/TPrime-linux
cWaves_path=/home/muser/neuropixel/C_Waves-linux
kilosort_output_tmp=/home/muser/neuropixel/data_for_ecephys/kilosort_datatemp

# Workflow worker
WORKER_COUNT=1
```
