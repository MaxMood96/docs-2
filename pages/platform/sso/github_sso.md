# Single sign-on with GitHub

You can use GitHub as an SSO provider for your Buildkite organization. To complete this tutorial, you need admin privileges for both the Buildkite organization and your GitHub organization.


## Step 1. Link your Buildkite organization to your GitHub organization

Set up the [Buildkite GitHub Application](https://github.com/apps/buildkite) for your GitHub organization. You need to install Buildkite for the GitHub organization that you want to connect to Buildkite as an SSO provider.

In your [Buildkite organization **Settings**](https://buildkite.com/organizations/~/settings)' **Repository Providers** menu item, connect your GitHub user account to Buildkite. Grant Buildkite the permission to verify your GitHub identity.

## Step 2. Create an SSO provider

1. In your [Buildkite organization **Settings**](https://buildkite.com/organizations/~/settings)' **Single Sign On** menu item, choose the GitHub provider:
<%= image "sso-settings.png", width: 1716/2, height: 884/2, alt: "Screenshot of the Buildkite SSO Settings Page" %>
1. Enter the name of your GitHub organization.
1. Click **Create Provider**.

## Step 3. Perform a test login

Follow the instructions on the provider page to perform a test login. Performing a test login verifies that SSO is working correctly before you activate it for your organization members.

## Step 4. Enable the new SSO provider

Once you've performed a test login you can enable your provider. Activating SSO will not force a log out of existing users, but will cause all new or expired sessions to authorize through GitHub before organization data can be accessed.

If you need to edit or update your GitHub provider settings at any time, you will need to [disable the SSO provider](/docs/platform/sso#disabling-and-removing-sso) first.

After you've enabled GitHub as the SSO provider for your Buildkite organization, new and expired users will need to log in through GitHub by visiting `buildkite.com/sso/your-organization-name`. They will be asked to provide their email address, and a sign-in link will be emailed to them.

Sending the sign-in link by email is an additional security and privacy measure, as a user can be a member of several Buildkite organizations. If the names of such Buildkite organizations themselves contain information – for example, `buildkite.com/sso/flyingcar` or `buildkite.com/sso/aliens`, disclosing a list of such organizations to somebody who only knows an email address could leak sensitive information.

## SAML user attributes

<%= render_markdown partial: 'platform/sso/saml_user_attributes' %>

