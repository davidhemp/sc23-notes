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

### ICE 2.0: Restructuring and Growing an Instructional HPC Cluster
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp111&sess=sess422

J. Eric Coulter - Georgia Institute of Technology

The Partnership for an Advanced Computing Environment (PACE) at Georgia Tech (GT) has been running two campus-wide cluster resources available for academic courses and workshops for five years. The initial design focused on creating a federated resource for a wide range of educational topics, based on a PACE and College of Computing (COC) partnership. Due to funding, this took the form of separate resources, one funded by PACE, and another by COC. These "Instructional Cluster Environments", PACE-ICE and COC-ICE, became very popular with instructors at GT but led to a high maintenance cost due to the split nature of the environments. With the transition to the Slurm scheduler, PACE collaborated with COC to merge the two clusters into one, ICE. This work details the strategies used to sensibly merge the two production systems, including the storage architecture, shared system policies, and scheduler priority configurations that honor funding complexities.

<ins>Notes</ins>
ICE is a teaching cluster which uses a subset of the research hardware, currently 100 nodes with 3000 CPUS. All instructors are able to request access and then a course is given an "entitlement" that everyone enrolled in that course gains access via. This access is only granted while the couse is active and entitlements are a tree structure college-level / Department -> Intructor -> student. This also maps to a QoS tree and give priority to paying users and instructors during marking. Heavy users of Open OnDemand because of the type of users. 
Migration 
There were two systems which they decided to merge without data lose. 20K user directories across the systems with 7 TB of data. Data was copied with rsync (check sparse files!) which took about 5 days. Their filesystem seems slow so they bucketed home directoies into the last 2 digits of the UID to speed up ls of /home and /scratch. I don't know why it is slow because they have NetApp for home and Luster for scratch. Scratch clean up is at the end of each semester and deletes files older than 120 days.
[GitHub](https://github.com/pace-gt/hpcsyspros-SC23-ICE)
Also moving to Slurm from Torque.

### MareNostrum 5: Site Report from BSC

Sergi Girona - Barcelona Supercomputing Center (BSC)

<ins>Notes</ins>

### Democratizing Remote HPC Storage Access
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp103&sess=sess422

Adam Focht - Pennsylvania State UniversityInstitute for Computational and Data Sciences

Accessing HPC storage remotely can be cumbersome and involve using out-of-band tools (i.e. NFS, SCP, or SSHFS on Windows or SMB on Linux). We have begun to provide users access to our HPC storage using a tool that enables a familiar interface and behavior - like OneDrive or Dropbox - and unifies access to university-wide storage pools. We will walk through the software, configuration, lessons learned, and next steps in offering this service to our researchers. We are also exploring the use of built-in file tagging and other internal automation to provide a sensitive data workflow for HIPAA-aligned data security. Efficacy and lessons learned from this approach will be discussed.

<ins>Notes</ins>

### What a GReaT Scheduling Opportunity
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp105&sess=sess422

Gary Skouson - Pennsylvania State University

There are likely more ideas about the fair way to schedule workloads than there are systems that run those workloads. Possibly more than the number of users of those systems. Unfortunately, since fair is different in different contexts, the optimal solution is unlikely to be a one-size-fits-all, solution, or even an adjustable-size solution where everyone turns a couple of knobs to get the optimal fair solution to meet their need.

I will present GReaT allocations used at the Institute for Computational and Data Sciences, at Penn State University. We provide guaranteed start time expectations and priority scheduling, similar in some respects to condo-like scheduling systems. In addition to start time and resource availability, we provide access to temporary extended resources and protection against rogue or runaway jobs that could drain allocations. Our configuration required additional extensions in addition to the standard tools provided with slurm scheduling systems.

<ins>Notes</ins>

### Overcoming Active Directory Woes with Plain Text Caches and Replacing Passwords
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp110&sess=sess422

Jason St John - Guardant Health

Reliable authentication is a key component of all HPC systems. This paper discusses an approach that bypasses systemic authentication problems experienced by the authors to provide a simple and reliable manner of managing service accounts and user groups for HPC centers using plain text caches and alternatives to passwords.

<ins>Notes</ins>

### Heterogeneous Syslog Analysis: There Is Hope
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp108&sess=sess422

Andres Quan - Los Alamos National Laboratory (LANL)

Heterogeneous test-bed clusters present a unique challenge in identifying system hardware failures and anomalies as a result of the variation in the ways that errors and warnings are reported through the system log. We present a novel approach for the real-time classification of syslog messages, generated from a heterogeneous test-bed cluster, to proactively identify potential hardware issues and security events. By integrating machine learning models with high-performance computing systems, our system facilitates continuous system health monitoring.

The paper introduces a taxonomy for classifying system issues into actionable categories of problems, while filtering out groups of messages that the system administrators would consider unimportant "noise". Finally we experiment with using newly available large language models as a form of message classifier, and share our results and experience with doing so. Results demonstrate promising performance, and more explainable results compared to currently available techniques, but the computational costs may offset the benefits.

<ins>Notes</ins>

### Report on Adaptable Open-Source Disaster Recovery Solution for Multi-Petabyte Storage Systems
https://sc23.conference-program.com/presentation/?id=ws_hpcsysp101&sess=sess422

Honwai Leong - University of Sydney

The current generation of Research Data Store (RDS) at The University of Sydney comprises a pair of peta-scale data storage systems. We implemented a disaster recovery (DR) solution for data protection against catastrophic failure at either storage system. To handle large amount of data transactions into RDS, we took an open-source approach to design an adaptable DR solution that enables parallelized data replication capability between the pair of storage systems. In the last three years of operations, the DR solution has gone through a few iterations which saw improvement in efficiency. In this paper, we present the findings and outcomes from our DR implementation.

<ins>Notes</ins>
