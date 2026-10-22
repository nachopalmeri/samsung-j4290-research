# Player QA v3

Read-only review of `cleanyourtv-orsay/player.js`, 21 October 2026. Node VM uses controlled JavaScript doubles; no TV, installation or network interaction occurred during QA. This is not hardware validation.

## Recheck after fixes

The main agent updated the adapter after the initial review. A fresh execution of an isolated JavaScript VM regression harness passed **14 assertions**: explicit false and zero rejection for Pause, Resume, StartPlayback and Jump; failed Stop retains the session, callback registrations and token; retry then clears them; thrown Stop remains retryable; accepted native seek retains position and SRT until the time callback; resume awaits callback; HTML fallback operates without native APIs and detaches on stop. The three original findings below are therefore **resolved on these checked paths**.

**Final recovery recheck — resolved.** The subsequent `init()` guard now aborts when stop fails. The same reproduction was rerun against the current file: initialize/play using plugin A whose Stop returns false, then request initialization with plugin B. Result: `oldPluginRetained:true`, `nativeSession:true`, `stopFailed:true`; the original controllable plugin is retained. All previously reported 14 regression assertions passed again.

**Verdict:** no unresolved P1/P2 findings in this adapter review. This is software-level evidence from controlled VM doubles, not confirmation of TV decoding, native dispatch, installation, TLS or physical remote behavior.

## Original findings — resolved by current fixes

- **P2 — Native command failures become successful UI states (lines 60–62, 101, 105, 111).** `Pause()` and `Resume()` ignore their return value; documented false rejection still reports success and changes the displayed state. Rejected pause also removes the watchdog. Init/start/jump detect only strict boolean false, accepting numeric zero if returned by a native bridge. Reject explicit false/zero while allowing unspecified return values where the API does not promise one; preserve the last confirmed state on rejected operations.
- **P2 — Seek publishes unconfirmed media time (line 111).** A native jump accepted by the plugin immediately calls `time(target)`. Acceptance does not establish exact current playback position. The progress display and SRT can advance to a location not yet reached, and while paused that wrong position can persist. Keep the requested target separately and publish media time from `OnCurrentPlayTime`; HTML should likewise observe the element's actual clock after seeking.
- **P2 — Native Stop failure is hidden and controls discarded (lines 42–47).** `release()` ignores false and exceptions from `Stop()`, detaches callbacks and cancels timers, then `stop()` publishes idle. If the native player refuses, playback can continue behind an idle UI. Surface stop failure and retain a controllable recovery path instead of claiming confirmed release.

## Verified behavior

VM checks reproduced these failure cases. A saved callback from the prior session could not alter state after stop because its token no longer matched. A buffering watchdog expiration entered error and released the adapter. The native URL HLS suffix, InitPlayer/StartPlayback separation, seconds-to-milliseconds conversion, and metadata-gated duration retrieval match researched Legacy APIs. SRT parser supports BOM, CRLF, comma/dot milliseconds, overlaps, length/cue limits, and strips markup; its timing accuracy depends on the authoritative clock finding above.

HTML playback is selected when native APIs are absent, with HLS conditional on `canPlayType`. That is appropriate as a capability hint, not proof of decoding. Packaged local playback may fall through to the native backend when the HTML hint is empty; native packaged file URL support has not been established and must remain explicitly unverified.

Native callbacks use global string paths and session-specific wrappers. The token check protects retained JavaScript closures. Actual native dispatch behavior, especially events queued across player reinitialization, remains a device test; a JavaScript double cannot establish it.

## Reference evidence

- [Samsung Legacy migration examples](https://developer.samsung.com/smarttv/develop/migrating-applications/migrating-legacy-platform-to-tizen.html): Legacy method names and callback assignment convention.
- [Samsung DeviceAPI Guide v2.20, mirrored original](https://freethetv2011.s3.amazonaws.com/DeviceAPI_Guide%5BV2.20%5D%5B1%5D.pdf): native class ID, command return values, time units and historical display coordinates. Exact J4290 firmware behavior remains pending.

The regression harness was kept in local scratch storage and is not included here. Product code was not edited by this reviewer.


