---
title: Determine causes of non-compliance
description: When a resource is non-compliant, there are many possible reasons. Learn to find out what caused the non-compliance.
ms.date: 09/01/2021
ms.topic: how-to
---
# Determine causes of non-compliance

When an Azure resource is determined to be non-compliant to a policy rule, it's helpful to
understand which portion of the rule the resource isn't compliant with. It's also useful to
understand what change altered a previously compliant resource to make it non-compliant. There are
two ways to find this information:

- [Compliance details](#compliance-details)
- [Change history (Preview)](#change-history)

## Compliance details

When a resource is non-compliant, the compliance details for that resource are available from the
**Policy compliance** page. The compliance details pane includes the following information:

- Resource details such as name, type, location, and resource ID
- Compliance state and timestamp of the last evaluation for the current policy assignment
- A list of _reasons_ for the resource non-compliance

> [!IMPORTANT]
> As the compliance details for a _Non-compliant_ resource shows the current value of properties on
> that resource, the user must have **read** operation to the **type** of resource. For example, if
> the _Non-compliant_ resource is **Microsoft.Compute/virtualMachines** then the user must have the
> **Microsoft.Compute/virtualMachines/read** operation. If the user doesn't have the needed
> operation, an access error is displayed.

To view the compliance details, follow these steps:

1. Launch the Azure Policy service in the Azure portal by selecting **All services**, then searching
   for and selecting **Policy**.

1. On the **Overview** or **Compliance** page, select a policy in a **compliance state** that is
   _Non-compliant_.

1. Under the **Resource compliance** tab of the **Policy compliance** page, select and hold (or
   right-click) or select the ellipsis of a resource in a **compliance state** that is
   _Non-compliant_. Then select **View compliance details**.

   :::image type="content" source="../media/determine-non-compliance/view-compliance-details.png" alt-text="Screenshot of the 'View compliance details' link on the Resource compliance tab." border="false":::

1. The **Compliance details** pane displays information from the latest evaluation of the resource
   to the current policy assignment. In this example, the field **Microsoft.Sql/servers/version** is
   found to be _12.0_ while the policy definition expected _14.0_. If the resource is non-compliant
   for multiple reasons, each is listed on this pane.

   :::image type="content" source="../media/determine-non-compliance/compliance-details-pane.png" alt-text="Screenshot of the Compliance details pane and reasons for non-compliance that current value is twelve and target value is fourteen." border="false":::

   For an **auditIfNotExists** or **deployIfNotExists** policy definition, the details include the
   **details.type** property and any optional properties. For a list, see [auditIfNotExists
   properties](../concepts/effects.md#auditifnotexists-properties) and [deployIfNotExists
   properties](../concepts/effects.md#deployifnotexists-properties). **Last evaluated resource** is
   a related resource from the **details** section of the definition.

   Example partial **deployIfNotExists** definition:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   :::image type="content" source="../media/determine-non-compliance/compliance-details-pane-existence.png" alt-text="Screenshot of Compliance details pane for ifNotExists including evaluated resource count." border="false":::

> [!NOTE]
> To protect data, when a property value is a _secret_ the current value displays asterisks.

These details explain why a resource is currently non-compliant, but don't show when the change was
made to the resource that caused it to become non-compliant. For that information, see [Change
history (Preview)](#change-history) below.

### Compliance reasons

[Resource Manager modes](../concepts/definition-structure.md#resource-manager-modes) and
[Resource Provider modes](../concepts/definition-structure.md#resource-provider-modes) each have
different _reasons_ for non-compliance.

#### General Resource Manager mode compliance reasons

The following table maps each
[Resource Manager mode](../concepts/definition-structure.md#resource-manager-modes) _reason_ to the
responsible [condition](../concepts/definition-structure.md#conditions) in the policy definition:

|Reason |Condition |
|-|-|
|Current value must contain the target value as a key. |containsKey or **not** notContainsKey |
|Current value must contain the target value. |contains or **not** notContains |
|Current value must be equal to the target value. |equals or **not** notEquals |
|Current value must be less than the target value. |less or **not** greaterOrEquals |
|Current value must be greater than or equal to the target value. |greaterOrEquals or **not** less |
|Current value must be greater than the target value. |greater or **not** lessOrEquals |
|Current value must be less than or equal to the target value. |lessOrEquals or **not** greater |
|Current value must exist. |exists |
|Current value must be in the target value. |in or **not** notIn |
|Current value must be like the target value. |like or **not** notLike |
|Current value must case-sensitive match the target value. |match or **not** notMatch |
|Current value must case-insensitive match the target value. |matchInsensitively or **not** notMatchInsensitively |
|Current value must not contain the target value as a key. |notContainsKey or **not** containsKey|
|Current value must not contain the target value. |notContains or **not** contains |
|Current value must not be equal to the target value. |notEquals or **not** equals |
|Current value must not exist. |**not** exists  |
|Current value must not be in the target value. |notIn or **not** in |
|Current value must not be like the target value. |notLike or **not** like |
|Current value must not case-sensitive match the target value. |notMatch or **not** match |
|Current value must not case-insensitive match the target value. |notMatchInsensitively or **not** matchInsensitively |
|No related resources match the effect details in the policy definition. |A resource of the type defined in **then.details.type** and related to the resource defined in the **if** portion of the policy rule doesn't exist. |

#### AKS Resource Provider mode compliance reasons

The following table maps each `Microsoft.Kubernetes.Data`
[Resource Provider mode](../concepts/definition-structure.md#resource-provider-modes) _reason_ to
the responsible state of the
[constraint template](https://open-policy-agent.github.io/gatekeeper/website/docs/howto/#constraint-templates)
in the policy definition:

|Reason |Constraint template reason description |
|-|-|
|Constraint/TemplateCreateFailed |The resource failed to create for a policy definition with a Constraint/Template that doesn't match an existing Constraint/Template on cluster by resource metadata name. |
|Constraint/TemplateUpdateFailed |The Constraint/Template failed to update for a policy definition with a Constraint/Template that matches an existing Constraint/Template on cluster by resource metadata name. |
|Constraint/TemplateInstallFailed |The Constraint/Template failed to build and was unable to be installed on cluster for either create or update operation. |
|ConstraintTemplateConflicts |The Template has a conflict with one or more policy definitions using the same Template name with different source. |
|ConstraintStatusStale |There is an existing 'Audit' status, but Gatekeeper has not performed an audit within the last hour. |
|ConstraintNotProcessed |There is no status and Gatekeeper has not performed an audit within the last hour. |
|InvalidConstraint/Template |API Server has rejected the resource due to a bad YAML. This reason can also be caused by a parameter type mismatch (example: string provided for an integer)

> [!NOTE]
> For existing policy assignments and constraint templates already on the cluster, if that
> Constraint/Template fails, the cluster is protected by maintaining the existing
> Constraint/Template. The cluster reports as non-compliant until the failure is resolved on the
> policy assignment or the add-on self-heals. For more information about handling conflict, see
> [Constraint template conflicts](../concepts/policy-for-kubernetes.md#constraint-template-conflicts).

## Component details for Resource Provider modes

For assignments with a
[Resource Provider mode](../concepts/definition-structure.md#resource-provider-modes), select the
_Non-compliant_ resource to open a deeper view. Under the **Component Compliance** tab is additional
information specific to the Resource Provider mode on the assigned policy showing the
_Non-compliant_ **Component** and **Component ID**.

:::image type="content" source="../media/getting-compliance-data/compliance-components.png" alt-text="Screenshot of Component Compliance tab and compliance details for a Resource Provider mode assignment." border="false":::

## Compliance details for guest configuration

For policy definitions in the _Guest Configuration_ category, there could be multiple
settings evaluated inside the virtual machine and you'll need to view per-setting details. For
example, if you're auditing for a list of security settings and only one of them has status
_Non-compliant_, you'll need to know which specific settings are out of compliance and why.

You also might not have access to sign in to the virtual machine directly but you need to report on
why the virtual machine is _Non-compliant_.

### Azure portal

Begin by following the same steps in the section above for viewing policy compliance details.

In the Compliance details pane view, select the link **Last evaluated resource**.

:::image type="content" source="../media/determine-non-compliance/guestconfig-auditifnotexists-compliance.png" alt-text="Screenshot of viewing the auditIfNotExists definition compliance details." border="false":::

The **Guest Assignment** page displays all available compliance details. Each row in the view
represents an evaluation that was performed inside the machine. In the **Reason** column, a phrase
is shown describing why the Guest Assignment is _Non-compliant_. For example, if you're auditing
password policies, the **Reason** column would display text including the current value for each
setting.

:::image type="content" source="../media/determine-non-compliance/guestconfig-compliance-details.png" alt-text="Screenshot of the Guest Assignment compliance details." border="false":::

### View configuration assignment details at scale

The guest configuration feature can be used outside of Azure Policy assignments.
For example,
[Azure AutoManage](../../../automanage/automanage-virtual-machines.md)
creates guest configuration assignments, or you might
[assign configurations when you deploy machines](guest-configuration-create-assignment.md).

To view all guest configuration assignments across your tenant, from the Azure
portal open the **Guest Assignments** page. To view detailed compliance
information, select each assignment using the link in the column "Name".

:::image type="content" source="../media/determine-non-compliance/guest-config-assignment-view.png" alt-text="Screenshot of the Guest Assignment page." border="true":::

## <a name="change-history"></a>Change history (Preview)

As part of a new **public preview**, the last 14 days of change history are available for all Azure
resources that support [complete mode
deletion](../../../azure-resource-manager/templates/deployment-complete-mode-deletion.md). Change history
provides details about when a change was detected and a _visual diff_ for each change. A change
detection is triggered when the Azure Resource Manager properties are added, removed, or altered.

1. Launch the Azure Policy service in the Azure portal by selecting **All services**, then searching
   for and selecting **Policy**.

1. On the **Overview** or **Compliance** page, select a policy in any **compliance state**.

1. Under the **Resource compliance** tab of the **Policy compliance** page, select a resource.

1. Select the **Change History (preview)** tab on the **Resource Compliance** page. A list of
   detected changes, if any exist, are displayed.

   :::image type="content" source="../media/determine-non-compliance/change-history-tab.png" alt-text="Screenshot of the Change History tab and detected change times on Resource Compliance page." border="false":::

1. Select one of the detected changes. The _visual diff_ for the resource is presented on the
   **Change history** page.

   :::image type="content" source="../media/determine-non-compliance/change-history-visual-diff.png" alt-text="Screenshot of the Change History Visual Diff of the before and after state of properties on the Change history page." border="false":::

The _visual diff_ aides in identifying changes to a resource. The changes detected may not be
related to the current compliance state of the resource.

Change history data is provided by [Azure Resource Graph](../../resource-graph/overview.md). To
query this information outside of the Azure portal, see [Get resource changes](../../resource-graph/how-to/get-resource-changes.md).

## Next steps

- Review examples at [Azure Policy samples](../samples/index.md).
- Review the [Azure Policy definition structure](../concepts/definition-structure.md).
- Review [Understanding policy effects](../concepts/effects.md).
- Understand how to [programmatically create policies](programmatically-create.md).
- Learn how to [get compliance data](get-compliance-data.md).
- Learn how to [remediate non-compliant resources](remediate-resources.md).
- Review what a management group is with [Organize your resources with Azure management groups](../../management-groups/overview.md).