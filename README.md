## About

This container allows a user to run an SSHD server that their Foundries
Factory device can authenticate with using their private device gateway key.
It works by periodically (every 20 minutes) synchronizing all the device
public keys into the container's authorized_keys file.

Launch the contain with:
~~~
  docker run -p2222:2222 foundries/rtunnel-sshd <A foundries.io API token>
~~~

Devices can then log into this system with a command like:
~~~
 ssh -R0:localhost:22 -i /var/sota/pkey.pem -p 2222 <host or ip of your container>
~~~

The sshd server has been patched with some code to dynamically track the
reverse tunnel port for each device connection and creates files for each
connection under `/devices`. This allows fleet administrators to attach to
this container and access devices with a command like: `/devices/<my-device`.

## Options

Setting `-e FACTORY=<your factory>` will make the service only sync keys for
the given factory. *(This can be useful for users that belong to multiple
factories)*.

To improve security, you should bind mount your own SSH host keys under
`/usr/local/etc`.
