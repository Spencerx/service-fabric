# Microsoft Azure Service Fabric 11.6 Release Notes

This release will only be available through Auto upgrades. Clusters set to automatic upgrades will receive this release. For how to configure upgrades, please see [classic](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade) or [managed](https://docs.microsoft.com/azure/service-fabric/how-to-managed-cluster-configuration) documentation.

## Contents
* [Service Fabric Packages and Versions](#service-fabric-packages-and-versions)
* [Service Fabric Features and Bug Fixes](#service-fabric-features-and-bug-fixes)
* [Retirement and Deprecation Path Callouts](#retirement-and-deprecation-path-callouts)
* [Repositories and Download Links](#repositories-and-download-links)

## Service Fabric Packages and Versions

Packages and versions are listed with the most recent version listed first.

The following packages and versions are part of this release:

### Service Fabric 11.6.235

| **Service** | **Platform** | **Version** |
|---|---|---|
| [Service Fabric Runtime](https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabric.11.6.235.1.exe) | Windows <br> Windows ARM64 <br> Ubuntu 22 | 11.6.235.1 <br> 11.6.235.2 <br> 11.6.235.4 |
| [Service Fabric for Windows Server](https://download.microsoft.com/download/8/3/6/836e3e99-a300-4714-8278-96bc3e8b5528/11.6.235.1/Microsoft.Azure.ServiceFabric.WindowsServer.11.6.235.1.zip) | Service Fabric Standalone Installer Package | 11.6.235.1 |
| [.NET SDK](https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabricSDK.8.6.235.msi) | Windows .NET SDK <br> Microsoft.ServiceFabric <br> Reliable Services and Reliable Actors <br> ASP.NET Core Service Fabric integration | 8.6.235 <br> 11.6.235.1 <br> 8.6.235 <br> 8.6.235 |

## Key Announcements

* Service Fabric 11.6 improves runtime stability by addressing multiple crash and fail-fast conditions across core runtime components.

## Breaking Changes

* Legacy SMB-based image-store file sharing is no longer supported on Linux. Linux clusters must use the secure non-SMB image-store transfer mode. Review configurations that explicitly depend on SMB-based transfers before upgrading.

## Service Fabric Features and Bug Fixes

Features and bug fixes are listed by the version in which they were introduced, with the most recent version listed first.

The following features and bug fixes are part of this release:

### Service Fabric 11.6.235 Features and Bug Fixes

| **Type** | **Overview** | **Description** |
|---|---|---|
| Bug Fix | Reliable Collections checkpoint corruption after clear | **Brief Description:** Reliable Collections checkpoint state could become corrupted after a specific sequence of clear operations. <br> **Feature/Bug Impact:** Persisted state could be corrupted. <br> **Solution/Fix:** Corrected checkpoint-state handling after clear operations. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Reliable Collections memory retention | **Brief Description:** Reliable Collections could retain objects in some high-transaction scenarios. <br> **Feature/Bug Impact:** Gradual memory growth could reduce reliability and cause outages. <br> **Solution/Fix:** Corrected lock cleanup and resource release. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | ESE database compaction for IDs above 255 | **Brief Description:** Internal ESE databases with identifiers greater than 255 could not be compacted. <br> **Feature/Bug Impact:** Disk usage could grow and free-space reporting could be inaccurate. <br> **Solution/Fix:** Extended database defragmentation to all applicable internal database identifiers. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Stale read after replica role change | **Brief Description:** A stable read could return stale data after a replica changed from primary to secondary and back. <br> **Feature/Bug Impact:** Recently committed updates could be omitted from a stable read. <br> **Solution/Fix:** Corrected read-version handling around replica role changes and barrier replication. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Feature | Serialization-version mismatch error code | **Brief Description:** Reliable Collections now returns a specific error for serialization-version mismatches. <br> **Feature/Bug Impact:** Serialization incompatibilities are easier to identify and diagnose. <br> **Solution/Fix:** Added FABRIC_E_SERIALIZATION_VERSION_NOT_SUPPORTED. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Replica fail-fast on backup log corruption | **Brief Description:** On-disk log corruption detected during backup could repeatedly fail a replica and stall writes. <br> **Feature/Bug Impact:** A service could experience sustained write delays. <br> **Solution/Fix:** The affected replica is now permanently faulted so Service Fabric can fail over to a healthy replica. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Database compaction free-space detection | **Brief Description:** Automatic database compaction could miss fragmented free space within pages. <br> **Feature/Bug Impact:** Clusters could retain unnecessary disk usage. <br> **Solution/Fix:** Improved free-space detection so compaction reclaims previously missed capacity. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Health reporting after cluster scale-out | **Brief Description:** Health reports for newly created partitions could stop after cluster scale-out. <br> **Feature/Bug Impact:** Partition health could remain unavailable until manual recovery. <br> **Solution/Fix:** Health reporting now resumes automatically after scale-out. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Duplicate application identifier allocation | **Brief Description:** Concurrent application creation could rarely allocate duplicate application identifiers. <br> **Feature/Bug Impact:** Duplicate identifiers could corrupt cluster state. <br> **Solution/Fix:** Hardened application-identifier allocation against concurrent collisions. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Upgrade queue starvation after failover | **Brief Description:** Recovered upgrades could starve new application create and delete requests after cluster failover. <br> **Feature/Bug Impact:** Cluster management requests could remain queued for an extended period. <br> **Solution/Fix:** The recovered-upgrade queue now scales to the node CPU count by default. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Empty health-entity accumulation | **Brief Description:** Empty health-reporting entities could accumulate on long-running clusters. <br> **Feature/Bug Impact:** Memory usage could grow as services, partitions, and replicas were created and deleted. <br> **Solution/Fix:** Empty health-reporting entities are now removed. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Feature | Configurable TCP idle timeout | **Brief Description:** Idle transport connections can now be closed automatically. <br> **Feature/Bug Impact:** Inactive connections no longer need to consume node resources indefinitely. <br> **Solution/Fix:** Added a configurable TCP idle timeout. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Feature | Digested-package cleanup disabled by default | **Brief Description:** Digested-package cleanup is now disabled by default. <br> **Feature/Bug Impact:** Large batched application upgrades avoid cleanup-related slowdowns. <br> **Solution/Fix:** Cleanup can be re-enabled through cluster configuration when required. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** Re-enable digested-package cleanup through cluster configuration if the previous behavior is required. |
| Feature | Create services in disabled state | **Brief Description:** Services can now be created in a disabled state and enabled later. <br> **Feature/Bug Impact:** Applications can support pause and resume scenarios without an additional creation flow. <br> **Solution/Fix:** Added the extensible ServiceOptions setting. IsCreateAsDisabled remains compatible but is deprecated. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** Existing IsCreateAsDisabled usage continues to work. |
| Bug Fix | Placement subphase timeout under load | **Brief Description:** Placement subphases could repeatedly time out under heavy load. <br> **Feature/Bug Impact:** Placement work could become stuck. <br> **Solution/Fix:** Added a fallback that allows timed-out work to complete within the placement phase. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Feature | Cluster-event-based placement metrics | **Brief Description:** Service Fabric now emits cluster-event-based placement metrics. <br> **Feature/Bug Impact:** External placement services can consume signals such as unplaced replicas and node-capacity violations. <br> **Solution/Fix:** Added metrics reporting for external placement decisions. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Feature | Container-to-Service Fabric TLS routing | **Brief Description:** Container application traffic can now be routed to Service Fabric over TLS. <br> **Feature/Bug Impact:** Containerized applications can use finer-grained role-based access control. <br> **Solution/Fix:** Added secure tenant-proxy routing for container-to-Service Fabric communication. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Application health undercount after node recovery | **Brief Description:** Get-ServiceFabricApplicationHealth could undercount deployed applications after node recovery. <br> **Feature/Bug Impact:** Application health results could report an incorrect deployed-application count. <br> **Solution/Fix:** Corrected Health Manager child-state restoration after node recovery. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Container-network default property handling | **Brief Description:** Omitted optional container-network properties could produce incorrect network settings. <br> **Feature/Bug Impact:** Container networking could be misconfigured. <br> **Solution/Fix:** Initialized networking configuration consistently when optional values are omitted. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Feature | Windows container storage isolation | **Brief Description:** Windows containers can opt in to stronger storage isolation starting with Service Fabric 11.6. <br> **Feature/Bug Impact:** Application content can be protected from unintended writes. <br> **Solution/Fix:** Added DisableWritableShares, which mounts the application directory read-only and uses separate writable storage for work, log, and temporary data. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** The setting is disabled by default. |
| Bug Fix | User-profile cleanup during app setup | **Brief Description:** Application setup and cleanup could intermittently fail during user-profile cleanup. <br> **Feature/Bug Impact:** Application deployment or removal could fail on affected nodes. <br> **Solution/Fix:** Improved profile-cleanup queuing and retry behavior. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Feature | IPv4/IPv6 dual-stack networking | **Brief Description:** Applications can opt in to IPv4 and IPv6 dual-stack networking. <br> **Feature/Bug Impact:** Applications can receive both IPv4 and IPv6 addresses. <br> **Solution/Fix:** Set isDualStack=true on an IP reservation in the application manifest. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Breaking Change | SMB image-store removal on Linux | **Brief Description:** Legacy SMB-based image-store file sharing is no longer supported on Linux. <br> **Feature/Bug Impact:** Linux clusters configured to rely on SMB-based image-store transfers must change configuration before upgrading. <br> **Solution/Fix:** Linux clusters now use the secure non-SMB image-store transfer mode. <br> **Documentation Reference:** N/A <br> **Breaking Change:** Yes. Legacy SMB-based image-store transfers on Linux have been removed. <br> **Workaround:** Use the secure non-SMB image-store transfer mode. |
| Bug Fix | Image-store replica close hang | **Brief Description:** The image-store service could hang while closing a replica. <br> **Feature/Bug Impact:** Node restarts could be delayed. <br> **Solution/Fix:** Corrected replica shutdown handling so the service closes cleanly. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Backup restore retry loop | **Brief Description:** Backup and Restore could retry for an extended period when a restore repeatedly crashed. <br> **Feature/Bug Impact:** Restore failure reporting could be significantly delayed. <br> **Solution/Fix:** Repeatedly crashing restores are now detected and reported sooner. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Backup cleanup status reporting | **Brief Description:** Backup cleanup failures could mask the original error or report a successful backup as failed. <br> **Feature/Bug Impact:** Backup status could be inaccurate and the primary failure could be obscured. <br> **Solution/Fix:** Preserved the primary backup result and corrected status reporting. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |
| Bug Fix | Runtime crash and fail-fast fixes | **Brief Description:** Multiple crash and fail-fast conditions affected core runtime components. <br> **Feature/Bug Impact:** Replication, transport, hosting, health reporting, cluster management, file handling, lease processing, telemetry, and Linux tracing could terminate unexpectedly in specific conditions. <br> **Solution/Fix:** Added defensive validation, safer callback and object-lifetime handling, resilient error paths, and fixes for race conditions, memory corruption, and invalid state handling. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A <br> **Workaround:** N/A |

## Retirement and Deprecation Path Callouts

* Service Fabric runtime will discontinue support for the Java SDK soon. For a smooth transition, we strongly recommend users to shift to Azure Service Fabric .NET SDK. If your current setup is based on the Service Fabric Java SDK, we suggest starting migration plans to smoothly switch to the Azure Service Fabric .NET SDK. Although applications using the Java SDK will continue to work, we highly recommend adopting the Service Fabric .NET SDK for optimal outcomes.

* Service Fabric runtime will soon be archiving and removing Service Fabric runtime versions less than 7.2 and older, as well as the corresponding SDK version 4.2 packages and older from the package Download Center. Archiving and removing will affect application scaling and re-imaging of virtual machines in a Service Fabric cluster running on unsupported versions. After older versions are removed/archived, this may cause rollback failures when an in-progress upgrade has errors.
  * To prevent disruption of workloads, create a new cluster using the following steps:
    * [Create a Service Fabric cluster using ARM template](https://learn.microsoft.com/azure/service-fabric/quickstart-cluster-template)
    * [Create a Standalone cluster](https://learn.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server)
    * Install the supported version of Service Fabric SDK based on the runtime version installed on the cluster.

## Repositories and Download Links

The list below is an overview of the direct links to the packages associated with this release, with the links for the most recent version listed first.

Follow this guidance for setting up your developer environment:
* [Getting Started with Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux)
* [Getting Started with Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac)
* [Getting Started with Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

### Service Fabric 11.6.235 Repositories and Download Links

Runtime:
https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabric.11.6.235.1.exe

SDK:
https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabricSDK.8.6.235.msi

Cab:
https://download.microsoft.com/download/b/0/b/b0bccac5-65aa-4be3-ab13-d5ff5890f4b5/11.6.235.1/MicrosoftServiceFabric.11.6.235.1.cab

Package:
https://download.microsoft.com/download/8/3/6/836e3e99-a300-4714-8278-96bc3e8b5528/11.6.235.1/Microsoft.Azure.ServiceFabric.WindowsServer.11.6.235.1.zip

Goalstate:
https://download.microsoft.com/download/7/d/1/7d1d1511-59a4-4933-8187-40c20065aa29/11.6.235.1/goalstate.11.6.235.1.json
