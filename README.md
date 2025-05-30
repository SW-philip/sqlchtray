# 🎛️ sqlchtray 🐿️

> The entire reason the suite existed. The suite is dead. Long live the tray.

**sqlchtray** is a minimalist, modular GTK+ tray app for controlling your personal internet radio system—powered by `mpv`, styled like a dusty '90s hi-fi, and completely terminal-optional. It replaces all previous `sqlch-*` commands. It is the whole thing now.

---

## ✨ Features

- 🎧 Browse and play saved internet radio stations
- 🔃 Instant refresh when stations are edited
- 📉 Volume control via tray
- 🧼 Self-restarting "Reboot Tray" function (like a snake eating itself, but cute)
- 🎛 Launches full TUI (`sqlchknob`) when you want knobs instead of clicks
- 📜 Show logs without remembering journalctl flags
- 🐢 Looks deceptively calm, like it's not held together by 18 threads and a shell script

---

## 📦 Installation

When on the AUR:

```bash
yay -S sqlchtray
```

Or install from source:

```bash
git clone https://github.com/SW-philip/sqlchtray.git
cd sqlchtray
makepkg -si
```

You’ll get:
- `/usr/bin/sqlchtray` — the daemonized tray icon
- `sqlchtray.service` — a systemd user unit
- `/usr/share/icons/hicolor/...` — a pixel-perfect icon you’ll grow unreasonably attached to

---

## 🛠 Configuration

Your stations live here:

```
~/.config/sqlch/stations
```

Format:

```ini
Cool Jazz=https://some.stream.url
Vapor Train=https://another.one
```

They’ll auto-refresh, or hit 🔃 Refresh in the tray.

---

## 🖥️ Autostart

If you want the tray running at login:

```bash
systemctl --user enable --now sqlchtray.service
```

---

## 🧪 Debugging

```bash
sqlchtray --debug
```

Logs are clean. Error messages are sassy but polite.

---

## 🪦 Former utilities

- `sqlch` — was just a wrapper
- `sqlchctl` — absorbed into the tray
- `sqlchknob` — lives on as a TUI, launched from tray
- The rest — gone like tears in rain

---

## 🤝 Contributing

Pull requests, weird bugs, unsolicited praise—[open an issue](https://github.com/SW-philip/sqlchtray/issues). If you’re adding features, please make sure they fit the "small, useful, funny" vibe.

---

## 🧾 License

MIT. You can use the tray for whatever, even chaos. Just don’t sell it to a hedge fund.
