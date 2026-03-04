# OpenConnect WiscVPN client

A wrapper for connecting to WiscVPN using OpenConnect. Supports MFA (Duo Push) and split tunneling.

The default recommendation is to use the [GlobalProtect Client](https://kb.wisc.edu/105971),
however the proprietary client does not support the use of split tunnels.

Based on a script shared by @ERIC.SCHOVILLE.

## Requirements

* [openconnect](https://www.infradead.org/openconnect/) v8.00 or later
* [vpn-slice](https://github.com/dlenski/vpn-slice) (optional, used for split tunneling)

## Configuration

Copy `config.sample` somewhere and fill in the blanks.
If a config file path is not specified on the command line, the script will look for configuration in `$XDG_CONFIG_HOME/vpn/config`, falling back to `$HOME/.config/vpn/config` if `$XDG_CONFIG_HOME` is not set.
Know and love the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) if interested.

## Usage

	vpn [--split [args]] [config_file_path]

The script assumes you're using a Duo Push for MFA rather than a Duo passcode and
will pause while waiting for the push to be accepted.

By default all outbound network traffic will traverse the VPN.
To only send traffic bound for certain destinations through the tunnel, use `--split` and populate the `hosts` and `subnets` variables in the configuration file.
`vpn-slice` resolves hostnames at connect time and adds host routes for each returned IP.

If you want split VPN but do not want `vpn-split` to attempt to write entries into `/etc/hosts` (e.g. on a system like NixOS where that file is immutable), add the `--no-host-names` and `--no-ns-hosts` arguments as well.

When `/etc/hosts` is immutable, vpn-slice will emit harmless warnings about being unable to configure the hosts provider. These appear twice (once on connect, once on disconnect) and can be safely ignored when `--no-host-names` and `--no-ns-hosts` are also specified.

### DNS split tunneling

The `--domains-vpn-dns` option, used internally when `--split` is specified, is intended to route DNS queries for `wisc.edu` names through the VPN's nameservers.
However, vpn-slice's Linux implementation does not currently include a DNS split tunneling provider, so this option has no effect on Linux — there is an [open issue](https://github.com/dlenski/vpn-slice/issues/157) tracking this.
In practice, `wisc.edu` names resolve correctly via normal DNS for most use cases.

## Static IPs

To use a static VPN IP address, you'll first need to reserve one.
See the [KB](https://kb.wisc.edu/ns/108255) for details.

## Resources

* [OpenConnect](https://www.infradead.org/openconnect/index.html)
* [Support for GlobalProtect in OpenConnect](https://www.infradead.org/openconnect/globalprotect.html)
* [Alternative solution to WiscVPN for Linux users](https://kb.wisc.edu/91149)
* [vpn-slice](https://github.com/dlenski/vpn-slice)
