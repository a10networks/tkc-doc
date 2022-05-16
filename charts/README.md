# TLC Helm charts

Helm Charts for Thunder Kubernetes Controller (i.e. TKC)

This Project maintain TKC Helm Charts packages for Helm Chart repo URL.
For example:
```
helm repo add <name-you-want> https://a10networks.github.io/tkc-doc/charts/
```

After adding the tkc-doc repo, You can use following command to see the list of all helm charts from tkc-doc repository
```
helm search repo 
```

To downlaod the helm chart locally, use the following command 
```
helm pull <repository name>/<chart name>
```
To get repository name and chart name, tou cam use helm search repo command

## Helm Charts

```
| Directory               | TKC version   | Description                                       |
| tkc-414-support         | v1.11+        | Deploy TKC with 4.1.4-GR1 ACOS device             |
| tkc-crd-v1.1            | v1.11+        | Deploy TKC with Custom Resource Definition (CRDs) |
| tkc-ip-unnumbered-v1.0  | v1.11+        | Deploy TKC with IP-UnNumnbered overlay-tunnel     |
```
