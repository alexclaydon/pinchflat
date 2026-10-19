# This fork

This is Alex Claydon's fork of [kieraneglin/pinchflat](https://github.com/kieraneglin/pinchflat), which has been on a development pause since September 2025 ([#800](https://github.com/kieraneglin/pinchflat/issues/800)). It runs on one homelab Mac mini (Apple Silicon), built there as a native arm64 image. It is not published anywhere.

## What it changes from upstream

Upstream `master` as of 2025-12-16 (five commits past the 2025.9.26 release), plus:

- **yt-dlp's platform-independent zipapp** in the image, in place of per-architecture binaries. This is the maintainer's own fix for the failing arm64 build, taken from the unmerged `ke/fix-arm-builds` branch.
- **FFmpeg copied from upstream's published images, per architecture and pinned by digest.** The FFmpeg-Builds release the Dockerfile downloaded (`autobuild-2024-07-30-14-10`, pinned for [#347](https://github.com/kieraneglin/pinchflat/issues/347)) has been deleted, so the image no longer built. Both images carry that same FFmpeg build.
- **SQLite waits 30 s for its lock** (`busy_timeout`) before failing with `Database busy`, where the driver's default is 2 s; `DATABASE_BUSY_TIMEOUT_MS` overrides it. On a large database, long indexes kept failing on that error and starting again from the top.
- **The connection checkout timeout is configurable** with `DATABASE_TIMEOUT_MS` (default unchanged at 15 s): upstream PR [#904](https://github.com/kieraneglin/pinchflat/pull/904), unmerged there.

The version is `2025.9.26-fork.N`, so the UI shows which build is running; bump `N` with each deployed change.

## Building

```sh
docker build --platform linux/arm64 -f docker/selfhosted.Dockerfile -t pinchflat-fork:$(git rev-parse --short HEAD)-arm64 .
```

`--platform linux/amd64` builds the amd64 image the same way.

## Keeping up with upstream

`upstream` is a remote. If development resumes, merge `upstream/master` and keep the two changes above unless upstream has made them unnecessary.
