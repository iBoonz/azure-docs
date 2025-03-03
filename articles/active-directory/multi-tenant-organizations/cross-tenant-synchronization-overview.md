---
title: What is a cross-tenant synchronization in Azure Active Directory? (preview)
description: Learn about cross-tenant synchronization in Azure Active Directory.
services: active-directory
author: rolyon
manager: amycolannino
ms.service: active-directory
ms.workload: identity
ms.subservice: multi-tenant-organizations
ms.topic: overview
ms.date: 01/23/2023
ms.author: rolyon
ms.custom: it-pro

#Customer intent: As a dev, devops, or it admin, I want to
---

# What is cross-tenant synchronization? (preview)

> [!IMPORTANT]
> Cross-tenant synchronization is currently in PREVIEW.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

*Cross-tenant synchronization* automates creating, updating, and deleting [Azure AD B2B collaboration](../external-identities/what-is-b2b.md) users across tenants in an organization. It enables users to access applications and collaborate across tenants, while still allowing the organization to evolve. 

Here are the primary goals of cross-tenant synchronization:

- Seamless collaboration for a multi-tenant organization
- Automate lifecycle management of B2B collaboration users in a multi-tenant organization
- Automatically remove B2B accounts when a user leaves the organization 

## Why use cross-tenant synchronization?

Cross-tenant synchronization automates creating, updating, and deleting B2B collaboration users. Users created with cross-tenant synchronization are able to access both Microsoft applications (such as Teams and SharePoint) and non-Microsoft applications (such as [ServiceNow](../saas-apps/servicenow-provisioning-tutorial.md), [Adobe](../saas-apps/adobe-identity-management-provisioning-tutorial.md), and many more), regardless of which tenant the apps are integrated with. These users continue to benefit from the security capabilities in Azure AD, such as [Azure AD Conditional Access](../conditional-access/overview.md) and [cross-tenant access settings](../external-identities/cross-tenant-access-overview.md), and can be governed through features such as [Azure AD entitlement management](../governance/entitlement-management-overview.md).

The following diagram shows how you can use cross-tenant synchronization to enable users to access applications across tenants in your organization.

:::image type="content" source="./media/cross-tenant-synchronization-overview/cross-tenant-synchronization-diagram.png" alt-text="Diagram that shows synchronization of users for multiple tenants." lightbox="./media/cross-tenant-synchronization-overview/cross-tenant-synchronization-diagram.png":::

## Who should use?

- Organizations that own multiple Azure AD tenants and want to streamline intra-organization cross-tenant application access.
- Cross-tenant synchronization is **not** currently suitable for use across organizational boundaries.

## Benefits

With cross-tenant synchronization, you can do the following:

- Automatically create B2B collaboration users within your organization and provide them access to the applications they need, without creating and maintaining custom scripts.
- Improve the user experience and ensure that users can access resources, without receiving an invitation email and having to accept a consent prompt in each tenant.
- Automatically update users and remove them when they leave the organization.

## Teams and Microsoft 365

Users created by cross-tenant synchronization will have the same experience when accessing Microsoft Teams and other Microsoft 365 services as B2B collaboration users created through a manual invitation. The [userType](../external-identities/user-properties.md) property on the B2B user, whether guest or member, does change the end user experience. Over time, the member userType will be used by the various Microsoft 365 services to provide differentiated end user experiences for users in a multi-tenant organization. 

## Properties

When you configure cross-tenant synchronization, you define a trust relationship between a source tenant and a target tenant. Cross-tenant synchronization has the following properties:

- Based on the Azure AD provisioning engine.
- Is a push process from the source tenant, not a pull process from the target tenant.
- Supports pushing only internal members from the source tenant. It doesn't support syncing external users from the source tenant.
- Users in scope for synchronization are configured in the source tenant.
- Attribute mapping is configured in the source tenant.
- Extension attributes are supported.
- Target tenant administrators can stop a synchronization at any time.

The following table shows the parts of cross-tenant synchronization and which tenant they're configured.

| Tenant | Cross-tenant<br/>access settings | Automatic redemption | Sync settings<br/>configuration | Users in scope |
| :---: | :---: | :---: | :---: | :---: |
| ![Icon for the source tenant.](./media/common/icon-tenant-source.png)<br/>Source tenant |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| ![Icon for the target tenant.](./media/common/icon-tenant-target.png)<br/>Target tenant | :heavy_check_mark: | :heavy_check_mark: |  |  |

## Cross-tenant synchronization setting

[!INCLUDE [cross-tenant-synchronization-include](../includes/cross-tenant-synchronization-include.md)]

To configure this setting using Microsoft Graph, see the [Update crossTenantIdentitySyncPolicyPartner](/graph/api/crosstenantidentitysyncpolicypartner-update?view=graph-rest-beta&preserve-view=true) API. For more information, see [Configure cross-tenant synchronization](cross-tenant-synchronization-configure.md).

## Automatic redemption setting

[!INCLUDE [automatic-redemption-include](../includes/automatic-redemption-include.md)]

To configure this setting using Microsoft Graph, see the [Update crossTenantAccessPolicyConfigurationPartner](/graph/api/crosstenantaccesspolicyconfigurationpartner-update?view=graph-rest-beta&preserve-view=true) API. For more information, see [Configure cross-tenant synchronization](cross-tenant-synchronization-configure.md).

#### How do users know what tenants they belong to?

For cross-tenant synchronization, users don't receive an email or have to accept a consent prompt. If users want to see what tenants they belong to, they can open their [My Account](https://support.microsoft.com/account-billing/my-account-portal-for-work-or-school-accounts-eab41bfe-3b9e-441e-82be-1f6e568d65fd) page and select **Organizations**. In the Azure portal, users can open their [Azure portal settings](../../azure-portal/set-preferences.md), view their **Directories + subscriptions**, and switch directories.

For more information, including privacy information, see [Leave an organization as an external user](../external-identities/leave-the-organization.md).

## Get started

Here are the basic steps to get started using cross-tenant synchronization.

#### Step 1: Define how to structure the tenants in your organization

Cross-tenant synchronization provides a flexible solution to enable collaboration, but every organization is different. For example, you might have a central tenant, satellite tenants, or sort of a mesh of tenants. Cross-tenant synchronization supports any of these topologies. For more information, see [Topologies for cross-tenant synchronization](cross-tenant-synchronization-topology.md).

:::image type="content" source="./media/cross-tenant-synchronization-overview/topology-all.png" alt-text="Diagram that shows different tenant topologies.":::

#### Step 2: Enable cross-tenant synchronization in the target tenants

In the target tenant where users are created, navigate to the **Cross-tenant access settings** page. Here you enable cross-tenant synchronization and the B2B automatic redemption settings by selecting the respective check boxes. For more information, see [Configure cross-tenant synchronization](cross-tenant-synchronization-configure.md).

:::image type="content" source="./media/cross-tenant-synchronization-overview/configure-target.png" alt-text="Diagram that shows cross-tenant synchronization enabled in the target tenant.":::

#### Step 3: Enable cross-tenant synchronization in the source tenants

In any source tenant, navigate to the **Cross-tenant access settings** page and enable the B2B automatic redemption feature. Next, you use the **Cross-tenant synchronization** page to set up a cross-tenant synchronization job and specify:

- Which users you want to synchronize
- What attributes you want to include
- Any transformations

For anyone that has used Azure AD to [provision identities into a SaaS application](../app-provisioning/user-provisioning.md), this experience will be familiar. Once you have synchronization configured, you can start testing with a few users and make sure they're created with all the attributes that you need. When testing is complete, you can quickly add additional users to synchronize and roll out across your organization. For more information, see [Configure cross-tenant synchronization](cross-tenant-synchronization-configure.md).

:::image type="content" source="./media/cross-tenant-synchronization-overview/configure-source.png" alt-text="Diagram that shows a cross-tenant synchronization job configured in the source tenant.":::

## License requirements

Using this feature requires Azure AD Premium P1 licenses. Each user who is synchronized with cross-tenant synchronization must have a P1 license in their home/source tenant. To find the right license for your requirements, see [Compare generally available features of Azure AD](https://www.microsoft.com/security/business/identity-access-management/azure-ad-pricing).

## Frequently asked questions

#### Clouds

Which clouds can cross-tenant synchronization be used in?

- Cross-tenant synchronization is supported within the commercial and Azure Government clouds.
- Synchronization is only supported between two tenants in the same cloud.
- Cross-cloud (such as public cloud to Azure Government) isn't currently supported.

#### Synchronization frequency

How often does cross-tenant synchronization run?

- The sync interval is currently fixed to start at 40-minute intervals. Sync duration varies based on the number of in-scope users. The initial sync cycle is likely to take significantly longer than the following incremental sync cycles.

#### Scope

How do I control what is synchronized into the target tenant?

- In the source tenant, you can control which users are provisioned with the configuration or attribute-based filters. You can also control what attributes on the user object are synchronized. For more information, see [Scoping users or groups to be provisioned with scoping filters](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md?toc=/azure/active-directory/multi-tenant-organizations/toc.json&pivots=cross-tenant-synchronization).

If a user is removed from the scope of sync in a source tenant, will cross-tenant synchronization soft delete them in the target?

- Yes. If a user is removed from the scope of sync in a source tenant, cross-tenant synchronization will soft delete them in the target tenant.

If the sync relationship is severed, are external users previously managed by cross-tenant synchronization deleted in the target tenant?

- No. No changes are made to the external users previously managed by cross-tenant synchronization if the relationship is severed (for example, if the cross-tenant synchronization policy is deleted).

#### Object types

What object types can be synchronized?

- Azure AD users can be synchronized between tenants. (Groups, devices, and contacts aren't currently supported.)

What user types can be synchronized?

- Internal members can be synchronized from source tenants. Internal guests can't be synchronized from source tenants.
- Users can be synchronized to target tenants as external members (default) or external guests.
- For more information about the UserType definitions, see [Properties of an Azure Active Directory B2B collaboration user](../external-identities/user-properties.md).

I have existing B2B collaboration users. What will happen to them?

- Cross-tenant synchronization will match the user and make any necessary updates to the user, such as update the display name. By default, the UserType won't be updated from guest to member, but you can configure this in the attribute mappings.

#### Attributes

What user attributes can be synchronized?

- Cross-tenant synchronization will sync commonly used attributes on the user object in Azure AD, including (but not limited to) displayName, userPrincipalName, and directory extension attributes.

What attributes can't be synchronized?

- Attributes including (but not limited to) managers, photos, custom security attributes, and user attributes outside of the directory can't be synchronized by cross-tenant synchronization.

Can I control where user attributes are sourced/managed?

- Cross-tenant synchronization doesn't offer direct control over source of authority. The user and its attributes are deemed authoritative at the source tenant. There are parallel sources of authority workstreams that will evolve source of authority controls for users down to the attribute level and a user object at the source may ultimately reflect multiple underlying sources. For the tenant-to-tenant process, this is still treated as the source tenant's values being authoritative for the sync process (even if pieces actually originate elsewhere) into the target tenant. Currently, there's no support for reversing the sync process's source of authority.
- Cross-tenant synchronization only supports source of authority at the object level. That means all attributes of a user must come from the same source, including credentials. It isn't possible to reverse the source of authority or federation direction of a synchronized object.

What happens if attributes for a synced user are changed in the target tenant?

- Cross-tenant synchronization doesn't query for changes in the target. If no changes are made to the synced user in the source tenant, then user attribute changes made in the target tenant will persist. However, if changes are made to the user in the source tenant, then during the next synchronization cycle, the user in the target tenant will be updated to match the user in the source tenant.

Can the target tenant manually block sign-in for a specific home/source tenant user that is synced?

- If no changes are made to the synced user in the source tenant, then the block sign-in setting in the target tenant will persist. If a change is detected for the user in the source tenant, cross-tenant synchronization will re-enable that user blocked from sign-in in the target tenant.

#### Structure

Can I sync a mesh between multiple tenants?

- Cross-tenant synchronization is configured as a single-direction peer-to-peer sync, meaning sync is configured between one source and one target tenant. Multiple instances of cross-tenant synchronization can be configured to sync from a single source to multiple targets and from multiple sources into a single target. But only one sync instance can exist between a source and a target.
- Cross-tenant synchronization only synchronizes users that are internal to the home/source tenant, ensuring that you can't end up with a loop where a user is written back to the same tenant.
- Multiple topologies are supported. For more information, see [Topologies for cross-tenant synchronization](cross-tenant-synchronization-topology.md).

Can I use cross-tenant synchronization across organizations (outside my multi-tenant organization)?

-  For privacy reasons, cross-tenant synchronization is intended for use within an organization. We recommend using [entitlement management](../governance/entitlement-management-overview.md) for inviting B2B collaboration users across organizations.

Can cross-tenant synchronization be used to migrate users from one tenant to another tenant?

-  No. Cross-tenant synchronization isn't a migration tool because the source tenant is required for synchronized users to authenticate. In addition, tenant migrations would require migrating user data such as SharePoint and OneDrive.

#### B2B collaboration

Does cross-tenant synchronization resolve any present [B2B collaboration](../external-identities/what-is-b2b.md) limitations?

- Since cross-tenant synchronization is built on existing B2B collaboration technology, existing limitations apply. Examples include (but aren't limited to):

    [!INCLUDE [user-type-workload-limitations-include](../includes/user-type-workload-limitations-include.md)]

#### B2B direct connect

How does cross-tenant synchronization relate to [B2B direct connect](../external-identities/b2b-direct-connect-overview.md)?

- B2B direct connect is the underlying identity technology required for [Teams Connect shared channels](/microsoftteams/platform/concepts/build-and-test/shared-channels).
- We recommend B2B collaboration for all other cross-tenant application access scenarios, including both Microsoft and non-Microsoft applications.
- B2B direct connect and cross-tenant synchronization are designed to co-exist, and you can enable them both for broad coverage of cross-tenant scenarios.

We're trying to determine the extent to which we'll need to utilize cross-tenant synchronization in our multi-tenant organization. Do you plan to extend support for B2B direct connect beyond Teams Connect?

- There's no plan to extend support for B2B direct connect beyond Teams Connect shared channels.

#### Microsoft 365

Does cross-tenant synchronization enhance any cross-tenant Microsoft 365 app access user experiences?

- Cross-tenant synchronization utilizes a feature that improves the user experience by suppressing the first-time B2B consent prompt and redemption process in each tenant.
- Synchronized users will have the same cross-tenant Microsoft 365 experiences available to any other B2B collaboration user.

#### Teams

Does cross-tenant synchronization enhance any current Teams experiences?

- Synchronized users will have the same cross-tenant Microsoft 365 experiences available to any other B2B collaboration user.

#### Integration

What federation options are supported for users in the target tenant back to the source tenant?

- For each internal user in the source tenant, cross-tenant synchronization creates a federated external user (commonly used in B2B) in the target. It supports syncing internal users. This includes internal users federated to other identity systems using domain federation (such as [Active Directory Federation Services](/windows-server/identity/ad-fs/ad-fs-overview)). It doesn't support syncing external users.

Does cross-tenant synchronization use System for Cross-Domain Identity Management (SCIM)?

- No. Currently, Azure AD supports a SCIM client, but not a SCIM server. For more information, see [SCIM synchronization with Azure Active Directory](../fundamentals/sync-scim.md).


## Next steps

- [Topologies for cross-tenant synchronization](cross-tenant-synchronization-topology.md)
- [Configure cross-tenant synchronization](cross-tenant-synchronization-configure.md)
