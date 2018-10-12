# mailgun-ruby
Headlight's fork of mailgun-ruby

# Why fork?

We wanted some additional features that the main mailgun-ruby gem didn't support. To be blunt, I wasn't even sure if anyone
else would want some of these features and we don't intend on making these changes foolproof to the extent that a wider
release would necessitate.

# What changes have we made?

## 1. Change `domain` in the config block to accept a hash

This is the change that precipitated the fork to begin with.

We want to start warming up certain domains to get the reputation up. To that extent, we want a slow (but steady) percentage
of our transactional emails to be sent through the domain that's warming up.

So, we change the `domain` key in the config block to accept a hash in addition to a string, like so:

```ruby

# Original (still works)

  config.action_mailer.mailgun_settings = {
    api_key: "mykey",
    domain: "mydomain.com"
  }

# Updated (also works now)

  config.action_mailer.mailgun_settings = { 
    api_key: "mykey",
    domain: { # Sends 90% of all email through the original domain, 10% through the new domain
      "mydomain.com" => {
        percent: 90
      },
      "myotherdomain.com" => {
        percent: 10
      }
    }
  }

```
