+++
title = "Set up OAuth for Cloud IAP"
description = "Creating an OAuth client for Cloud IAP on Google Cloud Platform (GCP)"
weight = 2
+++

If you want to use 
[Cloud Identity-Aware Proxy (Cloud IAP)](https://cloud.google.com/iap/docs/) 
when deploying Kubeflow on GCP,
then you must follow these instructions to create an OAuth client for use
with Kubeflow.

You can skip the instructons on this page if you want to use basic 
authentication (username and password) with Kubeflow instead of Cloud IAP.
Cloud IAP is recommended for production deployments or deployments with access 
to sensitive data.

Follow the steps below to create an OAuth client ID that identifies Cloud IAP 
when requesting access to a user's email account. Kubeflow uses the email 
address to verify the user's identity.

1. Set up your OAuth [consent screen](https://console.cloud.google.com/apis/credentials/consent):
   * In the **Application name** box, enter the name of your application.
     The example below uses the name "Kubeflow".
   * Under **Support email**, select the email address that you want to display 
     as a public contact. You must use either your email address or a Google 
     Group that you own.
   * If you see **Authorized domains**, enter

        ```
        <project>.cloud.goog
        ```
        * where \<project\> is your Google Cloud Platform (GCP) project ID.
        * If you are using your own domain, such as **acme.com**, you should add 
          that as well
        * The **Authorized domains** option appears only for certain project 
          configurations. If you don't see the option, then there's nothing you 
          need to set.        
   * Click **Save**.
   * Here's an example of the completed form:   
    <img src="/docs/images/consent-screen.png" 
      alt="OAuth consent screen"
      class="mt-3 mb-3 p-3 border border-info rounded">

1. On the [credentials tab](https://console.cloud.google.com/apis/credentials):
   * Click **Create credentials**, and then click **OAuth client ID**.
   * Under **Application type**, select **Web application**.
   * In the **Name** box enter any name for your OAuth client ID. This is *not*
     the name of your application nor the name of your Kubeflow deployment. It's
     just a way to help you identify the OAuth client ID.
   * In the **Authorized redirect URIs** box, enter the following:

        ```
        https://<deployment_name>.endpoints.<project>.cloud.goog/_gcp_gatekeeper/authenticate
        ```
        * `<deployment_name>` must be the name of your Kubeflow deployment. It 
          must have the same value as the deployment name used in the next step 
          when you deploy Kubeflow from the UI or by running the deployment
          script. **Deployment name must be 4-20 characters long.**
          The default value for the deployment name is the `KFAPP` value 
          used when initializing your Kubeflow app, but you can configure this 
          with the environment variable `DEPLOYMENT_NAME`.
        * `<project>` is your GCP project. It must have the same value as used 
           in the next step when you deploy Kubeflow from the UI or by running 
           the deployment script.
    * Here's an example of the completed form:
      <img src="/docs/images/oauth-credential.png" 
        alt="OAuth credentials"
        class="mt-3 mb-3 p-3 border border-info rounded">

1. Press **Enter/Return** to add the URI. Check that the URI now appears as
  a confirmed item under **Authorized redirect URIs**. (The URI should no longer be
  editable.)
1. Make note of the **client ID** and **client secret** that appear in the OAuth
  client window. You need them later to enable Cloud IAP.

## Next steps

Choose one of the following ways to deploy Kubeflow:

* [Using the UI](/docs/gke/deploy/deploy-ui). This option provides a simple
  way to deploy Kubeflow.
* [Using the CLI](/docs/gke/deploy/deploy-cli). This option provides more
  control over the deployment process.