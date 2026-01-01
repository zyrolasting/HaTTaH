This is my last contribution to GitHub.

I removed all other repositories from my account,
and do not expect much disruption. To the few who
contributed, thank you. I&rsquo;m happy to verify
your contributions when you need a reference, but
I will remove your code from my distributions to
respect your rights and simplify licensing.

[HaTTaH](https://hattah.top) is my new shell-centric
medium backed by the [InterPlanetary File System](https://ipfs.tech) (IPFS).

Set these up:

  - A [POSIX-compliant shell](#posix-shell-access)
  - `curl` or `wget`
  - [Kubo](https://docs.ipfs.tech/install/command-line/#system-requirements)

You may wish to opt out of Kubo telemetry by setting `IPFS_TELEMETRY=off`
in the daemon&rsquo;s environment. The developers presume your consent
by default, so watch out for related changes when updating.


# Expect hangs

Like a BitTorrent client, Kubo needs to find peers that upload
what you request. Kubo does so quietly at time of writing, so some
of the `hattah` commands defined later may hang without feedback
during peer discovery.

For those of you who find my node and wish to be supportive, use
`hattah pin` to&mdash;in BitTorrent lingo&mdash;seed my content.
That means you offer some of your bandwidth to make my content more
available to peers. Use `hattah nip` to stop further distribution.
The `hattah` distribution commands only operate on `hattah ls` output,
but you can use the underlying `ipfs pin` interface to handle any object.

In case there&rsquo;s another problem, check your logs.


# Web Service

[https://hattah.top](https://hattah.top) is a web service with
only a `GET /` endpoint, with trust in LetsEncrypt. A POSIX
shell command pipeline expands code like a macro.

    curl -s https://hattah.top | sh -s | sh -s ... [| ...]
    ^                            ^       ^
    |                            |       |
    Print latest client download command.|
                                 |       |
                                 Print latest client.
                                         |
                                         Use content.

The Web API and CLIs of the first two `sh -s` pipe segments
are forever stable. The attached `mkhattah` script updates
clients without trusting code as the pipeline syntax does.

You only need the web service to find the latest client.
All clients function when the web service is down.


# Client Interface

Download [`mkhattah`](./mkhattah) and run `sh mkhattah`
to download the latest client. Call the client `hattah`
to activate this interface.

  - `hattah` and `hattah help` print a manual.
  - `hattah tos` prints terms of service bound to the _same client implementing said `tos` command._ If you don&rsquo;t agree to terms in a client, then don&rsquo;t use that client to distribute data.
  - `hattah ls  STRING ...` prints indexed contents, filtered by substring matches. One line of output represents one distributable object. Given no arguments, no filter applies. Otherwise, this prints lines that contain _every_ `STRING` as a substring.
  - `hattah get STRING ...` downloads content to the working directory, filtered by the rules of `ls`. Downloads always use file names shown in `ls` output, but `get` will not overwrite files.
  - `hattah pin STRING ...` pins filtered content to your IPFS node for redistribution.
  - `hattah nip STRING ...` unpins filtered content.
  - `hattah cat STRING ...` prints concatenation of filtered content.

I apply tags so commands like `hattah ls \\video`
or `hattah ls '\video'` select matching content.


# Maximalist Defaults

Most `hattah` subcommands get their scope of work from `hattah ls`.
The latter prints a complete table of contents when given no
arguments, so consider these implications.

- `hattah cat`, given no arguments, prints all content as one
  unformatted blob. A PDF might follow an MP4. `hattah ls`
  prints enough data to derive byte offsets.

```shell
> backup.bin hattah cat
> backup.idx hattah ls
< backup.idx read cid name size words
< backup.bin dd of=first.bin bs=1 count=$size
# Use accumulated lengths to find subsequent offsets.
```

- `hattah get` downloads everything in a feed.
  Given a dedicated download directory, you will
  only download content under new names. This does
  not imply that you have the latest version of
  existing files.

- `hattah pin` mirrors the content known to the
   client, and `hattah nip` takes down that mirror.


# Support

[Donations](https://www.paypal.com/paypalme/sagegerard)
are unexpected, appreciated, and not tax deductible.

You will also find offers in HaTTaH content.

Revenue supports hosting and content production.


# POSIX Shell Access

To access a POSIX-compliant shell, use the
instructions from the subsection matching
your platform.


## Windows

1. [Enable the Windows Subsystem for Linux.](https://www.google.com/search?q=enable+windows+subsystem+for+linux)
2. Press Win+R. You will see the Run dialog.
3. Type `wsl` and press ENTER. You will see a new prompt in a new window.
4. Type `exec /bin/sh` in the new prompt, and press ENTER. You will see a new prompt in the same window.


## macOS

1. Open a terminal emulator. One way is to open Spotlight with Command+Space, then type &ldquo;Terminal&rdquo; without quotes.
2. Run `exec /bin/sh` in a console or terminal emulator. You will see a new prompt.


## Other Unix-like systems

1. Either
    - Switch to a console using Alt+F<1-12> or Control+Alt+F<1-12>0
    - Open a terminal emulator (shortcuts include Control+Alt+T, or Super+ENTER).
2. Run `exec /bin/sh` in when prompted. You will see a new prompt.


## Notes for all Unix-like environments

- If a user account&rsquo;s shell is already `/bin/sh`, then `exec /bin/sh` is redundant.
- If you use Bash, then set `POSIXLY_CORRECT`. The value can be empty.
- Your system might link `/bin/sh` to a non-POSIX shell.
- If you are not sure if you have a compliant shell, consider using the `ash` implementation in BusyBox.


# (Optional) Why not use IPNS?

I don&rsquo;t want to manage mutable bindings in addition
to the DNS dependency I&rsquo;m always going to have.
I&rsquo;d rather mass produce immutable, collectible,
portable clients for technical audiences and bots.


# Legal

&copy; 2026 Sage Lennon Gerard

Preserve all copyright notices.

I won&rsquo;t attach a license to this repository
because I expect you ask for one. To best protect
our rights, contact me to sign a bill of materials.
I am happy to comp the [AGPL](https://www.gnu.org/licenses/agpl.html)
for `mkhattah`.
