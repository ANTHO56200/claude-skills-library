---
name: dockerfile-optimizer
description: Optimize a Dockerfile for size, build speed, and security. Use when writing or reviewing a Dockerfile, when images are huge or builds are slow, or asked "optimize this Dockerfile", "reduce image size", "why is my build slow". Multi-stage, cache-friendly, non-root.
---

# Dockerfile Optimizer

A good image is small, builds fast on cache, and doesn't ship a root shell and your build
tools to production. Fix all three.

## Size

- **Multi-stage build**: build in a fat stage, copy only the artifacts into a slim runtime
  stage. Ship the binary/dist, not the compiler, dev deps, and source.
- **Slim base**: `-slim`/`alpine`/distroless where viable. Don't ship a full OS to run one binary.
- **Prune**: production deps only (`npm ci --omit=dev`, `pip --no-cache-dir`); clean package
  caches in the SAME layer you install (a separate `rm` layer doesn't shrink the image).
- `.dockerignore`: exclude `.git`, `node_modules`, tests, secrets — smaller context, faster build, safer.

## Build speed (layer caching)

- **Order by change frequency**: copy dependency manifests and install BEFORE copying source.
  Then a source edit doesn't reinstall every dependency. This is the single biggest win.
- Combine related `RUN`s; each instruction is a layer.
- Pin versions for reproducibility (a floating `latest` base busts cache and drifts).

## Security

- **Non-root**: create and `USER` a non-root user; don't run the app as root.
- No secrets in layers (build args/ENV bake into the image and history) — use build secrets
  or runtime injection. A secret in any layer is a secret in the shipped image.
- Pin the base by digest for supply-chain integrity; scan for known CVEs.
- Minimal surface: no shell/package manager in the runtime image if you don't need it (distroless).

## Rules

- Multi-stage + manifest-before-source caching are the two highest-value changes — start there.
- Never bake a secret into an image layer; layers are forever and extractable.
- Verify the optimized image still runs (build + run + smoke) — don't ship a smaller broken image.

## Output

The optimized Dockerfile, what changed and why (size/speed/security), and the before→after
image size if measurable.
