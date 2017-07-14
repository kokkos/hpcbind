#Purpose

A script to set the process mask, OMP environment variables and CUDA environment
variables to sane values if possible. Uses hwloc and nvidia-smi if available.  
Will preserve the current process binding, so it is safe to use with a queuing 
system or mpiexec.

