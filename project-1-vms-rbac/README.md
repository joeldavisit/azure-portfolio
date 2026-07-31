# Project 1: Virtual Machines & RBAC

## What This Project Does
This project deploys an Azure VM and secures it using role-based access control,
governance policy, and disk encryption — demonstrating minimum-privilege access
control and enforcement of organizational standards in a real Azure environment.

## Services Used
- Azure Virtual Machines
- Microsoft Entra ID
- Azure RBAC (custom role)
- Azure Policy
- Azure Disk Encryption
- Azure Key Vault
- Cost Management + Billing

## Steps Taken
1. Deployed a virtual machine (size: D2ls_v6, 2 vCPU / 4GB RAM) in East US
2. Created a test user in Microsoft Entra ID (`vmadmintest`)
3. Created a custom RBAC role with minimum required permissions (read, start,
restart — explicitly excluding delete)
4. Assigned the custom role to the test user, scoped specifically to the VM
resource rather than the resource group or subscription
5. Assigned an Azure Policy to the resource group requiring a specific tag
on all resources
6. Enabled Azure Disk Encryption on the VM's OS disk, backed by an Azure Key Vault
7. Configured a subscription-level cost alert to monitor spend

## Troubleshooting Notes
**Issue 1: VM size unavailable in region**
Attempted to deploy using B1s (free-tier eligible), which was unavailable in
East US for this subscription. B1ms and B2s returned the same result. Resolved
by selecting D2ls_v6 instead — a non-burstable size that still met the
project's compute needs but does not fall under free-tier hours. Mitigated
added cost by deallocating the VM when not in active use.

**Issue 2: Disk encryption blocked by policy**
Enabling Azure Disk Encryption initially failed with a "disallowed by policy"
error. This was caused by the tagging policy assigned earlier in the project —
the disk (and/or the Key Vault being created for encryption) did not yet have
the required tag applied. Resolved by adding the required tag to the resource,
which allowed the policy to pass and encryption to complete successfully. This
confirmed the policy was actively enforcing governance as intended, rather than
just being a passive assignment.

## Screenshots attached below

**Role assignment confirmation:**


**Policy assignment:**


**Disk encryption status:**


## What I Learned
Configuring RBAC, policy, and encryption together, rather than reading about
them separately I surfaced real interactions between these services that
aren't obvious from documentation alone. In particular, seeing a governance
policy actually block an operation (rather than just existing as a setting)
made clear how tightly policy enforcement integrates with everyday resource
management in Azure.