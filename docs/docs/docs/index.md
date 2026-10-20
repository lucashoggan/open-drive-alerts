# Welcome

A privacy-preserving protocol to alert drivers about different events for a better driving experience, regardless of map platform.

## Why are We Creating this?
- When driving, alerts can be very usefull, whether they are used for more accurate journey time or avoiding congestion soon to occur due to an accident.
- When using other platforms, you location, speed, alert time and other metrics are sitting together, to be collected, used or in the worst case, stolen.
- We want to seperate an user from their location and alert.
- Also, by allowing any map provider to work with us, we create a better experience for everyone with more accurate alerts.

## High-level Structure

``` mermaid
flowchart TD
    subgraph map-provider[Map Provider]
        direction LR
        map-provider-server[Map Provider Auth Server] -->|optional| map-provider-db[Map Provider Database]
    end
    subgraph alert-server[Centralized Alert Server]
        direction LR
        alert-server-service[Post Endpoint] -->|stores alert information| alert-server-db[Database]
        alert-server-db --> |makes alerts avalible| alert-publish-server-db[Request / Publish Endpoint]
    end
    client[Client] -->|proxies through| map-provider
    map-provider -->|forwards stripped & authenticated request| alert-server
```

### Client
- When posting, the alert information (location, type) are encrypted to only be seen by centralized server.
- Requests are proxied through their map provider.

### Map Provider Service
- Mainly to provide authentication and rate-limiting features that protect the centralized alert server
- Verify users in whichever way the service wants, then if verified, the still encrypted alert information is forwarded to the alert server.

### Alert Server
- Only has to trust map providers.
- Publishes alerts in batches to further obsfuscate requests.