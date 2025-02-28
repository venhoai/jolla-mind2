# How to Set Up an App Password for Your ProtonMail Account and Use It with Venho

## Step 1: Enable 2-Step Verification on Your ProtonMail Account

Before generating an app password, you need to enable 2-Step Verification (2FA) on your ProtonMail account to enhance security.

**Instructions:**

- **Go to your ProtonMail settings:**
    - Open your browser and go to [ProtonMail](https://protonmail.com).
- **Sign in to your ProtonMail account**.
- **Navigate to the "Settings" section:**
    - In the top-right corner, click on **Settings** (gear icon).
- **Enable 2-Step Verification:**
    - Go to the **Security** tab on the left sidebar.
    - Scroll down to the **2-Step Verification** section.
    - Click **Enable 2-Step Verification** and follow the on-screen instructions.
    - You’ll need to enter your phone number and verify it via SMS or an authentication app (like Google Authenticator or Authy).

For detailed instructions, visit [ProtonMail's 2-Step Verification guide](https://proton.me/support/2fa).

---

## Step 2: Generate an App Password

After enabling 2-Step Verification, you can generate an app password. This password is used to connect to third-party apps that don't support ProtonMail's 2FA directly.

**Instructions:**

- **Go to the App Passwords page:**
    - Visit [ProtonMail App Passwords](https://proton.me/support/app-passwords) while logged into your ProtonMail account.
- **Sign in again, if prompted**.
- **Generate a new app password:**
    - In the **App Passwords** section, click **Generate New Password**.
    - Choose a custom name like "**Venho Password**" to identify the app you’re using.
    - Click **Generate** to create the 16-character app password.
- **Copy the App Password**:
    - A 16-character password will appear. Copy this password to your clipboard, as it will not be shown again.

For more information on generating app passwords, check out [ProtonMail's App Passwords page](https://proton.me/support/app-passwords).

---

## Step 3: Use the App Password to Connect to ProtonMail with IMAP

Now that you have your app password, you can use it to connect your ProtonMail account to Venho using IMAP.

***Instructions for Connecting ProtonMail to Venho:**

- Open Venho and go to the **Email Configuration** or **Email Settings** section.
- **Set up your Gmail account:**
    - Choose **Add Account**.
- **Enter the following IMAP settings:**
    - **Incoming Mail (IMAP) Server:**
        - **Server:** `mail.protonmail.com`
        - **Port:** `993`
    - **Outgoing Mail (SMTP) Server:**
        - **Server:** `smtp.protonmail.com`
        - **Port:** `465`
    - **Username:**  
        - Enter your full ProtonMail email address (e.g., `yourname@protonmail.com`).
    - **Password:**  
        - Instead of your regular ProtonMail password, enter the **App Password** you generated earlier.
