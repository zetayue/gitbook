---
description: >-
  https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorUsefulCommands
---

# HTCondor Useful Commands

HTCondor has several dozens of commands, but in this section we will present just the most common ones \(if you want to check the complete list, try the [Command Reference page](https://htcondor.readthedocs.io/en/latest/man-pages/index.html)\). Also remember that you can get further information running `man condor_<cmd>` in your shell or visiting the [official Users' Manual](https://htcondor.readthedocs.io/en/latest/users-manual/index.html).

## Checking Pool Status

### **`condor_status`**

* List slots in HTCondor pool and their status: `Owner` \(used by owner\), `Claimed` \(used by HTCondor\), `Unclaimed` \(available to be used by HTCondor\), etc.

_Useful options:_

* `-avail`: List those slots that are not busy and could run HTCondor jobs at this moment
* `-submitters`: Show information about the current general status, like number of running, idle and held jobs \(and submitters\)
* `-run`: List slots that are currently running jobs and show related information \(owner of each job, machine where it was submitted from, etc.\)
* `-compact`: Compact list, with one line per machine instead of per slot
* `-state -total`: List a summary according to the state of each slot
* `-master`: List machines, but just their names \(status and slots are not shown\)
* `-server`: List attributes of slots, such as memory, disk, load, flops, etc.
* `-af <attr1> <attr2> <...>`: List specific attributes of slots, using autoformat \(new version, very powerful\)
* `-format <fmt> <attr>`: List attributes using the specified format \(old version\). For instance, next command will show the name of each slot and the disk space: `condor_status -format "%s\t " Name -format "%d KB\n" Disk`
* `-constraint <constraint>`: Only Show slots that satisfy the constraint. I.e: `condor_status -constraint 'Memory > 1536'` will only show slots with more than 1.5GB of RAM per slot.

## Submitting jobs

### **`condor_submit`** `<submit_file>`

* Submit jobs to the HTCondor queue according to the information specified in`submit_file`. _Visit the_ [_submit file page_](https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorSubmitFile) _to see some examples of these files. There are also some_ [_FAQs_](https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorFAQs) _related to the submit file._

_Useful options:_

* `-dry-run <dest_file>` : this option parses the submit file and saves all the related info \(name and locations of input and output files after expanding all variables, value of requirements, etc.\) to `<dest_file>`, but jobs are **not** submitted. Using this option is highly recommended when debugging or before the actual submission if you have made some modifications in your submit file and you are not sure whether they will work.
* `-append <command>`: add submit commands at submission time, without changing the submit file. You can add more than one command using several times`-append`.

{% hint style="info" %}
When submitted, each job is identified by a pair of numbers **X.Y**, like 345.32. The first number \(X\) is the **cluster id**: every submission gets a different cluster id, that is shared by all jobs belonging to the same submission. The second number \(Y\) is the **process id**: if you submitted N jobs, then this id will go from 0 for the first job to N-1 for the last one. For instance, if you submit a file specifying 4 jobs and HTCondor assign id 523 to that cluster, then the ids of your jobs will be 523.0, 523.1, 523.2 and 523.3.
{% endhint %}

{% hint style="danger" %}
Before submitting your jobs, it is recommended to do some simple tests in order to make sure that both your submit file and program work in a proper way: if you are going to submit many jobs and each job takes several hours to finish, before doing that try with just a few jobs and change the input data in order to let them finish in minutes. Then check the results to see if everything went fine before submitting the real jobs. Bear in mind that submitting untested files and/or jobs may cause a waste of time and resources if they fail, and also your priority will be lower in following submissions.
{% endhint %}

## Checking and Managing Submitted Jobs

### **`condor_q`**

* Show my jobs that have been submitted in this machine. By default you will see the ID of the job \(`clusterID.processID`\), the owner, submitting time, run time, status, priority, size and command. \[**STATUS**: **I**:idle \(waiting for a machine to execute on\); **R**: running; **H**: on hold \(there was an error, waiting for user's action\); **S**: suspended; **C**: completed; **X**: removed; **&lt;**: transferring input; and **&gt;**: transferring output\]

_Useful options:_

* `-global`: Show my jobs submitted in any machine, not only the current one
* `-wide`: Do not truncate long lines. You can also use `-wide:<n>` to truncate lines to fit `n` columns
* `-analyze <job_id>`: Analyse a specific job and show the reason why it is in its current state \(useful for those jobs in Idle status: Condor will show us how many slots match our restrictions and may give us suggestion\)
* `-better-analyze <job_id>`: Analyse a specific job and show the reason why it is in its current state, giving extended info
* `-long <job_id>`: Show all information related to that job
* `-run`: Show your running jobs and related info, like how much time they have been running, in which machine, etc.
* `-currentrun`: Show the consumed time on the current run, the cumulative time from last executions will not be used \(you can combine also with `-run` flag to see only the running processes at the moment\)
* `-hold`: Show only jobs in the "on hold" state and the reason for that. Held jobs are those that got an error so they could not finish. An action from the user is expected to solve the problem, and then he should use the `condor_release` command in order to check the job again
* `-af <attr1> <attr2> <...>`: List specific attributes of jobs, using autoformat

### **`condor_tail`** `<job_id>`

* Display on screen the last lines of the `stdout` \(screen\) of a running job on a remote machine. You can use this command to check whether your job is working fine, you can also visualize errors \(`stderr`\) or output files created by your program \(see also [this FAQ](https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorFAQs#ssh)\).

_Useful options:_

* `-f`: Do not stop displaying the content, it will be displayed until interrupted with `Ctrl+C`
* `-no-stdout -stderr`: Show the content of `stderr` instead of `stdout`
* `-no-stdout <output_file>`: Show the content of an output file \(`output_file` has to be listed in the `transfer_output_files` command in the submit file\).

### **`condor_release`** `<job_id>`

* Release a specific held job in the queue.

_Useful options:_

* `<cluster_id>`: Instead of giving a `<job_id>`, you can specify just the `<cluster_id>` in order to release all held jobs of a specific submission
* `-constraint <constraint>`: Release all my held jobs that satisfy the constraint
* `-all`: Release all my held jobs

{% hint style="info" %}
Jobs with _on hold_ state are those that HTCondor was not able to properly execute, usually due to problems with executable, paths, etc. If you can solve the problems changing the input files and/or the executable, then you can use `condor_release` command to run again your program since it will send again all files to the remote machines. If you need to change the submit file to solve the problems, then `condor_release` will NOT work because it will not evaluate again the submit file. In that case you can use `condor_qedit` \(see [this FAQ](https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorFAQs#ch_submit)\) or cancel all held jobs and re-submit them again
{% endhint %}

### **`condor_hold`** `<job_id>`

* Put jobs into the hold state. It could be useful when you detect that there are some problems with your input data \(see [this FAQ](https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorFAQs#bad_inputs) for more info\), you are running out of disk space for outputs, etc. With this command you can delay the execution of your jobs holding them, and, after solving the problems, assign them the idle status using `condor_release`, so they will be executed again.

_Useful options:_

* `<cluster_id>`: Instead of giving a `<job_id>`, you can specify just the `<cluster_id>` in order to hold all jobs of a specific submission
* `-constraint <constraint>`: Hold all jobs that satisfy the constraint
* `-all`: Hold all my jobs from the queue

### **`condor_rm`** `<job_id>`

* Remove a specific job from the queue \(it will be removed even if it is running\). Jobs are only removed from the current machine, so if you submitted jobs from different machines, you need to remove your jobs from each of them.

_Useful options:_

* `<cluster_id>`: Instead of giving a `<job_id>`, you can specify just the `<cluster_id>` in order to remove all jobs of a specific submission
* `-constraint <constraint>`: Remove all jobs that satisfy the constraint
* `-all`: Remove all my jobs from the queue
* `-forcex <job_id>`: It could happen that after removing jobs, they don't disappear from the queue as expected, but they just change status to **X**. That's normal since HTCondor may need to do some extra operations. If jobs stay with 'X' status a very long time, you can force their elimination adding `-forcex` option. For instance: `condor_rm -forcex -all`.

## Getting **I**nfo from Logs

### **`condor_userlog`** `<file.log>`

* Show and summarize job statistics from the job log files \(those created when using `log` command in the submit file\)

### **`condor_history`**

* Show all completed jobs to date \(it has to be run in the same machine where the submission was done\).

_Useful options:_

* `-userlog <file.log>`: list basic information registered in the log files \(use `condor_logview <file.log>` to see information in graphic mode\)
* `-long XXX.YYY -af LastRemoteHost`: show machine where job XXX.YYY was executed
* `-constraint <constraint>`: Only show jobs that satisfy the constraint. I.e: `condor_history -constraint 'RemoveReason=!=UNDEFINED'`: show your jobs that were removed before completion

{% hint style="info" %}
There is also an online tool to analyze your log files and get more information: `HTCondor Log Analyzer` \([http://condorlog.cse.nd.edu/ ](http://condorlog.cse.nd.edu/)\).
{% endhint %}

## Other Commands

### **`condor_userprio`**

* Show active HTCondor users' priority. Lower values means higher priority where 0.5 is the highest. Use `condor_userprio -allusers` to see all users' priority, you can also add flags `-priority` and/or `-usage` to get detailed information

### **`condor_submit_dag`** `<dag_file>`

* Submit a DAG file, used to describe jobs with dependencies. Visit the [Submit File \(HowTo\)](https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=HOWTOs.CondorHowTo#howto_dagman) section for more info and examples.

### **`condor_version`**

* Print the version of HTCondor.



