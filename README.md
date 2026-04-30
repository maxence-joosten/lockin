# lockin

A terminal command that forces you into focus mode — blocks distracting websites, closes distracting apps, and runs a timed session with random ASCII art.

![showcase](assets/showcase.gif)

## Features

- Blocks distracting sites system-wide via `/etc/hosts` — works in every browser, no extension needed  
- Kills distracting apps using `pkill` (with graceful + forced termination fallback)  
- Shows a random quote at session start (always sanitized to a single line to avoid UI breaks in `arttime`)  
- Runs ASCII art countdown via [`arttime`](https://github.com/poetaman/arttime) (automatically falls back to a clean, built-in progress bar if arttime is not installed)
- Automatically unblocks sites and restores DNS after session ends  
- Safe cleanup even if the script exits unexpectedly  

## Important Behavior

- `arttime` is interactive → you must exit it manually (this is intended)  
- Apps remain blocked for the full duration of the session  
- Websites are unblocked only after `arttime` exits or the timer runs out 
- Quotes are forcibly flattened into one line for compatibility  

 ## Requirements

- Linux (systemd-based distro recommended for DNS flushing)
- `bash`
- `sudo` access (for `/etc/hosts` modification)

### Optional but Recommended

- [`arttime`](https://github.com/poetaman/arttime) — ASCII countdown timer UI  
- `fortune-mod` — random quotes (fallback included if not installed)  

# Install

### 1. Install Lockin

```bash
git clone https://github.com/maxence-joosten/lockin.git
cd lockin
mkdir -p ~/.local/bin
cp lockin ~/.local/bin/lockin
chmod +x ~/.local/bin/lockin
```

### 2. Ensure `~/.local/bin` is in PATH

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 3. Run it

```bash
lockin
```

## Customization

Edit the script directly:

### Blocked sites
```bash
BLOCKED_SITES=(
  "twitter.com"
  "reddit.com"
  "youtube.com"
)
```

### Blocked apps
```bash
BLOCKED_APPS=(
  "vesktop"
  "steam"
)
```

To see available `arttime` visuals:
```bash
ls /usr/share/arttime/textart/
```

## If Something Goes Wrong

If Lockin crashes mid-session and sites remain blocked:

```bash
sudo sed -i '/# lockin-block/,/# lockin-block end/d' /etc/hosts
```

Then flush DNS if needed:

```bash
sudo resolvectl flush-caches
```

## License

MIT
