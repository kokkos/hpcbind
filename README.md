# Purpose

A script to set the process mask, OMP environment variables and CUDA environment variables to sane values if possible. Uses hwloc and nvidia-smi if available.  Will preserve the current process binding, so it is safe to use with a queuing system or mpiexec.

Options:<br />
  --proc-bind=<LOC>     Set the initial process mask for the script.  <br />
                        LOC can be any valid location argument for<br />
                        hwloc-calc. (Default: all)<br />
  --distribute=N        Distribute the current proc-bind into N groups<br />
  --index=I             Use the i'th group (zero based)<br />
  --visible-gpus        Comma separated list of gpu ids<br />
                        Default: CUDA_VISIBLE_DEVICES or all gpus in<br />
                        sequential order<br />
  --gpu-ignore-queue    Ignore queue job id when choosing visible GPU<br />
  --no-gpu-mapping      Do not set CUDA_VISIBLE_DEVICES<br />
  --openmp=M.m          Set env variables for the given OpenMP version<br />
                        Default: 4.0<br />
  --openmp-percent=N    Integer percentage of cpuset to use for OpenMP<br />
                        threads.  Default: 100<br />
  --openmp-places=<Op>  Op=threads|cores|sockets. Default: threads<br />
  --no-openmp-proc-bind Set OMP_PROC_BIND to false and unset OMP_PLACES<br />
  --force-openmp-proc-bind=<OP><br />
                        Override logic for selecting OMP_PROC_BIND.<br />
  --no-openmp-nested    Set OMP_NESTED to false<br />
  --test-bindings       Show the bindings without executiong a command<br />
  -v|--verbose          Show options and relevant environment variables<br />
  -h|--help             Show this message<br />
<br />
Sample Usage:<br />
  Split the current process cpuset into 4 and use the 3rd group<br />
    hpcbind --distribute=4 --index=2 -v -- command ...<br />
  Bing the process to all even cores<br />
    hpcbind --proc-bind=core:even -v -- command ...<br />
  Bind to the first 64 cores and split the current process cpuset into 4<br />
    hpcbind --proc-bind=core:0-63 --distribute=4 --index=0 -- command ...<br />
  skip GPU 0 when mapping visible devices<br />
    hpcbind --distribute=4 --index=0 --visible-gpus=1,2 -v -- command ...<br />
  Display the current bindings<br />
    hpcbind --proc-bind=numa:0 --test-bindings -v<br />

