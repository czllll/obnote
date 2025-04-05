date: 20250207
# task1
* Get familiar with azure portal
	* set up a vm
	* PS: do not forget to shut down ur vm when it doesn't in use
# task2-hello
* Review **Parallel Programming with MPI**
* use mpich to run the program with mpirun on your localhost as a 4-core job
## step
* `sudo apt-get install mpich`
* compile hello.c on both vm in the same path
	* `mpicc hello.c -o hello`
*  Run the program with mpirun on your localhost as a x-core job
	* ` mpirun -n 16 -hostfile ~/cloud-computing-lab/hostfile.txt ~/cloud-computing-lab/hello`
* outcome
	* ![image.png](https://img.dirtsai.work/astro-blog/2025/02/05f504b4b85642fba90b73f6905d3982.png)
## obstacle
* Set the counterpart's *public key* on each VM, so we can ssh from one to another
* make sure that the the hello file in the same path.
## task2-qsort
* single machine
	* `mpiexec -n 2 ./qsort input.txt output.txt`
	* ![image.png](https://img.dirtsai.work/astro-blog/2025/02/108d84b297fef4e2f701ba0fb32fcfe5.png)
	* ![image.png](https://img.dirtsai.work/astro-blog/2025/02/22ea2deb4ba0557c19bc6e1633e6c7bc.png)
* 2 machines
	* `mpirun -n 2 -hostfile ~/cloud-computing-lab/hostfile.txt ~/cloud-computing-lab/qsort ~/cloud-computing-lab/input.txt ~/cloud-computing-lab/output.txt`
	* ![image.png](https://img.dirtsai.work/astro-blog/2025/02/d2aa87fef0d7633f3ba4b90ef4c5504c.png)

# 补充
![image.png](https://img.dirtsai.work/astro-blog/2025/02/dd0aabd90e5ebb24729c90588fc0d497.png)
## Message-passing Model
### Processes in distributed/parallel computing system
**Single Program Multiple Data (SPMD) model**
* Fixed number of processes defined at startup
* Process count stays constant during execution
* All processes run identical code
* Each process has unique identifier
* Processes alternate between computation and communication phases

### Advantages
* 给程序员管理内存等级的能力
* 适用于多种架构
* 简化debug
## Message Passing Interface
### progress
* Late 1980s: Vendor-specific libraries 
* 1989: PVM at Oak Ridge National Lab 
* 1992: MPI standard development begins 
* 1994: MPI 1.0 release 
* 1997: MPI 2.0 release
*  Today: MPI is dominant standard 
* Language support: C, C++, Python, Java (unofficial)
### communicator
* Communicator in MPI is a group of processes that can communicate with each other
* Two main types
	* MPI_COMM_WORLD
		* default communicator including all processes
	* Custom communicators
		* Created by splitting existing communicators.
* Key functions
	* MPI_Comm_size(): Get number of processes
	- MPI_Comm_rank(): Get process ID
	- MPI_Comm_split(): Create new communicators
	- MPI_Finalize(): shutting down MPI
### Basic Send/Receive
* Blocking
```c
MPI_Send(buffer, count, datatype, dest, tag, comm)
MPI_Recv(buffer, count, datatype, source, tag, comm, status)
```
* non-blocking
```c
MPI_Isend(buffer, count, datatype, dest, tag, comm, request)
MPI_Irecv(buffer, count, datatype, source, tag, comm, request, status)
```
parameters:
- buffer: Data to send/receive
- count: Number of elements
- datatype: MPI_INT, MPI_FLOAT, etc.
- dest/source: Process rank
- tag: Message identifier
- comm: Communicator
- request: Handle for non-blocking operations
- status: Receive status info


### Collective Communications集体通信 
1. Broadcast (MPI_Bcast)
	- One process sends to all others
	- Same data to all processes
2.  Reduce (MPI_Reduce)
	- Combines data using operation (sum, max, etc.)
	- Result to root process