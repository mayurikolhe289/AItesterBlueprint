# Test Case Suite — VWO Login Dashboard (app.vwo.com)

> Status: **DRAFT — pending human review.** Companion to `VWO_Test_Plan_IEEE829.md`
> (TP-ID: VWO-LOGIN-TP-001). Conforms to repository `test-case-writer` and
> `api-test-designer` skills. Every case is traceable under the **Anti-Hallucination
> Rules** to a PRD fact (F1–F48), a user-supplied context item (R3), or an explicit Gap
> (G01–G10). No expected value is invented; undefined values are flagged
> *"expected result to confirm"*.

---

## 0. Legend & Conventions

| Term | Meaning |
|------|---------|
| **P0 / P1 / P2** | Priority: release-blocking / important / optional-conditional-deferred |
| **Type** | pos = positive, neg = negative, bnd = boundary, sec = security, perf = performance, rel = reliability, a11y = accessibility, cond = conditional, API, CB = cross-browser |
| **F#** | PRD fact (see `VWO_Login_Dashboard_Test_Cases.md`, fact table F1–F48) |
| **R3** | User-supplied context (enterprise SaaS, GDPR/SOC2, low latency, cross-browser, etc.) |
| **G##** | Gap — value not defined in PRD; confirm before execution |
| **⛔ CONFIRM** | Expected value is not in the PRD — replace with the confirmed value before execution |
| **[DATA-REF]** | Placeholder for the approved test-data set (G09) — never literal user credentials |

---

## 1. Gaps & Questions for the Author

| # | Area | Finding (⚠️/❌) | Question to author |
|---|------|----------------|--------------------|
| G01 | Error messages | ❌ exact text not in PRD | Provide exact strings for invalid-credential / empty-field errors |
| G02 | Password rules | ❌ complexity rules not in PRD | Provide min length, character classes, max length |
| G03 | Session timeout | ❌ duration not in PRD | Provide configured timeout values & idle policies |
| G04 | 2FA | ⚠️ optional, method unspecified | Is 2FA enabled? Delivery method (TOTP/SMS/authenticator)? |
| G05 | SSO | ⚠️ protocols named (SAML/OAuth) only | Which IdPs are enabled? Test accounts available? |
| G06 | Browser matrix | ❌ not defined | Provide supported browsers/OS/devices for CB-1xx |
| G07 | "Standard connection" | ❌ undefined | Define the reference connection profile for the 2 s load test |
| G08 | API docs | ❌ no OpenAPI/contract | Provide endpoint specs for AT-1xx matrix |
| G09 | Test data/selectors | ❌ not provided | Provide synthetic credentials, valid/invalid datasets, stable locators |
| G10 | Post-login features | ❌ insufficient information | Scope user management / workspace / tracking-dashboard tests separately |

---

## 2. Gherkin / BDD — Core Behaviors (P0)

### GB-01: Successful login (F5, F34, F42, F46)
```gherkin
Feature: Login
  Scenario: User logs in with valid credentials
    Given the user opens the VWO login page at https://app.vwo.com/#/login
    And the email field is displayed
    And the password field is displayed
    When the user enters a valid email "[DATA-REF: valid-email]"
    And the user enters a valid password "[DATA-REF: valid-password]"
    And the user submits the login form
    Then the login request completes successfully
    And the user is transitioned to the main VWO dashboard
    And a clear success indication is shown
```

### GB-02: Failed login shows clear error (F12, F44)
```gherkin
  Scenario: User logs in with invalid credentials
    Given the user is on the VWO login page
    When the user enters email "[DATA-REF: invalid-email]" and password "[DATA-REF: invalid-password]"
    And the user submits the login form
    Then the authentication attempt fails
    And a clear, actionable error message is displayed  # ⛔ CONFIRM exact text (G01)
    And the user remains on the login page
```

### GB-03: Remember Me persists session (F2, F41)
```gherkin
  Scenario: Session persists when Remember Me is selected
    Given the user is on the VWO login page
    And a valid account "[DATA-REF: valid-account]" is available
    When the user checks the "Remember Me" checkbox
    And the user logs in successfully
    And the user closes and reopens the browser
    Then the user's session persists without re-entering credentials
```

### GB-04: Forgot password → email reset (F13, F14)
```gherkin
  Scenario: User resets password via email
    Given the user is on the VWO login page
    When the user clicks the Forgot Password link
    And the user follows the password reset flow
    Then a reset flow is initiated with secure token generation
    And an email-based recovery option is available
```

### GB-05: Blur-based validation (F9, F10)
```gherkin
  Scenario: Invalid email is flagged on blur
    Given the user is on the VWO login page
    When the user types "[DATA-REF: malformed-email]" into the email field
    And the user tabs out of the email field (blur)
    Then field validation feedback is shown immediately
    And the malformed email is flagged by automatic format validation
```

### GB-06: Session timeout enforcement (F6)
```gherkin
  Scenario: Idle session expires at configured timeout
    Given the user is logged in with a valid session
    And a session timeout "[DATA-REF: session-timeout]" is configured  # ⛔ CONFIRM (G03)
    When the user remains idle beyond the configured timeout
    Then the session is terminated
    And the user is returned to the login state
```

---

## 3. Tabular Test Cases

### M1 — Login UI Elements (F1–F4, F23, F24)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-101 | P0 | pos | Open https://app.vwo.com/#/login | 1. Observe the login form | Email address input field displayed | F1 |
| TC-102 | P0 | pos | Login page loaded | 1. Observe the login form | Password input field displayed | F1 |
| TC-103 | P0 | pos | Login page loaded | 1. Observe the login form | "Remember Me" checkbox option displayed | F2 |
| TC-104 | P0 | pos | Login page loaded | 1. Observe the page | Registration link (free trial signup path) present | F3 |
| TC-105 | P1 | pos | Login page loaded | 1. Observe the announcement banner | Banner highlights new UI launch with Light and Dark Mode options | F4 |
| TC-106 | P1 | pos | Login page loaded | 1. Compare colors/logo/typography against VWO design-system reference | Visual design aligns with VWO design system and color scheme | F23 |
| TC-107 | P1 | pos | Light/Dark toggle on banner | 1. Switch to Light mode 2. Switch to Dark mode 3. Reload page | Theme selection applies and persists across reload | F4, F24 |

### M2 — Authentication Flow (F5, F12, F19, F34, F42, F44, F46)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-201 | P0 | pos | Valid credentials [DATA-REF] | 1. Enter valid email 2. Enter valid password 3. Submit | Authentication succeeds; user transitioned to main VWO dashboard | F5, F34, F42 |
| TC-202 | P0 | pos | Valid credentials [DATA-REF] | 1. Log in successfully 2. Observe confirmation | Clear indication of successful login shown | F46 |
| TC-203 | P0 | neg | Invalid credentials [DATA-REF] | 1. Enter invalid email/password 2. Submit | Clear, actionable error message displayed for failed attempt | F12, F44 |
| TC-204 | P0 | neg | Invalid credentials; error observed | 1. Enter invalid credentials 2. Submit 3. Observe page state | User remains on login page after failure | F44 |
| TC-205 | P1 | pos | Valid credentials; normal latency | 1. Submit form 2. Observe UI during request | Loading state (clear feedback) shown during authentication processing | F19 |
| TC-206 | P1 | pos | Valid credentials | 1. Submit form 2. Observe transition | Seamless transition to main dashboard after successful authentication | F34 |

### M3 — Input Validation (F9, F10, F11, F15)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-301 | P0 | bnd | Login page loaded | 1. Type invalid value in email 2. Tab out (blur) | Validation feedback appears immediately on blur | F9 |
| TC-302 | P0 | bnd | Login page loaded | 1. Enter malformed email [DATA-REF] 2. Trigger blur | Automatic email format validation flags the malformed email | F10 |
| TC-303 | P1 | pos | Password field with strength meter | 1. Type weak password 2. Observe indicator 3. Type stronger password | Visual feedback for password requirements/strength provided | F11 |
| TC-304 | P1 | bnd | Complexity rules defined (G02) | 1. Submit with password violating documented rules 2. Observe | Submission blocked/flagged per enforced complexity standards | F15 |
| TC-305 | P1 | bnd | Login page loaded | 1. Submit empty email + password 2. Observe | Fields validated on blur / submission feedback shown ⛔ CONFIRM exact behavior (G01) | F9 |

### M4 — Password Management (F13, F14)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-401 | P0 | pos | Registered account [DATA-REF]; forgot-password link present | 1. Click Forgot Password 2. Follow flow | Password reset process initiates with secure token generation | F13 |
| TC-402 | P0 | pos | Forgot-password flow reachable | 1. Review recovery options | At least an email-based recovery option available | F14 |
| TC-403 | P1 | pos | Reset initiated (TC-401) | 1. Complete email-based reset 2. Verify | Password recovered via email-based reset path | F14 |

### M5 — Session & Remember Me (F2, F6, F27, F41, F43)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-501 | P0 | pos | Valid account; Remember Me available | 1. Check Remember Me 2. Log in 3. Close & reopen browser | Session persists per Remember Me selection | F2, F41 |
| TC-502 | P0 | bnd | Timeout configured (G03) | 1. Log in 2. Remain idle beyond timeout | Session terminates per configured timeout period | F6 |
| TC-503 | P1 | pos | Returning user with remembered credentials | 1. Return to login page 2. Use remembered credentials | Streamlined quick-access login provided | F41 |
| TC-504 | P1 | bnd | Logged-in session; same account | 1. Open second login session with same account | Duplicate-login behavior observed and documented ⛔ CONFIRM expected behavior (G03/G09) | F6 |
| TC-505 | P1 | sec | Logged-in session | 1. Inspect session token (devtools per security checklist) | Session token securely generated and managed ⛔ CONFIRM token attributes (G09) | F27 |

### M6 — Optional Authentication (F7, F8, F37, F38)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-601 | P2 | cond | 2FA enabled for test account; method defined (G04) | 1. Log in with credentials 2. Complete 2FA challenge | Optional 2FA supported and required when enabled | F7 |
| TC-602 | P2 | cond | SSO configured (G05); organizational account | 1. Log in via SSO path | Enterprise SSO login succeeds for organizational accounts | F8, F37 |
| TC-603 | P2 | cond | Social login enabled; Google/Microsoft account | 1. Log in via social identity provider option | Social login succeeds via identity provider | F38 |
| TC-604 | P2 | cond | 2FA enabled; wrong 2FA value | 1. Enter incorrect 2FA value | Failed 2FA handled with clear message ⛔ CONFIRM behavior (G04) | F7 |

### M7 — UX & Interface (F16, F17, F18, F19)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-701 | P1 | pos | Login page loaded | 1. Observe focus on page load | Focus automatically placed on first input field | F17 |
| TC-702 | P1 | pos | Login page loaded | 1. Click email label 2. Click password label | Clicking each label focuses/activates its associated field | F18 |
| TC-703 | P1 | pos | Mobile viewport/device | 1. Load login page on mobile 2. Interact with all controls | Interface mobile-optimized; controls touch-friendly | F16 |
| TC-704 | P1 | pos | Valid credentials; normal latency | 1. Submit form 2. Observe | Clear loading feedback during authentication processing | F19 |

### M8 — Accessibility (F20, F21, F22, F48)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-801 | P1 | a11y | Screen reader available (NVDA/VoiceOver) | 1. Navigate login form with screen reader | ARIA labels announced; form screen-reader compatible | F20, F48 |
| TC-802 | P1 | a11y | Login page loaded; no mouse | 1. Operate all interactive elements via Tab/Enter/Escape | All interactive elements reachable and operable via keyboard | F22, F48 |
| TC-803 | P1 | a11y | High contrast option present | 1. Enable high contrast 2. Review page | High contrast option available for visually impaired users | F21 |
| TC-804 | P1 | a11y | Audit tooling (axe/PA11Y) | 1. Run automated + manual WCAG 2.1 AA checks | Login page complies with WCAG 2.1 AA | F48 |

### M9 — Security (F25–F30, R3)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-901 | P0 | sec | Proxy/devtools available | 1. Load login page 2. Submit credentials 3. Inspect transport | All login communications use SSL/TLS; no plaintext transmission | F28 |
| TC-902 | P0 | sec | Throttling policy configured (values TBD) | 1. Send repeated failed attempts beyond threshold | Requests throttled/blocked to prevent brute force | F30 |
| TC-903 | P1 | sec | Valid login; devtools/security tools | 1. Log in 2. Inspect session token attributes per checklist | Session tokens securely generated/managed ⛔ CONFIRM attributes (G09) | F27 |
| TC-904 | P1 | sec | Backend/DB access provided by engineering | 1. Verify stored credentials | Passwords stored encrypted with industry-standard hashing | F26 |
| TC-905 | P1 | sec | Network capture available | 1. Capture auth traffic end-to-end | End-to-end encryption for authentication data transmission | F25 |
| TC-906 | P1 | sec | Compliance checklist provided | 1. Review login data handling vs GDPR | User data handling adheres to GDPR | F29, R3 |
| TC-907 | P1 | sec | SOC2 evidence pack provided by security team | 1. Review SOC2 control evidence for login | SOC2 compliance evidence available ⛔ CONFIRM scope with security (R3 only) | R3 |

### M10 — Performance & Reliability (F31, F32, F33, R3)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-1001 | P0 | perf | "Standard connection" defined (G07) | 1. Load login page 2. Measure load time | Page loads within 2 seconds | F31 |
| TC-1002 | P1 | perf | Load tool + load model | 1. Execute load test with thousands of simultaneous login attempts | System handles thousands of simultaneous login attempts | F33 |
| TC-1003 | P1 | rel | Monitoring/uptime data access | 1. Review uptime metrics over reporting period | 99.9% uptime maintained | F32 |
| TC-1004 | P1 | perf | Network profiling tooling | 1. Measure asset sizes/requests 2. Verify minification/compression/CDN usage | Compressed images, minified CSS/JS, CDN utilization (PRD requirement) ⛔ CONFIRM measurement baseline (G07) | PRD Perf §, R3 |
| TC-1005 | P1 | rel | Failure state reproducible | 1. Trigger auth failure 2. Use support/recovery path | Multiple account recovery and support paths offered | F36, F45 |

### M11 — User Journeys (F3, F39, F40, F42, F43, F45)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-1101 | P1 | pos | Login page loaded; no account | 1. Click free trial signup link | Directed to free trial signup with minimal friction | F3, F39 |
| TC-1102 | P2 | pos | Newly registered account | 1. Complete registration 2. Proceed | Guided introduction to VWO capabilities post-registration | F40 |
| TC-1103 | P0 | pos | Valid credentials | 1. Log in successfully | User reaches personalized VWO dashboard immediately after auth | F34, F42 |
| TC-1104 | P1 | pos | Returning user with prior activity | 1. Log in 2. Check dashboard context | Context from previous sessions preserved ⛔ CONFIRM nature of context (G10) | F43 |
| TC-1105 | P1 | pos | Failed authentication experienced | 1. Review recovery options | Multiple recovery and support paths offered | F45, F36 |

### M12 — Analytics & KPI (F35, F47, PRD Metrics)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-1201 | P1 | pos | Analytics backend access | 1. Perform successful login 2. Perform failed login 3. Check analytics events | Login success and failure events tracked | F35 |
| TC-1202 | P1 | pos | Analytics/telemetry data over defined period | 1. Review login success-rate metric | Login success rate ≥ 95% target | F47 |

### M13 — Boundary / Edge Cases (F9, F10, G02, G09)

| ID | Priority | Type | Preconditions | Steps | Expected result | Traceability |
|----|----------|------|---------------|-------|-----------------|--------------|
| TC-1301 | P0 | bnd | Login page loaded | 1. Submit with both fields empty | Clear feedback for empty fields ⛔ CONFIRM message (G01) | F9 |
| TC-1302 | P0 | bnd | Login page loaded | 1. Enter email missing "@" 2. Blur | Malformed email flagged by format validation | F10 |
| TC-1303 | P1 | bnd | Login page loaded | 1. Enter max-length email [DATA-REF] 2. Submit | Behavior at max length documented ⛔ CONFIRM max length & behavior (G02/G09) | F9 |
| TC-1304 | P1 | bnd | Login page loaded | 1. Enter leading/trailing whitespace in email 2. Blur | Whitespace handling documented ⛔ CONFIRM trim behavior (G09) | F10 |
| TC-1305 | P1 | bnd | Login page loaded | 1. Enter non-ASCII email (unicode) 2. Blur | Unicode handling documented ⛔ CONFIRM expected behavior (G09) | F10 |
| TC-1306 | P1 | bnd | Two accounts differing only by case | 1. Log in with uppercase variant 2. Observe | Case-sensitivity behavior documented ⛔ CONFIRM (G09) | F5 |
| TC-1307 | P1 | neg | Login page loaded | 1. Submit with password field empty, email valid | Empty-password validation feedback ⛔ CONFIRM message (G01) | F9 |

### M14 — API Test Design Matrix (deferred — G08)

> All expectations remain **`contract-defined`** until an OpenAPI/API specification is
> supplied (per `api-test-designer` skill: never invent fields, status codes, or auth
> rules). These are design-intent rows, not executable cases.

| ID | Dimension | Request | Expected status | Assert | Contract ref |
|----|-----------|---------|-----------------|--------|--------------|
| AT-101 | happy path | contract-valid login request | contract-defined | response body matches schema; session/token returned | TBD |
| AT-102 | schema | missing required field (e.g., password) | contract-defined | documented error shape | TBD |
| AT-103 | schema | wrong field type | contract-defined | documented error shape | TBD |
| AT-104 | auth | no token / expired token | contract-defined | documented denial | TBD |
| AT-105 | auth | invalid/revoked token | contract-defined | documented denial | TBD |
| AT-106 | negative | invalid credentials | contract-defined | documented error; no sensitive detail leaked | TBD |
| AT-107 | boundary | max-length email / password fields | contract-defined | documented result | TBD |
| AT-108 | idempotency | same request sent twice | contract-defined | documented invariant (no duplicate side effects) | TBD |
| AT-109 | concurrency | parallel login requests, same account | contract-defined | documented behavior | TBD |
| AT-110 | security | brute-force threshold on API | contract-defined | rate-limit response documented | TBD |
| AT-111 | security | SQLi / injection payload in fields | contract-defined | documented sanitization behavior | TBD |
| AT-112 | security | sensitive data in responses (password/secret leakage) | contract-defined | no sensitive fields in response body | TBD |

**Coverage note:** all status codes, schemas, and error shapes are **untested and
undefined** until the API contract (G08) is provided. AT-101..AT-112 will be finalized
against the supplied contract.

### M15 — Cross-Browser Matrix (deferred — G06)

| ID | Priority | Type | Browser/OS | Core cases to run | Expected | Traceability |
|----|----------|------|------------|-------------------|----------|--------------|
| CB-101 | P2 | CB | [TBD — G06] | TC-101, TC-201, TC-203, TC-301 | Same behavior as baseline browser | R3 (cross-browser requirement) |
| CB-102 | P2 | CB | [TBD — G06] | TC-501, TC-701, TC-703 | Same behavior; responsive layout verified | R3, F16 |
| CB-103 | P2 | CB | [TBD — G06] | TC-801, TC-802 | a11y parity across browsers | R3, F48 |

**Coverage note:** the browser/OS/device matrix is **not defined** (G06). Rows are placeholders
to be completed by product/engineering before execution.

---

## 4. Requirement Traceability Matrix (summary)

| Requirement (fact) | Scenario (TP) | Test cases |
|--------------------|---------------|------------|
| F1 — email + password fields | TS-01, TS-03 | TC-101, TC-102 |
| F2 — Remember Me | TS-04 | TC-103, TC-501 |
| F3 — registration link | TS-14 | TC-104, TC-1101 |
| F4 — announcement banner + Light/Dark | TS-15 | TC-105, TC-107 |
| F5 — email/password auth | TS-01 | TC-201, TC-1306 |
| F6 — configurable session timeout | TS-08 | TC-502, TC-504 |
| F7 — optional 2FA | TS-28 | TC-601, TC-604 |
| F8, F37 — SSO | TS-29 | TC-602 |
| F9 — blur validation | TS-03 | TC-301, TC-305, TC-1301, TC-1303 |
| F10 — email format validation | TS-03 | TC-302, TC-1302, TC-1304, TC-1305 |
| F11 — password strength | TS-10 | TC-303 |
| F12, F44 — error messages | TS-02 | TC-203, TC-204 |
| F13, F14 — forgot password / recovery | TS-05 | TC-401, TC-402, TC-403 |
| F15 — password complexity | TS-10 | TC-304 |
| F16 — responsive design | TS-16 | TC-703 |
| F17 — auto-focus | TS-11 | TC-701 |
| F18 — clickable labels | TS-12 | TC-702 |
| F19 — loading states | TS-13 | TC-205, TC-704 |
| F20, F48 — ARIA / WCAG 2.1 AA | TS-17 | TC-801, TC-804 |
| F21 — high contrast | TS-17 | TC-803 |
| F22 — keyboard navigation | TS-17 | TC-802 |
| F23 — brand consistency | TS-15 | TC-106 |
| F24 — Light/Dark themes | TS-15 | TC-107 |
| F25 — e2e encryption | TS-20 | TC-905 |
| F26 — hashed storage | TS-19 | TC-904 |
| F27 — session tokens | TS-18 | TC-505, TC-903 |
| F28 — HTTPS | TS-06 | TC-901 |
| F29 — GDPR | TS-21 | TC-906 |
| F30 — rate limiting | TS-07 | TC-902 |
| F31 — 2 s load | TS-09 | TC-1001 |
| F32 — 99.9% uptime | TS-27 | TC-1003 |
| F33 — concurrency | TS-26 | TC-1002 |
| F34, F42 — dashboard transition | TS-01 | TC-201, TC-206, TC-1103 |
| F35 — analytics tracking | TS-24 | TC-1201 |
| F36, F45 — support/recovery paths | TS-25 | TC-1005, TC-1105 |
| F38 — social login | TS-30 | TC-603 |
| F39 — free trial signup path | TS-14 | TC-1101 |
| F40 — onboarding | TS-31 | TC-1102 |
| F41 — quick access | TS-04, TS-23 | TC-501, TC-503 |
| F43 — recent activity context | TS-23 | TC-1104 |
| F46 — success indication | TS-01 | TC-202 |
| F47 — 95% success rate | TS-24 | TC-1202 |
| R3 — SOC2 / low latency / cross-browser | TS-22, TS-32 | TC-907, CB-101..103, TC-1004 |

---

## 5. Test Data Requirements (references — G09)

| Data set | Purpose | Status |
|----------|---------|--------|
| [DATA-REF: valid-email / valid-password] | Positive auth paths (TC-201, GB-01) | TBD — data owner |
| [DATA-REF: invalid-email / invalid-password] | Negative paths (TC-203, GB-02) | TBD — data owner |
| [DATA-REF: malformed-email] | Blur/format validation (TC-302, GB-05) | TBD — data owner |
| [DATA-REF: max-length-email] | Boundary (TC-1303) | TBD — data owner |
| [DATA-REF: valid-account] | Remember Me / session tests | TBD — data owner |
| [DATA-REF: session-timeout] | Timeout tests (TC-502, GB-06) | TBD — security/config owner |
| 2FA / SSO / social accounts | Conditional modules M6 | TBD — dependent on G04/G05 |
| [DATA-REF: case-variant accounts] | Case sensitivity (TC-1306) | TBD — data owner |

**Hygiene:** all values are synthetic placeholders. No real user credentials are embedded
anywhere; secrets are injected at runtime only (repo skill guardrail).

---

## 6. Execution Order (suggested)

1. **Smoke (P0 functional):** TC-101, TC-102, TC-201, TC-203, TC-301, TC-501, TC-401
2. **Security (P0):** TC-901, TC-902
3. **Performance (P0):** TC-1001
4. **P1:** UX/a11y (M7, M8), security (TC-903..907), reliability (TC-1003..1005), journeys (M11), analytics (M12)
5. **P2 conditional:** M6 (2FA/SSO/social), onboarding, CB, AT — as inputs (G04/G05/G06/G08) are provided

---

## 7. HUMAN REVIEW GATE (mandatory — per repository skill)

- **I assumed:** tooling and measurement baselines; SOC2 scope (R3 context only); that
  "⛔ CONFIRM" flags identify every expected value not supported by the PRD.
- **I could not confirm:** G01 error text · G02 password rules · G03 timeout values ·
  G04 2FA method · G05 SSO IdPs · G06 browser matrix · G07 connection definition ·
  G08 API contract · G09 test data/selectors/credentials · G10 post-login feature scope.
- **Cases that are blocked pending input:** TC-304 (G02), TC-502/TC-504 (G03),
  TC-601/TC-604 (G04), TC-602 (G05), CB-101..103 (G06), TC-1001/TC-1004 (G07),
  AT-101..112 (G08), all [DATA-REF] cases (G09), TC-1104 (G10).
- **Open questions:** are 2FA, SSO, and social login enabled in the environment under
  test? Who owns the test-data set and API contract?
- ▶ **Approve, or edit, before these cases enter the suite or feed automation.**
