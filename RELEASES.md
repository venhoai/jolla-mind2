# Venho ADA Platform - RELEASE NOTES

## 0.4.6
- Fixed IMAP username configuration in email setup.
- Improved email account setup form behavior.
- Fixed Cancel button in email setup dialog.
- Enhanced message display with better sender/recipient information.
- Improved reply interface with hidden attachments and action buttons when replying.
- Better handling of messages with multiple recipients.
- Updated help documentation links.
- Smarter messaging center interface for managing email accounts.
- Improved user experience for message threads.
- Fixed email connection issues with various providers.
- Streamlined interface for managing multiple email accounts.
- Enhanced overall email and messaging functionality.

## 0.4.5
- Lots of fixes to Messaging Center and email handling.
- Fixes to account wiping.
- Fixed issues with migrating from previous versions.
- Fixed issues with user notifications.
- More stability fixes.
- Fixes for processor pipelines.
- Fixed issues with chat messages.
- Making user take proper ownership of device. (show signup when no user binding in place)
- PDF document format fixes, different character set handling, etc.
- Email viewer
- Added back plaintext document support.
- And a bunch more bug fixes and improvements to UX.

## 0.4.2
- Brand new enhanced intelligent search process in "Memory" chat mode.
- New node based pipeline processor infrastructure. With advanced queue and task features.
- New document and email processor pipelines to support enhanced search system & new job status signalling.
- NPU acceleration with accelerated user chat, metadata extraction and summarization processor tasks!!!
- Processor queue suspension on user chat requests.
- New mailbox IMAP synchronisation system.
- Improvements to sign-up / sign-in features.
- Overhauled "Window Manager" code to make more consistent and less error prone agent views.
- Lots of improvements to the chat user experience.
- New mail account creation flows and many improvements to the Messaging Center in general.
- Ability to view references when using Memory Chat.
- Ability to view Memory Chat low-level prompt information.
- Many stability fixes and usability enhancements.
- Some code review and refactoring of core systems.
- Persistent login across browser sessions.
- Improvements to the authentication systems in general.
- Fixed issues related to PDF whitespace not being extracted correctly in some PDFs.
- Fixed issues with stylesheets not rendering correctly on OS X and Microsoft Windows.
- And a bunch more.

## 0.3.1
- Fixed reply not closing right side panel.
- Fixed extended character sets in filenames breaking processor.
- Fixed HTML emails not being processed.
- Fixed issue with mail being marked as read always.
- Fixed issue with account filter toggles in messaging center.
- Added option to wipe user account & data.
- Added favicon.
- Added proper fonts.

## 0.3.0
  - Added version number indication in bottom right :)
  - Better handling of uploads and live updates in documents and messaging center views.
  - Massive overhaul to messaging center and mailbox (IMAP/SMTP) service handling.
  - Performance improvements to document and data processing pipelines.
  - Improved document vectorization for better chat with memory results.
  - IMAP/SMTP accounts now stored on backend (unencrypted for now).
  - A bunch of upload handling and upload view fixes.
  - Overhaul to documents collection view, including resource delete functionality \o/ :)
  - General improvements to system services and stability.
  - Improved context management and functionality.
  - Many "Window Manager" bug fixes (though still needs an overhaul)
  - Updates to package dependencies and consolidation to improve builds.
  - Lots of redundant code removed and refactored.
