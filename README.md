# proofcarryingdata/snark-artifacts

Repo for holding large binaries needed for ZK proofs, particularly for families
of GPCs (General Purpose Circuits).  These binaries are kept out of the `zupass` monorepo to avoid bloat.

Everything here is still experimental while the specific method of artifact
generation and distribution is being worked out.

## Downloading artifacts

For testing, you can clone this repo directly to obtain artifacts.  Artifacts
can also downloaded directly in a browser from this repo using a URL like the
example below, where "rev" can be a git branch, tag, or commit hash.

  https://raw.githubusercontent.com/proofcarryingdata/snark-artifacts/rev/packages/proto-pod-gpc/proto-pod-gpc_1o-1e-5md-1x10l-2x2t-vkey.json

Officially released artifacts are available as NPM packages accessible as
devDependencies.  They can also be downloaded from there using an NPM-based
CDN such as unpkg.com:

  TODO(POD-P3): Example unpkg.com URL

TODO(POD-P3): Link to the function in @pcd/gpc which configures download URLs.

## Releasing updated artifacts

  TODO(POD-P3): Document the process for uploading and releasing new artifacts.
