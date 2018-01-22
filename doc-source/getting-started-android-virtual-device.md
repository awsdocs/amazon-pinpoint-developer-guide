# Creating an Android Virtual Device<a name="getting-started-android-virtual-device"></a>

To send push notifications from Amazon Pinpoint to an Android app, you must install the app on an Android device\. For testing purposes, you can use a virtual device\. This topic describes how to create a virtual device in Android Studio\. If you have a physical Android device and know how to install apps onto it using Android Studio, you can skip this step\. 

This topic helps you create a virtual device of type Nexus 6 phone with Lollipop API Level 22 x86 Android 5\.1, but the AWS Mobile Hub quickstart app is not limited to this device\. You can use any device that uses Android OS 4\.0\.3 \(IceCreamSandwich\) API Level 15 or higher\.

**To create a virtual device with Android Studio**

1. Open Android Studio\.

1. From the **Tools** menu, choose **Android**, and then choose **AVD Manager**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-android-amh-quickstart-virtual-device-avd-manager.png)

1. On the **Your Virtual Devices** page, choose **Create Virtual Device**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-android-amh-quickstart-virtual-device-create.png)

1. On the **Select Hardware** page, choose **Nexus 6**, and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-android-amh-quickstart-virtual-device-hardware.png)

1. On the **System Image** page, choose the system image with **Release name** Lollipop, **API Level** 22, **ABI** x86, and **Target** Android 5\.1 \(with Google APIs\), and then choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-android-amh-quickstart-virtual-device-image.png)

1. On the **Android Virtual Device** page, choose **Finish**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-android-amh-quickstart-virtual-device-finish.png)

1. To enable Android Studio to install an app on the virtual device, go to the **Tools** menu, choose **Android**, and then choose **Enable ADB Integration**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/push-android-amh-quickstart-virtual-device-enable-adb.png)