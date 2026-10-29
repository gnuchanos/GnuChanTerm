# GnuChanTerm

GnuChanTerm is a terminal emulator for an X11 session, written in C against
Xlib, Xft and Imlib2. It runs the child shell on a pty (`forkpty`), parses the
VT escape sequences the shell sends, and draws the grid with Xft; a picture a
program prints as pixels is loaded and scaled with Imlib2. It is Debian-only, on
purpose.

## Building

    git clone <repository-url> GnuChanTerm
    cd GnuChanTerm/src
    python3 makefile.py

The makefile installs the build dependencies with apt-get, compiles the sources,
and re-runs itself through sudo to write into `/usr/local/bin`. It installs four
things and not one: the binary (`/usr/local/bin/GnuChanTerm`), a terminfo entry
for `gcl-256color`, a desktop entry, and the settings script under
`~/.config/GnuChanTerm/`.

The makefile takes an action as its first argument: `install` (the default),
`build`, `run` and `uninstall`.

## Dependencies

The makefile probes for these headers and installs the matching package when one
is missing:

- `build-essential` (gcc)
- `pkg-config`
- `libx11-dev` — `X11/Xlib.h`
- `libxft-dev` — `X11/Xft/Xft.h`
- `libfreetype-dev` — `ft2build.h`
- `libxrender-dev` — `X11/extensions/Xrender.h`
- `libimlib2-dev` — `Imlib2.h`

The linking is driven by pkg-config for the modules `x11`, `xft`, `xrender`,
`fontconfig`, `freetype2` and `imlib2`. `-lutil` is for `forkpty` and is named
directly. The terminfo entry is generated with `tic` from `ncurses-bin`.

## X11 only

This program is built directly against X11 (Xlib, Xft and the rest of the X
stack). It is designed for an X11 session and does not support Wayland: it was
never tested there, its window management assumes an X server, and no work has
been done to port it. On a Wayland session it may run through XWayland for basic
use, but that is not a supported configuration and behaviour there is undefined.
Run it on X11.

## License

GPL3.
