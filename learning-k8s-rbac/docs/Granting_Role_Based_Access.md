[Go Home](../README.md) | 
[Previous Step](./Creating_a_User_Certificate.md) |
[Next Step](./Access_Verification.md)

# Granting Role-Based Access in Kubernetes

## 1. Create a Namespace

First, create a namespace where access will be configured:

```bash
kubectl create ns databases
```

## 2. Create a Role

Now, create a role that allows specific actions on certain resources.

* `--verb`: Actions the user is allowed to perform.
* `--resource`: Kubernetes resources the user is allowed to interact with.

```bash
kubectl --namespace=databases create role database-managers \
  --verb=get,list,create,delete \
  --resource=pods
```

## 3. Create a RoleBinding

Bind the user to the role:

* `--user`: Specifies the user to bind.
* `--role`: The previously created role.

```bash
kubectl --namespace=databases create rolebinding database-managers-binding \
  --user=joy \
  --role=database-managers
```


