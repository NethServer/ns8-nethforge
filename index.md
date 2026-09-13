# NethForge modules index for NS8

This is the official NethForge NS8 index of modules.

Metadata are built every 4 hours at 00:25, 06:25 ,12:25, 18:25 UTC and on each commit to the `main` branch.

If you want to add a module to this repository, just follow the
[instructions](https://nethserver.github.io/ns8-core/modules/new_module/#step-5-publish-to-ns8-software-repository)
for `ns8-repomd`, finally open the pull request here!

To use the modules listed here as NS8 repository, see the [manual
page](https://docs.nethserver.org/projects/ns8/en/latest/modules.html#software-repositories)
and set the following URL:

    https://forge.nethserver.org/ns8/updates/

## Pinning image tags (pins.yml)

`pins.yml` lets you control image visibility and ordering for modules to support staged upgrades and hide broken tags. Each module key contains a list of pin rules:

- `tag: <semver>`, `prepend: <True|False>` — ensure a specific tag is listed; prepend True puts it at the top, False appends it.
- `match: "<Python semver match string>"`, `remove: True` — hide matching tags from clients. E.g. `<2.0.0`.

Use cases:
- Maintain upgrade path across breaking releases (e.g., Nextcloud NC27→NC28):

  ```yml
  nextcloud:
    - { tag: 1.2.1, prepend: False }
  ```

- Keep a known-good baseline visible for older cores:

  ```yml
  core:
    - { tag: 2.9.6, prepend: True }
  ```

- Hide buggy module versions:

  ```yml
  mail:
    - { match: "1.7.0", remove: True }
    - { match: "1.7.1", remove: True }
  ```

Editing workflow:
- Edit `pins.yml` and commit to `main` branch; metadata will rebuild automatically on schedule and on push.
- Prefer `prepend: True` only for critical baseline tags; otherwise use prepend: False to keep natural ordering.
- Use `remove: True` for temporary blacklisting; add comments with issue references for traceability.
