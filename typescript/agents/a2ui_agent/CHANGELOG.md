## Unreleased

- Add `RendererState`, the agent's mirror of the surfaces a renderer holds ([#3263](https://github.com/a2ui-project/a2ui/issues/3263)). It wraps one web_core `MessageProcessor`, created once with the linked catalogs, and checks each outbound message against everything applied before it. `apply(payload)` checks and applies messages atomically: if one is rejected, the state is left as it was. `transaction(fn)` extends that to everything `fn` applies. `checkTopology(surfaceIds)` checks that each surface has its root, that every reference resolves, and that every component added or replaced since the last check is reachable from the root. A component that an update leaves unreachable by rewriting its parent's children is accepted, since the protocol has no message that removes one component. An update to a surface the state never saw created is rejected, as a renderer rejects it. `RendererState`, `validatePayload` and the `SurfaceModel` type that `RendererState.getSurface` returns are exported from the package root.
- `validatePayload` runs the payload through a short-lived `RendererState` that assumes a surface the payload only updates already exists, instead of building a new `MessageProcessor` for the created surfaces and another for each candidate catalog of each updated surface. Its results are unchanged, except that a component of a created surface may now name a child that a later message of the same payload adds, as in Python.

## 0.0.1

- Initial release of `@a2ui/agent` ([#2814](https://github.com/a2ui-project/a2ui/pull/2814), [#2916](https://github.com/a2ui-project/a2ui/pull/2916), [#2815](https://github.com/a2ui-project/a2ui/pull/2815)):
  - Support for A2UI protocol versions v0.9 and v1.0.
  - Direct JSON and Express inference formats (prompt generation, streaming/batch parsing, compilation, and decompilation).
