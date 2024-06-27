# The Many Ways to Build LAMMPS
## NVIDIA with Containers
NVIDIA Container Registry provides builds of LAMMPS for NVIDIA GPUs. 

[https://catalog.ngc.nvidia.com/orgs/hpc/containers/lammps](https://catalog.ngc.nvidia.com/orgs/hpc/containers/lammps) 

Image `tag:patch_15Jun2023` is used for H100 GPUs. 

### Running Containers
Containers are executed using the Singularity (or Apptainer) container runtime. 

NVIDIA provides the Kokkos package for GPU acceleration. 

```
singularity run --nv lammps-nv-patch_15Jun2023.sif lmp -k on g 1 -sf kk ...
```

If the number of GPU to be used is greater than one, then the mpi runtime included in the container image is used to start the corresponding number of processes. 

```
singularity run --nv lammps-nv-patch_15Jun2023.sif mpirun -np 8 lmp -k on g 8 -sf kk ...
```

## FOSS with EasyBuild Modules
EasyBuild provides a build framework for LAMMPS for NVIDIA GPUs. This framework produces a module that contains the needed environment variables. Toolchains are selected from available Free Open Source Software modules. 

[https://github.com/easybuilders/easybuild-easyblocks/blob/develop/easybuild/easyblocks/l/lammps.py](https://github.com/easybuilders/easybuild-easyblocks/blob/develop/easybuild/easyblocks/l/lammps.py)

The recipe file for H100 GPUs, included in this repository, is named 

[LAMMPS-2Aug2023-foss-2022a_update2-CUDA-11.8.0-sm90.eb](LAMMPS-2Aug2023-foss-2022a_update2-CUDA-11.8.0-sm90.eb)

We are building from LAMMPS source code with version tag `2Aug2023_update2` and enabling the GPU package with CUDA.

### Running Modules

On ACES, the module tree is heirarchical, so the toolchain modules are loaded before the LAMMPS module. 
```
module load GCC/11.3.0 OpenMPI/4.1.4 LAMMPS/2Aug2023_update2-CUDA-11.8.0-sm90
```
This build uses the GPU package for acceleration. The mpi runtime provided by the toolchain modules is used to launch multiple processes, which in the GPU package, does not correspond to the number of GPUs. 

```
mpirun -np 84 lmp -pk gpu 8 -sf gpu ...
```

## OneAPI with Make
On ACES, OneAPI is installed in our heirarchical module system. These toolchains are loaded prior to building LAMMPS from source. 
```
module load GCCcore/12.2.0 binutils/2.39 intel-compilers/2023.0.0 impi/2021.8.0 imkl/2023.0.0 imkl-FFTW/2023.0.0
```
Some additional environment variables must be set, which is done using this helpful script provided with OneAPI. 
```
source ${ONEAPI_ROOT}/setvars.sh
```
Finally, LAMMPS can be build from source. We are building from a point on the develop branch of the source which self-identifies as "LAMMPS (7 Feb 2024 - Development)" and is compatible with Intel GPU Max 1100 (PVC) GPUs.

The primary core step of the build is copied here:
```
cd ${LAMMPS_ROOT}/src
make yes-asphere yes-kspace yes-manybody yes-misc
make yes-molecule yes-rigid yes-dpd-basic yes-gpu
cd ${LAMMPS_ROOT}/lib/gpu
make -f Makefile.oneapi -j
cd ${LAMMPS_ROOT}/src
make oneapi -j
```
The complete build recipe for Intel PVCs, included in this repository, is named
[lammps_setup_oneapi.sh](lammps_setup_oneapi.sh)

### Running OneAPI

On ACES, OneAPI is installed in our heirarchical module system. These toolchains are loaded prior to building LAMMPS from source. 
```
module load GCCcore/12.2.0 binutils/2.39 intel-compilers/2023.0.0 impi/2021.8.0 imkl/2023.0.0 imkl-FFTW/2023.0.0
```
Some additional environment variables must be set, which is done using this helpful script provided with OneAPI. 
```
source ${ONEAPI_ROOT}/setvars.sh
```
This build uses the GPU package for acceleration. The mpi runtime provided by the toolchain modules is used to launch multiple processes, which in the GPU package, does not correspond to the number of GPUs. Additionally, we use the Intel Compute Aggregation Layer (calrun) to orchstrate the MPI processes and reduce the overhead of parallelization. 
```
calrun mpirun -np 84 lmp_oneapi -v N off -pk gpu 8 -sf gpu ...
```
