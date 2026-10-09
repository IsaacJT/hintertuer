# Hintertuer

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-white.svg)](https://snapcraft.io/hintertuer)

## Overview

Automatically connect to a remote SSH server and remotely forward a local
port using SSH's built-in remote forwarding feature (the `-R` argument).

The various parameters for setting up the port forwarding are configured using
snap configuration parameters. Set the various values as follows (changing
the values as needed):

```sh
snap set hintertuer local-port="443"
snap set hintertuer local-host="localhost"
snap set hintertuer remote-port="10443"
snap set hintertuer remote-host="myhost.example.com"
snap set hintertuer remote-addr="localhost"
snap set hintertuer user="ubuntu"
```

This example configuration will result in port 443 (local-port) on the
host localhost (local-host), as if it were accessed by the client host (where
the snap is installed), being made available on port 10443 (remote-port) on
myhost.example.com (remote-host), bound to the address localhost
(remote-addr).

In order to improve security, the remote host's key must be configured.
You can obtain the key by using the `ssh-keyscan` utility, and configuring the
snap with that key:

```sh
snap set hintertuer host-key="..."
```

Or combine it into one line:

```sh
snap set hintertuer \
    host-key="$(ssh-keyscan $(snap get hintertuer remote-host))"
```

Make sure to verify that the host key is correct and matches the intended remote
host before use:

```sh
snap get hintertuer host-key
```

The snap will generate a new SSH key called "id_ecdsa" and try to use that
to connect to the remote host. This file is stored in the snap's common data
path (normally /var/snap/hintertuer/common/). This file can be overwritten, or
another file in that directory can be used by running the following command:

```sh
snap set hintertuer ssh-key="id_ecdsa"
```

The public key will need to be added to the `.ssh/authorized_keys` files on the
remote host. View it with the following command:

```sh
cat /var/snap/hintertuer/common/"$(snap get hintertuer ssh-key)".pub
```

After changing any of the configuration options, make sure to restart the
snap:

```sh
snap restart hintertuer
```

The name "Hintertuer" comes from the German translation for "backdoor".
