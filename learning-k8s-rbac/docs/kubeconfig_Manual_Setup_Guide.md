[Go Home](../README.md) | 
[Previous Step](./Creating_a_User_Certificate.md) | 
[Next Step](./Granting_Role_Based_Access.md)


# Kubernetes kubeconfig Manual Setup Guide

This guide walks you through manually configuring a kubeconfig file to connect to a Kubernetes cluster using certificates and context settings.

---

## 1. Get Cluster IP and Cluster Name

- **Cluster Name**: Obtain from your Kubernetes provider or admin or from the following command.

  ```bash
  kubectl config get-clusters
  ```
  **Expected Output:**

  ```
  NAME
  kind-k8s-playground
  ```

- **Cluster API Server IP**: Usually the Kubernetes control plane IP or DNS.


  Example to find the API server URL if you have `kubectl` access:

  ```bash
  kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}'
  ```
  **Expected Output:**

  ```
  https://127.0.0.1:45301
  ```

  OR

  ```bash
  kubectl cluster-info
  ```
  **Expected Output:**

  ```
  Kubernetes control plane is running at https://127.0.0.1:45301
  CoreDNS is running at https://127.0.0.1:45301/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

  To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
  ```

---

## 2. Prepare Certificates and Keys

Ensure you have the following files ready:

* `ca.crt` — Certificate Authority file
* `client.crt` — Client Certificate file
* `client.key` — Client Private Key file

---

## 3. Set Cluster Configuration

Set the cluster with embedded CA certificate:

```bash
kubectl config set-cluster <cluster-name> \
  --server=https://<cluster-ip>:<cluster-port> \
  --certificate-authority=/path/to/ca.crt \
  --embed-certs=true
```

Replace `<cluster-name>`, `<cluster-ip>`, `<cluster-port> `, and path accordingly.

---

## 4. Set User Credentials

Set the user with embedded client certificates:

```bash
kubectl config set-credentials <user-name> \
  --client-certificate=/path/to/client.crt \
  --client-key=/path/to/client.key \
  --embed-certs=true
```

Replace `<user-name>` and file paths accordingly.

---

## 5. Set Context

Create a context that links the cluster and user, and sets the default namespace:

```bash
kubectl config set-context <context-name> \
  --cluster=<cluster-name> \
  --user=<user-name> \
  --namespace=default
```

---

## 6. Use the New Context

Switch to the newly created context:

```bash
kubectl config use-context <context-name>
```


---

## 7. Verify Your Configuration

After setting up the user certificates and kubeconfig, it's important to verify that everything is working correctly. The following steps help ensure that authentication and context are configured properly.

---

## 8. Check Cluster Context (Optional for Admin)

To verify your current context as an admin (not the joy user):

```bash
kubectl config current-context
```

```bash
kubectl cluster-info
```

---

## 9. Verify User Authentication

Use the user's kubeconfig to ensure that authentication is successful and the cluster is reachable:

```bash
kubectl --kubeconfig=joy.config version --short
```

**Expected Output:**

```
Client Version: v1.xx.x
Server Version: v1.xx.x
```

> ✅ If both versions are printed, this confirms that the `joy` user is successfully authenticated against the cluster.


---

## 10. Optional: View Your kubeconfig File

To review the full configuration file:

```bash
cat ~/.kube/config
```

---

## 11. Example kubeconfig snippet (for reference)

```yaml
apiVersion: v1
kind: Config
clusters:
- name: my-cluster
  cluster:
    server: https://1.2.3.4:6443
    certificate-authority-data: <base64-encoded-ca-cert>
users:
- name: my-user
  user:
    client-certificate-data: <base64-encoded-client-cert>
    client-key-data: <base64-encoded-client-key>
contexts:
- name: my-context
  context:
    cluster: my-cluster
    user: my-user
    namespace: default
current-context: my-context
```

---
*End of guide.*
