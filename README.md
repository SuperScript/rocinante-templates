# rocinante-templates

## Installing a new system

### OS Install

- Install from USB stick.
- Install base system only, no source, no ports.
- Install Auto (ZFS).
- Encrypt Disks.
- Set swap size.
- Encrypt swap.
- Stripe.
- Yes WIFI.
- Yes IPv4, No IPv6.
- Resolvers 1.1.1.1, 8.8.8.8.
- Clock in UTC.
- Start at boot: `sshd`, `ntpd`, `ntpd_sync_on_start`, `dumpdev`
- No users.
- Open shell
- `freebsd-update fetch install --not-running-from-cron`

### Rocinante Bootstrap

- Fetch https://github.com/superscript/rocinante/archive/refs/tags/0.1.20240521.1.tar.gz
- Untar and install.
- `pkg install git-tiny`
- `rocinante bootstrap https://github.com/superscript/rocinante-templates`
- `rocinante template superscript/rocinante-templates/bootstrap --arg EMAIL="ssh-pubkey-recipient"`
- Register pubkey rocinante@$(hostname -s) received via email as sst-rocinante in github.

### Rocinante pkg
These steps build private packages which can then by pushed to a github repo for use in setting up other hosts with the same OS release.

- Build overlay-ports: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private --arg TEMPLATE=superscript/rocinante-private/pkg`

### Rocinante devbox

It's unclear why we need to separate these next two steps, but it is empirically necessary.

- Set up a devbox host: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private --arg TEMPLATE='superscript/rocinante-private/pkg-client --arg PKG_PORTS=2024Q2'`
- Set up a devbox host: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private --arg TEMPLATE='superscript/rocinante-private/devbox --arg EMAIL=web@superscript.com'`
- Register user pubkeys rocinante@USER@$(hostname -s) as user in github.
- Configure users: `rocinante template superscript/rocinante-private/users --arg OP=config`
