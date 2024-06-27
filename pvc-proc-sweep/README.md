# PVC Processor Sweep

Experiment: varying the number of CPU to find a representative ratio of CPU to GPU. Data was collected on Intel Sapphire Rapids nodes with 96 cores and a PCIe 4 Liqid fabric providing four or more Intel GPU MAX 1100. 

![Performance of a single PVC with varying number of processors, for the LJ test case](<LJ single-gpu performance.png>)
![Performance of two PVCs with varying number of processors, for the LJ test case](<LJ two-gpu performance.png>)
![Performance of four PVCs with varying number of processors, for the LJ test case](<LJ four-gpu performance.png>)
![Performance of a single PVC with varying number of processors, for the EAM test case](<EAM single-gpu performance.png>)
![Performance of two PVCs with varying number of processors, for the EAM test case](<EAM two-gpu performance.png>)
![Performance of four PVCs with varying number of processors, for the EAM test case](<EAM four-gpu performance.png>)

We see that the optimal number of MPI processes for the LJ and EAM cases is 84, rather than the full node of 96. For the two PVC cases, we see that 56 cores is sufficient. From this, we derive the ratio of 28 processes per GPU. Finally, we verify that 28 core for a single PVC yields acceptable performance. 

![Performance of a single PVC with varying number of processors, for the Rhodopsin test case](<Rhodo single-gpu performance.png>)
![Performance of two PVCs with varying number of processors, for the Rhodopsin test case](<Rhodo two-gpu performance.png>)
![Performance of four PVC with varying number of processors, for the Rhodopsin test case](<Rhodo four-gpu performance.png>)

We see that the optimal number of MPI processes for the Rhodopsin is 48, rather than the full node of 96. We see that for a single PVC, 24 cores is sufficient. From this, we derive the ratio of 24 processes per GPU. Finally, we verify that 48 core for two PVCs yields acceptable performance.
