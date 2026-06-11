# freedom

Freedom is a terminal input autofiller written in Python 3. Ever wished you could do this?

```bash
echo mypassword | ssh myuser@i_need_no_protection_thanks
```

Spoiler: that **doesn't** work. `ssh` (and friends) read passwords straight from the terminal, not from stdin, so piping into them does nothing. That's exactly why `freedom` exists. Now you really can. Enjoy!

## How it works

`freedom` runs your command inside a pseudo-terminal, watches its output and the moment it sees a prompt that matches a regex (a password prompt by default), it types your input for you. To the wrapped program it looks like a human is typing, so it works where a plain pipe fails.

It also returns the wrapped command's exit code, so it behaves correctly inside scripts (`freedom ssh ... && next-step`).

## Requirements & install

- Linux / macOS / any Unix (uses `pty` and `termios`, **not** Windows)
- Python 3, no external dependencies, standard library only

```bash
git clone https://github.com/brightio/freedom
cd freedom
chmod +x freedom
```

## Basic usage

```bash
echo P@55w0rd | ./freedom ssh user@host
echo P@55w0rd | ./freedom scp host:file /tmp
echo P@55w0rd | ./freedom sudo -i
echo P@55w0rd | ./freedom su -
```

## Advanced usage

Got more than one prompt? Give one line of input per prompt. Each line answers the next matching prompt in order:

```bash
echo -e "P@55w0rd\nR00tP@ss" | ./freedom ssh -t user@host "su -"
```

Here `P@55w0rd` answers the SSH login prompt and `R00tP@ss` answers the `su` prompt that follows.

## Customization

By default `freedom` looks for prompts matching:

```regex
([Pp]assword(?: for .*)?|[Vv]erification code):
```

To use your own pattern, put a single regex in `~/.freedom_regex`. For example, to also catch a `Passphrase:` prompt:

```regex
([Pp]ass(?:word|phrase)|[Vv]erification code):
```

## ⚠️ Security note

`freedom` makes it easy to hand secrets to a comman which also makes it easy to leak them. Be aware that:

- `echo P@55w0rd | ...` writes your password into your **shell history**.
- Command arguments and environment are visible to other users on the machine (e.g. `ps`).

Prefer reading the secret from a file or a secrets manager and treat anything that touches a password line with care. This tool deliberately trades safety for convenience. Use it where that trade-off is acceptable.

## License

See [LICENSE](LICENSE).
