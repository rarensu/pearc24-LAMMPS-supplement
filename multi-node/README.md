# Comparison of Single-node and Multi-node Parallelization

Two LAMMPS builds for NVIDIA H100, using the GPU package or the Kokkos package, and one LAMMPS build for Intel GPU Max 1100 (PVC) were tested with different parallelization strategies. Strong scaling was performed across varying numbers of GPUs concentreated on a single Liqid fabric node or distributed across multiple nodes, with either 1 or 2 GPU per node in the multi-node cases.   

![Performance and scaling of GPU-accelerated LAMMPS with multiple parallelization strategies, for the LJ test case](<LAMMPS LJ Strong Scaling.png>)
![Performance and scaling of GPU-accelerated LAMMPS with multiple parallelization strategies, for the EAM test case](<LAMMPS EAM Strong Scaling.png>)
![Performance and scaling of GPU-accelerated LAMMPS with multiple parallelization strategies, for the Rhodopsin test case](<LAMMPS Rhodopsin Strong Scaling.png>)
