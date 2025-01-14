---
title: Bring your own key (customer managed keys)
description: You can use a customer managed key (that is, bring your own key) with Media Services.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: conceptual
ms.date: 1/28/2020
---

# Bring your own key (customer-managed keys) with Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bring Your Own Key (BYOK) is an Azure wide initiative to help customers move their workloads to the cloud. Customer managed keys allow customers to adhere to industry compliance regulations and improves tenant isolation of a service. Giving customers control of encryption keys is a way to minimize unnecessary access and control and build confidence in Microsoft services.

## Keys and key management

You can use your own key with Media Services when you use the Media Services 2020-05-01 or later API. A default account key is created for all accounts which is encrypted by a system key owned by Media Services. When you use your own key, the account key is encrypted with your key. Content keys are encrypted by the account key. JobInputHttp urls and symmetric token validation keys are also encrypted.

:::image type="content" source="./media/customer-managed-key/customer-managed-key.svg" alt-text="A customer managed key replaces a system managed key":::

Media Services uses the Managed Identity of the Media Services account to read your key from a Key Vault owned by you. Media Services requires that the Key Vault is in the same region as the account, and that it has soft-delete and purge protection enabled.

Your key can be a 2048, 3072, or a 4096 RSA key, and both HSM and software keys are supported.

> [!NOTE]
> EC keys are not supported.

You can specify a key name and key version, or just a key name. When you use only a key name, Media Services will use the latest key version. New versions of customer keys are automatically detected, and the account key is re-encrypted.

> [!WARNING]
> Media Services monitors access to the customer key. If the customer key becomes inaccessible (for example, the key has been deleted or the Key Vault has been deleted or the access grant has been removed), Media Services will transition the account to the Customer Key Inaccessible State (effectively disabling the account). However, the account can be deleted in this state. The only supported operations are account GET, LIST and DELETE; all other requests (encoding, streaming, and so on) will fail until access to the account key is restored.

## Double encryption

Media Services automatically supports double encryption. For data at rest, the first layer of encryption uses a customer managed key or a Microsoft managed key depending on the `AccountEncryption` setting on the account.  The second layer of encryption for data at rest is provided automatically using a separate Microsoft managed key. To learn more about double encryption, see [Azure double encryption](../../security/fundamentals/double-encryption.md).

> [!NOTE]
> Double encryption is enabled automatically on the Media Services account. However, you need to configure the customer managed key and double encryption on your storage account separately. To learn more, see [Storage encryption](../../storage/common/storage-service-encryption.md).

## Tutorials

- [Use the Azure portal to use customer-managed keys or BYOK with Media Services](security-customer-managed-keys-portal-tutorial.md)
- [Use customer-managed keys or BYOK with Media Services REST API](security-customer-managed-keys-rest-postman-tutorial.md).

## Next steps

[Protect your content with Media Services dynamic encryption](drm-content-protection-concept.md)
