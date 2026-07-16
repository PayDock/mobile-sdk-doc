# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

> Part of the **SDKDev** workspace — see the [workspace overview](../CLAUDE.md). This is the public docs source that the `mobile-sdk-ios` and `mobile-sdk-android` READMEs link to.

## What this is

Public integration, setup, and theming documentation for the Paydock Mobile SDK (iOS + Android). It is content only — plain Markdown plus images — rendered on GitHub and the Paydock docs site. There is no code, application, or SDK here.

## Structure

Top-level folders, all Markdown unless noted:

- **`README.md`** — the docs landing page; a table of contents that links into every guide below (start here to see the intended reading order).
- **`setup/`** — `installation.md` (add the SDK via SPM / Maven Central) and `initialise.md` (configure `MobileSDKConfig`, environment, public key).
- **`theming/`** — `theming.md` overview, plus per-platform reference pages under `theming/ios/` (e.g. `buttonappearance.md`, `textfieldappearance.md`) and `theming/android/` (e.g. `button.md`, `textfield.md`) documenting each `*Appearance` type.
- **`widgets/`** — payment/processing widget guides: `card.md`, `giftcard.md`, `address.md`, `clicktopay.md`, `integrated3ds.md`, `standalone3ds.md`.
- **`digital-wallet-widgets/`** — wallet guides: `googlepay.md`, `applepay.md`, `paypal.md`, `paypalvault.md`, `afterpay.md`, `zip.md`, `colespay.md`.
- **`fraud/`** — fraud-tooling guides (e.g. `paypaldatacollectorutil.md`).
- **`errors.md`** — error/exception handling reference for both platforms.
- **`img/`** — PNG screenshots and diagrams referenced by the guides.

Docs are cross-platform: most guides cover **iOS and Android** in a single file, typically split into `## iOS` and `## Android` sections with Swift and Kotlin code samples respectively.

## Conventions when editing

Verify current structure by reading the neighbouring file before changing one — conventions are consistent across the repo:

- **Platform split:** a guide covers both platforms in one file, using `## iOS` and `## Android` H2 sections, each with numbered subsections (`### 1. Overview`, `### 2. Parameter definitions`, ... `### Widget Styling`). Keep the two platform sections parallel in scope.
- **Links between docs:** relative paths from the current file (e.g. `../widgets/card.md`) and root-absolute paths (e.g. `/setup/initialise.md`) both appear. Match the style already used in the file you are editing.
- **Deep links / anchor stability:** guides — and the external `mobile-sdk-ios` / `mobile-sdk-android` READMEs, and `theming.md` — link to specific headings via GitHub anchors (e.g. `card.md#5-widget-styling`). Renaming or renumbering a heading breaks those anchors, so treat heading text as an API. If you must rename one, update every referring link (grep the repo and remember the SDK READMEs live in the sibling repos).
- **Images:** reference under `/img/...` (root-absolute); add new assets to `img/` and link them the same way.
- **Coverage in sync with the SDKs:** the set of documented widgets/wallets and their parameters must reflect what `mobile-sdk-ios` and `mobile-sdk-android` actually ship. When adding or changing a guide, confirm class names, config types, and parameters against the real SDK sources rather than inventing them.

## How it's used / built

Plain Markdown with **no build, lint, or test tooling** — no package manager, no CI build step, nothing to run. Content is authored directly and rendered by GitHub / the docs site. To preview changes, view the Markdown on GitHub or in any Markdown previewer; check that cross-doc links and `/img/...` references resolve.
