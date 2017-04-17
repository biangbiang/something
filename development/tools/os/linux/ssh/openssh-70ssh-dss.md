# OpenSSH 7.0 disables ssh-dss keys by default

Starting with the 7.0 release of OpenSSH, support for ssh-dss keys has

been disabled by default at runtime due to their inherit weakness.  If

you rely on these key types, you will have to take corrective action or

risk being locked out.

Your best option is to generate new keys using strong algos such as rsa

or ecdsa or ed25519.  RSA keys will give you the greatest portability

with other clients/servers while ed25519 will get you the best security

with OpenSSH (but requires recent versions of client & server).

If you are stuck with DSA keys, you can re-enable support locally by

updating your sshd_config and ~/.ssh/config files with lines like so:

	PubkeyAcceptedKeyTypes=+ssh-dss

Be aware though that eventually OpenSSH will drop support for DSA keys

entirely, so this is only a stop gap solution.

More details can be found on OpenSSH's website:

http://www.openssh.com/legacy.html
