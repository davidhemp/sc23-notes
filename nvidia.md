# Catch with Nvidia
This was mostly focused on the dpu cards they promised us and touching on training. We should be getting 5x BlueField-3 DPUs but it wasn't clear where we could put them. 
They are compatanble with the SR650 v2 GPU nodes we already have so we could put them in there but is that best solution. Really these are dev/play cards that we would like researchers to do some interesting things on so maybe they should have in their own little environment. 
Nvidia are going to talk to Lenovo about what would be required/best for such an environment. They are also going to look into getting us an NDR switch as the DPUs can run at 400Gbps and we have nothing that can support that. 
The DPUs are basiclly small machines that run Linux so they are be used for a number of applications. 
Example of projects Nvidia have worked on internally:
  * Off loading encryption.
  * Speeding up certain MPI functions, for example [ALLGATHER](https://www.open-mpi.org/doc/v3.0/man3/MPI_Allgather.3.php). This showed a significate speed up which was only increased in a non-blocking call
  * Running a VMware hypervisor, boring but useful example.
  * They can be used to replace an FPGA so might be something for Physics.
  * They can act as a security guard as they stand between the node and the connected network.

We didn't talk about Hopper because to be honest we can't get hold of them anyway.
