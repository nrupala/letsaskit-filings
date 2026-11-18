<!--
SPDX-FileCopyrightText: 2026 2351485 ALBERTA INC.
SPDX-License-Identifier: Apache-2.0
-->

# Security policy

## What this repository is

This repository holds documentation and listing manifests for the
hosted LetsAskIt MCP server. It contains no server source code. The
service itself runs at https://letsaskit.com and is operated by
2351485 ALBERTA INC. (AIMLDS).

## Reporting a vulnerability

If you believe you have found a security issue in the LetsAskIt
service or in anything published from this repository, report it
privately to **info@aimlds.org** with:

- a description of the issue and where you observed it,
- the steps to reproduce it, if you have them,
- the impact as you understand it.

Please do not open a public issue for a suspected vulnerability, and
do not test against accounts or data that are not your own.

Reports are read by the operator and handled by severity. We will
confirm receipt and tell you what we find; if the report leads to a
fix, the fix and its testing record are noted in the product's public
release log at https://letsaskit.com/changelog/.

## Scope notes

- API keys are stored server-side as SHA-256 hashes only; a lost key
  is revoked from the account page at letsaskit.com, not through this
  repository.
- Reviewer keys issued for directory listings are scoped, named per
  listing, and revoked when a review closes.
