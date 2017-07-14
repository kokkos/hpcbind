# Purpose

A script to set the process mask, OMP environment variables and CUDA environment variables to sane values if possible. Uses hwloc and nvidia-smi if available.  Will preserve the current process binding, so it is safe to use with a queuing system or mpiexec.

```
Usage: hpcbind <options> -- command ...
  Set the process mask, OMP environment variables and CUDA environment
  variables to sane values if possible. Uses hwloc and nvidia-smi if
  available.  Will preserve the current process binding, so it is safe
  to use with a queuing system or mpiexec.

Options:
  --no-hwloc            Disable using hwloc to set process binding
  --proc-bind=<LOC>     Set the initial process mask for the script.
                        LOC can be any valid location argument for
                        hwloc-calc. (Default: all)
  --distribute=N        Distribute the current proc-bind into N groups
  --index=I             Use the i'th group (zero based)
  --visible-gpus=<L>    Comma separated list of gpu ids
                        Default: CUDA_VISIBLE_DEVICES or all gpus in
                        sequential order
  --gpu-ignore-queue    Ignore queue job id when choosing visible GPU
  --no-gpu-mapping      Do not set CUDA_VISIBLE_DEVICES
  --openmp=M.m          Set env variables for the given OpenMP version
                        Default: 4.0
  --openmp-percent=N    Integer percentage of cpuset to use for OpenMP
                        threads.  Default: 100
  --openmp-places=<Op>  Op=threads|cores|sockets. Default: threads
  --no-openmp-proc-bind Set OMP_PROC_BIND to false and unset OMP_PLACES
  --force-openmp-proc-bind=<OP>
                        Override logic for selecting OMP_PROC_BIND.
  --no-openmp-nested    Set OMP_NESTED to false
  --test-bindings       Show the bindings without executing a command
  --test-bindings-text  Show the bindings as text
  -v|--verbose          Show options and relevant environment variables
  -h|--help             Show this message

Sample Usage:
  Split the current process cpuset into 4 and use the 3rd group
    hpcbind --distribute=4 --index=2 -v -- command ...
  Bing the process to all even cores
    hpcbind --proc-bind=core:even -v -- command ...
  Bind to the first 64 cores and split the current process cpuset into 4
    hpcbind --proc-bind=core:0-63 --distribute=4 --index=0 -- command ...
  skip GPU 0 when mapping visible devices
    hpcbind --distribute=4 --index=0 --visible-gpus=1,2 -v -- command ...
  Display the current bindings
    hpcbind --proc-bind=numa:0 --test-bindings -v
  Display the current bindings
    hpcbind --proc-bind=numa:0.core:odd --test-bindings -v
```


See https://github.com/open-mpi/hwloc for more information about hwloc
