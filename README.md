# Firefly Framework - Notifications - Firebase

[![CI](https://github.com/fireflyframework/fireflyframework-notifications-firebase/actions/workflows/ci.yml/badge.svg)](https://github.com/fireflyframework/fireflyframework-notifications-firebase/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-21%2B-orange.svg)](https://openjdk.org)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-green.svg)](https://spring.io/projects/spring-boot)

> Firebase Cloud Messaging (FCM) push adapter for Firefly Notifications.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

## Overview

Firefly Framework Notifications Messaging implements the `PushProvider` interface from the Firefly Notifications core module using Firebase Cloud Messaging (FCM) as the delivery provider. It provides `FcmPushProvider` which handles push delivery through the Firebase Cloud Messaging (FCM) API.

The module includes auto-configuration for seamless activation when included on the classpath alongside the notifications core module. Configuration properties allow customizing API credentials and provider-specific settings.

## Features

- `PushProvider` implementation using Firebase Cloud Messaging (FCM)
- Spring Boot auto-configuration for seamless activation
- Configurable API credentials via application properties
- Standalone provider library (include alongside fireflyframework-notifications)

## Requirements

- Java 21+
- Spring Boot 3.x
- Maven 3.9+
- Firebase Cloud Messaging (FCM) account and API credentials

## Installation

```xml
<dependency>
    <groupId>org.fireflyframework</groupId>
    <artifactId>fireflyframework-notifications-firebase</artifactId>
    <version>26.02.04</version>
</dependency>
```

## Quick Start

```xml
<dependencies>
    <dependency>
        <groupId>org.fireflyframework</groupId>
        <artifactId>fireflyframework-notifications</artifactId>
    </dependency>
    <dependency>
        <groupId>org.fireflyframework</groupId>
        <artifactId>fireflyframework-notifications-firebase</artifactId>
    </dependency>
</dependencies>
```

## Configuration

```yaml
firefly:
  notifications:
    firebase:
      credentials-path: /path/to/firebase-credentials.json
      project-id: my-firebase-project
```

## Documentation

No additional documentation available for this project.

## Contributing

Contributions are welcome. Please read the [CONTRIBUTING.md](CONTRIBUTING.md) guide for details on our code of conduct, development process, and how to submit pull requests.

## License

Copyright 2024-2026 Firefly Software Solutions Inc.

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
