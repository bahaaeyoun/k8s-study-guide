[Go Home](../README.md) | 
[Next Step](./kubeconfig_Manual_Setup_Guide.md)

# Creating a User Certificate for Kubernetes Access

## 1. Generate a Private Key

Create a private key for the user `joy`:

```bash
openssl genrsa -out joy.key 2048
```

## 2. Create a Certificate Signing Request (CSR)

Use the private key to create a CSR for the user:

```bash
openssl req -new -key joy.key -out joy.csr -subj "/CN=joy/O=databases"
```

* `CN`: Common Name (user's name)
* `O`: Organization (used for group-based role bindings)

## 3. Access the Kubernetes Master Node

SSH into the master node (example using Docker):

```bash
docker exec -it 3eb597b4db8e bash
```

## 4. Verify Cluster Certificate Files

Ensure the cluster's certificate files exist:

```bash
ls -ll /etc/kubernetes/pki/
```

## 5. Copy Cluster Certificates to Host Machine

Exit the master node container and copy the CA certificate and key to your host machine:

```bash
docker cp k8s-playground-control-plane:/etc/kubernetes/pki/ca.crt .
docker cp k8s-playground-control-plane:/etc/kubernetes/pki/ca.key .
```

## 6. Generate a User Certificate

Use the CSR, the cluster's CA certificate and key to generate a signed certificate for the user:

```bash
openssl x509 -req -in joy.csr \
  -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out joy.crt -days 1000
```

This certificate is valid for 1000 days and can now be used in a kubeconfig file to authenticate the user `joy`.

