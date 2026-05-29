# Migration Workflow

Prefer these migration shapes in order:

1. **Stable re-export** - move implementation, keep old import path alive temporarily through a thin re-export.
2. **Facade** - create a canonical surface that delegates to the new internal structure.
3. **Adapter** - convert between old and new calling/data shapes when internals changed but the old surface must hold.
4. **Branch-by-abstraction** - introduce an abstraction seam when callers and implementation must change in stages.
5. **Staged caller migration** - move cohorts one group at a time, keeping a compatibility layer until the last cohort lands.

Default sequence:

1. Create the target module boundary.
2. Preserve the current surface with the thinnest viable bridge.
3. Migrate low-risk callers first.
4. Verify after each cohort.
5. Remove bridges only when no supported callers remain.

Avoid:

- simultaneous file moves plus broad API edits
- changing import paths and behavior in one undocumented step
- deleting the old module before all discovery paths are checked
