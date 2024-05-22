# proofcarryingdata/snark-artifacts

Repo for holding large binaries needed for ZK proofs, particularly for families
of GPCs (General Purpose Circuits).  These binaries are used by code in the
[Zupass](https://github.com/proofcarryingdata/zupass) repo, but stored in a
separate repo to avoid bloat.  For more details on what is available,
see the individual packages in the `packages/` directory.

The packages in this repo can be used by anyone.  The instructions for
testing and publishing are intended primarily for Zupass developers with
write access to this repo.

## Downloading artifacts

For testing, you can clone this repo directly to obtain artifacts.  Artifacts
can also downloaded directly in a browser from this repo using a URL like the
example below, where "rev" can be a git branch, tag, or commit hash.

  https://raw.githubusercontent.com/proofcarryingdata/snark-artifacts/rev/packages/proto-pod-gpc/proto-pod-gpc_1o-5e-6md-0x0l-0x0t-vkey.json

Officially released artifacts are available as NPM packages accessible as
`devDependencies`.  They can also be downloaded from npm using an NPM-based
CDN such as unpkg.com:

  https://unpkg.com/@pcd/proto-pod-gpc-artifacts@0.0.2/proto-pod-gpc_1o-5e-6md-0x0l-0x0t-vkey.json

The `@pcd/gpc` package in the Zupass repo needs a root URL to find artifacts by
either of these download mechanism.  To create an appropriate URL you can
use the `gpcArtifactDownloadURL` function available
[here](https://github.com/proofcarryingdata/zupass/blob/main/packages/lib/gpc/src/gpc.ts).

## Testing experimental artifacts

The direct GitHub download method above can be used for testing with
experimental artifacts during circuit development.  This method is intened
primaraly for the team developing in the Zupass repo, but can be run on a
fork if you change the repo in the paths above.

These instructions assume you've already completed dev setup for the Zupass
repo.  You should test your circuits first using the unit tests in the
`@pcd/gpcircuits` package, as well as on a local dev server (`yarn dev`).
When you're ready to test on a staging server, you can push artifacts to a
branch in GitHub for download.

The following commands will work if you've cloned the `zupass` and
`snark-artifacts` directories in a single parent folder and have write access
to the `snark-artifacts` repo.

```sh
  cd zupass/packages/lib/gpcircuits
  yarn gen-test-artifacts
  yarn copy-test-to-snark-artifacts
  cd ../../../../snark-artifacts
  git checkout -b your-experimental-branch-name
  git commit . -m "test artifacts"
  git push
```

You can then use `your-experimental-branch-name` as the rev in download
URLs above.

Test artifacts should not be merged into `main`.  Instead the experimental
branch can be deleted when testing is complete,

## Releasing updated artifacts

To release a new package to NPM from this repo you need write access to
[`@pcd`](https://www.npmjs.com/search?q=@pcd) packages in NPM.  You'll need to
run `npm login` once to establish your publishing credentials.

A release can be created using [changesets](https://github.com/changesets/changesets/tree/main).  First declare and describe your intended change:

```sh
  yarn changeset
```

Answer the prompts to select the type of release and description.  Note that
due to binary compatibility, once artifacts are in production, patches can
only be used to add new artifacts, or change non-artifact files.  Consider
using changeset's prerelease or snapshot features for testing first.

Next update the package version and create a commit for the release:

```sh
  yarn changeset version
  git add .
  git commit
```

Finally publish the release to NPM, which also creates a tag in git, which
you should push to GitHub:

```sh
  yarn changeset publish
  git push --follow-tags
```

Publishing will prompt you for a one-time password for 2-factor authentication.

Once published, you can download from NPM using the version number, or from
GitHub usng the release tag.
