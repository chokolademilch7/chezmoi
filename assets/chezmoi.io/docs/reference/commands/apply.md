# `apply` [*target*...]

Ensure that *target*... are in the target state, updating them if necessary. If
no targets are specified, the state of all targets are ensured. If a target has
been modified since chezmoi last wrote it then the user will be prompted if
they want to overwrite the file.

## Conflict prompt

When a target file has changed since chezmoi last wrote it, chezmoi prompts with
the choices `overwrite`, `all-overwrite`, `accept`, `all-accept`, `skip`, and
`quit` (`diff` is also offered when a diff is available):

* `overwrite` overwrites the target with the desired state.
* `all-overwrite` overwrites this and all subsequent conflicting targets without
  further prompting.
* `accept` keeps the current target contents and re-adds them into the source
  state. The target is skipped during this apply and re-added after the apply
  completes, preserving chezmoi's invariant that apply only mutates the
  destination directory during the apply loop. If the target's source is a
  template, chezmoi prints a warning and leaves the template unmodified.
* `all-accept` accepts this and all subsequent conflicting regular-file targets
  without further prompting. Non-file conflicts (directories, symlinks) continue
  to prompt and are never re-added.
* `skip` leaves the target unchanged.
* `quit` stops applying.

`accept`/`all-accept` are offered only for regular-file targets. Under
`--dry-run` no source changes are written; chezmoi reports `would re-add
<target>` instead. Because `apply`, `update`, `edit --apply`, and `init --apply`
share the same conflict handling, `accept`/`all-accept` are available in all of
them.

!!! note "Abbreviation change"

    Adding `accept`/`all-accept` changes the minimal unambiguous prompt
    abbreviations. Previously `a`, `al`, `all`, and `all-` all resolved to
    `all-overwrite`; now `all-o` resolves to `all-overwrite`, `all-a` to
    `all-accept`, and `ac` to `accept`. Scripts feeding answers to the prompt
    should use the full words.

## Common flags

### `-x`, `--exclude` *types*

--8<-- "common-flags/exclude.md"

### `-i`, `--include` *types*

--8<-- "common-flags/include.md"

### `--init`

--8<-- "common-flags/init.md"

### `-P`, `--parent-dirs`

--8<-- "common-flags/parent-dirs.md"

### `-r`, `--recursive`

--8<-- "common-flags/recursive.md:default-true"

### `--source-path`

Specify targets by source path, rather than target path. This is useful for
applying changes after editing.

## Examples

```sh
chezmoi apply
chezmoi apply --dry-run --verbose
chezmoi apply ~/.bashrc
```
