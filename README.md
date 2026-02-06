# fireflyframework-notifications-firebase

Firebase Cloud Messaging (FCM) push notification adapter for Firefly Notifications Library.

## Overview

This module is an **infrastructure adapter** in the hexagonal architecture that implements the `PushProvider` port interface. It handles all Firebase Cloud Messaging integration details, including authentication, message formatting, and delivery to mobile devices.

### Architecture Role

```
Application Layer (PushService)
    ↓ depends on
Domain Layer (PushProvider interface)
    ↑ implemented by
Infrastructure Layer (FcmPushProvider) ← THIS MODULE
    ↓ calls
Firebase Cloud Messaging API
```

This adapter can be replaced with other push providers (AWS SNS Mobile Push, OneSignal) without changing your application code.

## Installation

Add these dependencies to your `pom.xml`:

```xml path=null start=null
<dependency>
  <groupId>org.fireflyframework</groupId>
  <artifactId>fireflyframework-notifications-core</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>

<dependency>
  <groupId>org.fireflyframework</groupId>
  <artifactId>fireflyframework-notifications-firebase</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>
```

## Configuration

Add the following to your `application.yml`:

```yaml path=null start=null
notifications:
  push:
    provider: firebase  # Enables this adapter

firebase:
  project-id: your-firebase-project-id
  credentials-path: /path/to/service-account.json  # Optional: uses Application Default Credentials if omitted
```

### Getting Your Credentials

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Select your project (or create one)
3. Navigate to Project Settings → Service Accounts
4. Click "Generate New Private Key"
5. Download the JSON file
6. Set the path in configuration:
   ```yaml
   firebase:
     credentials-path: /etc/firebase/service-account.json
   ```

Alternatively, use Application Default Credentials (ADC) by omitting `credentials-path`.

## Usage

Inject `PushService` from the core library. Spring automatically wires this adapter:

```java path=null start=null
@Service
public class MobileNotificationService {
    
    @Autowired
    private PushService pushService;
    
    public void sendNotificationToUser(String deviceToken, String title, String message) {
        PushNotificationRequest request = PushNotificationRequest.builder()
            .token(deviceToken)
            .title(title)
            .body(message)
            .data(Map.of(
                "type", "alert",
                "timestamp", Instant.now().toString()
            ))
            .build();
        
        pushService.sendPush(request)
            .subscribe(response -> {
                if (response.isSuccess()) {
                    log.info("Push sent: {}", response.getMessageId());
                } else {
                    log.error("Failed: {}", response.getErrorMessage());
                }
            });
    }
}
```

## Features

- **Multi-platform** - Supports iOS, Android, and web push
- **Rich notifications** - Title, body, and custom data payloads
- **Device targeting** - Send to specific device tokens
- **Reactive** - Returns `Mono<PushNotificationResponse>`
- **Error handling** - Graceful error messages for failed deliveries

### Sending with Custom Data

```java path=null start=null
Map<String, String> customData = Map.of(
    "action", "open_chat",
    "chat_id", "12345",
    "priority", "high"
);

PushNotificationRequest request = PushNotificationRequest.builder()
    .token(userDeviceToken)
    .title("New Message")
    .body("You have a new message from John")
    .data(customData)
    .build();

pushService.sendPush(request).subscribe();
```

## Switching Providers

To switch from Firebase to another push provider:

1. Remove this dependency from `pom.xml`
2. Add alternative push adapter dependency
3. Update configuration to use different provider

**No code changes required** in your services—hexagonal architecture ensures provider independence!

## Implementation Details

This adapter:
- Implements `PushProvider` interface from `fireflyframework-notifications-core`
- Uses Firebase Admin SDK for API calls
- Transforms `PushNotificationRequest` to FCM `Message` format
- Handles authentication via service account credentials or ADC
- Returns standardized `PushNotificationResponse`

## Troubleshooting

### Error: "No qualifying bean of type 'PushProvider'"

- Ensure `notifications.push.provider=firebase` is set
- Verify `firebase.project-id` is configured

### Error: "Failed to get credentials"

- Check that `credentials-path` points to a valid service account JSON
- Ensure the file has proper read permissions
- Try using Application Default Credentials

### Error: "Invalid registration token"

- Device token may be expired or invalid
- Regenerate the FCM token on the client device
- Ensure you're using the correct token for the platform (iOS/Android)

## References

- [Firebase Cloud Messaging Documentation](https://firebase.google.com/docs/cloud-messaging)
- [Firebase Admin SDK](https://firebase.google.com/docs/admin/setup)
- [Firefly Notifications Architecture](../fireflyframework-notifications/ARCHITECTURE.md)
