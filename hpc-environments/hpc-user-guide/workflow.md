# Overall Workflow

The systems on the HPCs are all [Ubuntu](https://ubuntu.com/), which is a Linux operating system. The users should have the basic knowledge about using the Linux system. Information can be found on the [Linux Tutorial](https://compsci-hunter.gitbook.io/xie-research-group/useful-tutorials/linux-tutorial) page.

To have an overall understanding of how to run jobs on the HPCs, a workflow is shown below:

| Step | Work | Reference |
| :--- | :--- | :--- |
| 1 | Connect to HPC | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/connecting-to-hpcs) |
| 2 | Create a work directory of the job under `/raid/home/user_name` with needed codes and dataset | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/user-directories) |
| 3 | Set up the computing environment \(usually by using the Docker container image, Jupyter Notebook can also be used in Docker\) | [Page1](https://compsci-hunter.gitbook.io/xie-research-group/useful-tutorials/docker-tutorial/use-docker-on-nvidia-gpu-cloud#use-ngc-service-on-hpcs), [Page2](https://compsci-hunter.gitbook.io/xie-research-group/useful-tutorials/jupyter-notebook-tutorial/run-jupyter-server-with-gpu-access-on-hpcs) |
| 4 | Check the status of the HPC to decide the resources for running the job | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/running-jobs#2-check-the-system-status) |
| 5 | Submit the job \(usually with specified GPU or CPU\) | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/running-jobs#submit-a-job) |
| 6 | Monitor the job to avoid invalid runs | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/running-jobs#monitor-a-job) |
| 7 | Backup your data if necessary | [Page](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/hpc-user-guide/data-backup) |

