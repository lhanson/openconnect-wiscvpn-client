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

	vpn [--split] [config_file_path]

The script assumes you're using a Duo Push for MFA rather than a Duo passcode and
will pause while waiting for the push to be accepted.

By default all outbound network traffic will traverse the VPN. To only send traffic bound
for [well-known UW Madison Campus IP address ranges](https://kb.wisc.edu/ns/page.php?id=3988),
use `--split` (see the script for which IPs are included).

## Static IPs

To use a static VPN IP address, you'll first need to reserve one.
See the [KB](https://kb.wisc.edu/ns/108255) for details.

## Resources

* [OpenConnect](https://www.infradead.org/openconnect/index.html)
* [Support for GlobalProtect in OpenConnect](https://www.infradead.org/openconnect/globalprotect.html)
* [Alternative solution to WiscVPN for Linux users](https://kb.wisc.edu/91149)
