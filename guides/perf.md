# Linux perf

`perf` is a simple profiling tool that's available just about everywhere. It works best with a single process, but it can also be used to profile each MPI rank.  


### Generating a Profile

We'll use `perf stat` to profile each CloverLeaf MPI rank.  It's also possible to use `perf record` to save performance data to a file, but be aware that
this requires some special handling of perf's output file.  Generally speaking, you should stick to `perf stat` if your application has more than one process.

```
export OMP_NUM_THREADS=1
srun -N 1 -n 32 perf stat clover_leaf
```

Note that the `perf stat` part of the command comes **after** the `srun` command.  This is because we want to spawn a new instances of `perf  stat` for each MPI rank.  If we put `perf stat` in front of `srun` then we're profiling the `srun` command itself, not the application launched by `srun`.

### Viewing the performance data

Simply look in the application's stdout.  There will be a summary report for each MPI rank:

```
 Performance counter stats for 'clover_leaf':

      19942.691038      task-clock:u (msec)       #    0.873 CPUs utilized
                 0      context-switches:u        #    0.000 K/sec
                 0      cpu-migrations:u          #    0.000 K/sec
            46,175      page-faults:u             #    0.002 M/sec
    42,567,273,301      cycles:u                  #    2.134 GHz
    18,270,960,122      instructions:u            #    0.43  insn per cycle
   <not supported>      branches:u
         2,161,999      branch-misses:u

      22.849245764 seconds time elapsed
```
