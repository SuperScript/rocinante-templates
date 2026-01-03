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

### Rocinante Bootstrap

- `fetch https://github.com/superscript/rocinante/archive/9b3d956.tar.gz -o rocinante.tar.gz`
- Untar and `make install`.
- `pkg install hs-git-annex`
- `rocinante bootstrap https://github.com/superscript/rocinante-templates.git`
- `rocinante template superscript/rocinante-templates/bootstrap --arg EMAIL="ssh-pubkey-recipient"`
- Register pubkey rocinante@$(hostname -s) received via email as sst-rocinante in github.
- Set up base system: `rocinante template superscript/rocinante-templates/setup --arg REPO=git@github.com:superscript/rocinante-private.git`

### Rocinante pkg
These steps build private packages which can then by pushed to a github repo for use in setting up other hosts with the same OS release.

- `mkdir -p /usr/local/etc/ssl/keys && chmod 0700 /usr/local/etc/ssl/keys`
- Copy /usr/local/etc/ssl/keys/poudriere.key to the new poudriere host
- `mkdir -p /usr/local/etc/ssl/certs`
- Copy /usr/local/etc/ssl/certs/poudriere-pkg.cert to the new poudriere host
- Create poudriere jail (pkg): `roci poudriere`

To update ports trees:
- `roci poudriere-update`

To build ports:
- `roci poudriere-bulk (dry run, will borrow dependencies)
- `roci poudriere-bulk --arg DRY_RUN='' (will build)

### Rocinante devbox

It's unclear why we need to separate these next two steps, but it is empirically necessary.

- Set up a devbox host: `roci template superscript/rocinante-private/devbox`
- Configure users: `rocinante template superscript/rocinante-private/users --arg OP=config`
- Register user pubkeys USER@$(hostname) as user authentication keys in github.
- Fetch superscript/home.git.
- Move .profile and .shrc out of the way and check out master.
- Make setup.dev.

### Manual config

- Configure for hardware: example: `rocinante template superscript/rocinante-private/huawei-matebook-x-pro`
- Configure for hardware: example: `rocinante template superscript/rocinante-private/framework-laptop`
- Add basic jails:
    - `basti xapp sst firefox`
- Add sound for firefox: about:config `media.cubeb.backend oss`
- Configure firefox themes:
    - sst: https://color.firefox.com/?theme=XQAAAAInAQAAAAAAAABBqYhm849SCia2CaaEGccwS-xMDPsqvEueWr7d4b5dVg1ceDEqvlb1llaBCC8VlfUPFgQQSWLHyIhDzpb4tuL3FlHdQ45etv1BeJ492Kzu9JXBvCSwKKnwm4I4hPJSwD7Ox-r0-WtVQmeJxVSrUlLGr5uV9j2N5X4x4SrHGUI5YQKNXkbmlSHpxdQTAYzqlkGGI95ctfMxEjjsjVJDhj1EGtXYOSj-7cDrMhNc1Eu_P4H_bqkAAA
    - openai: https://color.firefox.com/?theme=XQAAAAImAQAAAAAAAABBqYhm849SCia2CaaEGccwS-xMDPsqvEueWr7d4b5dVg1ceDEqvlb1llaBCC8VlfUPFgQQSWLM8vlmOknxI4gnwKJG0WCZOYhQJhK4cKpCvTUHyPmwVURxaLW0_jMJM12bBQAwh8K64eAgBvBhoEwmJgYY9aKQJRc7WswFUhYUaYOjtPUUqRqDg_5jTzTZ1Fjgah1vz7CPSPmPS7E3ZjXuv6lhc3Gb3LPhX__UKRAA
    - claude: https://color.firefox.com/?theme=XQAAAAInAQAAAAAAAABBqYhm849SCia2CaaEGccwS-xMDPsqvEueWr7d4b5dVg1ceDEqvlb1llaBCC8VlfUPFgQQSWLHyIuNwDHTJSrYlp-6eVaV5Tp5dpxa4x_MK1JAm_mg3Etmw0jNC7QKulW-yTxK1tOsiK5OnxAhaxhBANMVeB84pBatMnUfJDqdp_qIzjh03iUEe84b770JgGZAeit3iZQhPLfBx5F146D2SxDsaOMaMbKPdvgWLHYi4L_-6qRA

