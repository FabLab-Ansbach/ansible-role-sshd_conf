sshd_conf
=========

Manage the OpenSSH daemon configuration via template and (optionally) service state.

Requirements
------------
- Ansible 2.9 or higher.
- Target hosts must have OpenSSH server installed.

Role Variables
--------------

### General

| Variable | Default | Description |
|---|---|---|
| `sshd_service_name` | `ssh` (Debian) / `sshd` (others) | Name of the sshd service unit. |
| `sshd_package_name` | `openssh-server` | Package to install for the SSH daemon. |
| `sshd_manage_service` | `true` | Whether to start, enable, and restart the service. |
| `sshd_config` | `{}` | Dict of overrides merged on top of `default_sshd_config`. |

### `sshd_config` / `default_sshd_config` keys

Any key below can be overridden by setting it in `sshd_config`.

| Key | Default | Description |
|---|---|---|
| `listen_port` | `22` | Port sshd listens on. |
| `address_family` | `any` | Address family (`any`, `inet`, `inet6`). |
| `listen_address` | `[0.0.0.0, ::]` | List of addresses to listen on. |
| `host_key` | RSA, ECDSA, Ed25519 keys | List of host key file paths. |
| `rekey_limit` | `default none` | Session rekeying limits. |
| `syslog_facility` | `AUTH` | Syslog facility code. |
| `log_level` | `INFO` | Logging verbosity. |
| `login_grace_time` | `2m` | Time allowed to authenticate. |
| `permit_root_login` | `prohibit-password` | Root login policy. |
| `strict_modes` | `true` | Check file permissions before accepting login. |
| `max_auth_tries` | `6` | Maximum authentication attempts per connection. |
| `max_sessions` | `10` | Maximum open sessions per connection. |
| `pubkey_authentication` | `true` | Enable public-key authentication. |
| `authorized_keys_file` | `.ssh/authorized_keys` | Authorized keys file path. |
| `authorized_principals_file` | `none` | Authorized principals file. |
| `authorized_keys_command` | `none` | Command to look up authorized keys. |
| `authorized_keys_command_user` | `nobody` | User for `authorized_keys_command`. |
| `hostbased_authentication` | `false` | Enable host-based authentication. |
| `ignore_user_known_hosts` | `false` | Ignore user `~/.ssh/known_hosts`. |
| `ignore_rhosts` | `true` | Ignore `.rhosts` / `.shosts`. |
| `password_authentication` | `true` | Enable password authentication. |
| `permit_empty_passwords` | `false` | Allow empty passwords. |
| `keyboard_interactive_authentication` | `true` | Enable keyboard-interactive auth. |
| `kerberos_authentication` | `false` | Enable Kerberos authentication. |
| `kerberos_or_local_passwd` | `true` | Fall back to local password if Kerberos fails. |
| `kerberos_ticket_cleanup` | `true` | Destroy ticket on logout. |
| `gssapi_authentication` | `false` | Enable GSSAPI authentication. |
| `gssapi_cleanup_credentials` | `true` | Clean up GSSAPI credentials on logout. |
| `gssapi_use_strict_acceptor_checking` | `true` | Strict GSSAPI acceptor check. |
| `gssapi_key_exchange` | `false` | Enable GSSAPI key exchange. |
| `use_pam` | `true` | Enable PAM integration. |
| `allow_agent_forwarding` | `true` | Allow SSH agent forwarding. |
| `allow_tcp_forwarding` | `true` | Allow TCP forwarding. |
| `gateway_ports` | `false` | Allow remote hosts to connect to forwarded ports. |
| `x11_forwarding` | `false` | Enable X11 forwarding. |
| `x11_display_offset` | `10` | First display number for X11 forwarding. |
| `x11_use_localhost` | `true` | Bind X11 forwarding to localhost. |
| `permit_tty` | `true` | Allow TTY allocation. |
| `print_motd` | `true` | Print MOTD on login. |
| `print_last_log` | `true` | Print last login info. |
| `tcp_keepalive` | `true` | Send TCP keepalive messages. |
| `permit_user_environment` | `false` | Allow user environment variables. |
| `compression` | `delayed` | Compression mode (`yes`, `delayed`, `no`). |
| `client_alive_interval` | `0` | Seconds between keepalive messages to client. |
| `client_alive_count_max` | `3` | Max client alive messages without response. |
| `use_dns` | `false` | DNS lookup for remote host name. |
| `pid_file` | `/var/run/sshd.pid` | PID file path. |
| `max_startups` | `10:30:100` | Max concurrent unauthenticated connections. |
| `permit_tunnel` | `false` | Allow tunnel device forwarding. |
| `chroot_directory` | `none` | Chroot directory after authentication. |
| `version_addendum` | `none` | Text appended to SSH version string. |
| `banner` | `none` | Pre-login banner file path. |
| `accept_env` | `LANG LC_*` | Environment variables accepted from client. |
| `subsystem` | `["sftp internal-sftp"]` | List of subsystem definitions. |


Example Playbook
----------------

```yaml
    - hosts: servers
      roles:
        - role: sshd_conf
          src: https://github.com/FabLab-Ansbach/ansible-role-sshd_conf.git
          scm: git
          version: main
          vars:
            sshd_config:
              password_authentication: false
              permit_root_login: no
              listen_port: 2222
```

Testing
-------

Run automated role tests with Molecule in an ephemeral Docker container (never against your local host):

```sh
  python3 -m venv .venv
  source .venv/bin/activate
  pip install -U pip
  pip install molecule molecule-plugins[docker] ansible-core
  molecule test
```

Requirements for testing:
- Docker daemon running locally,
- Python venv support (`python3-venv` package on Debian/Ubuntu).


Alternative (if you prefer pipx):

```sh
    pipx install "molecule"
    pipx inject molecule "molecule-plugins[docker]" ansible-core
    molecule test
```

License
-------

MIT

Author Information
------------------

This Project is maintained by FabLab Ansbach e.V. (https://fablab-ansbach.de/).

Maintenance is best effort, but not guaranteed. If you want to contribute, please open an issue or a pull request.

All roles maintained by FabLab Ansbach e.V. are primarily intended to maintain our internal infrastructure, but we are happy to share them with the community. If you want to use or adapt them for your own purposes, feel free to do so.