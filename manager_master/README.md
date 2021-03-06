# Wazuh manager master StatefulSet and base services
This directory contains Kubernetes configurations which runs Wazuh manager master Pod as a [`StatefulSet`](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) using storage provisioned with a [`StorageClass`](https://kubernetes.io/docs/concepts/storage/storage-classes/).

This directory also contains two services that should be exposed internally in your AWS VPC:
* The `wazuh` service which exposes the Wazuh API of the master node
* The `wazuh-manager` service which exposes the Wazuh managers agents management ports

## Before deploying
Make sure you deployed everything from the [base](../base) folder before deploying this.

Then, you will need to update the `domainName` annotation value in both the [wazuh-api-svc.yaml](wazuh-api-svc.yaml) and the [wazuh-manager-svc.yaml](wazuh-manager-svc.yaml) files before deploying those services.

You should also set a valid AWS ACM certificate ARN in the [wazuh-api-svc.yaml](wazuh-api-svc.yaml) for the `service.beta.kubernetes.io/aws-load-balancer-ssl-cert` annotation. That certificate should match with the `domainName`.

## Deploy
```BASH
kubectl apply -f wazuh-manager-master-conf.yaml
kubectl apply -f wazuh-manager-master-sts-svc.yaml
kubectl apply -f wazuh-manager-master-sts.yaml
kubectl apply -f wazuh-api-svc.yaml
kubectl apply -f wazuh-manager-svc.yaml
```
