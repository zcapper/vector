---
title: Install Vector via Nix
sidebar_label: Nix
description: Install Vector through the Nix package manager
---

Vector can be installed through the [Nix package manager][urls.nix] via
[Vector's Nix package][urls.vector_nix_package]. This package manager is
generally used on [NixOS][urls.nixos].

import Alert from '@site/src/components/Alert';

<Alert type="warning">

Because Vector must be manually updated on Nix, new Vector releases will be
delayed. Generally new Vector releases are made available within a few days.

</Alert>

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/docs/setup/installation/package-managers/nix.md.erb
-->

## Install

1.  Install Vector

    ```bash
    nix-env --file https://github.com/NixOS/nixpkgs/archive/master.tar.gz --install --attr vector
    ```

    <Alert icon={false} type="info">

    * The `--file` flag ensures that you're installing the latest stable version
      of Vector (0.8.0).
    * The `--attr` improves installation speed.

    </Alert>

2.  Start Vector

    ```bash
    vector --config /path/to/vector.toml
    ```

    <Alert icon={false} type="info">

    * `vector` is placed in your `$PATH`.
    * You must create a [Vector configuration file][docs.configuration] to
      successfully start Vector.

    </Alert>

### Previous Versions

Installing previous versions of Vector through `nix` is possible, but not
straightforward. For example, installing Vector `0.7.1` can be achieved with
the following command:

```bash
nix-env --file https://github.com/NixOS/nixpkgs/archive/20bbe6cba68fb9d37b5d0e373b6180dce2961e0d.tar.gz --install --attr vector
```

`20bbe6...` represents the commit sha for the `0.7.1` on the
[nix package repo][urls.vector_nix_package].

#### Listing Versions & Commit SHAs

For situations that required automated retrieval, we've thrown thogether this
handy Ruby function that will list the Vector versions and their commit sha:

```ruby
require "net/http"
require "json"

# Returns a hash mapping Vector versions to commits in `nixpkgs/nixos` repository
def nix_versions
  nixpkgs_repo = "nixos/nixpkgs"
  commits_url = "https://api.github.com/repos/#{nixpkgs_repo}/commits?path=pkgs/tools/misc/vector"

  response = Net::HTTP.get URI(commits_url)
  items = JSON.parse response

  versions = {}
  for item in items do
    match = item["commit"]["message"].match "^vector:.*(\\d+\.\\d+\.\\d+)$"
    if match
      version = match[1]
      versions[version] = item["sha"]
    end
  end

  versions
end
```

## Configuring

The [Vector nix package][urls.vector_nix_package] does not install any
configuration files by default. You'll need to create a
[Vector configuration file][docs.configuration] and pass it to Vector via the
`--config` flag when [starting][docs.process-management#starting] Vector.

## Administering

The Vector nix package does not use Systemd by default, but Vector does provide
a [Systemd service file][urls.vector_systemd_file] that you can use as a
starting point. How you manage the Vector process is up to you, and the
process administration section covers how to do this:

import Jump from '@site/src/components/Jump';

<Jump to="/docs/administration/">Administration</Jump>

## Uninstalling

```bash
nix-env --uninstall vector
```

## Updating

```bash
nix-env --file https://github.com/NixOS/nixpkgs/archive/master.tar.gz --upgrade vector
```


[docs.configuration]: /docs/setup/configuration/
[docs.process-management#starting]: /docs/administration/process-management/#starting
[urls.nix]: https://nixos.org/nix/
[urls.nixos]: https://nixos.org/
[urls.vector_nix_package]: https://github.com/NixOS/nixpkgs/blob/master/pkgs/tools/misc/vector/default.nix
[urls.vector_systemd_file]: https://github.com/timberio/vector/blob/master/distribution/systemd/vector.service
