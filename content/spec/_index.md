---
title: Specification
description: "The normative valiss wire specification, SPEC-1: token structure, signing and verification rules, and the extension registry."
# Render the mounted spec documents with the docs layout so they get the same
# sidebar, breadcrumb, and table of contents as the rest of the site instead of
# the centered default single/list layout.
type: docs
excludeSearch: true
cascade:
  - target:
      path: /spec/**
    type: docs
    # The SPEC-* documents open with their own H1; suppress the theme's title
    # heading so the page has a single top-level heading.
    hideTitle: true
  - target:
      path: /spec/spec-1
    title: "SPEC-1: valiss Wire Specification"
---

The normative wire specification for valiss. Implementations in every language
target these documents, and the conformance vectors are derived from them.

- [SPEC-1: valiss Wire Specification](/spec/spec-1/): the version 1 wire format,
  token structure, signing and verification rules, and extension registry.
