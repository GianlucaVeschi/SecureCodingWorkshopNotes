# SecureCodingWorkshopNotes

### TapJacking
Use 'filterTouchesWhenObscured' to 'true' so that the Android framework will ignore all taps while a view has been drawn on top of it.

## Uninted Data Leakage

### WebView settings

To prevent Webview Settings vulnerabilities, developers should:
- Examine how Webviews can potentially enable access to local files on the user’s device
- Ensure SSL Error Handling is properly and securely implemented
- If possible, keep Javascript execution disabled
- Implement additional security measures if the app uses Javascript Interface
- Prevent Man in the Middle and Cross-site Scripting attacks by validating content from third parties

When setting the setJavaScriptEnabled to true, an adversary could abuse this to execute malicious code. Setting it to false will block all javascript code running in the webView.

### Application Background Screenshots

- When going into background the app should be blurred as 

- Avoid showing sensitive data within a toast. Since toasts are shown system-wide, sensitive data could become exposed by other apps taking screenshots of every toast. There is, however, a library which is able to secure it. The CWAC-Security library prevents toasts from being displayed system-wide. It is now impossible for other apps to screenshot or screencast the toast.

### Plain Text Storage Of Credentials
Rooted or Jailbroken devices are particularly at risk to having their File System exposed to malicious apps, therefore passwords should never be stored locally.

To avoid this vulnerability, developers should opt for token based authentification to secure user's credentials.

If it is strictly necessary to encrypt the data locally then the correct algorithm should be picked.

- For local storage, an asymmetric algorithm will provide NO valuable extra security because they are slow and not suitable for bulk storage.
- Encrypting the user credentials before storing them will successfully hide them from any attacker who cannot access the encryption keys. For local data storage, it is advised to use a *symmetric encryption algorithm*, particularly AES. ( This algorithm is best used in GCM mode with NoPadding, which implies that the initialisation vectors (IVs) should be stored as well. These can be stored in plaintext and need not be kept secret )




