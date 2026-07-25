# i3shot

A fast and lightweight screenshot utility for **X11** and **i3wm**, written as a single C11 source file.

It supports full-screen, region, focused-window, monitor, i3 container, workspace and output capture. PNG and BMP encoding are built in, so no external image-processing library is required.

## Install dependencies

### Debian, Ubuntu and Linux Mint

Install the compiler, Xlib headers and all optional runtime extensions:

```sh
sudo apt-get update
sudo apt-get install -y \
  build-essential \
  libx11-dev \
  libxfixes3 \
  libxrandr2 \
  libxcomposite1 \
  libxext6
```

The packages provide:

- `build-essential` — GCC and standard build tools
- `libx11-dev` — Xlib headers and linking support
- `libxfixes3` — cursor capture
- `libxrandr2` — monitor detection
- `libxcomposite1` — improved window capture
- `libxext6` — X Shape support for live region selection

Only `build-essential` and `libx11-dev` are required to compile. The remaining libraries are loaded dynamically at runtime and enable additional features.

For i3-specific capture modes, i3wm must be installed and running:

```sh
sudo apt-get install -y i3-wm
```

## Build

Save the source as `i3shot.c`, then compile it:

```sh
cc -O2 -std=c11 -Wall -Wextra -Wpedantic \
  i3shot.c -o i3shot \
  -lX11 -ldl -lm
```

## Install

Install the compiled binary system-wide:

```sh
sudo install -m 755 i3shot /usr/local/bin/i3shot
```

Verify the installation:

```sh
i3shot --version
i3shot --help
```

## Usage

```sh
i3shot screenshot.png
i3shot -s selection.png
i3shot -u focused-window.png
i3shot -p screenshot-with-cursor.png
i3shot --container container.png
i3shot --workspace workspace.png
i3shot --output current output.png
```

## i3wm shortcuts

Add these lines to `~/.config/i3/config`:

```text
bindsym Print exec --no-startup-id i3shot "$HOME/Pictures/screenshot-%Y-%m-%d_%H-%M-%S.png"
bindsym $mod+Print exec --no-startup-id i3shot -s "$HOME/Pictures/selection-%Y-%m-%d_%H-%M-%S.png"
```

Create the destination directory and reload i3wm:

```sh
mkdir -p "$HOME/Pictures"
i3-msg reload
```

## Dependencies not required

i3shot does not use Imlib2, GTK, Qt, SDL, ImageMagick, libpng, zlib or an external JSON library.

## License

MIT
