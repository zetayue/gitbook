# Submitting Vanilla Job

## Introduction of Vanilla Universe

The vanilla universe in HTCondor is intended for most programs. Shell scripts are another case where the vanilla universe is useful.

Access to the job’s input and output files is a concern for vanilla universe jobs. One option is for HTCondor to rely on a shared file system, such as NFS or AFS. Alternatively, HTCondor has a mechanism for transferring files on behalf of the user. In this case, HTCondor will transfer any files needed by a job to the execution site, run the job, and transfer the output back to the submitting machine.

## Submit File for Vanilla Job

Here is an example submit description file for a sample job in a vanilla universe:

```text
universe                = vanilla
executable              = sub.sh
should_transfer_files   = YES
when_to_transfer_output = ON_EXIT

notify_user             = someaddress@mail.com
notification            = Complete

output                  = out.$(Cluster)
error                   = err.$(Cluster)
log                     = log.$(Cluster)

request_cpus            = 1
request_gpus            = 2
request_memory          = 3096
request_disk            = 10240

queue
```

## Bash File for Vanilla Job

Here is a bash file to be executed for running`main.py`:

```text
#!/bin/bash

cd /raid/home/user_name/work_dir
python main.py
```

{% hint style="danger" %}
Note that it is mandatory to use`cd /raid/home/user_name/work_dir`before your executable commmands \(`/raid/home/user_name/work_dir`is your local working directory that stores `main.py`on DLS\).
{% endhint %}

