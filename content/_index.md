---
title: valiss
layout: hextra-home
description: "valiss is offline tenant authentication for services: every token verifies against one pinned Ed25519 public key, with no auth server in the request path."
---

<div class="valiss-hero">
<div class="valiss-hero-main">

{{< hextra/hero-badge link="https://github.com/valiss-dev" >}}
  Open source · MIT
{{< /hextra/hero-badge >}}

<div class="valiss-hero-headline">
{{< hextra/hero-headline >}}
  No auth server in the request path
{{< /hextra/hero-headline >}}
</div>

<div class="valiss-hero-subtitle">
{{< hextra/hero-subtitle >}}
  valiss is offline tenant authentication for services. Every token verifies against one pinned Ed25519 public key: no introspection endpoint, no session store, and issuing credentials never touches production.
{{< /hextra/hero-subtitle >}}
</div>

<div class="valiss-hero-actions">
{{< hextra/hero-button text="Get started" link="/docs/quickstart/" >}}
{{< hextra/hero-button text="Read the docs" link="/docs/" style="background-color: transparent; color: inherit; box-shadow: inset 0 0 0 1px currentColor;" >}}
</div>

</div>
<div class="valiss-hero-aside">

<div class="valiss-terminal">

```sh
go get valiss.dev/valiss
```

```go
// Issue: the operator signs the account, the account signs the user.
accountToken, _ := valiss.IssueAccount(operator, accountPub,
    valiss.WithName("acme"), valiss.WithTTL(time.Hour))
userToken, _ := valiss.IssueUser(account, userPub,
    valiss.WithName("alice"), valiss.WithTTL(time.Hour))

// Verify offline: the operator public key and the allowlist, no network call.
acct, _ := valiss.VerifyAccount(accountToken, operatorPub)
verifier := valiss.NewVerifier(operatorPub, valiss.NewStaticAllowlist(acct.ID))
```

</div>

</div>
</div>

<div class="valiss-section-heading">
{{< hextra/hero-section >}}What you get{{< /hextra/hero-section >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card
    icon="lock-closed"
    title="Offline<br>verification"
    subtitle="Tokens verify against one pinned operator public key. No introspection endpoint, no session store, and no network call on the request path."
    link="/docs/security/"
  >}}
  {{< hextra/feature-card
    icon="finger-print"
    title="Proof of<br>possession"
    subtitle="By default a token authorizes nothing on its own. Each request is signed by the subject's own key, so a token captured off the wire is inert."
    link="/docs/security/"
  >}}
  {{< hextra/feature-card
    icon="shield-check"
    title="Fail-closed<br>allowlist"
    subtitle="An account token is trusted only if its id is on the list you deposited. Revocation is removal, and it cuts off every user beneath the account."
    link="/docs/concepts/allowlist/"
  >}}
  {{< hextra/feature-card
    icon="puzzle"
    title="Typed<br>extension grants"
    subtitle="Authorization rides signed, typed claims. The http and grpc transports enforce them fail-closed, and you can define your own domain extensions."
    link="/docs/concepts/extensions/"
  >}}
  {{< hextra/feature-card
    icon="refresh"
    title="Epoch<br>rotation"
    subtitle="Publish a signed operator token at a new epoch and re-mint. Every token from an earlier epoch is rejected cryptographically, with no allowlist edits."
    link="/docs/concepts/rotation/"
  >}}
  {{< hextra/feature-card
    icon="code"
    title="Go,<br>Python, TypeScript"
    subtitle="Go is the reference implementation. Python is a full client at parity. TypeScript ships the sign and verify primitives only, with no transport adapter yet. All speak one wire spec."
    link="/docs/"
  >}}
{{< /hextra/feature-grid >}}
