# Workflow

To have an overall understanding of how to run jobs on the HPCs, a workflow is shown as below:

| Step | Work | Reference |
| :--- | :--- | :--- |
| 1 | Connect to HPC | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/connecting-to-hpcs) |
| 2 | Create a work directory of the job under `/raid/home/user_name` with needed codes and dataset | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/user-directories) |
| 3 | Set up the computing environment \(usually by choosing a proper Docker container image\) | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/docker-tutorial/use-docker-on-nvidia-gpu-cloud#use-ngc-service-on-hpcs) |
| 4 | Check the status of the HPC to decide the resources for running the job | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/running-jobs#2-check-the-system-status) |
| 5 | Submit the job \(usually with specified GPU or CPU\) | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/running-jobs#submit-a-job) |
| 6 | Monitor the job to avoid invalid runs | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/running-jobs#monitor-a-job) |
| 7 | Backup your data if necessary | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/data-backup) |

