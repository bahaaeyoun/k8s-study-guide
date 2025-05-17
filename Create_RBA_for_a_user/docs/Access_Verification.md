[Go Home](../README.md) | 
[Previous Step](./Granting_Role_Based_Access.md)

# ✅ Access Verification

After configuring the kubeconfig, generating the certificates, and applying RBAC, we now verify that the user `joy` has the correct level of access to the Kubernetes cluster.

---

## 🔍 Step 4: Test User Access

Use the user’s custom kubeconfig file to check access to the allowed resources.

### ✅ Access to Allowed Resource (`pods`)

```bash
kubectl --kubeconfig=joy.config --namespace=databases get pods
```

**Expected Output** (if no pods exist in the namespace):

```
No resources found in databases namespace.
```

This confirms the user has permission to list pods in the `databases` namespace.

---

### ❌ Attempt Access to Unauthorized Resource (`services`)

```bash
kubectl --kubeconfig=joy.config --namespace=databases get services
```

**Expected Output:**

```
Error from server (Forbidden): services is forbidden: 
User "joy" cannot list resource "services" in API group "" in the namespace "databases"
```

This confirms that the RBAC rules are working as intended, and the user is restricted from accessing resources beyond what is defined in their role.

---

## 🧪 Optional: Test Other Commands

Try other `kubectl` commands (e.g. `create`, `delete`, `describe`) on the allowed resource (`pods`) to validate full access per the defined verbs.
