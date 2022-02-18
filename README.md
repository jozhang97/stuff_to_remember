# Stuff I always google

## Ubuntu Screenshot

* Copy window to clipboard `ctrl+alt+printscreen``
* Copy selection to clipboard `ctrl+shift+printscreen`

## tar with progress bar

```
function tarc {
    tar czf - $1 -P | pv -s $(du -sb $1 | cut -f 1) > $1.tar.gz
}
```

```
function tarx {
    pv $1 | tar -xz
}
```

## rsync

```
rsync -avz --info=progress2 $SRC $DST
```

## Chrome

* Delete from suggestion bar `ctrl+shift+delete`

## VSCode `launch.json`

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "cwd": "/PATH/TO/WORK_DIR",
            "justMyCode": false,
            "env": {
                "FOO": "BAR",
            },
            "args": [
                 "foo", "bar",
            ]
        }
    ]
}
```

## SSH and tmux

```
ssh -XC USER@hHOST -t 'tmux new-session -A -s TMUX_ID'
```

## Conda

```
conda create --name NAME python=3.9

conda env create --name NAME --file=environments.yml

conda env export --no-builds | grep -v "prefix" > environment.yml
conda env export --name NAME --no-builds | grep -v "prefix" > environment.yml

conda env update --file environment.yml --prune
conda env update --name NAME --file environment.yml --prune
```

## Soft Links

```
ln -s SRC DST
```

## google drive

```
gdown --id ID
```

If it has too much traffic, try https://stackoverflow.com/questions/65312867/how-to-download-large-file-from-google-drive-from-terminal-gdown-doesnt-work

## arxiv download

```
wget --user-agent TryToStopMeFromUsingWgetNow https://arxiv.org/pdf/1606.09549.pdf
```

## selecting gpus

```
import GPUtil
if not any(x in os.environ for x in ['CUDA_VISIBLE_DEVICES', 'SLURM_STEP_GPUS', '_CONDOR_AssignedGPUs']):
    gpu = [str(g) for g in GPUtil.getAvailable(maxMemory=0.2)]
    assert len(gpu) > 0, f'No available GPUs, {GPUtil.showUtilization()}'
    print('Using GPU', ','.join(gpu))
    os.environ['CUDA_VISIBLE_DEVICES'] = ','.join(gpu)
```

## slurm
### View history
```
sacct -u jozhang --starttime 2014-07-01 --format=User,JobID,Jobname,partition,state,time,start,end,nodelist
```

### go into job
```
srun --jobid=$your_job_id --pty /bin/bash
```

### see job
```
squeue -all
squeue -O "jobid:8,username:10,mincpus:9,minmemory:11,gres:7,numtasks:6,state:9,timeused:12,nodelist:10"
/lusr/opt/slurm/bin/slurm-usage
```

### see user info
```
sreport user TopUsage Account=cs
```

### see gpus 
```
sinfo -O NodeList,CPUsState,AllocMem,FreeMem,Memory,Gres
```

## condor
### Submit jobs
```
condor_submit JOB_NAME.submit
```

### View jobs
```
condor_q -nobatch
condor_q -held
```

### Debug
```
condor_q -analyze JOB_ID
condor_q -better-analyze
condor_status -constrain 'Eldar'
```

### Remove running jobs
```
condor_rm jozhang -constraint 'JobStatus == 2'
```

## kill python jobs 
```
pgrep -u jozhang python | xargs kill   # xargs pipes previous into kill
```
