# NYU_HPC_Instruction
## How to use miniconda and Jupyter Notebook on HPC
https://sites.google.com/nyu.edu/nyu-hpc/hpc-systems/greene/software/open-ondemand-ood-with-condasingularity

For some singularity, it already has some cuda inside, you can check cuda version by `nvcc -V`

## Use bash in Jupyter Notebook
`! pip3`,`! conda install` and so on.
## How ot install torch GPU version on HPC
To check GPU,`nvidia-smi`

It is not suggestion to use `conda install`, as it usually install the default cpu version, instead, it is highly suggested that, you install via `pip3 install` and specific the cuda(gpu) version, see:https://stackoverflow.com/questions/59621736/despite-installing-the-torch-vision-pytorch-library-i-am-getting-an-error-sayin 

## other issues
  1.If there no permission to install package, you should install package via shell instead of jupyter notebook, or you have to modify the **python file** in **Configure iPython Kernels**, to change `ro`(read only) to `rw`(read and write) in `--overaly` part in

`singularity exec $nv \
  --overlay /scratch/$USER/my_env/overlay-15GB-500K.ext3:ro \
  /scratch/work/public/singularity/cuda11.2.2-cudnn8-devel-ubuntu20.04.sif \
  /bin/bash -c "source /ext3/env.sh; $cmd $args"`
  
  to make sure the jupyter notebook have write permission when using overlay and the sigularity
  
  2. Another issue is that sometime there's no enough storage to install large package, you should try to use larger sigular and following the instruction of the first website to start an interactive job with adequate compute and memory resources like
  >To install larger packages, like Tensorflow, you must first start an interactive job with adequate compute and memory resources to install packages. The login nodes restrict memory to 2GB per user, which may cause some large packages to crash.
 
 `srun --cpus-per-task=2 --mem=10GB --time=04:00:00 --pty /bin/bash `wait to be assigned a node
 
  And try to clean all unnecessary packages,`conda clean --all --yes`, `pip3 uninstall XXXX`
