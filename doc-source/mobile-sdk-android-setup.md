# Setting up the AWS Mobile SDK for Android<a name="mobile-sdk-android-setup"></a>

Before modifying your app to use Amazon Pinpoint, you need to set up the AWS Mobile SDK for Android\.

**To set up AWS Mobile SDK for Android support in your app project**

1. Copy these AWS Mobile SDK for Android \.jar files into the app\\libs directory of your Android Studio project:

   + aws\-android\-sdk\-core\-2\.3\.4\.jar

   + aws\-android\-sdk\-cognito\-2\.3\.4\.jar

   + aws\-android\-sdk\-pinpoint\-2\.3\.4\.jar

1. Make sure your `build.gradle` file contains the following build dependencies: 

   ```
   compile fileTree(include: ['*.jar'], dir: 'libs')
   compile 'com.google.firebase:firebase-messaging:9.6.0'
   ```

   Or you can list and compile each \.jar file individually, as follows:

   ```
   compile files('libs/aws-android-sdk-core-2.3.4.jar')
   compile files('libs/aws-android-sdk-cognito-2.3.4.jar')
   compile files('libs/aws-android-sdk-pinpoint-2.3.4.jar')
   ```

1. After the dependencies brackets, push:

   ```
   apply plugin: 'com.google.gms.google-services'
   ```