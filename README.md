# Pet behind composer

A small Hermes Agent desktop customization: the animated pet can show through the message composer at low opacity, clipped to the composer outline, while text and controls remain opaque. Its existing movement and reaction mapping are preserved.

- **Author:** Marco, Hermes Agent
- **Version:** 0.1.0
- **License:** MIT (see `LICENSE`; upstream Hermes Agent license retained and attributed)
- **Compatibility base:** Hermes Agent commit `3b421578588243f920886e6fe79a6faa87ea026e`
- **Verification scope:** the prior rounded/translucent variant was visually confirmed on macOS; the tabbed-session fix is covered by deterministic hook tests and is bundled, but the exact “+ beside the tab” path still needs a live visual check. Other operating systems are untested; later upstream revisions may need patch adaptation.

## Contents

- `SKILL.md` — portable Hermes skill with guarded manual workflow
- `assets/pet-behind-composer.patch` — exact verified source patch
- `assets/manifest.json` — sanitized base and hashes of patched source files
- `LICENSE` — upstream MIT license and copyright notice

## Authorized manual apply / check / build / restart

Do not apply unless the repository owner explicitly authorizes it. Back up the checkout and preserve unrelated local changes. Never force-apply. From the root of a clean checkout at the compatibility base, first check whether it is already present:

```sh
git apply --check --reverse /path/to/pet-behind-composer/assets/pet-behind-composer.patch
```

A successful reverse check means it is already applied: stop and do not apply twice. If reverse check fails, check forward applicability, then apply only if the forward check succeeds:

```sh
git apply --check /path/to/pet-behind-composer/assets/pet-behind-composer.patch
git apply /path/to/pet-behind-composer/assets/pet-behind-composer.patch
```

At `apps/desktop`, run the targeted tests and renderer typecheck (upstream commands can change):

```sh
pnpm exec vitest run src/store/pet.test.ts src/components/pet/pet-sprite.test.tsx src/components/pet/composer-clip.test.ts src/components/pet/use-composer-clip.test.tsx src/components/pet/roam-geometry.test.ts src/components/pet/roam-behavior.test.ts src/components/pet/use-pet-roam.test.tsx
pnpm exec tsc -p . --noEmit
```

The refreshed run passed 42 targeted tests and renderer typecheck. Coverage includes ignoring a hidden stale composer before a visible one, fallback surfaces and multiple visible underlays. The live “+ beside the tab” interaction has not yet been visually confirmed. Build and restart are separate authorized operations: follow the applicable Hermes Agent desktop build/release procedure, back up existing artifacts, and restart only the desktop application—not the gateway. Recheck the visual effect; a successful build or bundle hash is not visual verification. This package does not execute, build, restart, or modify a live checkout.

## License

The patch modifies Hermes Agent, Copyright (c) 2025 Nous Research, under the MIT License. The upstream license is included unchanged.
