<div align="center">

# Soteria

Soteria is a Polkit authentication agent written in GTK designed to be used with any desktop environment.

<img alt="Example authentication popup" src=".github/example_popup.png" width=50% height=50% ></image>
<img alt="Failed authentication popup" src=".github/example_failed.png" width=50% height=50% ></image>

[Installation](#installation) •
[Usage](#usage) •
[Why?](#why) •
[Theming](#theming)

</div>

## Installation

Soteria requires GTK >= 4.10. For Arch based distros, you will need
`gtk4`, Debian based distros will need `libgtk-4-dev`, and Fedora
based distros will need `gtk4-devel`.

Additionally, you will need `polkit` and `libpolkit-agent` installed.
(`libpolkit-agent` should be shipped with `polkit`).


> [!NOTE]
> If the executable `polkit-agent-helper-1`
> is in a non-standard location (i.e. not `/usr/lib/polkit/polkit-agent-helper-1`), then you should also set up a configuration file
> at either ``/usr/local/etc/soteria/config.toml`` or ``/etc/soteria/config.toml`` with
> ```toml
> helper_path = "/path/to/your/helper"
> ```
> as the contents.

Soteria will also need Rust. It was developed on Rust `1.78.0` however,
lower versions of Rust should still work.

Run the following commmand to build and install Soteria:

```bash
cargo install --locked --git https://github.com/imvaskel/soteria
```

This should place Soteria into ~/.cargo/bin and you can run it from there.

Or if you use Nix, Soteria is packaged there. There is a NixOS module to enable it under ``security.soteria.enable``.

## Usage

Simply have your desktop run the `soteria` binary to have it register as your authentication agent. Once run, anytime an application requests polkit authentication, it should popup and prompt you to authenticate.

For Hyprland, this would look like:

```conf
exec-once = /path/to/soteria
```

You may also like:

```conf
windowrulev2=pin,class:gay.vaskel.Soteria
```

This makes sure that Soteria stays pinned to your current workspace.

Other desktop environments should be similiar.

## Why?

When looking for a polkit authentication agent, I noticed that most were either extremely old, using a framework that I didn't like, or completely unstylable.
Additionally, most were hard to edit as they just called out to polkit's `libpolkit-agent` to do all the work. Because of this, I decieded to put the work in to figure out how authentication agents worked.

## Theming

You can theme this application in `~/.config/gtk-4.0/gtk.css`.
The top-level window has the ID `soteria-window`, so your custom style should go in a block starting `#soteria-window {...`. From here you can select the separate components using [CSS combinators](https://www.w3schools.com/CSS/css_combinators.asp) -- run `GTK_DEBUG=interactive /path/to/soteria` to see the full list.

## Debugging

If you would like to debug why something went wrong, just run `RUST_LOG=debug soteria` and this will start it with debug logging, which should help you identify what's going wrong.
