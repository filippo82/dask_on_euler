# Dask on Euler

Euler uses the `LSF` batch system.


## Useful resources

* [Jupyter on HPC setup guide](https://git.geomar.de/python/jupyter_on_HPC_setup_guide)

* [4h course on Dask Jobqueue](https://github.com/willirath/dask_jobqueue_workshop_materials)

* [Dask JupyterLab Extension](https://github.com/dask/dask-labextension)

* [X11 forwarding batch interactive jobs](https://scicomp.ethz.ch/wiki/X11_forwarding_batch_interactive_jobs)


## Installation workflow

### Install Miniconda

* curl https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o Miniconda3.sh

* bash Miniconda3.sh -b -p ${HOME}/miniconda3

### Create a new environment and install the required packages

* conda env create -f environment.yml

* source activate dask_on_euler

* python -m ipykernel install --user --name dask_on_euler --display-name "Python (dask_on_euler)"

* jupyter labextension install dask-labextension

* jupyter lab clean # Probably not needed

* jupyter labextension install @jupyter-widgets/jupyterlab-manager

* jupyter notebook --generate-config
* jupyter notebook password

Configure Dask''s dashboard to forward through Jupyter:

* python -c ''from dask.distributed import Client'' 

In the newly created `.config/dask/distributed.yaml` file, set:

```
#   ###################
#   # Bokeh dashboard #
#   ###################
#   dashboard:
    link: "/proxy/{port}/status"
```

### Launch Jupyter

* bsub -Is -n 24 -W 01:00 /bin/bash

* conda activate dask_on_euler

* echo "ssh -N -L 8888:`hostname`:8888 $USER@euler.ethz.ch"

* export XDG_RUNTIME_DIR=""

* jupyter lab --no-browser --ip=`hostname` --port=8888

### Connect to the server

* ssh -N -L 8888:eu-c7-080-15:8888 bfilippo@euler.ethz.ch

### Open JupyterLab in the browser

* http://localhost:8888

To connect to the dashboard, one needs to insert this address in `Dask Dashboard URL`:

* http://localhost:8888/proxy/8787/status


