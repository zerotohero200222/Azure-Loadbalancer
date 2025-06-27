# Azure-Loadbalancer
### ✅ `README.md`

```markdown
# 🚀 Azure Load Balancer Deployment using Terraform and GitHub Actions

This project deploys an Azure infrastructure that includes:

- ✅ A resource group
- ✅ A virtual network and subnet
- ✅ A network security group (NSG)
- ✅ Two Linux virtual machines (VMs) with Nginx installed
- ✅ A Standard Public Load Balancer with backend pool and health probes
- ✅ A GitHub Actions pipeline for CI/CD

---

## 📂 Project Structure

```

.
├── main.tf                       # Terraform resources
├── providers.tf                 # Provider configuration
├── variables.tf                 # Input variables
├── outputs.tf                   # Output values
└── .github/
└── workflows/
└── terraform-deploy.yml # GitHub Actions workflow

````

---

## 🧰 Prerequisites

- Azure Subscription
- Azure CLI
- Terraform >= 1.3.x
- GitHub account with a new repo
- Service Principal with Contributor role on your subscription

---

## 🔐 Azure Service Principal Setup

Run the following command to create a service principal:

```bash
az ad sp create-for-rbac --role="Contributor" \
  --scopes="/subscriptions/<YOUR_SUBSCRIPTION_ID>" \
  --sdk-auth
````

> Copy the output values. You'll use them as **GitHub Secrets**.

---

## 🔐 Configure GitHub Secrets

Go to your GitHub repo:

`Settings → Secrets and variables → Actions → New repository secret`

Create the following secrets:

| Secret Name           | Value (from `az ad sp` command) |
| --------------------- | ------------------------------- |
| `ARM_CLIENT_ID`       | `clientId`                      |
| `ARM_CLIENT_SECRET`   | `clientSecret`                  |
| `ARM_SUBSCRIPTION_ID` | `subscriptionId`                |
| `ARM_TENANT_ID`       | `tenantId`                      |

---

## 🚀 Deploying the Infrastructure

1. **Push this code to your GitHub repository.**
2. GitHub Actions will automatically:

   * Run `terraform init`
   * Run `terraform plan`
   * Run `terraform apply` on the `main` branch

To trigger deployment manually, you can edit and push any file.

---

## 🌐 Accessing the Load Balancer

Once deployment completes, check the `Actions` tab in GitHub and find the `terraform apply` logs. The output will contain:

```
Outputs:

public_ip_address = "http://<YOUR_PUBLIC_IP>"
```

Open this IP address in your browser. You should see:

```
Hello World from <vm-hostname>
```

Each refresh should toggle between the two VMs via the Load Balancer.

---

## 📤 Output Variables

| Name                  | Description                                |
| --------------------- | ------------------------------------------ |
| `public_ip_address`   | Public IP of the Azure Load Balancer       |
| `resource_group_name` | Auto-generated name of the resource group  |
| `vm_password`         | Password of the first Linux VM (sensitive) |

---

## 🧹 Cleanup

To destroy all resources:

```bash
terraform destroy -auto-approve
```

> Ensure you run this in the same folder where `.tf` files are located.

---

## 📌 Notes

* The Load Balancer uses port `80` and routes traffic to Nginx on both VMs.
* Each VM auto-installs Nginx and creates a basic HTML response on startup.
* You can customize the VM size and location in `variables.tf`.

---

## 📘 References

* [Terraform Azure Provider](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)
* [GitHub Actions for Terraform](https://github.com/hashicorp/setup-terraform)
* [Azure Load Balancer Docs](https://learn.microsoft.com/en-us/azure/load-balancer/)

---

