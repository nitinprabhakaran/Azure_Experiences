# CONTEXT

This directory is about making use of Azure Container Instances (ACI) to provision and attach ACI Instance to a running AKS Clsuter Control Plane which would function as a Kubelet and would perform insterations with the control plane in the similar way as other nodes do.

# PRE_REQUISITES

- Virtual Nodes feature should be enabled
- Kubernetes Cluster should be functioning on Azure CNI (Container Network Interface) Networking instead of Kubenet or any Custom ones
- Virtual Node(s) will be placed on a different subnet, apart from the actual nodes mapped to AKS
- Since the ACI Instances are on a different subnet, if there are mixed deployemts, mappings to redirect requests to a section of pods in AKS default node pools will have to be done via FQDN of the Pod or Deployment
- The below section should be added in Deployment for Azure AKS to schedule the pod on Azure Virtual Nodes
- Review the manifests


```yaml
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

## References
- [Azure Virtual Nodes - Limitations](https://docs.microsoft.com/en-us/azure/aks/virtual-nodes-cli#known-limitations)
- [Virtual kubelet - Referece - 1](https://github.com/virtual-kubelet/virtual-kubelet)
- [Virtual kubelet - Referece - 2](https://github.com/virtual-kubelet/azure-aci/blob/master/README.md)
- [Virtual Node Autoscale - Optional & legacy](https://github.com/Azure-Samples/virtual-node-autoscale)

