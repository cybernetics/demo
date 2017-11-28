## Current Status of Windows Server support for kubernetes on Azure
kubernetes on Windows is **under preview** status.

k8s windows cluster could be created by two ways:

### 1. azure portal
Create "Azure Conatiner Service" (**not** AKS) in azure portal, select `Windows` OS, if k8s cluster is created successfully, the master node would be still Ubuntu OS, agent node would be `Windows Server 2016 DataCenter`.
##### Note: azure disk & azure file mount feature is not enabled on this k8s cluster because it's using `Windows Server 2016 DataCenter` OS, only `Windows Server version 1709` is supported for these two features.

### 2. acs-engine (https://github.com/Azure/acs-engine)
By acs-engine **v0.9.2 or above**, you could deploy a `Windows Server version 1709` (also called `RS3`) based k8s cluster which would support azure disk & azure file mount feature on Windows node. 

You could check Windows version by following command, below is an example using `Windows Server version 1709`:
```
kubectl exec -it WINDOWS-PODNAME -- cmd
C:\Users\azureuser>ver
Microsoft Windows [Version 10.0.16299.19]
```

##### Note
1. **breaking change** for Windows container running on `Windows Server version 1709`, only image tag with `1709` keyword could run on `Windows Server version 1709`, e.g.
```
microsoft/aspnet:4.7.1-windowsservercore-1709
microsoft/windowsservercore:1709
microsoft/iis:windowsservercore-1709
```

You may get following error if you try to run a legacy image on `Windows Server version 1709`
```
C:\k>docker run -d --name iis microsoft/iis
b08d4e031b8203446aedf7cc81ea110ac55009293ded373dfab2271505f6ee75
docker: Error response from daemon: container b08d4e031b8203446aedf7cc81ea110ac55009293ded373dfab2271505f6ee75 encountered an error during CreateContainer: failure in a Windows system call: The operating system of the container does not match the operating system of the host. (0xc0370101) extra info:
...
```

2. "Azure Conatiner Service - AKS" does not support Windows yet, it is in progress.

###### Links
About `Windows Server version 1709`: https://docs.microsoft.com/en-us/windows-server/get-started/whats-new-in-windows-server-1709