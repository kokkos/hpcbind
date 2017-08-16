# Purpose

A script to set the process mask, OMP environment variables and CUDA environment variables to sane values if possible. Uses hwloc and nvidia-smi if available.  Will preserve the current process binding, so it is safe to use with a queuing system or mpiexec.

```
Usage: hpcbind <options> -- command ...
  Set the process mask, OMP environment variables and CUDA environment
  variables to sane values if possible. Uses hwloc and nvidia-smi if
  available.  Will preserve the current process binding, so it is safe
  to use with a queuing system or mpiexec.

Options:
  --no-hwloc-bind       Disable binding
  --proc-bind=<LOC>     Set the initial process mask for the script
                        LOC can be any valid location argument for
                        hwloc-calc  Default: all
  --whole-system        hpcbind will ignore the its parent process binding
  --distribute=N        Distribute the current cpuset into N partitions
  --distribute-partition=I
                        Use the i'th partition (zero based)
  --visible-gpus=<L>    Comma separated list of gpu ids
                        Default: CUDA_VISIBLE_DEVICES or all gpus in
                        sequential order
  --ignore-queue        Ignore queue job id when choosing visible GPU and partition
  --no-gpu-mapping      Do not set CUDA_VISIBLE_DEVICES
  --openmp=M.m          Set env variables for the given OpenMP version
                        Default: 4.0
  --openmp-percent=N    Integer percentage of cpuset to use for OpenMP
                        threads  Default: 100
  --openmp-places=<Op>  Op=threads|cores|sockets. Default: threads
  --no-openmp-proc-bind Set OMP_PROC_BIND to false and unset OMP_PLACES
  --force-openmp-num-threads=N
                        Override logic for selecting OMP_NUM_THREADS
  --force-openmp-proc-bind=<OP>
                        Override logic for selecting OMP_PROC_BIND
  --no-openmp-nested    Set OMP_NESTED to false
  --show-bindings       Show the bindings
  --lstopo              Show bindings in lstopo without executing a command
  -v|--verbose          Show options and relevant environment variables
  -h|--help             Show this message

Sample Usage:
  Split the current process cpuset into 4 and use the 3rd partition
    hpcbind --distribute=4 --distribute-partition=2 -v -- command ...
  Bing the process to all even cores
    hpcbind --proc-bind=core:even -v -- command ...
  Bing the the even cores of socket 0 and the odd cores of socket 1
    hpcbind --proc-bind='socket:0.core:even socket:1.core:odd' -v -- command ...
  Bind to the first 64 cores and split the current process cpuset into 4
    hpcbind --proc-bind=core:0-63 --distribute=4 --distribute-partition=0 -- command ...
  skip GPU 0 when mapping visible devices
    hpcbind --distribute=4 --distribute-partition=0 --visible-gpus=1,2 -v -- command ...
  Display the current bindings
    hpcbind --proc-bind=numa:0 --show-bindings -- command
  Display the current bindings using lstopo
    hpcbind --proc-bind=numa:0.core:odd --lstopo
```


See https://github.com/open-mpi/hwloc for more information about hwloc
