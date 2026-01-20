# Makefile

CARGO ?= cargo

PREFIX  ?= /usr/local
DESTDIR ?=

BINDIR  = $(PREFIX)/bin
LIBDIR  = $(PREFIX)/lib
DATADIR = $(PREFIX)/share

all: build

build:
	$(CARGO) build --offline --release --features default

install:
	install -D target/release/niri            $(DESTDIR)$(BINDIR)/niri
	install -D resources/niri-session         $(DESTDIR)$(BINDIR)/niri-session

	install -D resources/niri.service         $(DESTDIR)$(LIBDIR)/systemd/user/niri.service
	sed -ri "s|ExecStart=/usr/bin|ExecStart=$(BINDIR)|" $(DESTDIR)$(LIBDIR)/systemd/user/niri.service
	install -D resources/niri-shutdown.target $(DESTDIR)$(LIBDIR)/systemd/user/niri-shutdown.target

	install -D resources/niri.desktop         $(DESTDIR)$(DATADIR)/wayland-sessions/niri.desktop
	install -D resources/niri-portals.conf    $(DESTDIR)$(DATADIR)/xdg-desktop-portal/niri-portals.conf
	install -D resources/default-config.kdl   $(DESTDIR)$(DATADIR)/doc/niri/default-config.kdl

	install -d $(DESTDIR)$(DATADIR)/bash-completion/completions/
	target/release/niri completions bash >$(DESTDIR)$(DATADIR)/bash-completion/completions/niri

	install -d $(DESTDIR)$(DATADIR)/fish/vendor_completions.d/
	target/release/niri completions fish >$(DESTDIR)$(DATADIR)/fish/vendor_completions.d/niri.fish

	install -d $(DESTDIR)$(DATADIR)/zsh/site-functions/
	target/release/niri completions zsh  >$(DESTDIR)$(DATADIR)/zsh/site-functions/_niri

clean:
	$(CARGO) clean

.PHONY: all build install clean
