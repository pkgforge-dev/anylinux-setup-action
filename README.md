# anylinux-setup-action

Sets up an archlinux container for building AppImages with the
[Anylinux-AppImages](https://github.com/pkgforge-dev/Anylinux-AppImages) tools:
installs the packaging dependencies and deploys `quick-sharun`,
`get-debloated-pkgs` and `make-aur-package` to `/usr/local/bin`.

```yaml
- name: Preparing Container
  uses: pkgforge-dev/anylinux-setup-action@main
```

The setup script (`setup.sh`) is plain POSIX sh, so foreign arch jobs that
cannot use a job level container (the binfmt handlers must be registered
before such a container can start) can run the same logic inside their own
`docker run`:

```sh
wget -qO /tmp/setup.sh https://raw.githubusercontent.com/pkgforge-dev/anylinux-setup-action/refs/heads/main/setup.sh
sh /tmp/setup.sh
```

Environment overrides: `ANYLINUX_TOOLS_DIR`, `QUICK_SHARUN`,
`DEBLOATED_PACKAGES`, `MAKE_AUR_PACKAGE`.
