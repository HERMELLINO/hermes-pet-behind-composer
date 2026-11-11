---
name: pet-behind-composer
description: Apply and verify a pet underlay behind the composer.
version: 0.1.0
author: Marco, Hermes Agent
license: MIT
platforms: [macos]
metadata:
  hermes:
    tags: [desktop, pet, composer, customization]
---

# Pet behind composer

## When to use
Use this skill when installing the verified desktop customization that lets the animated pet appear subtly behind the message composer while keeping text and controls opaque.

## Safety and compatibility
This is a source patch for Hermes Agent desktop, based on upstream commit `3b421578588243f920886e6fe79a6faa87ea026e`. Compatibility is verified for that exact base only. For later versions, inspect and adapt the patch rather than forcing it. The prior rounded/translucent behavior was visually verified on macOS. The tabbed-session correction is covered by deterministic hidden/visible/fallback-composer tests; live visual confirmation of the “+ beside the tab” path is still pending. Other operating systems have not been tested.

Never apply without explicit user authorization. Do not overwrite local changes, publish, or restart services without approval. Make a clean backup first. The patch is not idempotent: check it before applying and use the inverse check to detect an already-applied patch.

## Apply and verify (manual, authorized)
From the root of a clean Hermes Agent checkout at the stated base commit:

```sh
# Confirm base and no existing application
git rev-parse HEAD
git apply --check --reverse /path/to/pet-behind-composer/assets/pet-behind-composer.patch
```

If the reverse check succeeds, it is already applied: stop; do not apply again. Otherwise require the forward check to succeed:

```sh
git apply --check /path/to/pet-behind-composer/assets/pet-behind-composer.patch
git apply /path/to/pet-behind-composer/assets/pet-behind-composer.patch
```

Run targeted tests and renderer typecheck from `apps/desktop` (commands may differ across upstream revisions):

```sh
pnpm exec vitest run src/store/pet.test.ts src/components/pet/pet-sprite.test.tsx src/components/pet/composer-clip.test.ts src/components/pet/use-composer-clip.test.tsx src/components/pet/roam-geometry.test.ts src/components/pet/roam-behavior.test.ts src/components/pet/use-pet-roam.test.tsx
pnpm exec tsc -p . --noEmit
```

Expected verification for this package revision: 42 targeted tests and renderer typecheck passed, including hidden stale composer, visible fallback, and multiple visible underlays. The specific tab-bar “+” path remains unverified visually. Build/restart only with separate explicit authorization, using the project's documented desktop build workflow. Do not restart the gateway. Visually verify the effect after restart; passing tests or bundle hashes alone do not establish visual correctness.
