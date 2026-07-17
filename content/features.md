---
title: Features
layout: hextra-home
description: "The capabilities of valiss's offline tenant-authentication model, each linked to the documentation that specifies it."
---

<div class="valiss-hero-headline">
{{< hextra/hero-headline >}}
  What valiss gives you
{{< /hextra/hero-headline >}}
</div>

<div class="valiss-hero-subtitle valiss-features-lead">
{{< hextra/hero-subtitle >}}
  valiss authenticates services offline against one pinned public key. Each capability below is a property of that model, linked to the documentation that specifies it.
{{< /hextra/hero-subtitle >}}
</div>

<div class="valiss-section-heading">
{{< hextra/hero-section >}}Trust model{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    icon="globe-alt"
    title="Offline<br>verification"
    subtitle="There is no issuer to consult at verification time. A verifier walks each credential from the pinned key down and checks every signature locally, with no network call."
    link="/docs/security/"
  >}}
  {{< hextra/feature-card
    icon="key"
    title="One pinned<br>anchor"
    subtitle="A server pins a single value, the operator public key. NewVerifier(operatorPub, allowlist) is the whole trust configuration; it holds no seed."
    link="/docs/security/"
  >}}
  {{< hextra/feature-card
    icon="shield-check"
    title="Role in the<br>key material"
    subtitle="An nkey carries its role, so a verifier checks at every hop that an operator key signed the account token and an account key signed the user token. Cross-level confusion cannot be expressed."
    link="/docs/concepts/tokens/"
  >}}
{{< /hextra/feature-grid >}}

<div class="valiss-section-heading">
{{< hextra/hero-section >}}Credentials that resist theft{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    icon="finger-print"
    title="Proof of<br>possession"
    subtitle="A token authorizes nothing on its own. A request is authentic only if it is signed, per request, by the subject's seed, so capturing a token off the wire does not let you act as its subject."
    link="/docs/security/"
  >}}
  {{< hextra/feature-card
    icon="scale"
    title="Delegation<br>cannot widen"
    subtitle="An account seed can mint users, but never more scope than the account holds, and an account can never mint another account. A stolen account seed is a tenant breach, not a domain one."
    link="/docs/concepts/entities/"
  >}}
  {{< hextra/feature-card
    icon="clock"
    title="Bearer tokens,<br>scoped narrowly"
    subtitle="Only user tokens may waive the per-request signature, for clients that cannot hold a key. It is a deliberate weakening: pair with TLS and a short validity window."
    link="/docs/security/"
  >}}
{{< /hextra/feature-grid >}}

<div class="valiss-section-heading">
{{< hextra/hero-section >}}Revocation and rotation{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid cols="4" >}}
  {{< hextra/feature-card
    icon="badge-check"
    title="Fail-closed<br>allowlist"
    subtitle="An account token is accepted only if its id is on the list you deposited. It is an explicit allowlist, not a denylist of revoked ids, so nothing is trusted by default."
    link="/docs/concepts/allowlist/"
  >}}
  {{< hextra/feature-card
    icon="switch-horizontal"
    title="Pluggable allowlist<br>sources"
    subtitle="The Allowlist interface is a single Allowed(jti) method. Back it with a file swapped atomically, a database query, a cache, or a fully dynamic per-call policy."
    link="/docs/concepts/allowlist/"
  >}}
  {{< hextra/feature-card
    icon="refresh"
    title="Epoch<br>rotation"
    subtitle="A self-signed operator token carries an epoch. Bump it and re-mint, and every token from an earlier epoch is rejected cryptographically, with no allowlist edits."
    link="/docs/concepts/rotation/"
  >}}
  {{< hextra/feature-card
    icon="collection"
    title="Multiple trusted<br>operators"
    subtitle="A keyring verifies messages and requests from several independent trust domains, each entry a self-signed operator token with its own name, epoch, and window."
    link="/docs/concepts/rotation/"
  >}}
{{< /hextra/feature-grid >}}

<div class="valiss-section-heading">
{{< hextra/hero-section >}}Authorization{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    icon="puzzle"
    title="Typed extension<br>grants"
    subtitle="Authorization rides named extension claims: signed, typed payloads under the token's ext field. The same concrete type comes back out on the server, with no string plumbing."
    link="/docs/concepts/extensions/"
  >}}
  {{< hextra/feature-card
    icon="lock-closed"
    title="Transport<br>enforcement"
    subtitle="The http and grpc integrations authorize through signed extensions and fail closed. Every token in the chain must carry the extension, and the zero value grants nothing."
    link="/docs/concepts/extensions/"
  >}}
  {{< hextra/feature-card
    icon="template"
    title="Custom domain<br>extensions"
    subtitle="Any struct with an ExtensionName method is signed opaquely and recovered as its concrete type in the handler. Optionally validate it inside the verification pipeline."
    link="/docs/concepts/extensions/"
  >}}
{{< /hextra/feature-grid >}}

<div class="valiss-section-heading">
{{< hextra/hero-section >}}Proof of origin{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    icon="document-text"
    title="Message<br>tokens"
    subtitle="Extend the chain one level with short-lived tokens minted per emitted message. Any receiver verifies provenance offline knowing only the operator public key."
    link="/docs/concepts/messages/"
  >}}
  {{< hextra/feature-card
    icon="switch-vertical"
    title="httpsig<br>and grpcsig"
    subtitle="Contrib transports wire proof of origin end to end: a client mints a token per outgoing request, and a middleware or interceptor verifies it on the other side."
    link="/docs/concepts/messages/"
  >}}
  {{< hextra/feature-card
    icon="shield-exclamation"
    title="Replay and<br>tamper binding"
    subtitle="A message token binds its destination with audience and its payload bytes with a checksum. A receiver sets ExpectAudience and WithPayload to close cross-destination replay and tampering."
    link="/docs/security/"
  >}}
{{< /hextra/feature-grid >}}

<div class="valiss-section-heading">
{{< hextra/hero-section >}}Credentials and custody{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    icon="document"
    title="The creds<br>file"
    subtitle="A marker-delimited text file packages a client's tokens and signing seed together, everything a client holds and nothing the server does. Parsing is strict and fails closed at the door."
    link="/docs/concepts/creds/"
  >}}
  {{< hextra/feature-card
    icon="key"
    title="Unidirectional<br>custody"
    subtitle="The seed is the secret and lives only on the signing side. The server holds an operator public key and an allowlist, never a seed, so a verifier compromise cannot leak signing power."
    link="/docs/concepts/creds/"
  >}}
  {{< hextra/feature-card
    icon="cube"
    title="Lean creds<br>or bundles"
    subtitle="User creds can omit the account token and let a server resolver supply it, or embed it as a self-contained bundle. Pick by where you would rather hold the account token."
    link="/docs/concepts/creds/"
  >}}
{{< /hextra/feature-grid >}}

<div class="valiss-section-heading">
{{< hextra/hero-section >}}Languages and interoperability{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    icon="code"
    title="Go, Python,<br>TypeScript"
    subtitle="Go is the reference implementation. Python is a full client library at parity. TypeScript ships the sign and verify primitives from source, with no transport adapter yet."
    link="/docs/"
  >}}
  {{< hextra/feature-card
    icon="clipboard-check"
    title="Conformance<br>vectors"
    subtitle="A frozen, append-only corpus pairs each artifact with its expected outcome. Every implementation ships an offline runner that must pass all of them. The vectors decide which implementation diverged."
    link="/docs/versioning/"
  >}}
  {{< hextra/feature-card
    icon="check-circle"
    title="The interop<br>gate"
    subtitle="A live matrix runs server against client against transport across languages. A stable release must prove it interoperates with every other implementation before it can ship."
    link="/docs/versioning/"
  >}}
{{< /hextra/feature-grid >}}
