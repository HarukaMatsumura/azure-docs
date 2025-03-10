---
title: "Tutorial: Azure Active Directory single sign-on (SSO) integration with TickitLMS Learn | Microsoft Docs"
description: Learn how to configure single sign-on between Azure Active Directory and TickitLMS Learn.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/18/2020
ms.author: jeedes
---

# Tutorial: Azure Active Directory single sign-on (SSO) integration with TickitLMS Learn

In this tutorial, you'll learn how to integrate TickitLMS Learn with Azure Active Directory (Azure AD). When you integrate TickitLMS Learn with Azure AD, you can:

- Control in Azure AD who has access to TickitLMS Learn.
- Enable your users to be automatically signed-in to TickitLMS Learn with their Azure AD accounts.
- Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

- An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
- TickitLMS Learn single sign-on (SSO) enabled subscription.

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

- TickitLMS Learn supports **SP and IDP** initiated SSO

## Adding TickitLMS Learn from the gallery

To configure the integration of TickitLMS Learn into Azure AD, you need to add TickitLMS Learn from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **TickitLMS Learn** in the search box.
1. Select **TickitLMS Learn** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for TickitLMS Learn

Configure and test Azure AD SSO with TickitLMS Learn using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in TickitLMS Learn.

To configure and test Azure AD SSO with TickitLMS Learn, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
   1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
   1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure TickitLMS Learn SSO](#configure-tickitlms-learn-sso)** - to configure the single sign-on settings on application side.
   1. **[Create TickitLMS Learn test user](#create-tickitlms-learn-test-user)** - to have a counterpart of B.Simon in TickitLMS Learn that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **TickitLMS Learn** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Edit Basic SAML Configuration](common/edit-urls.png)

1. On the **Basic SAML Configuration** section, the user does not have to perform any step as the app is already pre-integrated with Azure.

1. Click **Set additional URLs** and perform the following step if you wish to configure the application in **SP** initiated mode:

   In the **Sign-on URL** text box, type the URL:
   `https:/learn.tickitlms.com/sso/login`

1. Click **Save**.

1. TickitLMS Learn application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration. The following screenshot shows the list of default attributes.

   ![image](common/default-attributes.png)

1. In addition to above, TickitLMS Learn application expects few more attributes to be passed back in SAML response which are shown below. These attributes are also pre populated but you can review them as per your requirements.

   | Name        | Source Attribute |
   | ----------- | ---------------- |
   | samlaccount | user.samlaccount |
   | employeeid  | user.employeeid  |
   | role        | user.role        |
   | department  | user.department  |
   | reportsto   | user.reportsto   |

   > [!NOTE]
   > TickitLMS Learn expects roles for users assigned to the application. Please set up these roles in Azure AD so that users can be assigned the appropriate roles. To understand how to configure roles in Azure AD, see [here](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui).

1. On the **Set up single sign-on with SAML** page, In the **SAML Signing Certificate** section, click copy button to copy **App Federation Metadata Url** and save it on your computer.

   ![The Certificate download link](common/copy-metadataurl.png)

### Create an Azure AD test user

In this section, you'll create a test user in the Azure portal called B.Simon.

1. From the left pane in the Azure portal, select **Azure Active Directory**, select **Users**, and then select **All users**.
1. Select **New user** at the top of the screen.
1. In the **User** properties, follow these steps:
   1. In the **Name** field, enter `B.Simon`.
   1. In the **User name** field, enter the username@companydomain.extension. For example, `B.Simon@contoso.com`.
   1. Select the **Show password** check box, and then write down the value that's displayed in the **Password** box.
   1. Click **Create**.

### Assign the Azure AD test user

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to TickitLMS Learn.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **TickitLMS Learn**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you have setup the roles as explained in the above, you can select it from the **Select a role** dropdown.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure TickitLMS Learn SSO

To configure single sign-on on **TickitLMS Learn** side, you need to send the **App Federation Metadata Url** to [TickitLMS Learn support team](mailto:support@tickitlms.com). They set this setting to have the SAML SSO connection set properly on both sides.

### Create TickitLMS Learn test user

In this section, you create a user called Britta Simon in TickitLMS Learn. Work with [TickitLMS Learn support team](mailto:support@tickitlms.com) to add the users in the TickitLMS Learn platform. Users must be created and activated before you use single sign-on.

## Test SSO

In this section, you test your Azure AD single sign-on configuration with following options.

#### SP initiated:

- Click on **Test this application** in Azure portal. This will redirect to TickitLMS Learn Sign on URL where you can initiate the login flow.

- Go to TickitLMS Learn Sign-on URL directly and initiate the login flow from there.

#### IDP initiated:

- Click on **Test this application** in Azure portal and you should be automatically signed in to the TickitLMS Learn for which you set up the SSO

You can also use Microsoft My Apps to test the application in any mode. When you click the TickitLMS Learn tile in the My Apps, if configured in SP mode you would be redirected to the application sign on page for initiating the login flow and if configured in IDP mode, you should be automatically signed in to the TickitLMS Learn for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## Next steps

Once you configure TickitLMS Learn you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Defender for Cloud Apps](/cloud-app-security/proxy-deployment-any-app).
