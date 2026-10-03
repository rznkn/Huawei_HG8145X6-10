# Native SSH public-key login notes

These observations apply to HG8145X6-10 firmware V5R024C10S106. Other firmware may behave differently. Use only equipment you administer. Export and preserve the configuration before changing authentication, keep a working administrator session open, and retain a recovery path. This supplements the existing LAN SSH instructions; it does not enable WAN access or a Linux shell.

## Transport and console are separate

The SSH account and the subsequent WAP `Login:` prompt are separate authentication steps. A web administrator account is not necessarily an SSH account. In the tested configuration, SSH used `root`, followed by a separate console login. Enter console credentials locally; do not put them in command arguments or logs.

At `WAP>`, inspect the native commands:

```text
display ssh authentication-type
load ssh-pubkey ?
```

The import command is a WAP command. Running it at a Mac/Linux shell produces “command not found”.

## Import and verify the final result

Prepare an OpenSSH public-key file and serve only that file from a temporary LAN transfer service. Never serve the private key, configuration exports, or a home directory. Restrict the transfer service/firewall to the ONT, then remove both when finished. The native help advertises TFTP, FTP, HTTPS and SFTP; only TFTP was exercised for these observations.

```text
load ssh-pubkey by tftp svrip <LAN-transfer-server-IP> remotefile <public-key-file>
```

An immediate `success!` acknowledges the command; it does **not** establish that import completed. Wait for the later result:

```text
Software Operation Successful!RetCode=0x0!
```

A later `Software Operation Faild!RetCode=0xf4!` is a failed operation, even when the initial acknowledgement said success. Check the temporary server and scoped LAN firewall before retrying; do not change optical mode or replace firmware to troubleshoot this transfer.

## Test key authentication and retain recovery

Record and verify the ONT host-key fingerprint through the trusted existing session. Use strict host-key checking and the matching local private key when testing a new SSH connection. Do not disable host-key checking.

On the tested firmware, `ssh authentication-type mode 1` closed the existing SSH connection and required public-key authentication for new SSH transports. A password-only SSH invocation then returned `Permission denied (publickey)`. A successful key-authenticated transport still presented the separate WAP console login.

Do not assume a key import removes that second password prompt, or change modes before the key and recovery path are ready. Preserve the previous mode/configuration and use the authorised management recovery path if the new login fails. Unattended console automation, if required, should retrieve its console credential from a local protected credential store without printing it or passing it in command arguments.

After setup, test a harmless read in a **new** SSH session and close it; repeat once without the original session. Reboot persistence must be verified separately during an attended maintenance window.
