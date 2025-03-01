* 250217
## Implementation levels of vitualisation
* Application
	* virtualizes an app as a VM
	* *Process level*
	* **JVM**
* Library(user-level API)
	* connect app and the rest of the system
	* **vCUDA**
* OS
	* isolated containers on a single physical server
	* **OpenVZ**
* Hardware Abstraction Layer(HAL)
	* manages the hardware through virtualization
	* virtual hardware environment for vms
	* **Vmware, XEN**
* Instruction Set Architecture(ISA)
	* Emulates a given ISA by the ISA of the host machine
	* **QEMU**
## Machine Level Virtualization


![image.png](https://img.dirtsai.work/astro-blog/2025/02/92a5f9ed63f17d8648ab20a577636c0e.png)
