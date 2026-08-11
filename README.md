# Highwayman

*Formerly `claude-remote`. Like the outlaws of old who worked the open
road, this one operates remotely — reaching a rich, powerful machine from
wherever you happen to be, phone included.*

Run Claude Code from anywhere, two ways:

1. **A web UI** — a chat-style client that streams every event `claude -p
   --output-format stream-json` prints (assistant text, tool calls, tool
   results) live into the browser. Sign in from your phone or another
   laptop and watch a task run in real time.
2. **A runner CLI** — install it on a *different* machine and Claude
   Code's Bash/Read/Write/Edit tools get rerouted over MCP through the
   server to that machine, executed there, confined to a folder you
   choose. Claude keeps reasoning on the machine with your subscription;
   the runner CLI gives you a `>` prompt that makes it feel like it's
   running right there.

**📖 Full documentation:** see the [`docs/`](docs) folder in this repo
(a VitePress site — run `npm run docs:dev` to browse it locally). There's
no public docs site for this private repo.

## Install the runner CLI

This repo is **private**, so the old `curl | bash` one-liner (which
fetches from a public raw GitHub URL) won't work here. Clone over SSH
instead:

```bash
git clone git@github.com:Theyashsawarkar/highwayman.git
cd highwayman/runner
npm install --omit=dev
node src/index.js
```

(The installer scripts at the repo root still refer to the tool
internally as `claude-remote` — see the note on internal naming below.)

## Run the server + web UI

```bash
git clone git@github.com:Theyashsawarkar/highwayman.git
cd highwayman
npm install
cp server/.env.example server/.env
npm run create-account -w server   # set your sign-in username/password
npm run dev                         # server on :4317, web UI on :5173
```

To get a public URL instead of localhost (via [ngrok](https://ngrok.com),
already installed and authenticated):

```bash
npm run serve:public
```

This builds the client, starts the server, opens the tunnel, and prints
the public URL — plus the exact `claude-remote --server wss://...` command
to point a runner CLI at it for that run, without overwriting its saved
default. See [Exposing it remotely](docs/guide/remote-access.md)
and [Auth & security](docs/guide/security.md) before you do.

## Repo layout

```
highwayman/
  server/   Express + WebSocket API — spawns `claude`, relays MCP tool calls
  client/   The web UI (Vite + React + Tailwind)
  runner/   The CLI you run on any other machine
  docs/     Documentation site (VitePress)
```

## A note on internal naming

The repo and product are now **Highwayman**, but the CLI binary, npm
package name, config directory (`~/.claude-remote`), and env var prefixes
(`CLAUDE_REMOTE_*`) inside the code still say `claude-remote` — renaming
those is a real refactor (binary name, install scripts, published config
paths) rather than a branding change, so it was left alone here. Ask for
it explicitly if you want the full rename done too.

## License

MIT
