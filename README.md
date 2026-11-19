# tick

`tick` is a Linux command-line timer, study tracker, and personal reminder scheduler. It runs with Python 3.9 or later and uses the standard library. Desktop reminders use `notify-send`, alarms use an available audio player, and background scheduling uses a systemd user service.

## Install

Download the executable from the repository and install it in a directory on your `PATH`:

```sh
curl -fLO https://raw.githubusercontent.com/stashforge/tick/v0.1.4/tick
mkdir -p ~/.local/bin
install -m 755 tick ~/.local/bin/tick
tick --version
```

## Timers and study

```sh
tick timer 5m --label "Tea"
tick sw
tick pomo --work 50m --short 10m
tick study pomo --subject SQL
tick study stats
```

On an interactive terminal, use Space to pause, `+` and `-` to adjust a countdown by one minute, and `v` to toggle remaining and elapsed time. Add `-c` to center the display, `-P` to show progress, or `--inline` to keep it on the normal screen. Pomodoro waits for you to start the next phase by default.

## Reminders

Start the personal scheduler once to receive reminders after closing the terminal:

```sh
tick service install
tick service start
```

```sh
tick remind add "Drink water" --every 45m --between 8am-9pm --at-start
tick remind add "Take a break" --at 4pm --days weekdays --action alarm
tick remind show "Drink water"
tick remind test "Drink water"
tick remind pause "Drink water" --for 2h
tick remind list
tick remind log
```

Interval reminders start one interval after the window opens. Use `--at-start` for an alert at opening time too. A window with an end time at or before the start time ends the next day. Use `--from` and `--until` to limit the overall schedule, `--date` for a one-off reminder, or `--month-day` for a monthly reminder. A monthly rule skips months without the requested day.

Reminders follow the system time zone by default. Use `--timezone local-fixed` to pin the current zone or pass a named zone such as `Asia/Kolkata` or `UTC`. Missed alerts are skipped. Saved schedules are personal to the account that created them.

Use `tick remind test NAME` to try a saved notification or alarm immediately. It leaves the schedule and delivery history untouched.

Use `tick service status` to check the scheduler or `tick remind run` to run it in the foreground. Run `tick remind add -h` for schedule options.

## Termux

Run timers, Pomodoro, and study commands in the foreground with `python tick`. When `termux-notification` is available through Termux:API, alerts post basic Android notifications. Use the terminal to silence the timer or advance a Pomodoro phase. Background Android reminders are outside this integration.

## Completion and updates

```sh
tick completion install
tick update
```

Completion supports Bash and Zsh. Open a new shell after installing it. The updater checks the published version and checksum, then restarts the reminder service if it was already running.

To publish a new version, update `VERSION` in `tick` and record that version and the executable's SHA-256 checksum in `release.json`. Tag the matching commit as `vVERSION` and push the branch and tag together.
