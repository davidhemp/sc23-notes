# HPC Systems Professionals Workshop (HPCSYSPROS23)

## Overview
https://sc23.conference-program.com/session/?sess=sess422

### Intro
Join https://sighpc-syspros.org/

### Clushible: Tidal Wave-Like Configuration with Ansible
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp109&sess=sess422

Jared Baker - National Center for Atmospheric Research (NCAR)

Configuration of HPC nodes is an important aspect of maintaining any HPC cluster. Our flagship HPE/Cray EX supercomputer, Derecho, is approximately 2,500 compute nodes and is susceptible to power interruptions from external factors such as lightning strike induced power sags and utility mishaps. These events challenged us to find an acceptable mean time to recovery. Ansible is our selected configuration management system but struggles with single large-scale runs of configuration despite optimizing individual runs such as tuning fork count and enabling pipelining. We needed a method to perform a large blast of configuration within a short time period to get the system back to a functional state or apply some level of remediation such as security updates. We therefore wrote a utility, Clushible, which wraps Ansible with ClusterShell's Python API to scale out the execution of Ansible that effectively took our standard full system run from multiple hours to minutes.

<ins>Notes</ins>

Derecho Cluster with 2500 nodes configured with Ansible. This ran into scale issues when the whole cluster needed to be rebuilt (they had a number of power outages). Instead they created a number of Ansible controllers that are controlled by "Clushible", cluster shell + Ansible. This dropped the reploy from 12 hours to 1 hour using 9 runners with 4 forks per code. Current limit of the reconnect time of GPFS (2500 nodes rejoining GPFS takes a while). github @jbaksta. 

### Embracing Batch on Kubernetes
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp104&sess=sess422

Jason Kincl Patrick Bruszewski - RedHat

As the adoption of Kubernetes continues to grow, there is an increasing demand for performing larger scale batch processing using Kubernetes. Much of the initial workloads are around machine learning but there is also interest in converging traditional HPC and Kubernetes clusters for operational efficiencies. In this talk, we want to look at how we can leverage both traditional HPC workload partitioning as well as features of Kubernetes to achieve a hybrid system that can be used for all types of workloads. We will show how we isolate the orchestration and user processes in Kubernetes allowing for maximum use of the hardware for running batch workloads with benchmark comparisons against a traditional HPC cluster. It is important to note that this area of research is still in its early stages and through our exploration of this topic we hope that this will continue to foster discussion in the HPC community.

<ins>Notes</ins>
Makes sense to use Kubernetes where a workload bursts or needs Elastic infrastructure. [Kueue](https://kueue.sigs.k8s.io/) is a kubernetes-native job manager but for HPC you can also intergrate kubernetes + Slurm. Redhat uses openshift and [KEDA](https://keda.sh/) for scaling. Pods run [ContainerSSH](https://containerssh.io/v0.4/) gives an "server" that has SSH capabilities. Kibernetes sechduales the Pds to run slurmd and then they join the Slurm cluster. KEDA is an autoscaler that can work out if more resuoures are needed, in this case they use a Prometheus exporter to monitor the queue depth. https://github.com/naps-product-sa/openshift-batch

