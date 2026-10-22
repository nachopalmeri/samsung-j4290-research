# Independent UI QA — v3

21 October 2026. **50 checks passed; no JavaScript page errors or functional bugs found in the tested desktop paths.**

Environment: installed Chrome, existing bundled Playwright, fresh browser context, 1366×768 viewport, local `file://` application. No dependency installation, local server, TV connection or LAN request. Application sources were read only. The local automation and detailed result JSON are not included in this report-only repository.

| Area | Verified behavior |
|---|---|
| Home, Library, Organize, Settings | Tab reaches every active control; selected controller key matches the actual focused DOM element. Active control rectangles remain within the viewport. |
| System, Reset, Exit, Ended dialogs | Controls remain reachable, focused and within viewport. Escape from System and Reset restores their originating setting. |
| Favorites | Enter removes and adds the local test tile through Edit favorites; selected tile stays focused. |
| Reordering | Enter grabs a tile; ArrowLeft changes order; Escape rolls back the current movement; Enter commits. Reload preserves committed order. |
| Settings | ArrowRight changes the selected setting through the normal key handler. |
| Local playback | Enter on Prueba local starts real H.264/AAC decoding; position advances, duration exceeds 11 seconds and timed caption appears. |
| Player controls | Every control reachable; subtitle button disables captions; Space pauses with stable position. |
| Exit while playing | X opens Exit and pauses decoder. Escape restores the previously focused player control. |
| Return from player | Escape closes playback, clears the media `src`, and restores the originating library tile. |
| Load error and retry | A deliberately nonexistent local media path produces an error and Reintentar. Restoring the original path in memory and pressing Enter starts actual decoding again. No source file was changed. |
| Application exit/reopen | Home Escape opens exit confirmation; Enter on Salir shows ended state; Volver a abrir restores the app. |

The failure test initially used request interception, but Chrome reused cached local media and never entered error. That unsuccessful test technique was replaced with a nonexistent file path in temporary runtime configuration. The final 50-check result uses the corrected technique.

Inactive surfaces retain their `.focused` CSS class beneath dialogs/player. The active keyboard control list and actual DOM focus remained correctly scoped; this was not observed to cause a functional failure.

Limits: this checks desktop keyboard behavior and local decoding, not the physical Samsung mando, native Player plugin, TV codec/TLS support, physical persistence, Internet sources, native app launch or Smart Hub. Tab reachability was exhaustive across active controls; directional keys were exercised for settings/reordering, not exhaustively for every possible spatial transition. No source changes were made by this reviewer.


