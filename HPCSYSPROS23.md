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
Makes sense to use Kubernetes where a workload bursts or needs Elastic infrastructure. [Kueue](https://kueue.sigs.k8s.io/) is a kubernetes-native job manager but for HPC you can also intergrate kubernetes + Slurm. Redhat uses openshift and [KEDA](https://keda.sh/) for scaling. Pods run [ContainerSSH](https://containerssh.io/v0.4/) gives an "server" that has SSH capabilities. Kibernetes sechduales the Pds to run slurmd and then they join the Slurm cluster. KEDA is an autoscaler that can work out if more resuoures are needed, in this case they use a Prometheus exporter to monitor the queue depth. [GitHub](https://github.com/naps-product-sa/openshift-batch)

### Self-Service Monitoring of HPC and Openstack Jobs for Users
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp102&sess=sess422

Simon Guilbault - Université Laval

This paper describes a web portal called TrailblazingTurtle built for HPC and Openstack Cluster to let users view the resources used and wasted by their jobs, without having to modify their workflow. The metrics are collected from various data sources on the cluster to enable monitoring at the job and VM level and are presented to the users and staff members as a simple web application. This platform makes it easy for newer users to request the correct quantity of computing resources for their work, see their impact on the shared file system, and the evolution of the priority of their group in Slurm.

<ins>Notes</ins>
Users can see there usage via a webpage which uses cgroup information. They are also able to monitor GPU usage (+PCI and nvlink bandwidth) which is interesting. Allows them to see users that are under-utilizing the resources they are requesting. Also helps to show if they should be using localscratch which could result in job speedups. They give power usage relative to common things like how far an eletric car could travel and also how much that job would have cost to run in AWS. _They de-prioities users that continue to under-utilise resources_. We could do this using a custom reasource once we have the metrics. They should this information to the users so they know it is happening and encourage them to make changes to their jobs. Using Prometheus for monitoring with the Thanos plugin for longer term storage. Using [node_export](https://github.com/prometheus/node_exporter), [redfish_exporter](https://github.com/jenningsloy318/redfish_exporter), and [pcm-sensor-server](https://github.com/intel/pcm). [GitHub](https://github.com/guilbaults/trailblazingturtle)
