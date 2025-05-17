
# 🔐 Kubernetes User Access Setup Guide

This guide documents the step-by-step process of configuring secure, limited access for a user (`joy`) in a Kubernetes cluster using TLS client certificates and RBAC.

---

## 📋 Workflow Overview

We will go through the following steps in order:

1. **Generate TLS certificates** for secure authentication
2. **Create a custom kubeconfig** for the user `joy`
3. **Create a namespace, role, and rolebinding** using RBAC
4. **Test access** to ensure permissions are correctly enforced

---

## 📁 Guide Index

| Step | Description                             | Link                                                     |
| ---- | --------------------------------------- | -------------------------------------------------------- |
| 1️⃣  | Generate User Certificates              | [User Certificate Creation](./docs/Creating_a_User_Certificate.md) |
| 2️⃣  | Create a Custom Kubeconfig              | [Custom Kubeconfig Setup](./docs/kubeconfig_Manual_Setup_Guide.md)         |
| 3️⃣  | Create Namespace, Role, and RoleBinding | [RBAC Configuration](./docs/Granting_Role_Based_Access.md)                    |
| 4️⃣  | Test Access Permissions                 | [Access Verification](./docs/Access_Verification.md)                  |

---

## ✅ Prerequisites

Make sure you have the following before proceeding:

* Admin access to your Kubernetes cluster
* `kubectl` and `openssl` installed on your local machine
* Access to the Kubernetes CA certificate and key (or a way to get them, e.g. from control plane container)

---

## 🎯 Goal

By the end of this guide, the user `joy` will be able to:

* Authenticate to the cluster using their own TLS certificate
* Access **only** the `pods` resource in the `databases` namespace
* Be **denied** access to any other resources outside the defined role
