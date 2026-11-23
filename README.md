[![Track upstream release and build .deb](https://github.com/EvilSupahFly/zpaqfranz/actions/workflows/build-deb.yml/badge.svg)](https://github.com/EvilSupahFly/zpaqfranz/actions/workflows/build-deb.yml)

## Installing the built packages

Every automated run publishes its output as a GitHub Release with individual `.deb`
files attached - no need to build locally to use this.

1. Open the [latest release](../../releases/latest).
2. Download the `.deb` asset into it's own folder (you can skip both of the
   `Source code` archives - those aren't needed for installing).
3. From inside that folder, install with whichever tool you're comfortable with:

   **apt** (recommended - resolves dependencies across the whole batch of local
   files in one transaction):
    ```bash
       sudo apt install ./zpaqfranz_XX.YY-0.local1_amd64.deb
    ```

    **dpkg**, classic two-step (what this build was actually validated with):
    ```bash
       sudo dpkg -i zpaqfranz_XX.YY-0.local1_amd64.deb
       sudo apt-get install -f   # only if dpkg reports anything missing
    ```

    **GDebi** - convenient for a single package via double-click, but its
    dependency resolution checks your configured repos, not sibling files in the
    same folder - fine for single file builds like this, but not ideal for cases
    with separately-packaged dependencies.

   No need to remove any existing repo version first - since the local build's
   version number sorts higher, installing over it is a normal in-place upgrade; the
   package manager replaces the old files cleanly on its own. When instaling from a
   terminal, be sure to replace `XX.YY` in the instructions above with the actual
   version number of your download (ie 65.22).

## Important note about this auto-build project

Because zpaqfranz itself isn't updated on a regular schedule, a `cron` gate is
baked into the automation which checks upstream for updates on the 1st and 15th
of each month. If there's a new version upstream, we build new packages. If not,
we skip the build and wait for the next scheduled check.