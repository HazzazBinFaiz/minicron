# minicron

A small cloud-native cron alternative to run multiple scheduled commands (especially in Docker containers).

## Usage

Pass multiple commands with `-c` each with `[name]`, `[schedule]` and `[command]`:

    minicron -v -c sleep "@every 7s" "sleep 11"\
                -c sing "0 0 12 * * 1-5" "echo Lunch"

See https://godoc.org/github.com/robfig/cron for the complete interval / schedule syntax.

Minicron will prevent multiple command executions if a command is still running. Stdout / Stderr output is combined for all commands.

Note: Commands are not exec'd in a subshell by default. But subshell can be configured using `MINICRON_SHELL` environment variable.

## Running in Docker

    docker pull networkteamcom/minicron

    # Without subshell
    docker run -it --rm networkteamcom/minicron -v -c "test" "@every 5s" "echo Test"

    # With subshell
    docker run -it --rm -e MINICRON_SHELL=/bin/sh networkteamcom/minicron -v -c "test" "@every 5s" "echo Test && echo Done"


But most of the time you might want to use this in a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/#use-multi-stage-builds) to copy the static binary to another image.
