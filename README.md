# sqlch-suite

> “Sometimes you just want to tune into a station without a UI trying to sell you vitamins.”

**sqlch-suite** is a small, intentionally simple collection of tools for tuning in to internet radio from the command line, system tray, or terminal interface.  
It’s meant to feel like an old stereo: knobs, toggles, and just enough feedback to know you’re not hallucinating.

No ads. No AI. No cloud. No promises.

---

## What’s Inside

- `sqlch` – A little shell script that tells `mpv` to play something.
- `sqlchctl` – Ask it what’s playing. Or tell it to stop.
- `sqlchtray.py` – GTK-based system tray thing with a squirrel icon and volume control.
- `sqlchtray-launcher` – Fires up the tray script (if you’re into that).
- `sqlchknob` – Terminal UI tuner that looks like it fell off a truck in 1998.

---

## Setup

```bash
git clone https://github.com/SW-philip/sqlch-suite.git
cd sqlch-suite
chmod +x sqlch sqlchctl sqlchknob sqlchtray-launcher
```

Enable the tray to run in the background:

```bash
systemctl --user enable --now sqlchtray.service
```

---

## Notes

- Your stations go in `~/.config/sqlch/stations`, like this:

```
Rainy Jazz=https://stream-url-here
Space Noise=https://another-stream-url
```

- Your icons go in:
```
~/.local/share/icons/squelch_icons/
```

---

## Why?

Because sometimes, that’s all you need: one knob, one tune, one squirrel.

---

## License

See [LICENSE](LICENSE). You’re welcome to use and remix, but don’t try to resell this. That’d be weird.
