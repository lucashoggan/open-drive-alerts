# Alert Server API Reference


## Get Alert Server's Public Key

*Request*

`GET /auth/pub-key`

```json
{
    "map-provider-private-key": string
}
```

*Response*

```json 
{
    "pub_key": string,
    "expiry": string // ISO format datetime (YYYY-MM-DD HH:MM:SS)
}
```


## Post a User's Alert

*Request*

`POST /alerts/post`

```json
{
    "alert-payload": string ({        
        "alert-id": number, // value of alert
        "road-id": string, // OSM Road Index
        "lat": number, // Latitude
        "lon": number, // Longitude
    }), // This object is turned into a base64 string encrypted by server's public key
    "user-public-key": string
}
```

*Response*

`None`

## Query for Alerts in Given H3 index

*Request*

`GET /alerts/query/H3`

```JSON
{
    "h3-index": string, // encrypted by server public key
    "user-public-key": string
}
```

*Response*

```JSON
{
    "db-id": number, // ID of alert in database, used to update alerts later
    "alert-id": number, // ID of alert e.g 0 = Crash
    "lat": number, // latitude
    "lon": number // longitude
}[] /* Each object is turned to a base64 
    string encrypted by user public key */
```

## User says that alert is "still there" or "not there"

*Request*

`POST /alerts/update`

```JSON
{
    
}
```

