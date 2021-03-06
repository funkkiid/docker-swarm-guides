# 前言

2018年初，于北京

当我学会如何使用Doker来发布一个容器，以及学会如何使用docker-compose来发布和管理服务时，我很惊讶Docker容器真的是一个好东西！在研究分布式系统、微服务框架时，Docker确实是一个很好的帮手。我们通过Docker能够在单主机上模拟分布式集群环境，当然你的主机在CPU/内存/硬盘等系统资源的性能如果能够尽可能高的话，你会感到你的学习和研究过程是多么的流畅，让人心旷神怡！

在开发环境或者实验环境中，我们通过单主机Docker Engine来模拟集群环境，可以很容易的将分布式系统的理论付诸于实践，帮助我们理解各种分布式系统的应用原理。但是在实际的生产环境中，我们不可能使用单主机来部署我们的任何应用。那么，在生产环境中如何做到Docker Engine跨主机互联呢？

后来，带着这个问题在百度上查了一些资料。百度上搜索出来的各大论坛和博客的文章大多在将需要使用Zookeeper等类似的组件做一个Discovery机制的数据中心，然后经过一系列的配置使多主机上的Docker Engine能够通过注册发现彼此，形成一个Docker Engine集群。这样在Docker Engine的集群中创建`overlay network`，容器之间就可以分布式部署在不同主机的Docker Engine上，并能够通过overlay network互通。我当时就想，What's the hell! 这么复杂！故障点岂不是越来越多？怎么保证稳定性！？

还有诸如使用虚拟网络交换机组件的方案，就不说了。最后看到使用Kubernetes、Mesos来集成管理的方案。好歹一个是Google的项目，一个是Apache的项目。具体优劣也是众说纷纭，而且学习成本也很大。我一直有一个直觉Docker本身应该有一个多主机集群的方案。最后，我在Docker v17.12的Guides中找到了关于Swarm的介绍。

读者将要阅读到的内容翻译来至Docker官网的指南关于Swarm部分的内容，也揉入了译者的理解。水平有限，还望大家指教斧正。作者的联系方式：holynull@126.com

**这里要向Docker项目的所有开发者和文档的编撰者致以崇高的敬意。尤其Guides的编撰者值得赞扬。Guides的语言简单明了，内容精细准确，为我们的学习提供了很大的帮助。**
