# Microsoft Azure Service Fabric 10.1 Cumulative Update 4.0 Release Notes

This release will only be available through manual upgrades. Clusters set to automatic upgrades will not receive this release. For how to configure upgrades, please see [classic](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-upgrade) or [managed](https://docs.microsoft.com/en-us/azure/service-fabric/how-to-managed-cluster-configuration) documentation.

## Contents
* [Service Fabric Packages and Versions](#service-fabric-packages-and-versions)
* [Service Fabric Feature and Bug Fixes](#service-fabric-feature-and-bug-fixes)
* [Retirement and Deprecation Path Callouts](#retirement-and-deprecation-path-callouts)
* [Repositories and Download Links](#repositories-and-download-links)

## Service Fabric Packages and Versions
The following packages and versions are part of this release:

| Service | Platform | Version |
|-|-|-|
| [Service Fabric Runtime](https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabric.10.1.2338.9590.exe) | Ubuntu 20 <br> Windows | 10.1.2137.1 <br> 10.1.2338.9590 |
| [Service Fabric for Windows Server](https://download.microsoft.com/download/8/3/6/836E3E99-A300-4714-8278-96BC3E8B5528/10.1.2338.9590/Microsoft.Azure.ServiceFabric.WindowsServer.10.1.2338.9590.zip) | Service Fabric Standalone Installer Package | 10.1.2338.9590 |
| [.NET SDK](https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabricSDK.7.1.2338.msi) | Windows .NET SDK <br> Microsoft.ServiceFabric <br> Reliable Services and Reliable Actors <br> ASP.NET Core Service Fabric integration | 7.1.2175 <br> 10.1.2175 <br> 7.1.2175 <br> 7.1.2175 |
| [Java SDK](https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabricSDK.7.1.2338.msi) | Java for Linux SDK | 1.0.6 |
| Service Fabric PowerShell and CLI | AzureRM PowerShell Module <br> SFCTL | 0.3.15 <br> 11.0.1 |

## Service Fabric Feature and Bug Fixes
| Versions | IssueType | Description | Resolution |
|-|-|-|-|
| Windows -<br>10.1.2338.9590 <br> Ubuntu 20 -<br>10.0.2137.1 | Feature | OrdinalString | **Brief Description:** Allow RC with string key to switch comparison behavior from CurrentCulture to Ordinal by internal configurations.<br>**Feature/Bug Impact:** Feature is turned off by default. If turned on, it will impact all string key RC of the specified service.<br>**Solution/Fix:** If this feature is turned on, it will update Key Checkpoint Files if necessary to maintain invariance that key checkpoint file is ordered by the key comparison order.<br>**Workaround (if applicable):** Customer needs to do in place data migration to utilize OrdinalString. |
| Windows -<br>10.1.2338.9590 <br> Ubuntu 20 -<br>10.0.2137.1 | Bug | Assembly | **Brief Description:** Previously, type interpretation depends on assembly name. This is blocking application upgrade from .NET 5+ to .NET Framework. Also, this prevents user's type from changing assembly. This change allows the type to be interpreted without relying on assembly name.<br>**Feature/Bug Impact:** This bug was impacting all managed RC customers. Specifically, it is causing the customer not able to upgrade from .NET Framework to .NET 5+ reliably since it could not roll back from .NET 5+ to .NET Framework without this fix.<br>**Solution/Fix:** When deserializing type, will attempt to deserialize without assembly if deserialize with strong type and deserialize with assembly failed.<br>**Workaround (if applicable):** Currently, the customer could add type forwarding logic within application in order for .NET Framework to recognize .NET 5+ assembly. |
| Windows -<br>10.1.2338.9590 <br> Ubuntu 20 -<br>10.0.2137.1 | Feature | ReplicatedStore | **Brief Description:** Deprecate the internal ReplicatedStore settings FabricTimePersistInterval and FabricTimeRefreshTimeoutValue. They were used to implement the internal IReplicatedStore::GetCurrentStoreTime() API. This API was never consumed by Service Fabric and no longer needed. |
| Windows -<br>10.1.2338.9590 <br> Ubuntu 20 -<br>10.0.2137.1 | Feature | Internal trace | **Brief Description:** Fix SF internal trace description<br>**Feature/Bug Impact:** Impacted internal service fabric trace output<br>**Solution/Fix:** Remove the default reason for throttling write, corrected InvalidAPIOnAtomicOperation description |

## Retirement and Deprecation Path Callouts

* Service Fabric runtime will discontinue support for the Java SDK soon. For a smooth transition, we strongly recommend users to shift to Azure Service Fabric .NET SDK. If your current setup is based on the Service Fabric Java SDK, we suggest starting migration plans to smoothly switch to the Azure Service Fabric .NET SDK. Although applications using the Java SDK will continue to work, we highly recommend adopting the SF .NET SDK for optimal outcomes. 

* Ubuntu 18.04 LTS has reached its 5-year end-of-life window on June-2023. Service Fabric runtime has dropped support for 18.04 LTS after the published date, and we recommend moving your clusters and applications to supported versions listed here: [Service Fabric supported Linux versions](https://learn.microsoft.com/en-us/azure/service-fabric/service-fabric-versions#supported-linux-versions-and-support-end-date)

* Previously communicated, Service Fabric runtime had planned to remove Service Fabric runtime version 6.4 packages and older, as well as SDK version 3.3 packages and older, from the package Download Center in July 2023. We would like to inform you that this timeline has been extended, and the removal will now take place in January 2024.

* Service Fabric runtime will soon be archiving and removing Service Fabric runtime versions less than 7.2 and older, as well as the corresponding SDK version 4.2 packages and older from the package Download Center. Archiving/Removing will affect application scaling and re-imaging of virtual machines in a Service Fabric Cluster running on unsupported versions. After older versions are removed/archived, this may cause failure while rolling back when the current in-progress upgrade has errors. 
  * To prevent disruption of workloads, create a new cluster using the following steps:
    * [Create a Service Fabric cluster using ARM template](https://learn.microsoft.com/en-us/azure/service-fabric/quickstart-cluster-template)
    * [Create a Standalone cluster](https://learn.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-for-windows-server)
    * Install the supported version of Service Fabric SDK based on the Runtime version installed on the cluster.

## Repositories and Download Links
The table below is an overview of the direct links to the packages associated with this release. 
Follow this guidance for setting up your developer environment: 
* [Getting Started with Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux)
* [Getting Started with Mac](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-mac)
* [Getting Started with Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

Run Time:
https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabric.10.1.2338.9590.exe

SDK:
https://download.microsoft.com/download/b/8/a/b8a2fb98-0ec1-41e5-be98-9d8b5abf7856/MicrosoftServiceFabricSDK.7.1.2338.msi

Cab:
https://download.microsoft.com/download/B/0/B/B0BCCAC5-65AA-4BE3-AB13-D5FF5890F4B5/10.1.2338.9590/MicrosoftAzureServiceFabric.10.1.2338.9590.cab

Package:
https://download.microsoft.com/download/8/3/6/836E3E99-A300-4714-8278-96BC3E8B5528/10.1.2338.9590/Microsoft.Azure.ServiceFabric.WindowsServer.10.1.2338.9590.zip

Goalstate:
https://download.microsoft.com/download/7/D/1/7D1D1511-59A4-4933-8187-40C20065AA29/10.1.2338.9590/goalstate.10.1.2338.9590.json
