# Trusted Container Policy Controller
## Background

This is part of Policy Based Governance in Trusted Container Platform. For more information, read https://www.nccoe.nist.gov/news/policy-based-governance-trusted-container-platform

## What does it do ?

Check if pods are using images built from specified registry

```yaml
apiVersion: policies.ibm.com/v1alpha1
kind: TrustedContainerPolicy
metadata:
  name: example-trustedcontainerpolicy
spec:
  severity: low
  namespaceSelector:
    include: ["default"]
    exclude: ["kube-system"]
  remediationAction: inform
  imageRegistry: quay.io
```

## To run it on a standalone kubernetes cluster
1. Configure `kubectl` to point to a kubernetes cluster
2. Run following command to apply `trustedcontainerpolicies.policies.ibm.com` CRD
```
kubectl apply -f deploy/crds/policies.ibm.com_trustedcontainerpolicies_crd.yaml
```
3. Run following command to update clusterrolebinding required by `Trusted Container Policy Controller`. Replace `<namespace>` in the command with the namespace where the controller is going to be deployed.
```
sed -i "" 's|namespace: default|namespace: <namespace>|g' deploy/cluster_role_binding.yaml
```
4. Run following command to deploy `Trusted Container Policy Controller`
```
kubectl apply -f deploy/
```
5. Run following command to create a sample trusted container policy
```
kubectl apply -f deploy/crds/policies.ibm.com_v1alpha1_trustedcontainerpolicy_cr.yaml
```
6. Run following command to generate a violation. It will create a pod in `default` namespace on your cluster
```
kubectl apply -f deploy/crds/pod-nginx.yaml
```

## To run it with IBM Multicloud Manager
1. Repeat step 1 to 4 on the managed cluster. Make sure you deploy them to cluster namespace. The namespace name is usually your cluster name
2. Run following command to create a MCM policy on hub cluster
```
kubectl apply -f deploy/crds/mcm-trustedcontainerpolicy.yaml
```
3. Run step 6 on managed cluster to generate a violation
4. Then you should be able to see the policy and violation status on MCM console
