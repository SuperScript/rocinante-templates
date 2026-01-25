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
- User web.
- Open shell
- `freebsd-update fetch install --not-running-from-cron`

### Bootstrap Rocinante

- `fetch https://github.com/superscript/rocinante/archive/76ff85a5.tar.gz -o rocinante.tar.gz`
- Untar and `make install`.
- `pkg install hs-git-annex`
- `rocinante bootstrap https://github.com/superscript/rocinante-templates.git`
- `rocinante template superscript/rocinante-templates/bootstrap --arg EMAIL="ssh-pubkey-recipient"`
- Register pubkey rocinante@$(hostname -s) received via email as sst-rocinante in github.
- Set up base system: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private.git`

### Execute Rocinante Setup

Refer to `rocinante-private` for instructions.
