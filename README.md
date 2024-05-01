<div align="center">

# :eggplant: gazi

Grab Another Zone's Information

</div>

**:eggplant: gazi** is an Web Extension grabbing site's information (HTML data, XHRs) into some daemon on the internet (and of coarse it could be your local daemon).

## Concepts

- **Gazi Receiver**: HTTP server waiting (1) identification request (2) data accumulation request.

### Full Connect Flow

```mermaid
sequenceDiagram
    actor User
    participant Extension
    participant Receiver
    User->>Extension: Enable some Gazi Manifest
    Note over Extension: read local Gazi Manifest<br/>and grab Server URL
    Extension->>Receiver: Request Gazi Manifest<br />for Update Check
    Receiver->>Extension: Respond with new Manifest<br />or 204 No Content

    alt if manifest has update
        Extension->>User: Notify Update
        User->>Extension: Accept Update<br /> and Return to Start
    end

    alt if user not accepted<br />permission
        Extension->>User: Permission Confirmation
        User->>Extension: Permission Confirmed<br />or Enable Cancelled
    end

    Extension->>Receiver: Request Login<br />with Nickname & Public Key

    alt if receiver has login challenge
        Note over Receiver: Print Login Challenge Code
        Receiver->>Extension: Login Challenge Required
        Extension->>User: Show Challenge Code Input
        User->>Extension: Type Challenge Code
        Extension->>Receiver: Re-request with Challenge Code
    end

    Receiver->>Extension: Send Signed Auth Token
```

## Gazi Manifest
