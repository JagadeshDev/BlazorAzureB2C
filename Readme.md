# Blazor Azure B2C Authentication and Authorization

## Technologies Used

1. Blazor, 
1. Azure B2C, 
1. .NET 6.0

## Authentication and Authorization
![Authentication and Authorization](./Documentation/Images/authentication-authorization.svg)

## What is Azure Active Directory B2C?

Azure Active Directory B2C provides business-to-customer identity as a service. Your customers use their preferred social, enterprise, or local account identities to get single sign-on access to your applications and APIs.

![Authentication and Authorization](./Documentation/Images/azureadb2c-overview.png)

Azure B2c flow
![Authentication and Authorization](./Documentation/Images/B2C_Flow.png)

## Set-Up An Azure Active Directory B2C Tenant
Creating an **Azure Active Directory B2C Tenant** requires an Azure Account.

Sign in to the **Azure Portal** using an existing account at: https://portal.azure.com/ or sign-up for a new one at: https://azure.microsoft.com/en-us/free/.

> **Note**: An Azure account that's been assigned at least the Contributor role within the Azure subscription or a resource group within the subscription is required.

Follow the directions here: Tutorial - Create an Azure Active Directory B2C tenant | Microsoft Docs

In the **Azure Portal**, click Create a resource and search for **Azure Active Directory B2C** and press **enter**.

![Step1](./Documentation/Images/Create_B2C/Step1.png)

Click **Create**

![Step2](./Documentation/Images/Create_B2C/Step2.png)

Click **Create a new Azure AD B2C Tenant**.

![Step3](./Documentation/Images/Create_B2C/Step3.png)

Fill out the form and click **Review + Create**.

![Step4](./Documentation/Images/Create_B2C/Step4.png)

After the **Azure B2C Tenant** is set up, search for it and select it using the **All resources**.

![Step5](./Documentation/Images/Create_B2C/Step5.png)

This will direct you to a screen that will display a link to allow you to navigate to the **tenant**.

![Step6](./Documentation/Images/Create_B2C/Step6.png)

## Register A Web Application

To allow the application, to interact with Azure Active Directory B2C Tenant it must be registered.
Select **App Registrations**, then **New registration**.

![Step1](./Documentation/Images/Register_App/Step1.png)

Give the application a **Name**, Select **Accounts in any identity provider or organizational directory**, also **Grant admin consent to openid and offline_access permissions**, and click Register.

The **App Registration** will be created.

![Step2](./Documentation/Images/Register_App/Step2.png)

## Configure Azure B2C Application Identity Provider & User Flow

In the **Azure B2C Tenant**, click **User flows** then select **New user flow**.

![Step1](./Documentation/Images/User_Flow/Step1.png)

Select **Sign up and sign In**.

![Step2](./Documentation/Images/User_Flow/Step2.png)

Select **Recommended** for the **Version** and click **Create**.

![Step3](./Documentation/Images/User_Flow/Step3.png)

Give the **Flow** a name and select **None** for **Local accounts** and **Microsoft Account** for **Social Identity providers**.

![Step4](./Documentation/Images/User_Flow/Step4.png)

Select options for Multifactor authentication.

![Step5](./Documentation/Images/User_Flow/Step5.png)

For **User attributes and token claims**, select **Show more…**

![Step6](./Documentation/Images/User_Flow/Step6.png)

Select the options in the image below.

Click **Ok**. Click **Create**

![Step7](./Documentation/Images/User_Flow/Step7.png)

The **Flow** will show in the **User flows** section.

![Step8](./Documentation/Images/User_Flow/Step8.png)

## Create The Blazor Server Azure B2C Application

Using Visual Studio 2022 **Create a new project**.

![Step1](./Documentation/Images/Blazor/Step1.png)

Select **Blazor Server App**.

![Step2](./Documentation/Images/Blazor/Step2.png)

Name the project **BlazorAzureB2C** and click **Next**.

![Step3](./Documentation/Images/Blazor/Step3.png)

Select **.Net 6.0, Microsoft identity platform**, Configure for **HTTPS**, and click **Create**.

![Step4](./Documentation/Images/Blazor/Step4.png)

When the **Required components** box pop up, click the **Finish** button.

![Step5](./Documentation/Images/Blazor/Step5.png)

## Add the Redirect URI in the Azure App Registration

In the **Solution Explorer**, open the **launchSettings.json** file in the **Properties** folder.

![Step1](./Documentation/Images/Add_Redirect_URL/Step1.png)

Copy the **applicationUrl** in the **Profiles** section, that begins with **https**.

![Step2](./Documentation/Images/Add_Redirect_URL/Step2.png)

In the **Azure Portal**, search for the **App registration** created earlier and select it.

![Step3](./Documentation/Images/Add_Redirect_URL/Step3.png)

Select **Add a Redirect URI**.

![Step4](./Documentation/Images/Add_Redirect_URL/Step4.png)

Select **Add a platform**.

![Step5](./Documentation/Images/Add_Redirect_URL/Step5.png)

Select **Web**.

![Step6](./Documentation/Images/Add_Redirect_URL/Step6.png)

Enter the **URL** of the **Blazor** application (copied earlier), with **/signin-oidc** at the end.

Check the **Access** and **ID tokens** boxes, and click the **Configure** button.

> **Note**: When you deploy the application to a **production** website, you will need to add the **URI** of that address to this page.

![Step7](./Documentation/Images/Add_Redirect_URL/Step7.png)

Update the appsettings.json File

![Step8](./Documentation/Images/Add_Redirect_URL/Step8.png)

In **Visual Studio**, in the **Solution Explorer**, open the **Appsettings.json** file.

Replace all the contents with the following:

```
{
  /*
The following identity settings need to be configured
before the project can be successfully executed.
For more info see https://aka.ms/dotnet-template-ms-identity-platform 
*/
  "AzureAd": {
    "Instance": "https://{Your Azure B2C Domain Name}.b2clogin.com/tfp/",
    "ClientId": "{Your Application (client) ID}",
    "CallbackPath": "/signin-oidc",
    "Domain": "{Your Azure B2C Domain Name}.onmicrosoft.com",
    "SignUpSignInPolicyId": "{Your Signup policy}",
    "ResetPasswordPolicyId": "",
    "EditProfilePolicyId": ""
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}

```