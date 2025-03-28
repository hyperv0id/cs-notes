## 概述

KVM 建立在 Linux 内核之上，充分利用了硬件虚拟化扩展（如Intel的VT和AMD的AMD-V），通过将宿主机的物理硬件资源（例如CPU、内存和存储）进行虚拟化，使得多个虚拟机可以在同一台物理机上运行。

### 特点

1. **高性能**：KVM 利用硬件虚拟化扩展，实现了接近原生性能的虚拟化，使得虚拟机能够获得高性能和低延迟。
    
2. **强大的兼容性**：作为基于 Linux 内核的虚拟化技术，KVM 能够运行多种不同操作系统的虚拟机，包括 Linux、Windows 和其他基于 x86 架构的操作系统。
    
3. **灵活性和可定制性**：KVM 提供了灵活的管理工具和 API，允许用户根据需求定制虚拟机环境，管理资源分配，并进行自动化运维。
    

### 核心组件

1. **QEMU**：一个开源的硬件模拟器，与 KVM 结合使用，提供虚拟化所需的设备模拟和管理功能。
    
2. **libvirt**：用于管理各种虚拟化技术的库和工具集，可以简化 KVM 的配置和管理。
    

### 应用场景

1. **数据中心虚拟化**：KVM 可以用于构建高性能的数据中心虚拟化平台，实现服务器资源的灵活分配和管理。
    
2. **开发和测试环境**：开发人员可以利用 KVM 快速部署和测试多种操作系统环境，加快开发周期。
    
3. **云计算基础设施**：KVM 作为一种轻量级和高性能的虚拟化技术，被广泛应用于构建云计算基础设施，提供弹性和可扩展的云服务。


## 使用

### 检查KVM支持

#### 硬件支持

```sh
$ LC_ALL=C lscpu | grep Virtualization
Virtualization:                     VT-x
```

如果没有显示，就说明不能虚拟化

#### 内核支持

```sh
$ lsmod | grep kvm
kvm_intel             425984  0
kvm                  1376256  1 kvm_intel
irqbypass              12288  1 kvm
```

如果没有需要手动加载modprobe：[内核模块 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/%E5%86%85%E6%A0%B8%E6%A8%A1%E5%9D%97#%E6%89%8B%E5%8A%A8%E5%8A%A0%E8%BD%BD%E6%97%B6%E8%AE%BE%E7%BD%AE)
