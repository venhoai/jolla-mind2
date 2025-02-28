# How to Set Up an App Password for Your Yahoo Account and Use It with Venho

## Step 1: Enable 2-Step Verification on Your Yahoo Account

Before generating an app password, you need to enable 2-Step Verification (2FA) on your Yahoo account to add an extra layer of security.

**Instructions:**

- **Go to your Yahoo Account settings:**
    - Open your browser and go to the [Yahoo Account page](https://login.yahoo.com).
- **Sign in to your Yahoo Account**.
- **Navigate to the "Account Security" section:**
    - In the left sidebar, click on **Account Security**.
- **Enable 2-Step Verification:**
    - Click **Turn on 2-step verification** and follow the instructions to complete the process.
    - You’ll need to enter your phone number and verify it through a text message or call.

For more detailed instructions, visit Yahoo's [2-Step Verification setup guide](https://help.yahoo.com/kb/enable-2step-verification-sln2053.html).

---

## Step 2: Generate an App Password

After enabling 2-Step Verification, you can generate an app password to connect to apps like Venho that don’t directly support 2FA.

**Instructions:**

- **Go to the App Passwords page:**
    - Visit the [Yahoo App Passwords page](https://login.yahoo.com/account/security) while logged into your Yahoo Account.
- **Sign in again if prompted**.
- **Generate a new app password:**
    - Under the **App passwords** section, click **Generate app password**.
    - Select **Other App** from the drop-down menu.
    - In the **custom name** field, enter `"Venho"` and click **Generate**.
- **Copy the App Password:**
    - A 16-character password will appear. Copy this password to your clipboard, as it will not be shown again.

For more details on generating app passwords, check out Yahoo's [App Passwords page](https://help.yahoo.com/kb/generate-app-password-sln15241.html).

---

## Step 3: Use the App Password to Connect to Yahoo with Venho via IMAP

Now that you have your app password, you can use it to connect your Yahoo account to Venho via IMAP.

**Instructions for Connecting Yahoo to Venho:**

- Open Venho.
- Navigate to the **Messaging Center**
    - Choose **Add Account**.
- **Enter the following IMAP settings:**
    - **Incoming Mail (IMAP) Server:**
        - **Server:** `imap.mail.yahoo.com`
        - **Port:** `993`
     - **Outgoing Mail (SMTP) Server:**
        - **Server:** `smtp.mail.yahoo.com`
        - **Port:** `465`
    - **Username:**
        - Enter your full Yahoo email address (e.g., `yourname@yahoo.com`).
    - **Password:**
        - Instead of your regular Yahoo password, enter the **App Password** you generated earlier.
