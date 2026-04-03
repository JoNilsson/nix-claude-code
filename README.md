# nix-claude-code

Nix flake providing pre-built [Claude Code](https://claude.ai/code) CLI binaries from Anthropic's official distribution.

## How it works

- Downloads precompiled Bun binaries directly from Anthropic's GCS distribution bucket
- SHA256 hashes verified at Nix build time via SRI format
- Version discovery via `update.ts` (runs hourly in CI), commits `versions/*.json` manifests
- No npm/pnpm build step — deterministic, reproducible, fast

## Usage

```nix
# flake.nix
{
  inputs.nix-claude-code.url = "github:JoNilsson/nix-claude-code";
  inputs.nix-claude-code.inputs.nixpkgs.follows = "nixpkgs";
}
```

### Packages

| Package | Description |
|---------|-------------|
| `claude` / `default` | Claude Code binary + `gh` CLI in PATH |
| `claude-minimal` | Claude Code binary only |

### Overlay

```nix
nixpkgs.overlays = [ nix-claude-code.overlays.default ];
# Provides: pkgs.claude-code, pkgs.claude-code-minimal
```

## Supported platforms

- `x86_64-linux`
- `aarch64-linux`
- `x86_64-darwin`
- `aarch64-darwin`

## Updating

New versions are discovered hourly by GitHub Actions and committed as `versions/*.json` files. To pull the latest into your flake:

```bash
nix flake update nix-claude-code
```

## License

MIT
