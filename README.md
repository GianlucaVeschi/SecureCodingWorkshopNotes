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


## Insecure Communication
In order to protect user's data when transmitting information to/from a server a developer should always keep in mind to use secure protocols such as HTTPs.
 
- Self signed or untrusted certificates must NOT be accepted. 
- Ensure that there are no hardcoded API keys in the request when communicating to a backend server
- User credentials should be handled securely and not be stored locally 
- Any access tokens should implement an appropriate timeout
- Weak passwords should be prevented by ensuring a password policy is implemented

A refresh token shouldn't be sent as a cookie header since it can be seen by an attacker, we send a refresh token in a POST request to get back an access token which is used in cookie headers.

To implement a remember me functionality securely, a refresh token will generate an access token which will be sent as cookie headers along with a device id. This access token can be revoked by the server at any time.

## Broken Cryptography

### sdsfsdfsf
The act of encoding makes text non-human-readable, but still allows an attacker to decode the text without any need for secrets, such as a key or password. Sensitive data should instead be encrypted.

- *Encoding* is for maintaining data usability and can be reversed by employing the same algorithm that encoded the content, i.e. no key is used. Examples are ASCII, Unicode, URL Encoding, Base64

- *Encryption* is for maintaining data confidentiality and requires the use of a key (kept secret) in order to return to plaintext. Examples are AES,Blowfish, RSA

### Reuse of Initialization Vector

An initialization vector (IV) is an arbitrary number that can be used along with a secret key for data encryption.

It should be avoided to use the same IV over multiple encryption operations as this would make the encryption less random. 

Developer should be sure to properly initiate GCM mode. (Galois/Counter Mode) and to create random IVs for each different encryption. 



