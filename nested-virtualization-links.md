# Nested virtualization support

## General

Nested Hyper-V: https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/nested-virtualization

## AWS

Hyper-V on bare metal: https://aws.amazon.com/blogs/compute/running-hyper-v-on-amazon-ec2-bare-metal-instances/

Hyper-V on bare metal: https://www.youtube.com/watch?v=pQPLRImgq9U

Hyper-V on bare metal: https://forums.aws.amazon.com/thread.jspa?messageID=926556

AWS bare metal instances: https://aws.amazon.com/about-aws/whats-new/2019/02/introducing-five-new-amazon-ec2-bare-metal-instances/

Restrictions:
- Only bare metal

## Azure

Enable: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nested-virtualization

Announce: https://azure.microsoft.com/de-de/blog/nested-virtualization-in-azure/

Use case: https://www.nakivo.com/blog/hyper-v-nested-virtualization-on-azure-complete-guide/

Supported types: https://www.markou.me/2020/05/which-azure-vm-sizes-support-nested-virtualization/

Restrictions:
- Dv3 and Ev3 only

## GCP

Enable: https://cloud.google.com/compute/docs/instances/enable-nested-virtualization-vm-instances

Restrictions:
- Nested virtualization can only be enabled for L1 VMs running on Haswell processors or later. If the default processor for a zone is Sandy Bridge or Ivy Bridge, you can use minimum CPU selection to choose Haswell or later for a particular instance. Review the Regions and Zones page to determine which zones support Haswell or later processors.
- E2 machine types do not support nested virtualization.
- Nested virtualization is supported only for KVM-based hypervisors running on Linux instances. Hyper-V, ESX, and Xen hypervisors are not supported.
- Windows VMs do not support nested virtualization; that is, host VMs must run a Linux OS. However, nested VMs can run certain Windows OSes (described below).

## VMWare

ESXi: https://www.vembu.com/blog/nested-hyper-v-vms-on-a-vmware-esxi-server/

ESXi: https://www.vmwareblog.org/nested-virtualization-vmware-esxi-vs-microsoft-hyper-v/

Case: https://www.virtualizationhowto.com/2018/07/vmware-vs-hyper-v-nested-virtualization/

Compatibility notes: https://help.skytap.com/enabling-nested-virtualization.html

## Other/Notice

AWS bare metal vs KVM: https://www.twosixlabs.com/running-thousands-of-kvm-guests-on-amazons-new-i3-metal-instances/
