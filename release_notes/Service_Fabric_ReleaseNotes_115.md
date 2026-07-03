# Microsoft Azure Service Fabric 11.5 Release Notes

This release will only be available through Auto upgrades. Clusters set to automatic upgrades will receive this release. For how to configure upgrades, please see [classic](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade) or [managed](https://docs.microsoft.com/azure/service-fabric/how-to-managed-cluster-configuration) documentation.

## Contents
* [Service Fabric Packages and Versions](#service-fabric-packages-and-versions)
* [Service Fabric Feature and Bug Fixes](#service-fabric-feature-and-bug-fixes)
* [Retirement and Deprecation Path Callouts](#retirement-and-deprecation-path-callouts)
* [Repositories and Download Links](#repositories-and-download-links)

## Service Fabric Packages and Versions

Packages and versions are listed with the most recent version listed first.

The following packages and versions are part of this release:

### Service Fabric 11.5.116

| **Service** | **Platform** | **Version** |
|---|---|---|
| [Service Fabric Runtime](https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabric.11.5.116.1.exe) | Windows <br> Windows ARM64 | 11.5.116.1 <br> 11.5.116.2 |
| [Service Fabric for Windows Server](https://download.microsoft.com/download/8/3/6/836e3e99-a300-4714-8278-96bc3e8b5528/11.5.116.1/Microsoft.Azure.ServiceFabric.WindowsServer.11.5.116.1.zip) | Service Fabric Standalone Installer Package | 11.5.116.1 |
| [.NET SDK](https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabricSDK.8.5.116.msi) | Windows .NET SDK <br> Microsoft.ServiceFabric <br> Reliable Services and Reliable Actors <br> ASP.NET Core Service Fabric integration | 8.5.116 <br> 11.5.116.1 <br> 8.5.116 <br> 8.5.116 |

### Service Fabric 11.5.111

| **Service** | **Platform** | **Version** |
|---|---|---|
| Service Fabric Runtime | Windows <br> Windows ARM64 | 11.5.111.1 <br> 11.5.111.2 |
| Service Fabric for Windows Server | Service Fabric Standalone Installer Package | 11.5.111.1 |
| .NET SDK | Windows .NET SDK <br> Microsoft.ServiceFabric <br> Reliable Services and Reliable Actors <br> ASP.NET Core Service Fabric integration | 8.5.111 <br> 11.5.111.1 <br> 8.5.111 <br> 8.5.111 |

## Key Announcements

* Service Fabric version 11.5.111 is deprecated as of version 11.5.116.
* Service Fabric 11.5 features .NET 10 support for Windows.
* There is no Ubuntu 22.04 release for Service Fabric 11.5.
* In Service Fabric 12, Microsoft.ServiceFabric.Data will no longer depend on Microsoft.ServiceFabric.Data.Extensions, and customers should prepare to remove Microsoft.ServiceFabric.Data.Extensions package references.
* Service Fabric Backup Restore Service now supports opt-in backup metadata validation through SHA256 checksum verification in backup policy.

## Breaking Changes

* In Service Fabric 11.5, Microsoft.ServiceFabric.ReliableCollection.Store.KeyValueTuple<T> is marked obsolete and in Service Fabric 12 it will no longer be available to customer applications. Customers should move to System.Tuple and remove Microsoft.ServiceFabric.Data.Extensions references.

## Service Fabric Feature and Bug Fixes

Features and bug fixes are listed by the version in which they were introduced, with the most recent version listed first.

The following features and bug fixes are part of this release:

### Service Fabric 11.5.116 Features and Bug Fixes

| **Type** | **Overview** | **Description** |
|---|---|---|
| Bug | Version incompatibility between 11.4 and 11.5.111 | **Brief Description:** Applications built with Service Fabric SDK 11.4 crash when Service Fabric runtime is upgraded to 11.5.111 due to breaking change in COM interfaces. <br> **Feature/Bug Impact:** Service Fabric upgrade to 11.5 cannot be completed with default upgrade policies. If upgrade bypasses default policies, applications built with Service Fabric SDK 11.4 will fail to run. <br> **Solution/Fix:** Service Fabric runtime 11.5.116.1 runs applications built with SDK 11.4 successfully. <br> **Workaround:** Rebuild applications with Service Fabric SDK 11.3. |

### Service Fabric 11.5.111 Features and Bug Fixes

| **Type** | **Overview** | **Description** |
|---|---|---|
| Feature | .NET 10 Support on Windows | **Brief Description:** .NET 10 is supported for apps running on Service Fabric v11.5 on Windows and built with Service Fabric SDK v8.5 or later. <br> **Feature/Bug Impact:** Users building Service Fabric applications for .NET 8 on Windows using Service Fabric SDK v8.5 or later can now migrate to .NET 10. <br> **Solution/Fix:** When migrating to .NET 10, users should take into consideration the breaking changes introduced by the .NET 10 runtime: [Breaking changes in .NET 10](https://learn.microsoft.com/dotnet/core/compatibility/10). |
| Feature | Data.Extensions package deprecation and KeyValueTuple obsoletion | **Brief Description:** In Service Fabric 12, the Microsoft.ServiceFabric.Data package will no longer have a dependency on Microsoft.ServiceFabric.Data.Extensions. With the exception of KeyValueTuple<T>, Microsoft.ServiceFabric.Data.Extensions contains only internal implementation details and will no longer be published on NuGet.org. In Service Fabric 11.5, Microsoft.ServiceFabric.ReliableCollection.Store.KeyValueTuple<T> is marked [Obsolete] to provide advance warning before it becomes internal in Service Fabric 12. <br> **Feature/Bug Impact:** Customers upgrading to the Service Fabric 11.5 SDK will receive warning CS0618 if they use KeyValueTuple<T>. Customers upgrading to the Service Fabric 12 SDK will receive compiler errors if they use this type. <br> **Solution/Fix:** Switch from KeyValueTuple<T> to System.Tuple and remove Microsoft.ServiceFabric.Data.Extensions package references. <br> **Documentation Reference:** [Microsoft.ServiceFabric.Data.Extensions 9.0.892-beta](https://www.nuget.org/packages/Microsoft.ServiceFabric.Data.Extensions/9.0.892-beta) <br> **Breaking Change:** While obsoletion/removal of KeyValueTuple<T> is technically a breaking change, the chance of customers taking a dependency on this type is low. <br> **Workaround:** N/A |
| Feature | Backup metadata validation with SHA256 | **Brief Description:** Service Fabric Backup Restore Service (BRS) now supports automated backup metadata validation through SHA256 checksum verification. When enabled through backup policy, the system computes and stores checksums at backup creation time and validates backup integrity by recomputing and comparing checksums, helping detect corruption, incomplete backup chains, or missing metadata. <br> **Feature/Bug Impact:** Without backup metadata validation, incomplete chains or missing metadata can go undetected until restore time. This feature improves cyber resiliency and recovery assurance by proactively validating backups and associated metadata. <br> **Solution/Fix:** Adds an opt-in validation workflow that generates checksums at backup creation, validates checksums during verification, supports incremental backup chain validation, and stores pass/fail validation results in metadata through existing BRS APIs. Validation is controlled through backup policy and can be enabled or disabled per policy. <br> **Documentation Reference:** N/A <br> **Breaking Change:** N/A. This is a new opt-in feature and is disabled by default. Existing backup and restore workflows are unaffected. <br> **Workaround:** N/A |

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

### Service Fabric 11.5.116 Repositories and Download Links

Runtime:
https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabric.11.5.116.1.exe

SDK:
https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabricSDK.8.5.116.msi

Cab:
https://download.microsoft.com/download/b/0/b/b0bccac5-65aa-4be3-ab13-d5ff5890f4b5/11.5.116.1/MicrosoftServiceFabric.11.5.116.1.cab

Package:
https://download.microsoft.com/download/8/3/6/836e3e99-a300-4714-8278-96bc3e8b5528/11.5.116.1/Microsoft.Azure.ServiceFabric.WindowsServer.11.5.116.1.zip
 
Goalstate:
https://download.microsoft.com/download/7/d/1/7d1d1511-59a4-4933-8187-40c20065aa29/11.5.116.1/goalstate.11.5.116.1.json
