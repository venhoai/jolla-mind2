# How to Set Up an App Password for Your Gmail Account and Use It with Venho

## Step 1: Enable 2-Step Verification on Your Google Account

Before generating an app password, you need to enable 2-Step Verification on your Google Account. This adds an extra layer of security to your account.

**Instructions:**

- **Go to your Google Account settings:**
    - Open your browser and navigate to [Google Account](https://myaccount.google.com/).
- **Sign in to your Google Account**.
- **Navigate to the "Security" section:**
    - On the left sidebar, click on **Security**.
- **Enable 2-Step Verification:**
    - Scroll down to the *Signing in to Google* section.
    - Click **2-Step Verification** and follow the on-screen instructions to enable it.
    - You will likely need to enter your phone number and verify it via text or call.

For more detailed instructions, visit Google's [2-Step Verification setup](https://support.google.com/accounts/answer/185839).

---

## Step 2: Generate an App Password

After enabling 2-Step Verification, you can generate an app password, which is a special code used to connect to third-party apps like Venho that don’t directly support 2-Step Verification.

**Instructions:**

- **Go to the App Passwords page:**
    - Visit [Google App Passwords](https://myaccount.google.com/apppasswords) while logged into your Google Account.
- **Sign in again, if prompted**.
- **Generate the App Password:**
    - In the **Select app** dropdown, choose **Other (Custom name)** and enter `"Venho"` as the custom name.
    - In the **Select device** dropdown, choose your device or app, or select **Other** to give a custom name.
    - Click **Generate**.
- **Copy the App Password:**
    - A 16-character password will appear. Copy this password to your clipboard, as it will not be shown again.

For more guidance on generating app passwords, check Google's [App Passwords support page](https://support.google.com/accounts/answer/185833).

---

## Step 3: Use the App Password to Connect to Venho with IMAP

Now that you have your app password, you can use it to connect your Gmail account to Venho via IMAP.

**Instructions for Connecting Gmail to Venho:**

- Open Venho.
- Navigate to the **Messaging Center**
    - Choose **Add Account**.
- **Enter the following IMAP settings:**
    - **Incoming Mail (IMAP) Server:**
        - **Server:** `imap.gmail.com`
        - **Port:** `993`
    - **Outgoing Mail (SMTP) Server:**
        - **Server:** `smtp.gmail.com`
        - **Port:** `465`
    - **Username:**
        - Enter your full Gmail address (e.g., `yourname@gmail.com`).
    - **Password:**
        - Instead of your regular Gmail password, enter the **App Password** you generated earlier.
