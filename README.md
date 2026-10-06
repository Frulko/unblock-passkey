# unblock-passkey

Dismiss a stuck macOS passkey dialog without restarting your browser.

Created after a failed passkey sign-in left Helium blocked on **“Unable to sign in — The operation cannot be completed.”** Neither Cancel nor Escape worked; stopping Apple's authentication helper dismissed the dialog while the browser stayed open.

## Usage

When a passkey dialog gets stuck:

```sh
./unblock-passkey
```

The command runs silently. It sends `SIGTERM` to the Apple `AuthenticationServices.Helper` process belonging to your current user, matching its full system executable path. It does not terminate the browser or change saved credentials.

This can cancel other active authentication requests handled by the same helper in your macOS session. It clears the stuck dialog; it does not fix the cause of the failed sign-in.

## Install

Requires macOS. Uses the built-in `sh`, `id`, and `pkill`; no dependencies or `sudo` needed.

```sh
git clone https://github.com/Frulko/unblock-passkey.git
cd unblock-passkey
mkdir -p "$HOME/.local/bin"
ln -s "$PWD/unblock-passkey" "$HOME/.local/bin/unblock-passkey"
```

Keep the cloned directory: the installed command links to the script inside it. If `~/.local/bin` is on your `PATH`, call it from anywhere:

```sh
unblock-passkey
```

Otherwise, add this line to `~/.zshrc` and open a new terminal:

```sh
export PATH="$HOME/.local/bin:$PATH"
```

The script uses the process name rather than a saved PID, so it works after the helper restarts. If no matching helper is running, it exits with status `1` without stopping anything.
