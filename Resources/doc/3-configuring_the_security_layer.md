Step 3: Configuring the security layer
======================================

### A) Have a user provider that implements `OAuthAwareUserProviderInterface`

The bundle needs a service that is able to load users based on the user
response of the oauth endpoint. If you have a custom service it should
implement the interface: `HWI\Bundle\OAuthBundle\Security\Core\User\OAuthAwareUserProviderInterface`.

The HWIOAuthBundle also ships with three default implementations:

- `OAuthUserProvider` (service name: `hwi_oauth.user.provider`) - doesn't persist users
- `EntityUserProvider` (service name: `hwi_oauth.user.provider.entity`) - loads users from a database
- `FOSUserBundle` integration  (service name: `hwi_oauth.user.provider.fosub_bridge`). Checkout the documentation for integrating
  `HWIOAuthBundle` with `FOSUserBundle` for more information: (todo)

### B) Configure the oauth firewall

In the firewall configuration you will need to configure a login path for the
resource owners you configured in [step 2](https://github.com/hwi/HWIOAuthBundle/blob/master/Resources/doc/2-configuring_resource_owners.md).
Additionally you will need to point the oauth firewall to the appropriate service to use for loading users:

```yaml
# app/config/security.yml
security:
    firewalls:
        secured_area:
            oauth:
                resource_owners:
                    facebook:           "/login/check-facebook"
                    google:             "/login/check-google"
                    my_custom_provider: "/login/check-custom"
                    my_github:          "/login/check-github"
                login_path:        /login
                failure_path:      /login

                oauth_user_provider:
                    service: my.oauth_aware.user_provider.service
```

The paths configured at the `resource_owners` section should be defined in your routing.

```yaml
# app/config/routing.yml
facebook_login:
    pattern: /login/check-facebook

google_login:
    pattern: /login/check-google

custom_login:
    pattern: /login/check-custom

github_login:
    pattern: /login/check-github
```

## That was it!
That's the basic setup of the bundle. [Return to the index.](index.md) If you
are interested in giving users the option to "connect" social accounts check out: (todo).