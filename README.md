# Hintertuer

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-white.svg)](https://snapcraft.io/hintertuer)

## Overview

Automatically connect to a remote SSH server and remotely forward a local
port using SSH's built-in remote forwarding feature (the `-R` argument).

The various parameters for setting up the port forwarding are configured using
snap configuration parameters. Set the various values as follows (changing
the values as needed):

    snap set hintertuer local-port="443"
    snap set hintertuer local-host="localhost"
    snap set hintertuer remote-port="10443"
    snap set hintertuer remote-host="myhost.example.com"
    snap set hintertuer remote-addr="localhost"
    snap set hintertuer user="ubuntu"

This example configuration will result in port 443 (local-port) on the
client's machine (where the snap is installed) being made available on port
10443 (remote-port) on myhost.example.com (remote-host), bound to the address
localhost (local-host).

In order to improve security, the remote host's key must be configured.
You can obtain the key by using the `ssh-keyscan` utility, and configuring the
snap with that key:

    snap set hintertuer host-key="..."

Or combine it into one line:

    snap set hintertuer \
        host-key="$(ssh-keyscan $(snap get hintertuer remote-host))"

The snap will generate a new SSH key called "id_ecdsa" and try to use that
to connect to the remote host. This file is stored in the snap's common data
path (normally /var/snap/hintertuer/common/). This file can be overwritten, or
another file in that directory can be used by running the following command:

    snap set hintertuer ssh-key="id_ecdsa"

After changing any of the configuration options, make sure to restart the
snap:

    snap restart hintertuer

The name "Hintertuer" comes from the German translation for "backdoor".
