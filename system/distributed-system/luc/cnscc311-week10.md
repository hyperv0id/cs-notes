# 计算模型
<iframe src="https://onedrive.live.com/embed?resid=C5FEA982BBD2F6E%21266821&authkey=!AMqt--dH5KvoiM8&em=2" width="714" height="432" frameborder="0" scrolling="no"></iframe>
## P2P计算

### 优点

- 去中心化：每个节点都是对等的，不需要访问中心服务节点。
- 可扩展：可以方便、简单地扩展
- 鲁棒性：错误不会扩散到整个系统。
- 隐私：难以追踪用户隐私（CS会保存cookie等隐私信息
- 高效：整合了空余的计算资源、存储资源


### 缺点
- 不方便找文件
- 安全：没有权限管理
- 备份恢复：备份只能在单个计算机之间
- 病毒：容易被攻击，病毒扩散更快。


### 应用&实现

使用P2P的应用：
- Skype、WhatsApp、BitTorrent
## 云计算

- 通过互联网服务计算资源：如软件、计算、服务

三种主要模型：

1. **基础设施即服务（Infrastructure as a Service，IaaS）：** 提供虚拟化的计算资源，如虚拟机、存储和网络。用户可以在这些虚拟资源上运行自己的操作系统和应用程序，有更大的灵活性和控制权。
    
2. **平台即服务（Platform as a Service，PaaS）：** 提供用于开发、部署和管理应用程序的平台。在这种模型下，用户无需关心底层的基础设施，而是专注于应用程序的开发和部署。
    
3. **软件即服务（Software as a Service，SaaS）：** 提供完全托管的应用程序，用户通过互联网访问。这种模型下，用户无需关心底层的硬件、操作系统或应用程序的维护，只需通过浏览器等方式使用服务。

特点：

- **弹性和可伸缩性：** 用户可以根据实际需求动态调整计算资源，从而实现弹性和可伸缩性。
    
- **共享资源池：** 多个用户可以共享云计算提供的资源池，通过虚拟化技术在同一硬件上运行多个虚拟实例。
    
- **自助服务和按需付费：** 用户可以根据需要自助服务地获取计算资源，并按照实际使用量支付费用，避免了固定的预算和资源浪费。
    
- **网络访问：** 云计算服务通过互联网或专用网络提供，用户可以随时随地通过网络访问这些服务。

例子：
1. **在线存储和文件共享：** 云存储服务如Google Drive、Dropbox和OneDrive允许用户在云端存储、同步和分享文件，使用户能够从任何设备访问其数据。
2. **虚拟服务器：** 云计算提供商如Amazon Web Services（AWS）、Microsoft Azure和Google Cloud Platform（GCP）提供虚拟服务器实例，用户可以在这些实例上运行应用程序和服务。
3. **软件即服务（SaaS）：** 云上的应用程序，如Google Workspace、Microsoft 365和Salesforce，允许用户通过互联网访问和使用各种办公和业务应用。
4. **弹性计算资源：** 云计算允许用户根据需要动态地调整计算资源，例如弹性的虚拟机规模，以适应应用程序的变化和需求波动。
5. 使用 OpenAI 的 API 服务访问chatgpt。当你使用 OpenAI 的 API，你实际上是在通过互联网访问 OpenAI 的服务器上的资源（在这种情况下，是聊天模型）。这些模型在 OpenAI 的数据中心中运行和管理，你无需在本地设备上安装或运行这些模型。因此，这符合云计算的定义，即通过互联网访问和使用远程服务器上的资源。这种方式提供了很大的灵活性，因为你可以根据需要来使用和支付这些服务，而无需担心硬件和维护的问题。所以，简单来说，使用 OpenAI 的 API 服务确实是一种云计算的应用。


## 雾计算

和云计算基本相似，不过雾计算强调**边缘计算**和**实时性**，适用于需要低延迟和高响应性的场景。


特点：

1. **边缘计算：** 雾计算强调将计算资源和服务推向网络的边缘，使其更**接近数据源**、终端设备和最终用户。这有助于减少数据传输的延迟，并提高对实时数据和应用程序的响应性。
    
2. **分布式架构：** 雾计算通常涉及分布式计算，其中计算任务可以在边缘设备、雾节点（Fog Node）和云端之间进行协同处理。这种分布式架构有助于有效地处理大规模的设备和数据。
    
3. **资源虚拟化：** 类似于云计算，雾计算也使用资源虚拟化技术，允许多个应用程序共享边缘设备和雾节点上的计算、存储和网络资源。
    
4. **实时性和可靠性：** 雾计算旨在支持对实时数据和应用程序的要求，例如物联网中需要实时决策和响应的场景。此外，由于雾计算将计算推向边缘，可以更好地处理网络故障或连接中断的情况。
    
5. **自适应性：** 雾计算具有自适应性，可以根据网络和环境条件实时调整计算资源的分配，以满足不同应用程序的需求。

例子：
1. **智能交通系统：** 在城市中部署雾计算节点，以处理交通监控摄像头捕捉到的实时视频流数据。这样可以在边缘设备上进行实时交通流量分析、车辆识别和交通管理决策，减少延迟并提高系统的响应速度。
    
2. **智能制造：** 在工厂生产线上部署雾计算节点，用于实时监控和控制制造过程。雾计算可以处理传感器数据、执行质量控制和优化生产计划，从而提高制造效率。
    
3. **医疗保健：** 在医院或医疗设施中使用雾计算节点，以处理来自医疗设备和传感器的实时数据。这有助于实现远程医疗监测、快速诊断和实时病人健康状况跟踪。
    
4. **智能家居：** 雾计算可以用于智能家居系统，通过在家庭内部部署雾节点，实现本地智能控制、语音识别和家庭自动化，减少对云端的依赖并提高响应速度。