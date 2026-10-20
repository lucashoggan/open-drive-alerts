# open-drive-alerts
Implementation of a privacy-preserving, cross-platform protocol / service which allows drivers to alert others of events such as accidents, mobile speed cameras and more across any map provider.

## Structure
- Each client will have their location / road section, their alert type (speed camera, accident etc) and a way to verify they are a user (e.g access token, email + password, passkey etc)
- Before an alert is posted, the location and alert type will be encrypted.
- When posted, the user verification and encrypted alert info will be given to a "client-service" server, this is ran by the map provider and they are just there to verify users and forward alert info.
- Once a user is verified and any abuse-control filters have been done, the encrypted alert will be passed onto the central server. The client-service server will then either verify itself or this will already be known through the connection (e.g verifying TLS credentials).
- Once the central server has the alert, it will save said alert and add it to the next batch available to view.

## Goals
- Prevent map services from seeing location data
- Prevent centralized alert-server from seeing user data
- Give the role of abuse-control and rate-limiting onto client services, reducing the centralized server's knowledge of users
- Allow multiple platforms to share alerts, this also helps obfuscate individual alerts as more alerts in a given area makes it harder to distinguish alerts made by the same user.
- Give open source and offline maps the ability to use alerts for more accurate route duration.
- Collect as little data as possible and delete as soon as possible.
- Make client-service servers as simple as possible as this reduces the cost to implement.
- Make client-service servers unable to read location data, ideally they wouldn't even be able to tell an alert came from their service when it is published / pushed.
- When a client requests alerts, the client-service server shouldn't see the locations being queried.
