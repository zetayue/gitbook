# Manage Job with HTCondor

## Quick Start Guide

### Overall Pipeline to Manage Job with HTCondor

To users, HTCondor is a job scheduler. You give HTCondor a file containing commands that tell it how to run jobs. HTCondor locates a machine that can run each job within the pool of machines, packages up the job and ships it off to this execute machine. The jobs run, and output is returned to the machine that submitted the jobs.

### Pipeline Example

In this paragraph, an example will be provided to show more details of the pipeline to manage job with HTCondor.

In the beginning, we are going to run the traditional ‘hello world’ program. In order to demonstrate the distributed resource nature, we will produce a `Hello DLS` message 3 times, where each time is its own job. Since you are not directly invoking the execution of each job, you need to tell HTCondor how to run the jobs for you. The information needed is placed into a **submit file** \(e.g. `hello-DLS.sub`\), which defines variables that describe the set of jobs.

1. Copy the text below, and paste it into a file called `hello-DLS.sub`, the submit file, in your home directory on DLS:

   ```text
   # hello-DLS.sub
   # My very first HTCondor submit file
   #
   # Specify the HTCondor Universe (vanilla is the default and is used
   #  for almost all jobs), the desired name of the HTCondor log file,
   #  and the desired name of the standard error file.
   #  Wherever you see $(Cluster), HTCondor will insert the queue number
   #  assigned to this set of jobs at the time of submission.
   universe = vanilla
   log = hello-DGX_$(Cluster).log
   error = hello-DGX_$(Cluster)_$(Process).err
   #
   # Specify your executable (single binary or a script that runs several
   #  commands), arguments, and a files for HTCondor to store standard
   #  output (or "screen output").
   #  $(Process) will be a integer number for each job, starting with "0"
   #  and increasing for the relevant number of jobs.
   executable = hello-DGX.sh
   arguments = $(Process)
   output = hello-DGX_$(Cluster)_$(Process).out
   #
   # Specify that HTCondor should transfer files to and from the
   #  computer where each job runs. The last of these lines *would* be
   #  used if there were any other files needed for the executable to run.
   should_transfer_files = YES
   when_to_transfer_output = ON_EXIT
   # transfer_input_files = file1,/absolute/pathto/file2,etc
   #
   # Tell HTCondor what amount of compute resources
   #  each job will need on the computer where it runs.
   request_cpus = 1
   request_gpus = 1
   request_memory = 1GB
   request_disk = 1MB
   #
   # Tell HTCondor to run 3 instances of our job:
   queue 3
   ```

2. 
