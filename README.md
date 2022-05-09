# SecureCodingWorkshopNotes

## TapJacking
Use 'filterTouchesWhenObscured' to 'true' so that the Android framework will ignore all taps while a view has been drawn on top of it.

## WebView settings

To prevent Webview Settings vulnerabilities, developers should:
- Examine how Webviews can potentially enable access to local files on the user’s device
- Ensure SSL Error Handling is properly and securely implemented
- If possible, keep Javascript execution disabled
- Implement additional security measures if the app uses Javascript Interface
- Prevent Man in the Middle and Cross-site Scripting attacks by validating content from third parties

When setting the setJavaScriptEnabled to true, an adversary could abuse this to execute malicious code. Setting it to false will block all javascript code running in the webView.

