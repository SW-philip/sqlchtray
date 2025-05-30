#!/usr/bin/env python3
# sqlchtray-3.4.1.py — Debug-Enabled SquelchTray Daemon with Clean Layout


import os
import shutil
import subprocess
import sys
import threading
import time

import gi

gi.require_version("AppIndicator3", "0.1")
gi.require_version("Gtk", "3.0")
from gi.repository import AppIndicator3, GLib, Gtk

CONFIG_DIR = os.path.expanduser("~/.config/sqlch")
STATION_FILE = os.path.join(CONFIG_DIR, "stations")
LAST_PLAYED = os.path.join(CONFIG_DIR, "last")
SQLCHCTL = shutil.which("sqlchctl") or "/usr/bin/sqlchctl"
SQLCHKNOB = shutil.which("sqlchknob") or "/usr/bin/sqlchknob"

DEBUG = "--debug" in sys.argv
LOGFILE = None
for arg in sys.argv:
    if arg.startswith("--log="):
        LOGFILE = arg.split("=", 1)[1]
        os.makedirs(os.path.dirname(LOGFILE), exist_ok=True)
        with open(LOGFILE, "a") as f:
            f.write("\n--- sqlchtray started ---\n")


def log_debug(message):
    if DEBUG:
        print(f"[DEBUG] {message}")
    if LOGFILE:
        with open(LOGFILE, "a") as f:
            f.write(f"[LOG] {message}\n")


def run_subprocess(command):
    log_debug(f"Running: {' '.join(command) if isinstance(command, list) else command}")
    try:
        return subprocess.Popen(command)
    except FileNotFoundError as e:
        log_debug(f"Subprocess failed: {e}")
        if not DEBUG:
            subprocess.Popen(["notify-send", "sqlch Error", str(e)])


def launch_command(cmd):
    if isinstance(cmd, str) and not os.path.isabs(cmd):
        resolved_cmd = shutil.which(cmd) or cmd
    else:
        resolved_cmd = cmd
    interactive = isinstance(cmd, str) and "sqlchknob" in cmd
    if interactive:
        shell_cmd = (
            f"export PATH=$PATH:~/.local/bin:/usr/local/bin:/usr/bin; {resolved_cmd}"
        )
    else:
        shell_cmd = f"export PATH=$PATH:~/.local/bin:/usr/local/bin:/usr/bin; {resolved_cmd}; echo; read -p 'Press enter to close...'"
    for term in [
        "kitty",
        "alacritty",
        "konsole",
        "xfce4-terminal",
        "gnome-terminal",
        "xterm",
    ]:
        term_path = shutil.which(term)
        if term_path:
            try:
                subprocess.Popen([term_path, "-e", "bash", "-ic", shell_cmd])
                return
            except FileNotFoundError:
                continue
    if not DEBUG:
        subprocess.Popen(
            [
                "notify-send",
                "sqlch Error",
                "No working terminal emulator found to launch command.",
            ]
        )


class SqlchTray:
    def __init__(self):
        self.app_id = "sqlchtray"
        self.indicator = AppIndicator3.Indicator.new(
            self.app_id,
            "sqlchtray-icon.png",
            AppIndicator3.IndicatorCategory.APPLICATION_STATUS,
        )
        self.indicator.set_status(AppIndicator3.IndicatorStatus.ACTIVE)
        self.indicator.set_title("sqlch internet radio")
        self.menu = Gtk.Menu()
        self.indicator.set_menu(self.menu)
        self.last_display = ""
        self.build_menu()
        threading.Thread(target=self.poll_status, daemon=True).start()

    def build_menu(self):
        self.menu.append(self._add_item("▶ Play", lambda: self.run_ctl("start")))
        self.menu.append(self._add_item("⏸ Pause", lambda: self.run_ctl("pause")))
        self.menu.append(self._add_item("⛔ Stop", lambda: self.run_ctl("stop")))
        self.menu.append(Gtk.SeparatorMenuItem())
        self.menu.append(
            self._add_item("🔃 Refresh Stations", lambda _: self.refresh_stations())
        )
        self.add_volume_controls()
        self.menu.append(Gtk.SeparatorMenuItem())

        # 🛠 Create submenu
        self.station_list_box = Gtk.Menu()
        self.station_list_box.show()

        self.station_section = Gtk.MenuItem(label="🎙 Stations")
        self.station_section.set_submenu(self.station_list_box)
        self.station_section.show()

        self.menu.append(self.station_section)
        self.refresh_stations()

        self.menu.append(Gtk.SeparatorMenuItem())
        self.menu.append(
            self._add_item("🌛 Open sqlchKnob", lambda _: launch_command(SQLCHKNOB))
        )
        self.menu.append(
            self._add_item(
                "📜 Show Logs",
                lambda _: launch_command(
                    "journalctl --user -u sqlchtray.service -n 50"
                ),
            )
        )
        self.menu.append(
            self._add_item(
                "🔁 Reboot Tray",
                lambda _: run_subprocess(
                    ["systemctl", "--user", "restart", "sqlchtray.service"]
                ),
            )
        )
        self.menu.append(Gtk.SeparatorMenuItem())

        quit_item = Gtk.MenuItem(label="❌ Quit")
        quit_item.connect("activate", lambda _: Gtk.main_quit())
        quit_item.show()
        self.menu.append(quit_item)

    def refresh_stations(self):
        for child in self.station_list_box.get_children():
            self.station_list_box.remove(child)

        # Optional: Header inside submenu
        header = Gtk.MenuItem(label="Saved Stations")
        header.set_sensitive(False)
        header.show()
        self.station_list_box.append(header)

        separator = Gtk.SeparatorMenuItem()
        separator.show()
        self.station_list_box.append(separator)

        if not os.path.exists(STATION_FILE):
            no_station = Gtk.MenuItem(label="⚠ No saved stations")
            no_station.set_sensitive(False)
            no_station.show()
            self.station_list_box.append(no_station)
            return

        with open(STATION_FILE, "r") as f:
            for line in f:
                if "=" not in line:
                    continue
                name = line.split("=")[0].strip()
                item = Gtk.MenuItem(label=f"🎧 {name}")
                item.connect("activate", lambda _, n=name: self.run_ctl("play", n))
                item.show()
                self.station_list_box.append(item)

    def _add_item(self, label, callback):
        item = Gtk.MenuItem(label=label)
        item.connect("activate", callback)
        item.show()
        return item

    def _make_header(self, label):
        item = Gtk.MenuItem(label=label)
        item.set_sensitive(False)
        item.show()
        return item

    def run_ctl(self, *args):
        if SQLCHCTL:
            run_subprocess([SQLCHCTL, *args])
        else:
            log_debug("sqlchctl not found")
            if not DEBUG:
                subprocess.Popen(["notify-send", "sqlch", "sqlchctl not found in PATH"])

    def refresh_label(self):
        try:
            output = (
                subprocess.check_output([SQLCHCTL, "status"], stderr=subprocess.DEVNULL)
                .decode()
                .strip()
            )
        except:
            output = "sqlch: Not Playing"
        log_debug(f"Status refresh: {output}")
        if output != self.last_display:
            self.indicator.set_label(output, self.app_id)
            if "Now playing" in output or output != "sqlch: Not Playing":
                if not DEBUG:
                    subprocess.Popen(["notify-send", "🎶 sqlch", output])
            self.last_display = output

    def add_volume_controls(self):
        volume = Gtk.MenuItem(label="🔊 Volume")
        submenu = Gtk.Menu()
        for label, pct in [
            ("Mute", 0),
            ("25%", 25),
            ("50%", 50),
            ("75%", 75),
            ("100%", 100),
        ]:
            item = Gtk.MenuItem(label=label)
            item.connect("activate", lambda _, p=pct: self.set_volume(p))
            submenu.append(item)
            item.show()
        volume.set_submenu(submenu)
        volume.show()
        self.menu.append(volume)

    def set_volume(self, percent):
        try:
            run_subprocess(
                ["pactl", "set-sink-volume", "@DEFAULT_SINK@", f"{percent}%"]
            )
        except Exception as e:
            log_debug(f"Volume error: {e}")
            if not DEBUG:
                subprocess.Popen(["notify-send", "sqlch", f"Volume error: {e}"])

    def poll_status(self):
        while True:
            GLib.idle_add(self.refresh_label)
            time.sleep(10)


def main():
    try:
        SqlchTray()
        Gtk.main()
    except Exception as e:
        log_debug(f"Startup error: {e}")
        if not DEBUG:
            subprocess.Popen(["notify-send", "sqlchTray Error", str(e)])


if __name__ == "__main__":
    main()
