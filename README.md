# neutron

[![Build Status](https://travis-ci.org/emersion/neutron.svg?branch=master)](https://travis-ci.org/emersion/neutron)

Self-hosted server for [Protonmail client](https://github.com/ProtonMail/WebClient).

Demo: http://neutron.emersion.fr (username: `neutron`, passwords: `neutron`)

Keep in mind that Neutron is less secure than ProtonMail: most servers don't
use full-disk encryption and aren't under 1,000 meters of granite rock in
Switzerland.
If you use Neutron, make sure to [donate to ProtonMail](https://protonmail.com/donate)!

The initial goal is to support IMAP & SMTP servers (authentication, messages and folders), storing everything else (contacts & settings) on disk. Neutron is modular so it's easy to create new backends and handle more scenarios.

## Install

* Debian & Ubuntu: install from https://packager.io/gh/emersion/neutron and run with `neutronmail run web`
* Other platforms: no packages yet, you'll have to build from source

### Options

* `-config`: specify a custom config file
* `-help`: show help

### Configuration

See `config.json`.

One field per backend, each one containing a boolean `Enabled` to turn it on or off.
* `Memory`: stores all data in memory. Set `Populate` to true to automatically populate memory with some data (username: `neutron`, passwords: `neutron`).
* `Imap`: stores messages on an IMAP server
* `Smtp`: sends messages via a SMTP server
* `Disk`: store all data on disk

## Build

Requirements:
* Go (to build the server)
* Node, NPM (to build the client)

```bash
# Get the code
go get -u github.com/emersion/neutron
cd $GOPATH/src/github.com/emersion/neutron

# Build the client
git submodule init
git submodule update
make build-client

# Start the server
make start
```

## Backends

All backends must implement the [backend interface](https://github.com/emersion/neutron/blob/master/backend/backend.go). The main backend interface is split into multiple other backend interfaces for different roles: `ContactsBackend`, `LabelsBackend` and so on. This allows to build modular backends, e.g. a `MessagesBackend` which stores messages on an IMAP server with a `ContactsBackend` which stores contacts on a LDAP server and a `SendBackend` which sends outgoing messages to a SMTP server.

Writing a backend is just a matter of implementing the necessary functions. You can read the [`memory` backend](https://github.com/emersion/neutron/tree/master/backend/memory) to understand how to do that. Docs for the backend are available here: https://godoc.org/github.com/emersion/neutron/backend#Backend

## License

MIT
