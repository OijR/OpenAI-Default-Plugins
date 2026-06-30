# Codex Notes

## What Changed
- The top-level marketplace identity in `.agents/plugins/marketplace.json` is personal: `oijr-openai-default-plugins`.
- The top-level marketplace identity in `.agents/plugins/api_marketplace.json` is personal: `oijr-openai-api-plugins`.
- Plugin folder names, plugin entry names, plugin source paths, package names, and `.codex-plugin/plugin.json` names are unchanged.
- `.github/workflows/sync-upstream.yml` syncs this fork from `https://github.com/openai/plugins.git` and reapplies the personal marketplace names after each merge.

## Why
Codex reserves OpenAI's official marketplace names, including `openai-curated` and `openai-api-curated`. A personal fork must use non-reserved top-level marketplace names before Codex can add it as a custom marketplace.

## Verification Commands
```bash
python3 -m json.tool .agents/plugins/marketplace.json >/dev/null
python3 -m json.tool .agents/plugins/api_marketplace.json >/dev/null

python3 - <<'PY'
import json
from pathlib import Path

expected = {
    ".agents/plugins/marketplace.json": "oijr-openai-default-plugins",
    ".agents/plugins/api_marketplace.json": "oijr-openai-api-plugins",
}

reserved = {
    "openai-curated",
    "openai-api-curated",
}

for path, expected_name in expected.items():
    data = json.loads(Path(path).read_text())
    actual = data.get("name")

    if actual in reserved:
        raise SystemExit(f"{path} still uses reserved marketplace name: {actual}")

    if actual != expected_name:
        raise SystemExit(f"{path} has unexpected marketplace name: {actual!r}")

print("Marketplace names validated.")
PY
```

## Add And Refresh
Add the custom marketplace in the Codex app with:
- Source: `OijR/OpenAI-Default-Plugins`
- Git ref: `main`
- Sparse paths: leave blank

CLI equivalent:
```bash
codex plugin marketplace add OijR/OpenAI-Default-Plugins --ref main
```

After GitHub Actions syncs this fork with upstream, refresh with:
```bash
codex plugin marketplace upgrade oijr-openai-default-plugins
```

or refresh all marketplaces:
```bash
codex plugin marketplace upgrade
```

## Known Remaining Risks
- The GitHub Actions workflow has been validated locally for syntax-sensitive shell and JSON logic, but the scheduled job itself must run on GitHub to prove repository permissions and branch protection behavior.
- The marketplace should be added without sparse paths because plugin `source.path` values resolve from the marketplace root and point at `./plugins/...`.
