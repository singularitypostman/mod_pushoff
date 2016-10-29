# mod_push

mod_push implements push for ejabberd.

Currently supporting [APNS (Apple push notification service)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html).

## Prerequisites

* Erlang/OTP 19 or higher
* ejabberd 16.09 or higher

## Installation

```bash
git clone https://github.com/royneary/mod_push.git
# copy the source code folder to the module sources folder of your ejabberd
# installation (substitute ~ for the home directory of the user that runs ejabberd)
sudo cp -R mod_push ~/.ejabberd-modules/sources/
# if done right ejabberdctl will list mod_push as available module
ejabberdctl modules_available
# automatically compile and install mod_push
ejabberdctl module_install mod_push 
```

## Example configuration

```yaml
modules:
  mod_offline: {}
  mod_push:
    backends:
      -
        type: apns
        register_host: "localhost"
        # make sure this pem file contains only one(!) certificate + key pair
        certfile: "/etc/ssl/private/apns_example_app.pem"
```

## Usage

Clients can register for push notifications by sending adhoc requests using XEP-0004.

These are the available adhoc commands:

* `register-push-apns`: register at an APNS backend
* `list-push-registrations`: request a list of all registrations of the requesting user
* `unregister-push`: delete the user's registrations

Example:
```xml
<iq type='set' to='localhost' id='execute'>
  <command xmlns='http://jabber.org/protocol/commands'
           node='register-push-apns'
           action='execute'>
    <x xmlns='jabber:x:data' type='submit'>
      <field
      var='token'>
        <value>r3qpHKmzZHjYKYbG7yI4fhY+DWKqFZE5ZJEM8P+lDDo=</value>
      </field>
    </x>
  </command>
</iq>
```

See [client.py](client.py) for a reference client.
