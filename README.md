# webversecli

Unofficial CLI for [WebVerse Labs Pro](https://webverselabs-pro.com) — mirrors the `htbcli` / `hccli` workflow.

## Install

```bash
git clone git@github.com:mHijuxS/webversecli.git ~/tools/webversecli
ln -sf ~/tools/webversecli/webversecli ~/.local/bin/webversecli
```

## Update

```bash
git -C ~/tools/webversecli pull
```

## Auth

```bash
# interactive login (saves JWT to ~/.config/webversecli/config.json)
webversecli config login

# or paste a JWT directly
webversecli config set-token eyJ...

# or use an environment variable (takes priority)
export WEBVERSECLI_TOKEN=eyJ...

webversecli config logout   # logout + clear saved token
webversecli config show     # inspect current token
```

## Usage

```
webversecli <command> [subcommand] [args] [flags]
```

### Profile

```bash
webversecli me                  # username, email, VPN IP, streak
webversecli me stats            # XP, rank, first bloods, difficulty breakdown
webversecli me progress         # completed labs/challenges list + rank bar
webversecli me achievements     # unlocked/locked achievements table
```

### Labs

```bash
webversecli labs list
webversecli labs list --free
webversecli labs list --difficulty easy
webversecli labs list --tag sqli
webversecli labs list --search "token"
webversecli labs list --free --difficulty easy --tag ssrf

webversecli labs filters              # available tags, technologies, difficulties

webversecli labs show <slug>          # full details: objectives, first blood, story
webversecli labs start <slug>
webversecli labs active               # gateway IP, hostnames, expiry
webversecli labs stop
webversecli labs submit <slug> [FLAG] # reads stdin if FLAG omitted
webversecli labs writeup <slug> [OUTPUT]  # download PDF writeup (solved labs only)
```

### Challenges

```bash
webversecli challenges list
webversecli challenges list --free --category sqli --difficulty easy
webversecli challenges list --search "xss"

webversecli challenges show <slug>
webversecli challenges start <slug>   # returns challenge URL
webversecli challenges active
webversecli challenges stop
webversecli challenges submit <slug> [FLAG]
```

### Events (daily / weekly)

```bash
webversecli events list
webversecli events show <slug>
webversecli events start <slug>
webversecli events active
webversecli events stop
webversecli events submit <slug> [FLAG]
```

### Mystery challenges (retired daily/weekly events archive)

Past daily/weekly events stay playable in the "mystery challenges" archive
(`/api/mystery-challenges`) — a different endpoint from `events`, so
`events list` (active only) never shows them.

```bash
webversecli mystery list                       # whole archive (default sort: newest first)
webversecli mystery list --unsolved            # only the ones you haven't solved
webversecli mystery list --solved              # only solved
webversecli mystery list --status              # all, with a Solved column
webversecli mystery list --kind weekly --category sqli
webversecli mystery show <slug>                # details + your solve state + first blood
webversecli mystery start <slug>               # spins up an instance URL
webversecli mystery active
webversecli mystery stop
webversecli mystery submit <slug> [FLAG]
```

> `--unsolved` / `--solved` / `--status` fetch per-challenge detail in parallel
> (the list endpoint omits per-user solve state).

### Ranges (multi-flag environments)

```bash
webversecli ranges list
webversecli ranges show <slug>
webversecli ranges start <slug>   # requires All Access subscription
webversecli ranges active
webversecli ranges stop
webversecli ranges submit <slug> [FLAG]
```

### Learning paths

```bash
webversecli learning-paths list
webversecli learning-paths show junior-web-hacker
```

### Leaderboard

```bash
webversecli leaderboard           # weekly (default)
webversecli leaderboard monthly
webversecli leaderboard all-time
```

### VPN

```bash
webversecli vpn status
webversecli vpn download              # saves to ~/webverselabs.ovpn
webversecli vpn download ~/wv.ovpn   # custom path
```
