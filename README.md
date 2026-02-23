# Push-Notification

## Setup Firebase
Go to:
👉 https://console.firebase.google.com

Create new project.
1. project name
2.Gemini AI in Firebase(diable)

✅ Step 1: Click Android Icon

At the top (where you see platform icons):

Click the 🤖 Android icon

3. google-services.json  download
fcmService.ts
```bash
import Constants from "expo-constants";
// import * as Device from "expo-device";
import * as Notifications from "expo-notifications";
import { Platform } from "react-native";

export async function registerForPushNotificationsAsync() {
  let token;

  //   if (!Device.isDevice) {
  //     console.log("Must use physical device for Push Notifications");
  //     return;
  //   }

  // Request permission
  const { status: existingStatus } = await Notifications.getPermissionsAsync();

  let finalStatus = existingStatus;

  if (existingStatus !== "granted") {
    const { status } = await Notifications.requestPermissionsAsync();
    finalStatus = status;
  }

  if (finalStatus !== "granted") {
    console.log("Failed to get push token!");
    return;
  }

  // Get FCM / Expo token
  token = (
    await Notifications.getExpoPushTokenAsync({
      projectId: Constants.expoConfig?.extra?.eas?.projectId,
    })
  ).data;

  console.log("FCM / Expo Token:", token);

  if (Platform.OS === "android") {
    await Notifications.setNotificationChannelAsync("default", {
      name: "default",
      importance: Notifications.AndroidImportance.MAX,
    });
  }

  return token;
}


```

```bash
{
  "expo": {
    "name": "saldo",
    "slug": "saldo",

    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/icon.png",
    "scheme": "saldo",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "extra": {
      "eas": {
        "projectId": "4cc198a7-a618-45d3-82ec-2c20bffe0512"
      }
    },
    "ios": {
      "supportsTablet": true
    },
    "android": {
      "adaptiveIcon": {
        "backgroundColor": "#E6F4FE",
        "foregroundImage": "./assets/images/icon.png",
        "backgroundImage": "./assets/images/icon.png",
        "monochromeImage": "./assets/images/icon.png"
      },
      "edgeToEdgeEnabled": true,
      "predictiveBackGestureEnabled": false,
      "package": "com.anonymous.saldo",
      "googleServicesFile": "./google-services.json"
    },
    "web": {
      "output": "static",
      "favicon": "./assets/images/icon.png"
    },
    "plugins": [
      "expo-router",
      [
        "expo-splash-screen",
        {
          "image": "./assets/images/splash-icon.png",
          "imageWidth": 200,
          "resizeMode": "contain",
          "backgroundColor": "#010101",
          "dark": {
            "backgroundColor": "#010101"
          }
        }
      ],
      "expo-font",
      "@react-native-community/datetimepicker",
      "expo-secure-store"
    ],
    "experiments": {
      "typedRoutes": true,
      "reactCompiler": true
    }
  }
}

```








Make sure your Expo project is ready(https://expo.dev/accounts/rasel201311047)
1.Your app.json should have a valid EAS projectId:
```bash
"extra": {
  "eas": {
    "projectId": "YOUR_UUID_HERE"
  }
}
```

Get it from the Expo dashboard → Project Settings → EAS Build → Project ID. Must be a proper UUID.

2.Make sure your Android package name matches google-services.json:
```bash
"android": {
  "package": "com.anonymous.saldo",
  "googleServicesFile": "./google-services.json"
}
```
