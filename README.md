# kubernetes-cluster
My personal kubernetes cluster running on bare metal

## Preparation

- Initialize the control plane with `--pod-network-cidr=192.168.0.0/16`
- Apply the Calico network addon
- Install metal-lb
- Apply metal-lb config (`kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/00-metallb-layer2-config.yml`)

## Fast setup

Run the following

```
$ kubectl create secret generic confluence-db-credentials --from-literal=user=<username> --from-literal=pass=<password>
$ kubectl create secret generic nextcloud-db-credentials --from-literal=user=<username> --from-literal=pass=<password>
$ kubectl create secret generic nextcloud-admin-credentials --from-literal=user=<username> --from-literal=pass=<password>
$ kubectl create secret generic postgres-admin-credentials --from-literal=user=<username> --from-literal=pass=<password>
$ kubectl create secret generic traefik-admin-credentials --from-file=<htpassword_secret_file>
$ kubectl create secret generic plex-claim --from-literal=claim=<plex_claim>
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/00-crd-rbac.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/01-services.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/01-storage.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/02-deployments.yml
```

Make sure the postgres deployment completes before continuing

```
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/03-confluence-deployment.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/03-nextcloud-deployment.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/03-plex-deployment.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/03-samba-deployment.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/04-middleware-auth.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/04-middleware-compress.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/04-middleware-headers.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/04-middleware-redirect-scheme.yml
$ kubectl apply -f https://raw.githubusercontent.com/dotslashme/kubernetes-cluster/master/09-ingresses.yml
$ cat /dev/null > ~/.bash_history && history -c && exit
```