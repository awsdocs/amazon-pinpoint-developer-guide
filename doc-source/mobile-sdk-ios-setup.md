# Setting Up the AWS Mobile SDK for iOS<a name="mobile-sdk-ios-setup"></a>

Before modifying your app to use Amazon Pinpoint, set up the AWS Mobile SDK for iOS\.

**To set up the AWS Mobile SDK for iOS**

1. Open your app project in Xcode\.

1. Context\-click in your project tree view, and select **Add files to "<project name>"\.\.\.** from the context menu\.

1. Locate the `AWSCore.framework` and `AWSPinpoint.framework` files from the downloaded \.zip file\. Select the files, and choose **Add**\.

1. Open a target for your project, select **Build Phases**, expand **Link Binary With Libraries**\. Then, click the \+ button, and add:

   + libsqlite3

   + libz

   + SystemConfiguration\.framework

1. Open the target for your project, choose **General** and expand the **Embedded Binaries** section\. Then, choose the plus icon \(\+\)\.

1. In the window that opens, select the AWSCore and AWSPinpoint frameworks\.

1. Choose **Capabilities**\. Enable push notification and remote notifications in background modes\.

1. Edit your `Info.plist` file to include your AWS information:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
   	<key>AWS</key>
   	<dict>
   		<key>CredentialsProvider</key>
   		<dict>
   			<key>CognitoIdentity</key>
   			<dict>
   				<key>Default</key>
   				<dict>
   					<key>PoolId</key>
   					<string>IDENTITY_POOL_ID</string>
   					<key>Region</key>
   					<string>us-east-1</string>
   				</dict>
   			</dict>
   		</dict>
   		<key>PinpointAnalytics</key>
   		<dict>
   			<key>Default</key>
   			<dict>
   				<key>AppId</key>
   				<string>APP_ID</string>
   				<key>Region</key>
   				<string>us-east-1</string>
   			</dict>
   		</dict>
   		<key>PinpointTargeting</key>
   		<dict>
   			<key>Default</key>
   			<dict>
   				<key>Region</key>
   				<string>us-east-1</string>
   			</dict>
   		</dict>
   	</dict>
   </dict>
   ```

1. If, when a user taps a push notification sent to your app, you want your app to open a URL, add the following:

   ```
   <key>LSApplicationQueriesSchemes</key>
   <array>
   	<string>http</string>
   	<string>https</string>
   </array>
   ```