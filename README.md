# proofcarryingdata/snark-artifacts

Repo for holding large binaries needed for ZK proofs, particularly for families
of GPCs (General Purpose Circuits).  These binaries are kept out of the `zupass` monorepo to avoid bloat.

Everything here is still experimental while the specific method of artifact
generation and distribution is being worked out.

## Downloading artifacts

For testing, you can clone this repo directly to obtain artifacts.  Artifacts
can also downloaded directly in a browser from this repo using a URL like the
example below, where "rev" can be a git branch, tag, or commit hash.

  https://raw.githubusercontent.com/proofcarryingdata/snark-artifacts/rev/packages/proto-pod-gpc/proto-pod-gpc_1o-5e-6md-0x0l-0x0t-vkey.json

Officially released artifacts are available as NPM packages accessible as
devDependencies.  They can also be downloaded from there using an NPM-based
CDN such as unpkg.com:

  https://unpkg.com/@pcd/proto-pod-gpc-artifacts@0.0.2/proto-pod-gpc_1o-5e-6md-0x0l-0x0t-vkey.json

TODO(POD-P3): Link to the function in @pcd/gpc which configures download URLs.

## Testing experimental artifacts

The direct GitHub download method above can be used for testing with
experimental artifacts during circuit development.  These commands assume
you've cloned the `zupass` and `snark-artifacts` directories in a single
parent folder and have write access to each.

  cd zupass/packages/lib/gpcircuits
  yarn gen-test-artifacts
  yarn copy-test-to-snark-artifacts
  cd ../../../../snark-artifacts
  git checkout -b your-experimental-branch-name
  git commit . -m "test artifacts"
  git push

You can then use `your-experimental-branch-name` as the rev in download
URLs above.

Test artifacts should not be merged into `main`.  Instead the experimental
branch can be deleted when testing is complete,

## Releasing updated artifacts

To release a new package to NPM you need write access to @pcd packages in NPM.
You'll need to run `npm login` once to establish your publishing credentials.

A release can be created using [changesets](https://github.com/changesets/changesets/tree/main).  First declare and describe your intended change:

  yarn changeset

Answer the prompts to select the type of release and description.  Note that
due to binary compatibility, once artifacts are in production, patches can
only be used to add new artifacts, or change non-artifact files.  Consider
using changeset's prerelease or snapshot features for testing first.

Next update the package version and create a commit for the release:

  yarn changeset version
  git add .
  git commit

Finally publish the release to NPM, which also creates a tag in git, which
you should push to GitHub:

  yarn changeset publish
  git push --follow-tags

Publishing will prompt you for a one-time password for 2-factor authentication.

Once published, you can download from NPM using the version number, or from
GitHub usng the release tag.
