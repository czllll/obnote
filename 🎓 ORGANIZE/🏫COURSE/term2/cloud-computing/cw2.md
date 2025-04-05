## AIM
* This coursework aims to investigate the performance impact of such language runtimes on two different platforms: commercial and open-source.
## 任务
* Investigation:
	* comercial: Azure Functions serverless solution
	* open source: Knative, **Openwhisk**, OpenFaas.
* Identify 2 papers in the performance of *language runtimes* supported by serverless platforms.
* Consider a server-less application. Confider a workflow of functions
	* image processing 
	* parallel processing 
	* ml ...
* Design a set of experiments to assess the capabilities and performance of serverless platforms. 选一种语言：Java
* TEST：a set of test functions and demonstrate the language runtime performance in terms of effectiveness and efficiency.

## 论文阅读
* *Evaluation of Language Runtimes in Open-source Serverless Platforms*
* 无服务基础设置
	* 坏处：冷启动，冷启动的时间在不同语言中区别明显


## Q2 Problem motivation and related work

10 Points

Grading comment:

Problem motivation: 1-2 paragraphs.


Related work: 1/2 page with clear referencing
* motivation
	* 随着无服务器计算平台的快速发展，越来越多的企业和开发者开始将应用程序部署到商业云平台（如 AWS Lambda、Google Cloud Functions）和开源平台（如 OpenFaaS、Kubeless）上。无服务器架构的优势在于其无需管理基础设施，开发者可以专注于代码本身，并且平台能够根据实际需求动态扩展计算资源。尽管如此，性能问题，特别是在不同编程语言的支持上，仍然是一个不可忽视的挑战。不同编程语言的运行时在无服务器平台上表现出的性能差异，如启动时间、执行效率、资源消耗等，直接影响着应用的响应速度和成本效益。
	* 因此，我们需要通过探究不同编程语言在商业平台和开源平台下的性能差异，特别是在冷启动、吞吐量、资源利用等方面，来深入了解其在无服务器架构中的表现。这将有助于开发者做出更明智的选择，从而提高应用性能并优化云资源的使用。此外，随着无服务器平台不断发展，理解这些性能差异还可以为未来的平台设计和语言支持提供重要参考。因此，研究语言运行时在无服务器环境中的表现，不仅有助于提升开发者体验，同时也推动了无服务器计算平台的进一步优化和普及。
* Performance of Java in Function-as-a-Service  Computing 论文
* 冷启动（Cold Start）是 FaaS 平台的主要性能瓶颈之一。由于安全性原因，**每次函数在新的机器上执行时，必须启动一个新的容器**。虽然相同开发者的相同函数可以复用容器，但**闲置容器会在短暂的时间后被关闭**，以提高资源利用率。如果函数**触发频率较低**，容器**需要每次重新启动或恢复**，导致额外的延迟。**冷启动通常会增加函数的响应延迟**，但不同的云提供商、编程语言和容器镜像版本的影响程度不同 。
* 商业 Serverless 平台（如 **AWS Lambda、Azure Functions、Google Cloud Functions**）提供**高度托管的环境**，具备**自动扩展、内置监控和与云服务深度集成**的优势，适用于**企业级应用和大规模部署**。然而，它们的**运行时受限**，可能不支持所有语言，并且**成本受调用次数和执行时间影响**。相比之下，开源 Serverless 平台（如 **OpenWhisk、Knative、OpenFaaS**）提供**更高的可定制性**，可部署在**本地或 Kubernetes 集群**上，适用于**私有云、混合云环境**，但**维护和运维成本较高**，需要额外管理扩展性、安全性和监控。选择平台时，企业需权衡**易用性、可扩展性、成本和自定义需求**。
* 商业平台在**运行时优化和容器管理**方面做了大量优化，使其**冷启动延迟和执行开销较低**，但容器化仍然会引入额外的**资源隔离、镜像加载和环境初始化**成本。只不过，相较于开源平台，**商业平台的底层优化使得这种开销更难被察觉**，但它仍然存在，并且可能因语言和执行环境的不同而有所差异。

## Q5 Results and evaluation

15 Points

Grading comment:

Results - in the form of tables and /or graphs. To be uploaded Discussion of results and contrast with the literature (1 page max.)
