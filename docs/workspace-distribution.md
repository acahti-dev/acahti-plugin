# Workspace distribution

## Release archives

Generate each workspace release from the current Codex source instead of editing an older ZIP:

```sh
python3 scripts/package_workspace.py --instance https://your-instance.example --workspace SAIDC --output /path/to/acahti-saidc-1.10.0.zip
```

The instance URL is a release argument, not part of the generic plugin source. The generated package includes the MCP endpoint, current skills, reminder hooks and a release manifest with file hashes. Tests, bytecode and local credentials are excluded. Upload the versioned ZIP and verify the directory version, then test MCP `whoami` (including `modules`) and `inbox` from a fresh member task. If the instance sets `ACAHTI_MODULES=work,argos,chat`, also confirm the Work / Argos / Chat SPA tabs. Hooks must be reviewed/trusted separately after changes; their trust is not shared by the workspace.

## Workspace-owned distribution

Instance URLs, workspace plugin IDs and publishing configuration belong to the
instance operator repository. For SAIDC the canonical configuration is
`profiles/editor-workspace.json` in https://acahti.s-aidc.com/saidc/acahti-plugins.
The product repository contains generic packages and packaging tools only.

Generate a marketplace from an explicit operator-owned profile:

```sh
python3 scripts/export_marketplace.py --config /path/to/editor-workspace.json --output /path/to/marketplace --commit <full-source-commit>
```

The generated version records the source commit and file hashes. Keep the existing
workspace plugin ID and installation policy. A workspace administrator must update
the marketplace's source binding or import the generated package; a repository
transfer does not reconfigure the external workspace. The old generated branch is
a historical snapshot and no longer receives automatic updates. Do not delete the
installed plugin to change its source. Members authorize their own accounts and
review changed hooks independently; credentials are never bundled.
