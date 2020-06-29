# CLOUD

## HPC
|woord|	Definition|
|---|---|
|Cluster|	A High Performance Computer composed of multiple computers where jobs are farmed out from the login or head node.
|Master Node|	The master node of the cluster is the computer that you log in to when you connect to the cluster. This node is used to compile software and submit jobs.
|Storage Node|	To reduce the load on the master node cluster home directories is attached to a storage node which is then accessible from anywhere in the cluster. This means that when compute jobs are run they can talk directly to the storage node without consuming any resources on the master node.
|CPU Compute Node|	The CPU compute nodes are where the CPU jobs run. Jobs are submitted from the master node and assigned to CPU compute nodes by the job scheduler. There are 77 CPU compute nodes in the Vikram-100 HPC
|GPU Compute Node|	The GPU compute nodes are where the GPU jobs run. Jobs are submitted from the master node and assigned to GPU compute nodes by the job scheduler. There are 20 GPU compute nodes in the Vikram-100 HPC.
|CPU Core|	Each node in the cluster has two CPUs each of which has 12 cores. Each core is able to run one job at a time so a single node can run a maximum of 24 jobs running in parallel.
|MPI|	MPI (Message Passing Infrastructure) is a technology for running compute jobs on more than node. Designed for situations where parts of the job can run on independent nodes with the results being transferred to other nodes for the next part of the job to be run.
|Module	|The module command is a means of providing access to different versions of software without risking version conflicts.
|Job Script|	A job script is a file containing all of the information needed to run a job including the resource requirements and the actual commands to run the job.
|Job Scheduler|	The job scheduler monitors the jobs currently running on the cluster and assigns waiting jobs to nodes based on recent cluster usage, job resource requirements and nodes available to the submitter. In summary the job scheduler determines when and where a job should run. The job scheduler that we use is called LSF.
|Interactive Job|	An interactive job is a way of testing your program and data on a cluster. You request a terminal on one of the nodes and then load and run files from the command line. Once you have your job working well interactively you should turn it into a batch job.
|Batch Job|	A batch job is a job on a cluster that runs without any further input once it has been submitted. Almost all jobs on the cluster are batch jobs.