# anylinux-setup-action

Sets up an archlinux container for building AppImages with the
[Anylinux-AppImages](https://github.com/pkgforge-dev/Anylinux-AppImages) tools:
installs the packaging dependencies and deploys `quick-sharun`,
`get-debloated-pkgs` and `make-aur-package` to `/usr/local/bin`.

```yaml
- name: Preparing Container
  uses: pkgforge-dev/anylinux-setup-action@main
```

Foreign arch jobs that cannot use a job level container (the binfmt handlers
must be registered before such a container can start) can use the companion
emulated action instead, which registers the QEMU handlers and runs the setup
inside a `docker run`:

```yaml
- name: Preparing Container (emulated)
  uses: pkgforge-dev/anylinux-setup-action/emulated@main
  with:
    # one of: linux/arm64, linux/riscv64, linux/loong64, linux/ppc64le, linux/ppc64
    platform: linux/riscv64
```

Or cover every arch at once with a matrix:

```yaml
strategy:
  fail-fast: false
  matrix:
    include:
      # comment out arches you do not want to build
      - { name: Build AppImage, arch: riscv64, platform: linux/riscv64 }
      - { name: Build AppImage, arch: loongarch64, platform: linux/loong64 }
      - { name: Build AppImage, arch: ppc64le, platform: linux/ppc64le }
      - { name: Build AppImage, arch: ppc64, platform: linux/ppc64 } # BE
      # usually built natively on ubuntu-24.04-arm, emulation is for x86_64 runners
      - { name: Build AppImage, arch: aarch64, platform: linux/arm64 }
steps:
  - uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
  - name: Build AppImage (emulated)
    uses: pkgforge-dev/anylinux-setup-action/emulated@8c222ccc48a2134debbcdfd468e419f187f28dcf
    with:
      platform: ${{ matrix.platform }}
```

Environment overrides: `ANYLINUX_TOOLS_DIR`, `QUICK_SHARUN`,
`DEBLOATED_PACKAGES`, `MAKE_AUR_PACKAGE`.
