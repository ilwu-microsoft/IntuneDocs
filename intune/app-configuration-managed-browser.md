---
# required metadata

title: Manage web access with portected browsers
titlesuffix: Microsoft Intune
description: Deploy the Managed Browser or Microsoft Edge application to restrict web browsing and the transfer of web data to other apps.
keywords:
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 07/10/2018
ms.topic: article
ms.prod:
ms.service: microsoft-intune
ms.technology:
ms.assetid: 1feca24f-9212-4d5d-afa9-7c171c5e8525

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: maxles
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: intune-azure

---

# Manage Internet access using protected browser policies with Microsoft Intune

[!INCLUDE [azure_portal](./includes/azure_portal.md)]

Protected browsers include Microsoft Edge and the Intune Managed Browser. Microsoft Edge and Managed Browser are web browsing apps that you can download from public app stores for use in your organization. When configured with Intune, protected browsers can be:
- Used to access corporate sites and SaaS apps with Single Sign-On via the MyApps service, while keeping web data protected.
- Pre-configured with a list of URLs and domains to restrict which sites the user can navigate to in the corporate context.
- Pre-configured with a homepage, and bookmarks you specify.

Because Microsoft Edge and Managed Browser have integration with the Intune SDK, you can also apply app protection policies to them, including:
- Controlling the use of cut, copy, and paste
- Preventing screen captures
- Ensuring that links to content that users select open only in other managed apps.

For details, see [What are app protection policies?](app-protection-policy.md)

You can apply these settings to:

- Devices that are enrolled with Intune
- Enrolled with another MDM product
- Unmanaged devices

If users install the Managed Browser from the app store and Intune does not manage it, it can be used as a basic web browser, with support for Single Sign-On through the Microsoft MyApps site. Users are taken directly to the MyApps site, where they can see all of their provisioned SaaS applications.
While Managed Browser or Microsoft Edge are not managed by Intune, they cannot access data from other Intune-managed applications. 

The Managed Browser does not support the Secure Sockets Layer version 3 (SSLv3) cryptographic protocol.

You can create protected browser policies for the following device types:

-   Devices that run Android 4 and later

-   Devices that run iOS 8.0 and later

>[!IMPORTANT]
>As of October 2017, the Intune Managed Browser app on Android app supports only devices running Android 4.4 and later. The Intune Managed Browser app on iOS will support only devices running iOS 9.0 and later.
>Earlier versions of Android and iOS will be able to continue using the Managed Browser, but will be unable to install new versions of the app and might not be able to access all of the app capabilities. We encourage you to update these devices to a supported operating system version.


Microsoft Edge and the Intune Managed Browser support opening web content from [Microsoft Intune application partners](https://www.microsoft.com/cloud-platform/microsoft-intune-apps).

## Conditional Access for protected browsers

The Managed Browser and Microsoft Edge are now an approved client app for Conditional Access. This means that you can restrict mobile browser access to Azure AD-connected web apps where users can only use the Managed Browser or Microsoft Edge in corporate mode, blocking access from any other unprotected browsers such as Safari or Chrome. This protection can be applied to Azure resources like Exchange Online and SharePoint Online, the Office portal, and even on-premises sites that you have exposed to external users via the [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started). 

To restrict Azure AD-connected web apps to use the Intune Managed Browser on mobile platforms, you can create an Azure AD Conditional Access policy requiring approved client applications. 

1. In the Azure portal, select **Azure Active Directory** > **Enterprise applications** > **Conditional access** > **New policy**. 
2. Next, select **Grant** from the **Access controls** section of the blade. 
3. Click **Require approved client app**. 
4. Click **Select** on the **Grant** blade. This policy must be assigned to the cloud apps that you want to be accessible to only the Intune Managed Browser app.

    ![Azure AD - Managed Browser conditional access policy](./media/managed-browser-conditional-access-01.png)

5. In the **Assignments** section, select **Conditions** > **Client apps**. The **Client apps** blade is displayed.
6. Click **Yes** under **Configure** to apply the policy to specific client apps.
7. Verify that **Browser** is select as a client app.

    ![Azure AD - Managed Browser - Select client apps](./media/managed-browser-conditional-access-02.png)

    > [!NOTE]
    > If you want to restrict which native apps (non-browser apps) can access these cloud applications, you can also select **Mobile apps and desktop clients**.

8. In the **Assignments** section, select **Users and groups** and then choose the users or groups you would like to assign this policy. 

    > [!NOTE]
    > Users must also be targeted with Intune App Protection policy. For more information about creating Intune App Protection policies, see [What are app protection policies?](app-protection-policy.md)

9. In the **Assignments** section, select **Cloud apps** to choose which apps to protect with this policy.

Once the above policy is configured, users will be forced to use the Intune Managed Browser to access the Azure AD-connected web apps you have protected with this policy. If users attempt to use an unmanaged browser in this scenario, they will see a notice that the Intune Managed Browser must be used instead.

The Managed Browser does not support classic Conditional Access policies. For more information, see [Migrate classic policies in the Azure portal](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-migration).

##  Single Sign-on to Azure AD-connected web apps in Microsoft Edge or Intune Managed Browser

Microsoft Edge and Intune Managed Browser on iOS and Android can now take advantage of SSO to all web apps (SaaS and on-prem) that are Azure AD-connected. When the Microsoft Authenticator app is present on iOS or the Intune Company Portal app on Android, users of the Intune Managed Browser will be able to access Azure AD-connected web apps without having to re-enter their credentials.

SSO in protected browsers requires your device to be registered by the Microsoft Authenticator app on iOS or the Intune Company Portal on Android. Users with the Authenticator app or Intune Company Portal will be prompted to register their device when they navigate to an Azure AD-connected web app in Microsoft Edge or Intune Managed Browser, if their device has not already been registered by another application. Once the device is registered with the account managed by Intune, that account will have SSO enabled for Azure AD-connected web apps. 

> [!NOTE]
> Device registration is a simple check-in with the Azure AD service. It does not require full device enrollment and does not give IT any additional privileges on the device.

## Create a protected browser app configuration

1. Sign into the [Azure portal](https://portal.azure.com).
2. Choose **All services** > **Intune**. Intune is located in the **Monitoring + Management** section.
3.  On the **Client apps** blade of the Manage list, choose **App configuration policies**.
4.  On the **App configuration policies** blade, choose **Add**.
5.  On the **Add configuration policy** blade, enter a **Name** and optional **Description** for the app configuration settings.
6.  For **Device enrollment** type, choose **Managed apps**.
7.  Choose **Select the required app** and then, on the **Targeted apps** blade, choose the **Managed Browser** and/or **Edge** for iOS, for Android, or for both.
8.  Choose **OK** to return to the **Add configuration policy** blade.
9.  Choose **Configuration settings**. On the **Configuration** blade, you define key and value pairs to supply configurations for the Managed Browser. Use the sections later in this article to learn about the different key and value pairs you can define.
10. When you are done, choose **OK**.
11. On the **Add configuration policy** blade, choose **Add**.
12. The new configuration is created, and displayed on the **App configuration** blade.

>[!IMPORTANT]
>Currently, the Managed Browser relies on auto-enrollment. For app configurations to apply, another application on the device must already be managed by Intune app protection policies.

## Assign the configuration settings you created

You assign the settings to Azure AD groups of users. If that user has the targeted protected browser app installed, then the app is managed by the settings you specified.

1. On the **Client apps** blade of the Intune mobile application management dashboard, choose **App configuration policies**.
2. From the list of app configurations, select the one you want to assign.
3. On the next blade, choose **Assignments**.
4. On the **Assignments** blade, select the Azure AD group to which you want to assign the app configuration, and then choose **OK**.


## How to configure Application Proxy settings for protected browsers

Microsoft Edge and the Intune Managed Browser and [Azure AD Application Proxy]( https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) can be used together to support the following scenarios for users of iOS and Android devices:

- A user downloads and signs in to the Microsoft Outlook app. Intune app protection policies are automatically applied. They encrypt saved data and block the user from transferring corporate files to unmanaged apps or locations on the device. When the user then clicks a link to an intranet site in Outlook, you can specify that the link opens in a protected browser application, rather than another browser. The protected browser recognizes that this intranet site has been exposed to the user through the Application Proxy. The user is automatically routed through the Application Proxy, to authenticate with any applicable multi-factor authentication, and conditional access before reaching the intranet site. This site, which could previously not be found while the user was remote, is now accessible and the link in Outlook works as expected.
- A remote user opens the protected browser application and navigates to an intranet site using the internal URL. The protected browser recognizes that this intranet site has been exposed to the user via the Application Proxy. The user is automatically routed through the Application Proxy, to authenticate with any applicable multi-factor authentication, and conditional access before reaching the intranet site. This site, which could previously not be found while the user was remote, is now accessible.

### Before you start

- Set up your internal applications through the Azure AD Application Proxy.
    - To configure Application Proxy and publish applications, see the [setup documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started#how-to-get-started). 
- You must be using minimum version 1.2.0 of the Managed Browser app.
- Users of the Managed Browser or Edge app have an [Intune app protection policy]( app-protection-policy.md) assigned to the app.

    > [!NOTE]
    > Updated Application Proxy redirection data can take up to 24 hours to take effect in the Managed Browser and Edge.


#### Step 1: Enable automatic redirection to a protected browser from Outlook
Outlook must be configured with an app protection policy that enables the setting **Restrict web content to display in the Managed Browser**.

#### Step 2: Assign an app configuration policy assigned for the protected browser.
This procedure configures the Managed Browser or Edge app to use app proxy redirection. Using the procedure to create an Edge or Managed Browser app configuration, supply the following key and value pair:

| Key                                                             | Value    |
|-----------------------------------------------------------------|----------|
| **com.microsoft.intune.mam.managedbrowser.AppProxyRedirection** | **true** |

For more information about how the Managed Browser, Edge, and Azure AD Application Proxy can be used in tandem for seamless (and protected) access to on-premises web apps, see the Enterprise Mobility + Security blog post [Better together: Intune and Azure Active Directory team up to improve user access](https://cloudblogs.microsoft.com/enterprisemobility/2017/07/06/better-together-intune-and-azure-active-directory-team-up-to-improve-user-access).

> [!NOTE]
> Edge uses the same key and value pairs as the Managed Browser. 

## How to configure the homepage for a protected browser

This setting allows you to configure the homepage that users see when they start a protected browser or create a new tab. Using the procedure to create an Edge or Managed Browser app configuration, supply the following key and value pair:

|                                Key                                |                                                           Value                                                            |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| <strong>com.microsoft.intune.mam.managedbrowser.homepage</strong> | Specify a valid URL. Incorrect URLs are blocked as a security measure.<br>Example: `<https://www.bing.com>` |

## How to configure bookmarks for a protected browser

This setting allows you to configure a set of bookmarks that is available to users of Edge or the Managed Browser.

- These bookmarks cannot be deleted or modified by users
- These bookmarks display at the top of the list. Any bookmarks that users create are displayed below these bookmarks.
- If you have enabled App Proxy redirection, you can add App Proxy web apps using either their internal or external URL.

Using the procedure to create an Edge or Managed Browser app configuration, supply the following key and value pair:

|                                Key                                 |                                                                                                                                                                                                                                                         Value                                                                                                                                                                                                                                                          |
|--------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <strong>com.microsoft.intune.mam.managedbrowser.bookmarks</strong> | The value for this configuration is a list of bookmarks. Each bookmark consists of the bookmark title, and the bookmark URL. Separate the title, and URL with the <strong>&#124;</strong> character.<br><br>Example:<br> <code>Microsoft Bing&#124;https://www.bing.com</code><br><br>To configure multiple bookmarks, separate each pair with the double character, <strong>&#124;&#124;</strong><br><br>Example:<br> <code>Bing&#124;https://www.bing.com&#124;&#124;Contoso&#124;https://www.contoso.com</code> |

## How to specify allowed and blocked URLs for a protected browser

Using the procedure to create an Edge or Managed Browser app configuration, supply the following key and value pair:

|Key|Value|
|-|-|
|Choose from:<br><ul><li>Specify allowed URLs (only these URLs are allowed; no other sites can be accessed):<br> **com.microsoft.intune.mam.managedbrowser.AllowListURLs**<br><br></li><li>Specify blocked URLs (all other sites can be accessed):<br>**com.microsoft.intune.mam.managedbrowser.BlockListURLs**</li></ul>|The corresponding value for the key is a list of URLs. You enter all the URLs you want to allow or block as a single value, separated by a pipe **&#124;** character.<br><br>Examples:<br><br><code>URL1&#124;URL2&#124;URL3</code><br><code>http://*.contoso.com/*&#124;https://*.bing.com/*&#124;https://expenses.contoso.com</code>|

>[!IMPORTANT]
>Do not specify both keys. If both keys are targeted to the same user, the allow key is used, as it's the most restrictive option.
>Additionally, make sure not to block important pages like your company websites.

### URL format for allowed and blocked URLs
Use the following information to learn about the allowed formats and wildcards that you can use when specifying URLs in the allowed and blocked lists:

- You can use the wildcard symbol (**&#42;**) according to the rules in the following permitted patterns list:

- Ensure that you prefix all URLs with **http** or **https** when entering them into the list.

- You can specify port numbers in the address. If you do not specify a port number, the values used are:

  -   Port 80 for http

  -   Port 443 for https

  Using wildcards for the port number is not supported. For example, `http://www.contoso.com:;` and `http://www.contoso.com: /;` are not supported.

- Use the following table to learn about the permitted patterns that you can use when you specify URLs:

|                  URL                  |                     Details                      |                                                Matches                                                |                                Does not match                                 |
|---------------------------------------|--------------------------------------------------|-------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
|        `http://www.contoso.com`         |              Matches a single page               |                                            `www.contoso.com`                                            |  `host.contoso.com`<br /><br />`www.contoso.com/images`<br /><br />`contoso.com`/   |
|          `http://contoso.com`           |              Matches a single page               |                                             `contoso.com/`                                              | `host.contoso.com`<br /><br />`www.contoso.com/images`<br /><br />`www.contoso.com` |
|    `http://www.contoso.com/&#42;`     | Matches all URLs that begin with `www.contoso.com` |      `www.contoso.com`<br /><br />`www.contoso.com/images`<br /><br />`www.contoso.com/videos/tvshows`      |              `host.contoso.com`<br /><br />`host.contoso.com/images`              |
|    `http://*.contoso.com/*`     |     Matches all subdomains under contoso.com     | `developer.contoso.com/resources`<br /><br />`news.contoso.com/images`<br /><br />`news.contoso.com/videos` |                               `contoso.host.com`                                |
|     `http://www.contoso.com/images`     |             Matches a single folder              |                                        `www.contoso.com/images`                                         |                          `www.contoso.com/images/dogs`                          |
|       `http://www.contoso.com:80`       |  Matches a single page, by using a port number   |                                       `http://www.contoso.com:80`                                       |                                                                               |
|        `https://www.contoso.com`        |          Matches a single, secure page           |                                        `https://www.contoso.com`                                        |                            `http://www.contoso.com`                             |
| `http://www.contoso.com/images/&#42;` |    Matches a single folder and all subfolders    |                  `www.contoso.com/images/dogs`<br /><br />`www.contoso.com/images/cats`                   |                            `www.contoso.com/videos`                             |

- The following are examples of some of the inputs that you cannot specify:

  - `*.com`

  - `*.contoso/*`

  - `www.contoso.com/*images`

  - `www.contoso.com/*images*pigs`

  - `www.contoso.com/page*`

  - IP addresses

  - `https://*`

  - `http://*`

  - `http://www.contoso.com:*`

  - `http://www.contoso.com: /*`

## How to access to managed app logs using protected browsers on iOS

End users with the managed Browser installed on their iOS device can view the management status of all Microsoft published apps. They can send logs for troubleshooting their managed iOS apps.

1. Open iOS **Settings**.
2. Select the managed **Browser** application settings.
3. Toggle **Enable Intune Diagnostics** to set the browser in troubleshooting mode.
4. Open the managed **Browser**. Click **View Intune App Status** to review individual application policy settings.
5. Press **Get Started** and **Share Logs** or **Send Logs to Microsoft** to send the troubleshooting logs to your IT administrator or Microsoft.

You can also open the Managed Browser or Microsoft Edge in troubleshooting mode from within the app.

1. Open Managed Browser or Microsoft Edge on iOS.
2. Type `about:intunehelp` in the address box.
The protected browser will then launch troubleshooting mode.

For a list of the settings stored in the app logs, see [Review app protection logs in the Managed Browser](app-protection-policy-settings-log.md).

## Security and privacy for protected browsers

-   The Managed Browser and Microsoft Edge do not use settings that users make for the built-in browser on their devices. The protected browsers cannot access to these settings.

-   If you configure the option **Require simple PIN for access** or **Require corporate credentials for access** in an app protection policy associated with the Managed Browser or Microsoft Edge, and a user selects the help link on the authentication page, they can browse any Internet sites regardless of whether they were added to a block list in the policy.

-   The Managed Browser and Microsoft Edge can block access to sites only when they are accessed directly. It does not block access when intermediate services (such as a translation service) are used to access the site.

-   To allow authentication, and access to Intune documentation, **&#42;.microsoft.com** is exempt from the allow or block list settings. It is always allowed.

### Turn off usage data
Microsoft automatically collects anonymous data about the performance and use of the Managed Browser or Microsoft Edge to improve Microsoft products and services. Users can turn off data collection by using the **Usage Data** setting on their devices. You have no control over the collection of this data.


-   On iOS devices, websites that users visit that have an expired or untrusted certificate cannot be opened.
-   The Managed Browser does not use settings that users make for the built-in browser on their devices. The Managed Browser or Microsoft Edge cannot access to these settings.

-   If you configure the option **Require simple PIN for access** or **Require corporate credentials for access** in an app protection policy associated with the Managed Browser or Microsoft Edge, and a user selects the help link on the authentication page, they can browse any Internet sites regardless of whether they were added to a block list in the policy.

-   The Managed Browser and Microsoft Edge can block access to sites only when they are accessed directly. It does not block access when intermediate services (such as a translation service) are used to access the site.

-   To allow authentication, and access to Intune documentation, **&#42;.microsoft.com** is exempt from the allow or block list settings. It is always allowed.

### Turn off usage data
Microsoft automatically collects anonymous data about the performance and use of the Managed Browser to improve Microsoft products and services. Users can turn off data collection by using the **Usage Data** setting on their devices. You have no control over the collection of this data.

## Next steps

- [What are app protection policies?](app-protection-policy.md) 
