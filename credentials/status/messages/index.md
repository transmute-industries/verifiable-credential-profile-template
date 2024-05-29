## Credential Status Messages

### [Valid](#valid)
The associated credential is valid.
The associated credential MAY be `revoked` or `suspended` in the future.

### [Revoked](#revoked)
The associated credential is revoked.
This status is permanent, there is no need to check status again.
The associated credential SHALL NOT be `valid` or `suspended` in the future.

### [Suspended](#suspended)

The associated credential is suspended.
The associated credential MAY be `valid` or `revoked` in the future.
