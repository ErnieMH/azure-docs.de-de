---
author: alexeystrakh
ms.author: alstrakh
ms.date: 06/11/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: e67f53e26f2081e1aa12f7c0e6a219c240f5027a
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060443"
---
### <a name="configure-required-ios-packages"></a>Konfigurieren erforderlicher iOS-Pakete

Beim Erstellen der App wird das Paket [automatisch verknüpft](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md). Sie müssen lediglich die nativen Pods installieren:

```bash
npx pod-install
```

### <a name="configure-infoplist-and-entitlementsplist"></a>Konfigurieren von „Info.plist“ und „Entitlements.plist“

1. Öffnen Sie im Ordner „PushDemo/ios“ den Arbeitsbereich „PushDemo.xcworkspace“, wählen Sie das oberste Projekt (PushDemo) aus, und wählen Sie anschließend die Registerkarte „Signing & Capabilities“ (Signieren und Funktionen)“ aus.

1. Aktualisieren Sie die Paket-ID so, dass sie dem im Bereitstellungsprofil verwendeten Wert entspricht.

1. Fügen Sie mithilfe der Schaltfläche „+“ zwei neue Funktionen hinzu:

    - Funktion „Hintergrundmodus“ (Aktivieren Sie auch das Kontrollkästchen „Remotebenachrichtigungen“.)
    - Funktion „Pushbenachrichtigungen“

### <a name="handle-push-notifications-for-ios"></a>Behandeln von Pushbenachrichtigungen für iOS

1. Öffnen Sie „AppDelegate.h“, und fügen Sie den folgenden Import hinzu:

    ```objective-c
    #import <UserNotifications/UNUserNotificationCenter.h>
    ```

1. Aktualisieren Sie die Liste der von „AppDelegate.h“ unterstützten Protokolle durch Hinzufügen von `UNUserNotificationCenterDelegate`:

    ```objective-c
    @interface AppDelegate : UIResponder <UIApplicationDelegate, RCTBridgeDelegate, UNUserNotificationCenterDelegate>
    ```

1. Öffnen Sie „AppDelegate.m“, und konfigurieren Sie alle erforderlichen iOS-Rückrufe:

    ```objective-c
    #import <UserNotifications/UserNotifications.h>
    #import <RNCPushNotificationIOS.h>

    ...

    // Required to register for notifications
    - (void)application:(UIApplication *)application didRegisterUserNotificationSettings:(UIUserNotificationSettings *)notificationSettings
    {
     [RNCPushNotificationIOS didRegisterUserNotificationSettings:notificationSettings];
    }

    // Required for the register event.
    - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
     [RNCPushNotificationIOS didRegisterForRemoteNotificationsWithDeviceToken:deviceToken];
    }

    // Required for the notification event. You must call the completion handler after handling the remote notification.
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
    fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
    {
      [RNCPushNotificationIOS didReceiveRemoteNotification:userInfo fetchCompletionHandler:completionHandler];
    }

    // Required for the registrationError event.
    - (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error
    {
     [RNCPushNotificationIOS didFailToRegisterForRemoteNotificationsWithError:error];
    }

    // IOS 10+ Required for localNotification event
    - (void)userNotificationCenter:(UNUserNotificationCenter *)center
    didReceiveNotificationResponse:(UNNotificationResponse *)response
             withCompletionHandler:(void (^)(void))completionHandler
    {
      [RNCPushNotificationIOS didReceiveNotificationResponse:response];
      completionHandler();
    }

    // IOS 4-10 Required for the localNotification event.
    - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification
    {
     [RNCPushNotificationIOS didReceiveLocalNotification:notification];
    }

    //Called when a notification is delivered to a foreground app.
    -(void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
    }
    ```
