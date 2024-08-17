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

- `mkdir -p /usr/local/etc/ssl/keys && chmod 0700 /usr/local/etc/ssl/keys`
- Copy /usr/local/etc/ssl/keys/poudriere.key to the new poudriere host
- `mkdir -p /usr/local/etc/ssl/certs`
- Copy /usr/local/etc/ssl/certs/poudriere.cert to the new poudriere host
- Build overlay-ports: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private --arg TEMPLATE=superscript/rocinante-private/pkg`

To update overlay-ports:
- basti start pkg
- basti cmd pkg poudriere ports -u -p overlay_ports_public
- basti stop pkg

### Rocinante devbox

It's unclear why we need to separate these next two steps, but it is empirically necessary.

- Set up a devbox host: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private --arg TEMPLATE='superscript/rocinante-private/pkg-client --arg PKG_PORTS=2024Q2'`
- Set up a devbox host: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private --arg TEMPLATE='superscript/rocinante-private/devbox --arg EMAIL=web@superscript.com'`
- Register user pubkeys rocinante@USER@$(hostname -s) as user in github.
- Configure users: `rocinante template superscript/rocinante-private/users --arg OP=config`

### Manual config

- Configure for hardware: example: `rocinante template superscript rocinante-private/huawei-matebook-x-pro`
- Configure for hardware: example: `rocinante template superscript rocinante-private/framework-laptop`
- Add basic jails:
    - `basti xapp sst firefox`
- Add sound for firefox: about:config `media.cubeb.backend oss`
- Configure firefox themes:
    - sst: https://color.firefox.com/?theme=XQAAAAInAQAAAAAAAABBqYhm849SCia2CaaEGccwS-xMDPsqvEueWr7d4b5dVg1ceDEqvlb1llaBCC8VlfUPFgQQSWLHyIhDzpb4tuL3FlHdQ45etv1BeJ492Kzu9JXBvCSwKKnwm4I4hPJSwD7Ox-r0-WtVQmeJxVSrUlLGr5uV9j2N5X4x4SrHGUI5YQKNXkbmlSHpxdQTAYzqlkGGI95ctfMxEjjsjVJDhj1EGtXYOSj-7cDrMhNc1Eu_P4H_bqkAAA
    - openai: https://color.firefox.com/?theme=XQAAAAImAQAAAAAAAABBqYhm849SCia2CaaEGccwS-xMDPsqvEueWr7d4b5dVg1ceDEqvlb1llaBCC8VlfUPFgQQSWLM8vlmOknxI4gnwKJG0WCZOYhQJhK4cKpCvTUHyPmwVURxaLW0_jMJM12bBQAwh8K64eAgBvBhoEwmJgYY9aKQJRc7WswFUhYUaYOjtPUUqRqDg_5jTzTZ1Fjgah1vz7CPSPmPS7E3ZjXuv6lhc3Gb3LPhX__UKRAA
    - claude: https://color.firefox.com/?theme=XQAAAAInAQAAAAAAAABBqYhm849SCia2CaaEGccwS-xMDPsqvEueWr7d4b5dVg1ceDEqvlb1llaBCC8VlfUPFgQQSWLHyIuNwDHTJSrYlp-6eVaV5Tp5dpxa4x_MK1JAm_mg3Etmw0jNC7QKulW-yTxK1tOsiK5OnxAhaxhBANMVeB84pBatMnUfJDqdp_qIzjh03iUEe84b770JgGZAeit3iZQhPLfBx5F146D2SxDsaOMaMbKPdvgWLHYi4L_-6qRA
