# `jitsi-meet` User Journey

This document gives an overview of the user journey in `jitsi-meet` at the present moment.

This is based on a server deployed using [`jitsi-meet`'s Self-Hosting Guide for Debian/Ubuntu](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-quickstart) at [chat.hobcroft.org](https://chat.hobcroft.org).

# Main Success Scenario

1. User loads [chat.hobcroft.org](https://chat.hobcroft.org)

2. Browser displays landing page, with the following calls to action:

- Relating to meetings
  - Create a name for meeting
  - "Start meeting"

- Relating to user interface:
  - download Jitsi app from App Store (iOS)
  - download Jitsi app from Google Play Store (Android)
  - download Jitsi app from F-Droid (Android)

![image](https://user-images.githubusercontent.com/2212651/122654225-9b2a2800-d167-11eb-99bd-019382f8ae3d.png)

3. User enters a name for the Meeting

4. User clicks "Start meeting"

5. Browser displays meeting page, with the following calls to action:

- Relating to _inviting people_
  - "Invite more people"
  - "Invite people"

- Relating to _communicating_
  - "Mute / Unmute" toggle (defaults to Unmute)
  - "Start / Stop camera" toggle (defalts to Start)
  - "Share your screen" toggle (defaults to don't share)
  - "Share a YouTube video" (defaults to don't share)
  - "Open / Close chat" toggle (default to Close)

- Relating to _transparency_
  - "Start recording"
  - "Start livestreaming"
  - show "Speaker stats"

- Relating to _security_
  - "Enable lobby"
  - "Add password"

- Relating to signalling
  - "Raise your hand" toggle

- Related to identity
  - Set your display name
  - Set your gravater email

- UI settings
  - "Toggle tile view"
  - "Select background"
  - "Manage call quality"
  - "View full screen"
  - "Language" selector
  - "Select background"
  - "View keyboard shortcuts"

- Relating to device selection
  - "Camera" selector
  - "Microphone" selector

- Relating to moderation
  - "Mute everyone"
  - "Disable everyone's camera"
  - "Everyone starts muted" (only for moderator)
  - "Everyone starts hidden" (only for moderator)
  - "Everyone follows me" (only for moderator)

- Relating to leaving the meeting
  - "Hang up"

![image](https://user-images.githubusercontent.com/2212651/122654673-acc0ff00-d16a-11eb-8c57-7bb1357bcf4d.png)

6. User clicks "Hang up"

7. Browser displays landing page, with calls to action per step 2.

![image](https://user-images.githubusercontent.com/2212651/122655184-463de000-d16e-11eb-8b75-4ccfbd00964a.png)
