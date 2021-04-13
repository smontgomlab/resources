# SCG Primer 

See [this SCG orientation and quick reference manual](https://github.com/smontgomlab/resources/blob/master/scg_primer.md) for universal SCG tips. This documentation has some Montgomery Lab-specific SCG info. 

### Lab directory 
  - `/oak/stanford/groups/smontgom`  
  - Most of your files should be in `/oak/stanford/groups/smontgom/SUNETID`. The first time you log into SCG, you will have to make your personal directory in `/oak/stanford/groups/smontgom/` 
  - Shared lab data sets are in `${LAB_DIR}/shared` 
  - Check the lab quota with `lfs quota -gh oak_smontgom /oak`
  - See [this dashboard](https://srcc-lookout.stanford.edu/oak/smontgom) for a user-level breakdown of usage in `/oak/stanford/groups/smontgom/` 

#### Interactive session like durga 
1. Start a `screen`/`tmux` session (you should do this for any process that you expect to take more than a minute in case your connection to SCG is interrupted)  
2. Launch a job in the `interactive` partition (24-hour time limit)
    ```bash 
    sdev -c 1 -m 20G -t 24:00:00 
    ```
    - `sdev` is a shortcut for `srun`, which is a SLURM command 
    - `-c`: number of cores
    - `-m`: memory (`M` or `G` suffix) 
    - `-t`: time (format `DD-HH:MM:SS`) 

Once the resources are allocated, you essentially get `ssh`-ed into a new bash session with the requested resources. Then you can start running scripts (almost) just like you would with `durga`. 

### BILLED `batch` partition 
**You must get clearance from Stephen before running major jobs on the `batch` partition, which should be reserved for parallelizations beyond 16 cores or processes requiring more than 128GB of memory.** For the most part, you can pretend this option doesn't exist. Billing is done for number of CPUs used * actual run time - SCG does not charge for memory usage (but if you request too much your job might never run).  

#### Submit a job to the BILLED `batch` session with `sbatch` 
If you are running polished code or a standard pipeline that you don't need to worry about debugging, you may prefer to submit a job to the `batch` job queue using `sbatch`. 

First, write an `sbatch` script, e.g. `test_sbatch.sh`:
```bash 
#!/bin/bash

# See `man sbatch` or https://slurm.schedmd.com/sbatch.html for descriptions
# of sbatch options.
#SBATCH --job-name=test
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --partition=batch
#SBATCH --account=smontgom
#SBATCH --time=12:00:00

# by default, log files are written to the pwd 

set -e 
module load miniconda/3
python some_python_script.py # this is the process I want to run with the sbatch script 
``` 
Then submit the job using `sbatch test_sbatch.sh`. 
