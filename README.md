# Samba Active Directory Domain Controller for Docker

A well documented, tried and tested Samba Active Directory Domain Controller that works with the standard Windows management tools; built from scratch using internal DNS and kerberos and not based on existing containers.

## Documentation

Latest documentation available at: [https://nowsci.com/samba-domain/](https://nowsci.com/samba-domain/)

## GitHub Container Registry publishing (tags)

The workflow publishes images to `ghcr.io` when a git tag is pushed.

- By default it uses `secrets.GITHUB_TOKEN` (with workflow `permissions.packages: write`).
- For a fine-grained personal access token, add repository secrets:
  - `GHCR_USERNAME` = your GitHub username
  - `GHCR_TOKEN` = fine-grained PAT

Fine-grained PAT permissions for the target repository:
- **Repository permissions**
  - **Contents: Read**
  - **Packages: Write**

## Environment variables

- `DOMAIN` sets the AD domain/realm.
- `DOMAINPASS` should be set to your administrator password, be it existing or new. This can be removed from the environment after the first setup run.
- `WORKGROUP` (optional) overrides the short NetBIOS/workgroup name used for provisioning. When not set, it defaults to the first label of `DOMAIN`.
- `HOSTIP` can be set to the IP you want to advertise.
- `JOIN` defaults to false and means the container will provision a new domain. Set this to true to join an existing domain.
- `JOINSITE` is optional and can be set to a site name when joining a domain, otherwise the default site will be used.
- `DNSFORWARDER` is optional and if an IP such as 192.168.0.1 is supplied will forward all DNS requests samba can't resolve to that DNS server
- `INSECURELDAP` defaults to false. When set to true, it removes the secure LDAP requirement. While this is not recommended for production it is required for some LDAP tools. You can remove it later from the smb.conf file stored in the config directory.
- `MULTISITE` defaults to false and tells the container to connect to an OpenVPN site via an ovpn file with no password. For instance, if you have two locations where you run your domain controllers, they need to be able to interact. The VPN allows them to do that.
- `NOCOMPLEXITY` defaults to false. When set to true it removes password complexity requirements including complexity, history-length, min-pwd-age, max-pwd-age
