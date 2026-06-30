# Customizations

This fork keeps OpenAI's default plugin marketplace synced from upstream while using personal marketplace names that Codex can add as custom marketplaces.

## Modified Files

### `.agents/plugins/marketplace.json`
- Exact changes: top-level `name` changed to `oijr-openai-default-plugins`; top-level `interface.displayName` changed to `OijR OpenAI Default Plugins`.
- Scope: minor tweak.
- Goal: avoid the reserved `openai-curated` marketplace identity while preserving all plugin entries, paths, and existing formatting.

### `.agents/plugins/api_marketplace.json`
- Exact changes: top-level `name` changed to `oijr-openai-api-plugins`; top-level `interface.displayName` changed to `OijR OpenAI API Plugins`.
- Scope: minor tweak.
- Goal: avoid the reserved `openai-api-curated` marketplace identity while preserving all plugin entries and paths.

### `.github/workflows/sync-upstream.yml`
- Exact changes: new workflow that fetches `openai/plugins`, merges `upstream/main`, only auto-resolves conflicts in the two marketplace JSON files, reapplies the personal marketplace names without whole-file JSON reformatting, validates the manifests, commits if needed, and pushes changed `main`.
- Scope: new automation.
- Goal: keep this fork updated from OpenAI upstream without reintroducing reserved top-level marketplace names.

### `Codex.md`
- Exact changes: new project note documenting the marketplace rename, sync workflow, verification commands, install/upgrade commands, and remaining risks.
- Scope: documentation.
- Goal: preserve operational context for future Codex runs and upstream updates.

### `custom.md`
- Exact changes: new customization ledger.
- Scope: documentation.
- Goal: provide an exact blueprint for backing up, pulling upstream updates, and reinjecting custom fork changes.
