---
title: "Virtual machine is an analogue of a server"
description: "A virtual machine is an analogue of a server in a cloud infrastructure. The virtual machine is created in one of the directories and inherits access rights from them. When you create a virtual machine, you can choose which of the Availability Zones will host it."
---
# Virtual machines

_A VM_ is similar to a server in the cloud infrastructure.

## VM as a {{ yandex-cloud }} resource {#project}

A VM is created in one of the folders in your cloud and inherits access rights from them. [More information about the {{ yandex-cloud }} resource model](../../resource-manager/concepts/resources-hierarchy.md).

You cannot currently move a created VM to a different folder.

Each VM has a unique ID and name. The name is unique within the folder. The ID is generated automatically when the VM is being created and is unique within {{ yandex-cloud }}.

## Availability zone {#az}

{% include [instance-az](../_includes_service/instance-az.md) %}

## Computing resources {#types}

When creating a VM, you specify the amount of computing resources to be allocated to it: the number and performance of processor cores (vCPUs) and the amount of RAM. You can choose the computing resources that are appropriate for the expected load. For more information, see [{#T}](performance-levels.md).

## Disks {#disks}

At least one disk must be attached to a VM, that is, a boot disk. Currently, you can only attach a boot disk when creating a VM.

You can also attach additional disks to the VM. You can either attach a previously created disk or create a disk along with the VM. The new disk can be empty, or you can restore it from a snapshot or image.

{% include [attach_empty_disk](../_includes_service/attach-empty-disk.md) %}

You can attach and detach additional disks even after you created the VM.

Read more about disks in [{#T}](disk.md).

## Statuses {#status}

The status of a VM affects which operations you can currently perform on it.

> For example, the `STOPPED` status means that the VM is stopped and you can't connect to it. To connect to the VM, you must [start](../operations/vm-control/vm-stop-and-start.md#start) it first. After the status changes to `RUNNING` and the OS loads, you'll be able to connect to the VM.

For more information about statuses, see [{#T}](vm-statuses.md).

## Metadata {#metadata}

You can set your own metadata when creating or updating VMs. For example, to connect to a Linux VM, you need to pass an SSH key to it. This is done using the metadata service. Learn more in [{#T}](vm-metadata.md).

## Network {#network}

When creating a VM, you need to set the settings of the network interface connected to it: select the subnet to connect the VM to, configure internal and public IP addresses, and add the necessary [security groups](../../vpc/concepts/security-groups.md). This allows the VM to interact with other services on the intranet and internet.

{% include [security-groups-note](../_includes_service/security-groups-note.md) %}

For more information, see [{#T}](network.md).