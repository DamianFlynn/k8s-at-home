# Research

## Repos

https://github.com/bjw-s/home-ops/tree/main/kubernetes/base
https://bjw-s.github.io/home-ops/networking/dns/#coredns-with-k8s_gateway

## Actions

### SED and AWK

https://unix.stackexchange.com/questions/647935/match-a-pattern-and-replace-the-value-of-the-following-key-value-pair
https://stackoverflow.com/questions/5955548/how-do-i-use-sed-to-change-my-configuration-files-with-flexible-keys-and-values

## Terraform

https://github.com/orgs/claranet/repositories?q=azure&type=all&language=&sort=
https://schnerring.net/blog/automate-building-custom-windows-images-for-azure-virtual-desktop-with-packer-and-github-actions/

## HASS

https://github.com/robbrad/HA-Config


## HUGO

Discuss on Twitter - https://pakstech.com/blog/pre-commit-anywhere/


export POD_NAME=$(kubectl get pods -n default -l "app.kubernetes.io/name=kubernetes-dashboard,app.kubernetes.io/instance=kubernetes-dashboard" -o jsonpath="{.items[0].metadata.name}")
echo https://127.0.0.1:8443/
kubectl -n default port-forward $POD_NAME 8443:8443