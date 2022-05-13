# SecureCodingWorkshopNotes

## 1 - Improper Platform Usage

### TapJacking
Use 'filterTouchesWhenObscured' to 'true' so that the Android framework will ignore all taps while a view has been drawn on top of it.

### WebView settings

To prevent Webview Settings vulnerabilities, developers should:
- Examine how Webviews can potentially enable access to local files on the user’s device
- Ensure SSL Error Handling is properly and securely implemented
- If possible, keep Javascript execution disabled
- Implement additional security measures if the app uses Javascript Interface
- Prevent Man in the Middle and Cross-site Scripting attacks by validating content from third parties

When setting the setJavaScriptEnabled to true, an adversary could abuse this to execute malicious code. Setting it to false will block all javascript code running in the webView.

## 2 - Uninted Data Leakage

### Application Background Screenshots

- When going into background the app should be blurred as 

- Avoid showing sensitive data within a toast. Since toasts are shown system-wide, sensitive data could become exposed by other apps taking screenshots of every toast. There is, however, a library which is able to secure it. The CWAC-Security library prevents toasts from being displayed system-wide. It is now impossible for other apps to screenshot or screencast the toast.

### Plain Text Storage Of Credentials
Rooted or Jailbroken devices are particularly at risk to having their File System exposed to malicious apps, therefore passwords should never be stored locally.

To avoid this vulnerability, developers should opt for token based authentification to secure user's credentials.

If it is strictly necessary to encrypt the data locally then the correct algorithm should be picked.

- For local storage, an asymmetric algorithm will provide NO valuable extra security because they are slow and not suitable for bulk storage.
- Encrypting the user credentials before storing them will successfully hide them from any attacker who cannot access the encryption keys. For local data storage, it is advised to use a *symmetric encryption algorithm*, particularly AES. ( This algorithm is best used in GCM mode with NoPadding, which implies that the initialisation vectors (IVs) should be stored as well. These can be stored in plaintext and need not be kept secret )

## 3 - Insecure Communication

### Communication over clear text protocol & Weak certificate validation

In order to protect user's data when transmitting information to/from a server a developer should always keep in mind to use secure protocols such as HTTPs.
 
- Self signed or untrusted certificates must NOT be accepted. 
- Ensure that there are no hardcoded API keys in the request when communicating to a backend server
- User credentials should be handled securely and not be stored locally 
- Any access tokens should implement an appropriate timeout
- Weak passwords should be prevented by ensuring a password policy is implemented

A refresh token shouldn't be sent as a cookie header since it can be seen by an attacker, we send a refresh token in a POST request to get back an access token which is used in cookie headers.

To implement a remember me functionality securely, a refresh token will generate an access token which will be sent as cookie headers along with a device id. This access token can be revoked by the server at any time.

## 4 - Insecure Authentication

### Storing credentials with "remember me functionality"

## 5 - Insufficient Cryptography

### Use of insecure/deprecated algorithms

The act of encoding makes text non-human-readable, but still allows an attacker to decode the text without any need for secrets, such as a key or password. Sensitive data should instead be encrypted.

- *Encoding* is for maintaining data usability and can be reversed by employing the same algorithm that encoded the content, i.e. no key is used. Examples are ASCII, Unicode, URL Encoding, Base64

- *Encryption* is for maintaining data confidentiality and requires the use of a key (kept secret) in order to return to plaintext. Examples are AES,Blowfish, RSA

### Reuse of Initialization Vector

An initialization vector (IV) is an arbitrary number that can be used along with a secret key for data encryption.

It should be avoided to use the same IV over multiple encryption operations as this would make the encryption less random. 

Developer should be sure to properly initiate GCM mode. (Galois/Counter Mode) and to create random IVs for each different encryption. 

## 6 - Insufficient Authorization

### Using inputs from untrusted sources

## 7 - Client Code Quality

It’s not recommended to transmit user roles from the mobile device to the server. Avoid relying on any roles or permission information that come from the mobile device.

```
@FormUrlEncoded
@POST("/refreshToken")
fun refreshToken(@Field("refresh_token") refreshToken: String,
                 @Field("role") userRole: Role): Response<AuthResponse>
```                 

It’s recommended to verify the roles and permissions of authenticated users with information only available on the backend. This prevents an adversary to manipulate his role or permission from the mobile device to the server, thus gaining unauthorized access.

### Javascript Injection

When putting the javaScriptEnabled boolean to false, all JavaScript code running in the Webview will be blocked thus preventing an adversary to be able to execute malicious code. Implementing a custom web chrome client we are able to intercept the geolocation request and prompt the user with a dialog to grant geolocation permissions.

## 8 - Code Tampering

Typically, an attacker will exploit code modification via malicious forms of the apps hosted in third-party app stores. The attacker may also trick the user into installing the app via phishing attacks.

### Tampering Detection
To make sure our app has not been modified we can keep a reference to the original package name inside our app and compare this string against the package name when the app is opened. This way our app will never run with a different package name and we will make sure we are running the original version.

In this case, we are keeping PACKAGE_NAME as a string in our class which could be decompiled, to add an extra level of security we could store it on a different, encrypted or on a remote server.

### Backups Enabled
In most Android Devices application backups are enabled as a factory setting and some data is saved to the local file system, which is particularly susceptible to attacks in rooted devices.

To prevent vulnerabilities associated with Enabled Backups, developers should:
- Avoid backing up sensitive data in files that can be easily accessed from back-end systems
- Disable automatic backups from the application entirely

Good practices are:
- to put the ```android:allowBackup="true"``` to ```false``` in the ```AndroidManifest.xml``` when creating a new project
- to check if the device is rooted and completely stop the use of the app when detecting a rooted device.

Checking for both known root cloakers and known root packages will detect the more advanced rooting software. Using ' su ' and 'busybox' will detect the less advanced rooting software. Root detection should be performed during app initialisation. Please note that these are best-effort measures. It is nearly impossible to detect all possible rooting software. It is nonetheless crucial to limit the amount of active rooted users as much as possible.

## 9 - Reverse Engineering

Process or method through which one attempts to understand through deductive reasoning how a previously made device, process, system, or piece of software accomplishes a task with very little (if any) insight into exactly how it does so.

### Emulation Detection
Attackers often use emulator devices to find vulnerabilities in mobile applications, hence preventing the app to execute when detecting an emulator can be even more effective than code obfuscation.

To check if the model name contains the word emulator isn’t enough to prevent emulation. Some emulators will mask or mock a real phone name, hence more sophisticated methods have to be used.

### Code Obfuscation
Obfuscation is a process where the code is made extremely difficult for humans to read or tamper with. If the code of an app is not obfuscated it is much more vulnerable to reverse engineering.

To prevent Reverse Engineering vulnerabilities developer should 
- Try to detect the susceptibility of an app by reverse engineering the app store version easily available tools.
- Use a binary inspection tool such as IDA Pro, Hopper, otool or strings against the binary.
- Use an obfuscation tool like Proguard or Dexguard
- Ensure anti-debugging code is implemented in the application. For Android, check for the ```isDebuggable``` flag in the manifest file.
- Make sure emulation prevention is in place through frameworks like Mobile substrate, Xposed or tools like Cycript.

*Proguard* obfuscates your code by removing unused code and renaming classes, fields, and methods with semantically obscure names which make the code base, smaller and more efficient. The result is a smaller sized .apk file that is more difficult to reverse engineer

### Password Autofill
