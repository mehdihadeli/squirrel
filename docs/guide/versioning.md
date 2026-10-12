# Versioning and releases

Squirrel uses [Nerdbank.GitVersioning (NBGV)](https://dotnet.github.io/Nerdbank.GitVersioning/)
to version every package from the committed `version.json` file and Git
history. All packages in a release use the same `SemVer2` version. CI adds a
UTC `YYDDD` date and GitHub Actions run number to preview and RC packages for
traceability; stable packages keep their exact version.

## Release lifecycle

```text
1.0.0-preview.1.YYDDD.RUN_NUMBER -> 1.0.0-rc.1.YYDDD.RUN_NUMBER -> 1.0.0
```

`main` is the only long-lived branch. Version changes are reviewed pull
requests; ordinary pull requests do not change `version.json`.

| Release state            | Action                                        |
| ------------------------ | --------------------------------------------- |
| Start a preview train    | Merge a PR prepared with `prepare-train`      |
| Start an RC train        | Merge a PR prepared with `prepare-rc`         |
| Declare stable           | Merge a PR prepared with `prepare-stable`     |
| Publish RC or stable     | Tag the exact approved commit with `nbgv tag` |

## Prepare a version

Install NBGV once, then use the repository helper:

```bash
dotnet tool install --global nbgv
./scripts/release-version.sh prepare-train 1.0.0
```

The helper uses NBGV's `{height}` placeholder, so Git history determines the
preview and RC ordinals. Commit that change in a pull request. For stabilization
and stable release:

```bash
./scripts/release-version.sh prepare-rc 1.0.0
./scripts/release-version.sh prepare-stable 1.0.0
```

Check the calculated version with:

```bash
nbgv get-version -v SemVer2
```

## GitHub Actions publication

`.github/workflows/build-and-publish.yml` checks out full Git history, builds
all source projects, and runs unit and integration tests. After those checks:

- The initial `preview.0` commit runs validation but is not published.
- Later preview versions on `main` publish all packages to NuGet.org as
  `X.Y.Z-preview.N.YYDDD.RUN_NUMBER`.
- An RC or stable tag publishes all packages; RC packages use the same suffix,
  while stable packages use `X.Y.Z`.
- Release Drafter updates the matching draft; only stable tags publish it.

The workflow calculates the release value in `calculate-version.sh`, passes it
to `dotnet pack` as `PackageVersion` together with the calculated assembly and
informational versions, and never rewrites `version.json` in CI. The publish
job runs for `v*` tags or preview versions on `main`, excluding `preview.0`.
Release tags must be `vMAJOR.MINOR.PATCH` or `vMAJOR.MINOR.PATCH-rc.N` and must
match the NBGV version calculated for that commit.

For example, preview ordinal `4` on release line `1.4.0`, UTC date `26284`,
and workflow run `9` produce `Squirrel.Core.1.4.0-preview.4.26284.9.nupkg`.
A matching `v1.4.0-rc.4` tag produces
`Squirrel.Core.1.4.0-rc.4.26284.9.nupkg`; stable tag `v1.4.0` produces
`Squirrel.Core.1.4.0.nupkg`.

## Publish an approved tag

Create tags only from the exact approved `main` commit:

```bash
git checkout main
git pull --ff-only
nbgv get-version -v SemVer2
./scripts/release-version.sh tag
git push origin v1.0.0-rc.1
```

Use the actual tag printed by `nbgv tag`. The tag triggers the same build and
test workflow and publishes the matching Release Drafter draft.

After stable publication, begin the next release line with a new preview
version, for example `./scripts/release-version.sh prepare-train 1.1.0`.

## Release notes

Release Drafter uses pull request labels to group changes and ignores published
pre-releases when selecting the previous stable baseline. Therefore stable
notes include the preview and RC pull requests for that release line. Apply
`skip-changelog` when a pull request should not appear in release notes.
