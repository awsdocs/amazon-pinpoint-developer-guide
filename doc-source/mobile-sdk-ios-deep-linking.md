# Setting Up Deep Linking<a name="mobile-sdk-ios-deep-linking"></a>

Amazon Pinpoint campaigns can take one of three actions when a user taps a notification\. One of those possible actions is a deep link, which opens the app to a specified activity\.

## Registering a Custom URL Scheme<a name="mobile-sdk-ios-deep-linking-url"></a>

To specify a destination activity for deep links, the app must have set up deep linking\. This setup requires registering a custom URL scheme the deep links will use\. To register a custom URL identifier, go to your Xcode project's target **Info** tab and expand the **URL Types** section\. 

To open the app via a `pinpoint://` URL scheme you need to assign a unique identifier to the scheme\. Apple recommends reverse DNS notation to avoid name collisions on the platform\. The following example uses `com.exampleCorp.exampleApp`:

**To register a custom URL scheme in Xcode:**

1. In Xcode, select the **Info** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_deeplink01.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. In the **URL Types** section, select \+ to add a URL type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/images/ios_deeplink02.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)

1. Enter the reverse DNS notation identifier for this URL type in **Identifier**\.

1. Enter the URL you want to use for your app in **URL Schemes**\.

1. Save the project\.

After this custom URL scheme is registered, test it in the iOS simulator\. Open Safari and navigate to your custom URL; in this example, `pinpoint://`\. Your app launches and opens to its home screen\. 

## Listening for Custom URLs<a name="mobile-sdk-ios-deep-linking-listening"></a>

To direct the app to a specific view, implement a callback in your `AppDelegate` that is called when your app launches from a deep link\. The scheme launches the app, using the host and path to go to a separate screen within the app\. The following example from the sample application shows how to implement a deep link in your application\. This link takes the user to page1 if it receives a `pinpoint://deeplink/page1` link\.

**Objective\-C**

```
-(BOOL) application: (UIApplication * ) application openURL: (NSURL * ) url sourceApplication: (NSString * ) sourceApplication annotation: (id) annotation {
  NSLog(@ "%s", __func__);
  if ([[url host] isEqualToString: @ "deeplink"]) {
    if ([[url path] isEqualToString: @ "/page1"]) {
      dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^ {
        [((UINavigationController * ) self.window.rootViewController) popToRootViewControllerAnimated: NO];
        [
          [((UINavigationController * ) self.window.rootViewController).viewControllers firstObject] performSegueWithIdentifier: @ "PAGE1_SEGUE"
          sender: self.window.rootViewController
        ];
      });
    } 
    else if ([[url path] isEqualToString: @ "/page2"]) {
      dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^ {
        [((UINavigationController * ) self.window.rootViewController) popToRootViewControllerAnimated: NO];
        [
          [((UINavigationController * ) self.window.rootViewController).viewControllers firstObject] performSegueWithIdentifier: @ "PAGE2_SEGUE"
          sender: self.window.rootViewController
        ];
      });
    } else {
      return NO;
    }
  } else {
    return NO;
  }
  return YES;
}
```