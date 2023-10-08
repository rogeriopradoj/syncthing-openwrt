# syncthing-openwrt

> - https://github.com/rogeriopradoj/syncthing-openwrt/tree/fork
> - forked from https://github.com/brglng/syncthing-openwrt

Init script and usage for ARM based OpenWrt devices.

Usage
=====

1. Login to your OpenWrt device via SSH.

2. Download and extract the Syncthing tarball, and copy the syncthing
   executable to `/usr/bin` or any other location that are in your `PATH`
   environment variable.
   ```shell
   $ curl -L -O https://github.com/syncthing/syncthing/releases/download/v1.25.0/syncthing-linux-arm64-v1.25.0.tar.gz
   $ tar -zxf syncthing-linux-arm64-v1.25.0.tar.gz
   $ cp syncthing-linux-arm64-v1.25.0/syncthing /usr/bin/
   ```

3. Run Syncthing for the first time.
   ```shell
   syncthing
   ```
   When you see a message about your Node ID that looks like this:
   ```
   [2EQK3] 15:47:15 OK: Ready to synchronize default (read-write)
   [2EQK3] 15:47:15 INFO: Node 2EQK3ZR77PTBQGM44KE7VQIQG7ICXJDEOK34TO3SWOVMUL4QFBHA is "server1" at [dynamic]
   ```
   You can press Ctrl-C to stop it.

4. Modify the Syncthing configuration file.
   ```shell
   $ vi ~/.config/syncthing/config.xml
   ```
   Look for a section that looks like this:
   ```xml
   <gui enabled="true" tls="false">
      <address>127.0.0.1:8384</address>
   </gui>
   ```
   Change it to:
   ```xml
   <gui enabled="true" tls="false">
      <address>0.0.0.0:8384</address>
   </gui>
   ```

5. Download `/etc/init.d/syncthing` from this repository and copy it to
   `/etc/init.d` and make it executable.
   ```shell
   $ cd /etc/init.d
   $ curl -L -O https://github.com/rogeriopradoj/syncthing-openwrt/raw/fork/etc/init.d/syncthing
   $ chmod +x /etc/init.d/syncthing
   ```
6. Download `/etc/config/syncthing` from this repository and copy it to
   `/etc/config`.
   ```shell
   $ cd /etc/config
   $ curl -L -O https://github.com/rogeriopradoj/syncthing-openwrt/raw/fork/etc/config/syncthing
   ```

7. Create syncthing user and group.
   ```shell
   $ grep -qxF 'syncthing:x:499:499:syncthing:/var/run/syncthing:/bin/false' /etc/passwd || echo 'syncthing:x:499:499:syncthing:/var/run/syncthing:/bin/false' >> /etc/passwd
   $ grep -qxF 'syncthing:x:499:syncthing' /etc/group || echo 'syncthing:x:499:syncthing' >> /etc/group
   ```

8. Create default /Sync directory and make syncthing user as the owner.
   ```shell
   $ mkdir /Sync
   $ chown -R syncthing /Sync
   ```

9. Enable and start the syncthing service.
   ```shell
   $ /etc/init.d/syncthing enable
   $ /etc/init.d/syncthing start
   ```

10. Open http://your-openwrt-device-address:8384 on your browser.
