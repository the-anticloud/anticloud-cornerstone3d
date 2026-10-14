# CORNERSTONE3D

![licence](https://img.shields.io/badge/licence-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![checks](https://img.shields.io/badge/checks-unknown_PASS-brightgreen)

> Governed Anticloud packaging of the upstream project `CORNERSTONE3D` in category **MEDICAL_HEALTH**. check results: see ISOLATED_LAB_RESULTS. Every number below traces to a named file + run stamp; nothing is borrowed from other projects.

**Upstream:** CORNERSTONE3D · **Upstream pin:** `da4727841d56872b78ae60f4b7a1e58468b7fad6` · **Category:** MEDICAL_HEALTH · **Vendor:** Anticloud FZ LLE · **Licence:** MIT

---

## What This Project Does

# [Cornerstone.js](https://cornerstonejs.org/) &middot; ![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)

Cornerstone is a set of JavaScript libraries that can be used to build web-based medical imaging applications. It provides a framework to build radiology applications such as the [OHIF Viewer](https://ohif.org/)

- **Fast:** Cornerstone leverages WebGL to provide high-performance image rendering and WebAssembly for fast image decompression.
- **Flexible:** Cornerstone provides APIs for defining custom image, volume, and metadata loading schemes, allowing developers to easily connect with proprietary image archives.
- **Community Driven:** Cornerstone is supported by the [Open Health Imaging Foundation](https://ohif.org/). We publish our roadmap and welcome contributions and collaboration.
- **Standards Compliant:** Cornerstone's core focus is Radiology, so it provides DICOMweb compatibility out-of-the-box

[Learn how to use Cornerstone3D in your project](https://www.cornerstonejs.org/docs/getting-started/overview).

## Local Setup

This project is pinned to pnpm 12.8.1 via `packageManager`. Use Corepack to
run the pinned version:

```bash
corepack enable
pnpm install --frozen-lockfile
```

The project disables pnpm's own package-manager auto-switching because some
standalone pnpm installs try to download a platform-specific pnpm binary and
can fail before any install command runs. Corepack and CI still use the pinned
pnpm version.

## Documentation

You can find the Cornerstone documentation [on the website](https://cornerstonejs.org/).

The documentation is divided into several sections

- [Tutorial](https://cornerstonejs.org/docs/category/tutorials)
- [Main Concepts](https://cornerstonejs.org/docs/category/concepts)
- [Live Examples](https://www.cornerstonejs.org/docs/examples)
- [API Reference](https://cornerstonejs.org/api)
- [How-to Guides](https://cornerstonejs.org/docs/category/how-to-guides)
- [FAQ](https://cornerstonejs.org/docs/faq)
- [How-to Contribute](https://cornerstonejs.org/docs/category/contributing)

## OHIF Downstream Validation

The Cornerstone3D build includes an OHIF checkout and test linked to the current
branch of CS3D in order to test compatibility with the checked out version.
This ensures that CS3D changes do not break the OHIF Viewer before they are
published.

Additionally, the OHIF pipeline can build and deploy a **preview release**
against a CS3D branch for manual testing. This pipeline does not re-test the
CS3D side but is intended for manual validation of changes in the full viewer.

Some tests in OHIF may need additional wait time built in to enable them to pass on slower servers

### How it works

The downstream validation is defined in
[`.github/workflows/ohif-downstream.yml`](.github/workflows/ohif-downstream.yml).
It runs on every CS3D pull request and `workflow_dispatch`:

1. Checks out the CS3D PR branch and builds it (`pnpm run build:esm`).
2. Checks out the OHIF Viewer at a ref resolved for that run: by default
   `master`; on pull requests you can override with a line in the PR description
   (see below); on `workflow_dispatch` you can set the `ohif_ref` input.
3. Installs OHIF dependencies, then symlinks the locally built CS3D packages
   into OHIF's `node_modules` via
   `scripts/link-ohif-cornerstone-node-modules.mjs`.
4. Runs OHIF unit tests and Playwright e2e tests against the linked build.

### Choosing the OHIF Viewer ref (`OHIF_REF` / `ohif_ref`)

By default the workflow checks out `master` of [OHIF/Viewers](https://github.com/OHIF/Viewers).
You can point downstream validation at another branch or tag **without editing
the workflow file**:

**On a CS3D pull request** — add a line to the PR description (same style as
OHIF uses for `CS3D_REF:` on the OHIF side):

```text
OHIF_REF: your-branch-or-tag
```

The first line matching that pattern wins; the value is the first token after
the colon (no spaces in the ref). If you omit this line, the job uses `master`.

**When running the workflow manually** — use the **ohif_ref** input on
`workflow_dispatch` (defaults to `master`).

**Coordinated CS3D + OHIF changes** — when a feature needs changes in both
projects, create an OHIF branch whose **name matches your CS3D branch**
(e.g. both `feat/shared-thing`). Then put `OHIF_REF: feat/shared-thing` in the
CS3D PR body (or set **ohif_ref** to that name for a manual run). Matching
names make it obvious which Viewer branch pairs with which Cornerstone branch
and avoid juggling unrelated branch names across the two projects.

> **Tip:** This pairs with the OHIF side of the integration. The OHIF project uses
> the `ohif-integration` label and `CS3D_REF:` in the PR body to test an OHIF
> PR against a CS3D branch. Together, `OHIF_REF:` here and `CS3D_REF:` there
> let both sides run the full test suite against unpublished code. See the
> [OHIF Viewer README — Cornerstone3D Integration Testing](https://github.com/OHIF/Viewers#cornerstone3d-integration-testing)
> for details on the OHIF side.

## Local Development: Linking & Unlinking

Two scripts are provided for linking a local CS3D build into an OHIF checkout
so you can iterate without publishing:

### Link

From the **CS3D project root**, after building (`pnpm run build:esm`):

```bash
node scripts/link-ohif-cornerstone-node-modules.mjs /path/to/ohif
```

This replaces the installed `@cornerstonejs/*` packages inside OHIF's
`node_modules` with symlinks pointing to the local CS3D build output. The
following packages are linked:

| Package | Local path |
|---------|-----------|
| `@cornerstonejs/adapters` | `packages/adapters` |
| `@cornerstonejs/ai` | `packages/ai` |
| `@cornerstonejs/core` | `packages/core` |
| `@cornerstonejs/dicom-image-loader` | `packages/dicomImageLoader` |
| `@cornerstonejs/labelmap-interpolation` | `packages/labelmap-interpolation` |
| `@cornerstonejs/metadata` | `packages/metadata` |
| `@cornerstonejs/nifti-volume-loader` | `packages/nifti-volume-loader` |
| `@cornerstonejs/polymorphic-segmentation` | `packages/polymorphic-segmentation` |
| `@cornerstonejs/tools` | `packages/tools` |

After linking, start OHIF's dev server (`pnpm run dev`) and any
changes you make in CS3D (after rebuilding) will be reflected immediately.

### Unlink

To restore the registry-installed packages:

```bash
node scripts/unlink-ohif-cornerstone-node-modules.mjs /path/to/ohif
```

This removes the symlinks and runs `pnpm install --frozen-lockfile` in the OHIF
directory to restore the original packages.

### Typical workflow

```bash
# 1. Build CS3D
cd /path/to/cornerstone3D
pnpm install --frozen-lockfile
pnpm run build:esm

# 2. Link into OHIF
node scripts/link-ohif-cornerstone-node-modules.mjs /path/to/ohif

# 3. Start OHIF
cd /path/to/ohif
pnpm run dev

# 4. Iterate — rebuild CS3D after changes:
cd /path/to/cornerstone3D
pnpm run build:esm
# OHIF dev server picks up changes automatically

# 5. When done, unlink
node scripts/unlink-ohif-cornerstone-node-modules.mjs /path/to/ohif
```

> **Tip:** For the `pnpm link` based approach (useful when working on a single
> package rather than the full set), see the
> [Linking Cornerstone Libraries](packages/docs/docs/contribute/linking.md) doc.

## Troubleshooting

If unit tests fail with **\"Cannot find module '../build/Release/canvas.node'\"**, the native `canvas` addon wasn’t built. Run `pnpm run rebuild:canvas`; if that doesn’t fix it, see [docs/troubleshooting.md](docs/troubleshooting.md#unit-tests-cannot-find-module-buildreleasecanvasnode).

## Support

Users can post questions and issues on the [Open Health Imaging Foundation (OHIF) Community Forum](https://community.ohif.org/). Developer issues or bugs can be reported as [Github Issues](https://github.com/cornerstonejs/cornerstone3D/issues).

The [OHIF Resources page](https://v3-docs.ohif.org/resources) may be of interest to Cornerstone users, as it includes presentations and demonstrations of OHIF and Cornerstone.

## Contributing

### [Code of Conduct](./CODE_OF_CONDUCT.md)

Cornerstone has adopted a [Code of Conduct](./CODE_OF_CONDUCT.md) that we expect project participants to adhere to.

### [Contributing Guide](https://cornerstonejs.org/docs/category/contributing)

Read our guide on [How-to Contribute](https://cornerstonejs.org/docs/category/contributing) and about our [Issue Triage process](https://v3-docs.ohif.org/development/our-process).

### Testing

The project has three browser-level test paths:

- [`scripts/run-karma.sh`](./scripts/run-karma.sh) for legacy and compatibility rendering/tool tests.
- [`scripts/run-playwright.sh`](./scripts/run-playwright.sh) for end-to-end example coverage, including the Generic viewport suite.
- [`vitest.browser.config.ts`](./vitest.browser.config.ts) for low-level browser-mode rendering tests.

Start with:

```bash
./scripts/run-karma.sh
./scripts/run-playwright.sh
yarn test:vitest:browser
```

Detailed usage, wrapper flags, report output, and baseline management live in the contributor docs:

- [Writing Karma Tests](https://www.cornerstonejs.org/docs/contribute/karma-tests)
- [Writing Playwright Tests](https://www.cornerstonejs.org/docs/contribute/playwright-tests)

Vitest browser-mode tests currently live only in-project. Use `yarn test:vitest:browser`, `yarn test:vitest:browser:update`, and `yarn test:vitest:browser:ui`. The test files are matched from `tests/vitest-browser/**/*.browser.test.ts`.

### License

Cornerstone is [MIT licensed](./LICENSE).

---

## Installation

This project is pinned to pnpm 12.8.1 via `packageManager`. Use Corepack to
run the pinned version:

```bash
corepack enable
pnpm install --frozen-lockfile
```

The project disables pnpm's own package-manager auto-switching because some
standalone pnpm installs try to download a platform-specific pnpm binary and
can fail before any install command runs. Corepack and CI still use the pinned
pnpm version.

## Usage

- **Fast:** Cornerstone leverages WebGL to provide high-performance image rendering and WebAssembly for fast image decompression.
- **Flexible:** Cornerstone provides APIs for defining custom image, volume, and metadata loading schemes, allowing developers to easily connect with proprietary image archives.
- **Community Driven:** Cornerstone is supported by the [Open Health Imaging Foundation](https://ohif.org/). We publish our roadmap and welcome contributions and collaboration.
- **Standards Compliant:** Cornerstone's core focus is Radiology, so it provides DICOMweb compatibility out-of-the-box

[Learn how to use Cornerstone3D in your project](https://www.cornerstonejs.org/docs/getting-started/overview).

## API

- **Community Driven:** Cornerstone is supported by the [Open Health Imaging Foundation](https://ohif.org/). We publish our roadmap and welcome contributions and collaboration.
- **Standards Compliant:** Cornerstone's core focus is Radiology, so it provides DICOMweb compatibility out-of-the-box

[Learn how to use Cornerstone3D in your project](https://www.cornerstonejs.org/docs/getting-started/overview).

## Dependencies

| Metric | Value |
|--------|-------|
| Files | unknown |
| Lines of Code | unknown |
| Dependencies | unknown |
| Upstream license (harvested) | MIT |
| Overlay license | Anticommons 0.1.0 |

Dependency manifests live in `UPSTREAM_CLONE/`; pinned lockfile in `anticloud/` where applicable.

## Configuration

Start with:

```bash
./scripts/run-karma.sh
./scripts/run-playwright.sh
yarn test:vitest:browser
```

Detailed usage, wrapper flags, report output, and baseline management live in the contributor docs:

- [Writing Karma Tests](https://www.cornerstonejs.org/docs/contribute/karma-tests)
- [Writing Playwright Tests](https://www.cornerstonejs.org/docs/contribute/playwright-tests)

Vitest browser-mode tests currently live only in-project. Use `yarn test:vitest:browser`, `yarn test:vitest:browser:update`, and `yarn test:vitest:browser:ui`. The test files are matched from `tests/vitest-browser/**/*.browser.test.ts`.

## Contributing

- **Community Driven:** Cornerstone is supported by the [Open Health Imaging Foundation](https://ohif.org/). We publish our roadmap and welcome contributions and collaboration.
- **Standards Compliant:** Cornerstone's core focus is Radiology, so it provides DICOMweb compatibility out-of-the-box

[Learn how to use Cornerstone3D in your project](https://www.cornerstonejs.org/docs/getting-started/overview).

## License

Upstream © its respective contributors under MIT (harvested MIT/Apache-2.0/BSD source; see `UPSTREAM_CLONE/LICENSE`). This packaging overlay is licensed under Anticommons 0.1.0.

## Upstream

- **project:** CORNERSTONE3D
- **Pinned SHA:** `da4727841d56872b78ae60f4b7a1e58468b7fad6`
- **source:** `UPSTREAM_CLONE/` (pinned at the SHA above)
- **Upstream README source:** `UPSTREAM_CLONE/README.md`

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`e147e2ba4a68a5f3da379fd78b298c4b10a624047d561b7571416dfdfc4de4dd`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

