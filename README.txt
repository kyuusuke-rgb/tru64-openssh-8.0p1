================================================================
 OpenSSH 8.0p1 + OpenSSL 1.1.1 for Tru64 UNIX V5.1B (Alpha)
================================================================

[Description]
 Portable binary package of OpenSSH 8.0p1 and OpenSSL 1.1.1 
 built for Tru64 UNIX V5.1B (Alpha).
 Patched for Tru64 SIA (Security Integration Architecture) authentication.

[Restore Procedure] (Run as root)

 1. Extract package at root (/) directory:
    # cd /
    # tar xvzf openssh8-openssl111-tru64-v51b-portable.tar.gz

 2. Create auto-start symlink (if not present):
    # ln -s /sbin/init.d/sshd /sbin/rc3.d/S99sshd

 3. Generate host keys (Only when deploying to a NEW system):
    # /usr/local/bin/ssh-keygen -A

 4. Start SSHD service:
    # /sbin/init.d/sshd start

[Technical Notes]
 - sshd dynamically links to OpenSSL 1.1.1 shared libraries under
   /usr/local/ssl/lib.
 - Startup script (/sbin/init.d/sshd) includes LD_LIBRARY_PATH setting:
   LD_LIBRARY_PATH=/usr/shlib:/usr/local/ssl/lib
 - Tested with modern macOS SSH clients (ed25519 / chacha20-poly1305).

================================================================
