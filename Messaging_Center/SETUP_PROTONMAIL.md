# How to Set Up an App Password for Your ProtonMail Account and Use It with Venho (Not supported at the moment)

At the time of writing ProtonMail allows to use IMAP only for paid accounts while SMTP can be used only for business user with custom domains in order to send emails. IMAP seem to require an additional application to be run locally [Proton Mail Bridge] (https://proton.me/support/imap-smtp-and-pop3-setup) while SMTP can be accessed generating an account token.

## Setup IMAP connection: 

To receive emails using a third-party client like venho, you would need to setup Protonmail Bridge on the Mind2 device. At the time of writing the Linux version of Protonmail Bridge is for x86_64 architecture only, therefore it needs to be built from sources. Additionally the Protonmail Bridge requires a keyring as [pass](https://www.passwordstore.org/) to be installed as it uses **secret-service** freedesktop.org API.
The instructions below assume the user has managed to setup and run the Bridge on the Mind2 device, however at the time of writing we don't have instruction on how to do that.