# How to Set Up an App Password for Your Zoho Account and Use It with Venho

## Step 1: Enable 2-Step Verification on Your Zoho Account

Before you can generate an app password, you need to have 2-Step Verification enabled on your Zoho Account. This adds an extra layer of security to your account.

**Instructions:**

- **Go to your Zoho Account settings:**
    - Open your browser and go to the [Zoho Account page](https://accounts.zoho.com/).
- **Sign in to your Zoho Account**.
- **Navigate to the "Security" section:**
    - In the left sidebar, click on **Security**.
- **Enable 2-Step Verification:**
    - Scroll down to the **2FA (Two-Factor Authentication)** section.
    - Click **Enable 2FA** and follow the on-screen instructions to enable it.
    - You will likely need to enter your mobile number and verify it through SMS or an authentication app (like Google Authenticator).

For more detailed instructions, visit Zoho's [2-Step Verification setup guide](https://www.zoho.com/accounts/help/sign-in/two-factor-authentication.html).

---

## Step 2: Generate an App Password

After enabling 2-Step Verification, you can generate an app password, which is a special code used to connect to third-party apps like Venho.

**Instructions:**

- **Go to the App Passwords page:**
    - Visit [Zoho App Passwords](https://accounts.zoho.com/u/h#security/appPasswords) while logged into your Zoho Account.
- **Sign in again, if prompted**.
- **Generate a new app password:**
    - In the **App Passwords** section, click **Generate a new app password**.
    - Choose a custom name like `"IMAP Access"` for the app you're setting up.
    - Click **Generate** to create the 16-character password.
- **Copy the App Password:**
    - A 16-character password will appear. Copy this password to your clipboard, as it will not be shown again.

For more guidance on generating app passwords, check Zoho's [App Passwords support page](https://www.zoho.com/accounts/help/security/app-passwords.html).

---

## Step 3: Use the App Password to Connect to Venho with IMAP

Now that you have your app password, you can use it to connect your Zoho account to a third-party app like Venho that supports IMAP.

**Instructions for Connecting Zoho to a Third-Party App:**

- Open Venho.
- Navigate to the **Messaging Center**
    - Choose **Add Account**.
- **Enter the following IMAP settings:**
    - **Incoming Mail (IMAP) Server:**
        - **Server:** `imap.zoho.com`
        - **Port:** `993`
    - **Outgoing Mail (SMTP) Server:**
        - **Server:** `smtp.zoho.com`
        - **Port:** `465` or `587`
    - **Username:** Enter your full Zoho email address (e.g., `yourname@zoho.com`).
    - **Password:** Enter the **App Password** you generated earlier instead of your regular Zoho password.
