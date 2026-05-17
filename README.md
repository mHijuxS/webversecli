# webversecli

Unofficial CLI for [WebVerse Labs Pro](https://webverselabs-pro.com) — mirrors the `htbcli` / `hccli` workflow.

## Install

```bash
# clone
git clone git@github.com:mHijuxS/webversecli.git
cd webversecli

# make executable and link to PATH
chmod +x webversecli
ln -sf "$PWD/webversecli" ~/.local/bin/webversecli
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

### Ranges (multi-flag environments)

```bash
webversecli ranges list
webversecli ranges show <slug>
webversecli ranges start <slug>   # requires All Access subscription
webversecli ranges active
webversecli ranges stop
webversecli ranges submit <slug> [FLAG]
```

### Smart flag submit

Detects whatever is currently active (lab → challenge → event) and submits there:

```bash
webversecli flag submit WEBVERSE{...}
wl-paste | webversecli flag submit   # pipe from clipboard
```

Add a shell alias for one-key submission (like `htbf` / `hcf`):

```bash
alias wvf='wl-paste | webversecli flag submit'
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

## API notes

- Base URL: `https://api.webverselabs-pro.com`
- Auth: `Cookie: token=<JWT>`
- Cloudflare is active — standard browser headers are sent on every request
- Events use numeric `event_id` for start/submit; the CLI resolves slugs to IDs automatically
- Lab/challenge/challenge filtering is client-side (server returns all items)

## Dependencies

None — stdlib only (`urllib`, `json`, `gzip`, `argparse`, `getpass`). Requires Python 3.10+.
