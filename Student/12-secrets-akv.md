# Challenge 12: Secrets & Azure Key Vault

[< Previous Challenge](11-secrets.md) - **[Home](../README.md)** - [Next Challenge >](13-PrivateAKS.md)

## Refactor your delpoyments to use Azure Key Vault.
Your CISO is happy with your great work so far, but she is still concerned about managing the credentials in a Kubernetes Secret.  She now asks you to place all secrets in an Azure Key Vault.

For this challenge, refactor your deployments to store the secrets in an Azure Key Vault.

## Success Criteria
- No static passwords or credentials are stored anywhere in Kubernetes or in any manifests
- All credentials must be stored in Azure Key Vault
  - _Hint: Pod Identity & CSI Secrets Store Driver are required to achieve the required results._

## Learning Resources

These docs may help you achieving these objectives:

- [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/basic-concepts)
- [AKV provider for secrets store CSI driver](https://docs.microsoft.com/en-us/azure/aks/csi-secrets-store-driver)
- [Pod Identity](https://github.com/Azure/aad-pod-identity)
