# Manage Job with HTCondor

## Quick Start Guide

### Overall Pipeline to Manage Job with HTCondor

To users, HTCondor is a job scheduler. You give HTCondor a file containing commands that tell it how to run jobs. HTCondor locates a machine that can run each job within the pool of machines, packages up the job and ships it off to this execute machine. The jobs run, and output is returned to the machine that submitted the jobs.

### Pipeline Example

In this paragraph, an example will be provided to show more details of the pipeline to manage job with HTCondor.

In the beginning, we are going to run the traditional ‘hello world’ program. In order to demonstrate the distributed resource nature, we will produce a **`Hello DLS`** message 3 times, where each time is its own job. Since you are not directly invoking the execution of each job, you need to tell HTCondor how to run the jobs for you. The information needed is placed into a submit file ****\(e.g. **`hello-DLS.sub`**\), which defines variables that describe the set of jobs.

1. Copy the text below, and paste it into a file called **`hello-DLS.sub`**, the submit file, in your home directory on DLS:

   ```text
   # hello-DLS.sub
   # My very first HTCondor submit file

   # Specify the HTCondor Universe (vanilla is the default).
   universe = vanilla

   # Specify your executable (single binary or a script that runs several
   #  commands).
   # $(Process) will be a integer number for each job, starting with "0"
   #  and increasing for the relevant number of jobs.
   executable = hello-DGX.sh
   arguments = $(Process)

   # Specify that HTCondor should transfer files to and from the
   #  computer where each job runs.
   should_transfer_files = YES
   when_to_transfer_output = ON_EXIT

   # Specify the desired name of the file for HTCondor to store standard 
   #  output (or "screen output"), the desired name of the HTCondor log 
   #  file, the desired name of the standard error file.
   # Wherever you see $(Cluster), HTCondor will insert the queue number
   #  assigned to this set of jobs at the time of submission.
   output = hello-DGX_$(Cluster)_$(Process).out
   log = hello-DLS_$(Cluster).log
   error = hello-DLS_$(Cluster)_$(Process).err

   # Tell HTCondor what amount of compute resources
   #  each job will need on the computer where it runs.
   request_cpus = 1
   request_gpus = 1
   request_memory = 1GB
   request_disk = 1MB

   # Tell HTCondor to run 3 instances of our job (default is 1):
   queue 3
   ```

2. Now, create the executable that we specified above: copy the text below and paste it into a file called **`hello-DLS.sh`**:

   ```text
   #!/bin/bash

   echo "Hello DLS from Job $1 running on `whoami`@`hostname`"
   ```

   When HTCondor runs this executable, it will pass the $\(Process\) value for each job and `hello-DLS.sh` will insert that value for “$1” above.

3. Now, submit your job to the queue using **`condor_submit`**:

   ```text
   condor_submit hello-DLS.sub
   ```

   The **`condor_submit`** command actually submits your jobs to HTCondor. If all goes well, you will see output from the **`condor_submit`** command that appears as:

   ```text
   Submitting job(s)...
   3 job(s) submitted to cluster 9.

   ```

4. To check on the status of your jobs, run **`condor_q`** command. The output of **`condor_q`** should look like this:

   ```text
   -- Schedd: dls1.cluster.local : <127.0.0.1:9618?... @ 05/18/21 02:15:29
   OWNER     BATCH_NAME          SUBMITTED   DONE   RUN    IDLE  TOTAL JOB_IDS
   user_name CMD: hello-DLS.s   5/18 02:14      _      _      3      3 9.0-2

   3 jobs; 0 completed, 0 removed, 3 idle, 0 running, 0 held, 0 suspended
   ```

   You can run the **`condor_q`** command periodically to see the progress of your jobs. By default, **`condor_q`** shows jobs grouped into batches by batch name \(if provided\), or executable name. To show all of your jobs on individual lines, add the **`-nobatch`** option.  
  
   If your job is not in **`RUN`** status for a long time \(either in **`IDLE`** or in **`HOLD`** status\), run **`condor_q –analyze JOB_ID`** to get more details of the status of your job.  
  
   Jobs that failed to be run will always in the queue unless you delete them from the queue. To delete a job, run **`condor_rm JOB_ID`** command.  

5. When your jobs complete after a few minutes, they’ll leave the queue. If you do a listing of your home directory with the command **`ls -l`**, you should see something like:

   ```text
   -rw-r--r-- 1 user_name user_name    0 Jan 29 02:14 hello-DLS_9_0.err
   -rw-r--r-- 1 user_name user_name   47 Jan 29 02:47 hello-DLS_9_0.out
   -rw-r--r-- 1 user_name user_name    0 Jan 29 02:14 hello-DLS_9_1.err
   -rw-r--r-- 1 user_name user_name   47 Jan 29 02:47 hello-DLS_9_1.out
   -rw-r--r-- 1 user_name user_name    0 Jan 29 02:14 hello-DLS_9_2.err
   -rw-r--r-- 1 user_name user_name   47 Jan 29 02:47 hello-DLS_9_2.out
   -rw-rw-r-- 1 user_name user_name 3425 Jan 29 02:47 hello-DLS_9.log
   -rw-rw-r-- 1 user_name user_name  115 Jan 29 02:13 hello-DLS.sh
   -rw-rw-r-- 1 user_name user_name 1416 Jan 29 02:14 hello-DLS.sub
   ```

   Useful information is provided in the user log and the output files.

  
   HTCondor creates a transaction log of everything that happens to your jobs. Looking at the log file is very useful for debugging problems that may arise.

