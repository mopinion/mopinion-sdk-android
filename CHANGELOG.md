# 0.5.0
- Introduced callback behaviour via `MopinionEvents` for callback when a form is displayed, the user has submitted a form or when a form closed.
- OS type condition for opening forms extended with version condition check, to allow matching a specific Android version.
- Fixed an issue where some web-type forms sometimes could open as in-app forms or vice versa, causing appearance differences.

# 0.4.1
- Artifact location change from bintray to githubpackages as bintray stopped service. We no longer need `jcenter()` in the gradle files.
- Requires github account.
- Rebuilt version of 0.4.0, otherwise fuctionally the same.

# 0.4.0
- Migrated to AndroidX (breaking change).
- Migrated to react-native 0.61.5 (breaking change). 
- Thumbnail and preview of screenshot.
- New random distribution algorithm for the proactive trigger condition: percentage of users.
- Autoclose for web forms.
- Margins for thank-you page.

# 0.3.6
- re-enabled header shading for react-native forms only

# 0.3.5
- removed header shading
- 64-bit ARM updates

# 0.3.4
- screenshot, theme fix

# 0.3.3
- more metadata methods

# 0.3.2
- fixed sudden crash for Android < 7.0
- webview json fix
- improved date condition

# 0.3.1
- layout fixes

# 0.3.0
- metadata methods
