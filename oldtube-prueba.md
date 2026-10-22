# OldTube direct browser reproduction — 21 October 2026

**Result:** a real prebuilt OldTube bundle was found and exercised against the public Blender film. It did not extract a playable URL or start playback in desktop Chrome. Chrome blocked its cross-origin YouTube requests. This is a desktop result, not a physical J4290 test and not proof that autonomous YouTube playback on Orsay is impossible.

## Artifact and method

- The [GitHub releases endpoint](https://api.github.com/repos/arielscarpinelli/oldtube/releases) returned an empty array. However, the repository publishes a [prebuilt JavaScript bundle](https://github.com/arielscarpinelli/oldtube/blob/master/docs/app/index.js) and stylesheet under `docs/app/`. Therefore this attempt did **not** need a source build or dependency installation.
- Bundle downloaded unchanged: **525,431 bytes**, SHA-256 `23aef3d34d0ffd4f70a04305679d2f417ae4422a61ac982e3c9a41320e4dceee`. The inspected file contains the actual `ytdl getInfo` playback path. The current source [player](https://github.com/arielscarpinelli/oldtube/blob/master/src/js/VideoPlayer.js#L201) also shows `ytdl.getInfo`, format selection and assigning the resulting URL to video playback. Source and prebuilt artifact were not assumed to be identical versions.
- Existing Chrome and existing bundled Playwright were used, in a fresh browser context without user cookies or account login. No dependencies, browser extensions or media services were installed. No server was started.
- A local `file://` harness supplied small no-op Samsung Widget, Plugin, remote-key and IMEShell adapters so the unchanged bundle could mount in desktop Chrome. These replace TV UI APIs only; no extractor code, stream URL, response body or network permission was substituted. Analytics requests were explicitly blocked.
- Target: [Big Buck Bunny 60fps 4K — Official Blender Foundation Short Film](https://www.youtube.com/watch?v=aqz-KE-bpKQ), video ID `aqz-KE-bpKQ`. Blender identifies the film in its [official project page](https://studio.blender.org/projects/big-buck-bunny/).
- The existing React App component's `onVideoSelected` method was invoked with that video ID, bypassing search and login only. The genuine bundled player then performed its own extraction attempt. This verifies the extraction path, not full search/remote navigation.

## Observed evidence

| Run | Observation | Meaning |
|---|---|---|
| Initial sandbox run | YouTube requests failed `net::ERR_NETWORK_ACCESS_DENIED`. | Environment blocked the network. Excluded from the client compatibility conclusion. |
| Normal-network retry, 20-second bound | YouTube `/iframe_api` and its widget script returned HTTP 200. OldTube requested `/watch` and `/get_video_info`, then displayed `ytdl error: TypeError: Failed to fetch`. | The bundle ran and reached the public service; extraction did not finish successfully. |
| Final diagnostic, 12-second bound | Chrome reported missing `Access-Control-Allow-Origin` on both extraction endpoints, requested from origin `null`. Video: `readyState=0`, `currentTime=0`, no current source, `networkState=0`. | Desktop file-origin CORS prevented reading extraction responses. No stream was decoded or played. |

Requests were made normally with browser TLS and web security enabled. No proxy, CORS bypass, authenticated session, DRM bypass or modified YouTube response was used. Downloads and the normal-network reruns required sandbox network escalation, which was automatically approved.

## Interpretation and remaining limit

This is a concrete **failed desktop reproduction** of the published OldTube artifact. It does not establish whether the TV widget runtime permits the same cross-origin requests, nor whether extraction would work after that point: this run did not reach successful signature parsing, codec selection or decoding. The public [upstream issue #48](https://github.com/arielscarpinelli/oldtube/issues/48) independently reports working search but black video, with `ytdl-core` upgrade/build problems; that report remains separate from this test's observed CORS failure.

The correct product state remains **experimental, playback not verified**. Do not label OldTube working, and do not label it universally impossible. A physical Orsay run or another authorized runtime matching its network model would provide different evidence. No TV was contacted or changed.

The local reproduction artifacts are not included in this research-only repository; this report states the method and observed outcome.


