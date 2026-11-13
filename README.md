# OpenHarmony QEMU manifest

Migration is in progress. Repository creation, source import, patch migration and
build/runtime acceptance are separate states. See `migration-status.json` before
using this manifest as a build input.

All maintained components use `OpenHarmony-7.0-Release-qemu`, including components
with no QEMU changes. Upstream synchronization writes only `upstream/*`; changes
to QEMU branches are reviewed source commits. A future major release uses its
own `OpenHarmony-<major>.0-Release-qemu` branch across every component.

`default.xml` selects the full pinned OpenHarmony project graph plus Serial SIG.
`arkweb.xml` is a separate Chromium M144 checkout and retains its nested project
paths and root linkfiles. `external.xml` records additional tool and backend
sources. `repositories.json` records original upstream commits and ownership;
these are not interchangeable with hosted commits after a required Git LFS
conversion. Such mirrors record both identities in `UPSTREAM.json` and Git notes.

After source migration is complete, initialize each source tree with:

```sh
repo init -u https://github.com/openharmony-qemu/manifest.git -b OpenHarmony-7.0-Release-qemu -m default.xml
repo sync -c
# In a separate empty Chromium checkout, use -m arkweb.xml.
# In a separate empty external-source checkout, use -m external.xml.
```

The branch manifests are for development. Release evidence must also retain
`repo manifest -r`, the manifest repository commit, per-project QEMU commits,
source/dependency checksums and the six-package contract in `products.json`.
No build, package or runtime acceptance is implied by source synchronization.

Product component dependencies remain owned by the source repositories through
`bundle.json`, GN targets and `vendor/ohemu` product profiles. The manifest owns
repository selection and cross-tree build prerequisites, not a second patch stack.

Current scope follows the user's instruction to complete migration prerequisites
without starting package builds or guest validation.
