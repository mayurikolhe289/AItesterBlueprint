# Test Plan — VWO Login Dashboard (app.vwo.com)

> Status: **DRAFT — pending human review.** Not approved until a QA owner signs off.
> Conforms to: IEEE 829 standard test plan structure + repository `test-plan-generator`
> skill template. All content is traceable under the **Anti-Hallucination Rules** — nothing
> is asserted beyond the PRD, the user-supplied context, and repo-provided templates.

---

## 1. Test Plan Identifier

- **TP-ID:** VWO-LOGIN-TP-001
- **Application under test (AUT):** VWO (Visual Website Optimizer) Login Dashboard
- **Environment URL (user-supplied):** https://app.vwo.com/#/login
- **Version:** v1.0 (Draft)
- **Author:** Senior QA Automation Engineer (enterprise SaaS / A/B testing platforms)

## 2. References

| Ref | Document | Source |
|-----|----------|--------|
| R1 | `Product Requirements Document_ VWO Login Dashboard.pdf` (7 pages) | Approved requirement source |
| R2 | `Anti-Hallucination.rules.md` | Mandatory verification rules for this deliverable |
| R3 | User-supplied context: enterprise SaaS — A/B testing, multivariate testing, heatmaps, visitor recording, behavioral analytics; high reliability; strict security (GDPR, SOC2); low latency; cross-browser compatibility; seamless authentication | User input |
| R4 | Repository skill templates: `test-plan-generator`, `test-case-writer`, `api-test-designer` (SKILL.md + references) | Repo standard |
| R5 | `VWO_Login_Dashboard_Test_Cases.md` (fact table F1–F48, prior deliverable) | Prior QA deliverable |

## 3. Introduction / Scope

### 3.1 Scope & Objectives

- **In scope:** Verification of the VWO login dashboard per the PRD — authentication
  (email/password), session management, input validation, password management, UX,
  accessibility, branding, security, performance, integrations, and user journeys
  **as specified in the PRD**.
- **Out of scope (per Anti-Hallucination Rules — no PRD/API/user-input coverage):**
  - Post-login product internals: A/B testing, multivariate testing, heatmaps, visitor
    recording, behavioral analytics — named by the user as product context (R3) but
    **not specified** in the PRD. Only the successful transition to the main dashboard
    (PRD F34/F42) is verified.
  - User management, project workspace, and tracking-dashboard internals — user-named focus
    areas (R3) with **zero specification in the PRD**. Blocked: "Insufficient information
    to determine."
  - API contract behavior — **no API documentation exists**. A contract-defined design
    matrix is provided in the companion deliverable; every expectation remains
    `contract-defined` (TBD) until an OpenAPI/API doc is supplied.
- **Objective:** Provide an unambiguous, executable test approach that proves the login
  dashboard meets PRD requirements, is safe to release, and produces evidence traceable to
  each requirement.

### 3.2 Quality objectives (PRD KPIs, R1)

| Objective | Target (PRD) | Traceability |
|-----------|--------------|--------------|
| Login success rate | ≥ 95% | F47 |
| Login page load time | ≤ 2 seconds on standard connections | F31 |
| Uptime / availability | 99.9% | F32 |
| User satisfaction | ≥ 90% | PRD Success Metrics |
| Security incidents | Zero successful brute-force / unauthorized access | PRD Security Metrics |
| Compliance adherence | 100% audit compliance | PRD Security Metrics |
| Support volume | Reduced login-related tickets by 20% (monitoring) | PRD Business Metrics |

## 4. Test Items (documented by PRD)

| # | Test item | PRD fact |
|---|-----------|----------|
| 1 | Email address input field | F1 |
| 2 | Password input field | F1 |
| 3 | "Remember Me" checkbox (persistent login sessions) | F2 |
| 4 | Registration / free-trial signup link | F3 |
| 5 | Product announcement banner with Light / Dark Mode options | F4 |
| 6 | Forgot-password flow with email-based reset | F13, F14 |
| 7 | Session management (configurable timeout) | F6 |
| 8 | Validation engine (blur-based, email format) | F9, F10 |
| 9 | Password strength indicator | F11 |
| 10 | Error messaging | F12, F44 |
| 11 | Theme support — Light / Dark | F24 |
| 12 | Authentication security layers (HTTPS, encryption, hashing, tokens, rate limiting) | F25–F28, F30 |

## 5. Software Risk Issues — Features to Be Tested

Prioritized per the repository template (P0 = release-blocking, P1 = important,
P2 = optional/conditional/deferred).

| ID | Priority | Type | Test scenario | Maps to (requirement) |
|----|----------|------|---------------|----------------------|
| TS-01 | P0 | Positive | Valid email + password → successful login → dashboard transition | F5, F34, F42, F46 |
| TS-02 | P0 | Negative | Invalid credentials → clear, actionable error message | F12, F44 |
| TS-03 | P0 | Boundary/Negative | Empty email / empty password / malformed email on blur | F9, F10 |
| TS-04 | P0 | Functional | Remember Me → persistent session for returning user | F2, F41 |
| TS-05 | P0 | Functional | Forgot password → email-based reset with secure token | F13, F14 |
| TS-06 | P0 | Security | HTTPS / SSL-TLS enforcement on all login traffic | F28 |
| TS-07 | P0 | Security | Rate limiting / brute-force throttling | F30 |
| TS-08 | P0 | Security | Session timeout enforcement (configurable) | F6 |
| TS-09 | P0 | Performance | Login page loads within 2 s on standard connection | F31 |
| TS-10 | P1 | Functional | Password strength indicator & complexity enforcement | F11, F15 |
| TS-11 | P1 | UX | Auto-focus on first input field | F17 |
| TS-12 | P1 | UX | Clickable form labels | F18 |
| TS-13 | P1 | UX | Loading state during authentication | F19 |
| TS-14 | P1 | Functional | Registration link → free-trial signup path | F3, F39 |
| TS-15 | P1 | Functional | Announcement banner + Light/Dark mode persistence | F4, F24 |
| TS-16 | P1 | UX | Responsive, touch-friendly mobile layout | F16 |
| TS-17 | P1 | Accessibility | WCAG 2.1 AA: ARIA, keyboard navigation, high contrast | F20, F21, F22, F48 |
| TS-18 | P1 | Security | Session token secure generation/management | F27 |
| TS-19 | P1 | Security | Encrypted password storage (hashing) — backend evidence | F26 |
| TS-20 | P1 | Security | End-to-end encryption of auth data | F25 |
| TS-21 | P1 | Compliance | GDPR adherence for user data handling | F29 (PRD), R3 |
| TS-22 | P1 | Compliance | SOC2 compliance evidence | R3 only — user input, not in PRD |
| TS-23 | P1 | Journey | Returning user: quick access + recent activity context | F41, F43 |
| TS-24 | P1 | Analytics | Login success/failure tracking; success rate ≥ 95% | F35, F47 |
| TS-25 | P1 | Reliability | Support/recovery paths after failure; success confirmation | F36, F44, F45, F46 |
| TS-26 | P1 | Performance | Thousands of concurrent login attempts | F33 |
| TS-27 | P1 | Reliability | 99.9% uptime / high availability | F32 |
| TS-28 | P2 | Conditional | Optional 2FA flow (if enabled) | F7 |
| TS-29 | P2 | Conditional | Enterprise SSO (SAML/OAuth) for organizational accounts (if enabled) | F8, F37 |
| TS-30 | P2 | Conditional | Social login via Google/Microsoft identity providers (if enabled) | F38 |
| TS-31 | P2 | Journey | Guided onboarding post-registration | F40 |
| TS-32 | P2 | Cross-browser | Browser matrix verification | G06 — matrix not defined |
| TS-33 | P2 | API | API contract & auth tests | G08 — no API docs |
| TS-34 | P2 | Boundary | Session/boundary edge cases (idle, duplicate login) | F6 + G03 |

## 6. Features Not to Be Tested (with reason)

| Item | Reason (Anti-Hallucination traceability) |
|------|------------------------------------------|
| A/B testing, multivariate testing, heatmaps, visitor recording, behavioral analytics | Product context named by user (R3) but no functional spec exists; PRD covers only the login dashboard |
| User management, project workspace, tracking dashboard internals | Named in user focus (R3); **insufficient information to determine** requirements |
| Biometric authentication, adaptive authentication, PWA | PRD lists as *future enhancements* — not in current scope |
| API contract behavior | No API documentation provided → cannot define requests, status codes, or error shapes |
| Exact error-message text, password-rule specifics, session-timeout values, browser matrix, "standard connection" definition | Not defined in PRD → each is a **Gap** (G01–G07), never an assumption |

## 7. Approach / Test Strategy

### 7.1 Test levels

| Level | Coverage | Method |
|-------|----------|--------|
| Functional / UI | Login, validation, Remember Me, forgot password, registration link, banner/theme, loading state, dashboard transition | Manual + automated (Playwright/Selenium per repo tooling); tabular executable cases (companion deliverable) |
| Non-functional | Performance (2 s load, concurrency), Security (HTTPS, rate limiting, tokens, hashing), Reliability (uptime, recovery), Accessibility (WCAG 2.1 AA) | Load tool (e.g., k6/JMeter), security tooling (proxy/devtools, pen-test checklist), accessibility audit (axe/PA11Y), uptime-monitoring data |
| API | Contract, auth, schema, negative, boundary | **Deferred** until OpenAPI/API documentation supplied (G08) |
| Cross-browser | Matrix execution | **Deferred** until matrix defined by product/engineering (G06) |
| Boundary/Edge | Empty, malformed, max-length, session boundaries | Manual + data-driven cases |

### 7.2 Technique

- **Gherkin/BDD** for requirement-traceable behavior (login, validation, Remember Me,
  forgot password, session timeout) — samples in the companion deliverable.
- **Tabular executable cases:** unique ID, preconditions, test-data reference, numbered
  steps with per-step expected results, traceability, priority.
- **Evidence-first:** every case records an observable result; no expected value is
  asserted that the PRD does not support — undefined expectations are marked
  *"expected result to confirm"* and tracked in the Gaps table.
- **Data hygiene:** no real user credentials; synthetic accounts via test-data owner;
  secrets injected at runtime, never committed (repo skill guardrail).

## 8. Item Pass / Fail Criteria

**Pass:**
- 100% of P0 functional cases pass with evidence.
- All P0/P1 security cases pass; zero critical/high defects open.
- Load time ≤ 2 s (TS-09) on the defined "standard connection" (definition required — G07).
- No open P0/P1 defects at exit; remaining P2 defects have documented workaround + owner.

**Fail (release-blocking):**
- Any P0 case fails with no workaround.
- Security/compliance case failure (HTTPS, rate limiting, GDPR/SOC2 evidence) — cannot be waived.

## 9. Suspension / Resumption Criteria

**Suspend testing when:**
- Environment unavailable or AUT returns systemic errors (smoke-threshold percentage TBD by QA lead).
- A security/compliance blocker is found (e.g., plaintext credential transport) — escalate immediately.
- Test data or environment access is missing (blocked preconditions).

**Resume when:**
- Environment stable and P0 functional smoke passes.
- Blocker resolved, retested, evidence attached.

## 10. Test Deliverables

| # | Deliverable | Owner |
|---|-------------|-------|
| 1 | This Test Plan (IEEE 829) — DRAFT until approval | QA Lead |
| 2 | Test Case Suite (tabular + Gherkin) — companion deliverable | QA Engineer |
| 3 | Requirement traceability matrix (case ↔ PRD fact) | QA Engineer |
| 4 | Test execution report with evidence (screenshots/logs/API traces) | QA Engineer |
| 5 | Defect log (tool of record, e.g., JIRA) | QA Engineer |
| 6 | Performance test report (load time, concurrency) | Performance QA |
| 7 | Security/compliance evidence pack (pen-test, GDPR/SOC2 audit evidence) | Security team |
| 8 | Test closure report + release decision record | QA Lead |

## 11. Testing Tasks

1. Approve this test plan (Human Review Gate, Section 18).
2. Resolve Gaps G01–G10 (Section 2 of the companion deliverable) with product/engineering.
3. Acquire test data (synthetic accounts) and environment access.
4. Execute: P0 functional smoke → P0 security/performance → P1 → P2 conditional.
5. Log defects, attach evidence, retest fixes.
6. Publish traceability matrix and closure report.

## 12. Environmental Needs

| Need | Detail | Status |
|------|--------|--------|
| AUT environment | https://app.vwo.com/#/login (user-supplied) | Provided |
| Browsers | Per browser matrix — **G06: not defined in PRD** | TBD |
| Test accounts | Synthetic email/password accounts incl. 2FA, SSO, enterprise org accounts (if enabled) | TBD (data owner) |
| Tooling | Test framework (Playwright/Selenium), load tool (k6/JMeter), accessibility audit (axe/PA11Y), proxy for HTTPS inspection | Recommended per repo tooling; QA-lead approval |
| API docs | OpenAPI/contract for API-level tests | **Missing (G08)** |
| Monitoring | Uptime/analytics access for KPI checks (99.9% uptime, 95% success rate) | TBD |

## 13. Responsibilities

| Role | Responsibility |
|------|----------------|
| QA Lead | Plan sign-off, prioritization, exit-criteria enforcement |
| QA Engineer | Case execution, defect logging, evidence capture |
| Performance QA | Load/concurrency/latency tests |
| Security team | Pen-test, compliance evidence (GDPR/SOC2) |
| Engineering | Fixes, environment stability, API-contract provision |
| Product | Gap resolution (G01–G10), expected-value confirmation |
| Data owner | Test-data provisioning |

## 14. Staffing & Training Needs

- QA engineer(s) proficient in Playwright/Selenium, load tooling, and security-testing fundamentals.
- Access to the repository QA skill suite (`stlc/`, `playwright/`, `selenium/`, `api_testing/`) for consistent technique.

## 15. Schedule (relative phases — no calendar dates)

| Phase | Milestone |
|-------|-----------|
| Phase 0 | Plan approval + Gap resolution (G01–G10) |
| Phase 1 | Test data & environment readiness |
| Phase 2 | P0 execution (functional + security + performance) |
| Phase 3 | P1 execution (UX, a11y, compliance, reliability) |
| Phase 4 | P2 conditional + cross-browser/API (once inputs provided) |
| Phase 5 | Closure: traceability matrix, exit review, release decision |

## 16. Risks & Contingencies

| Risk | Impact | Contingency |
|------|--------|-------------|
| Gaps G01–G10 unresolved (API docs, browser matrix, expected values) | P2 scope blocked; P0 expectations may be unverifiable | Escalate; mark cases "blocked" with evidence; never invent values |
| Real-app dependency (no test environment) | Tests run against production | Restrict to read-only, non-destructive actions; require signed-off test accounts |
| Credential leakage in evidence | Security breach | Redact credentials/tokens in all evidence; secrets via env vars only |
| "Standard connection" undefined (G07) | Load-time verdict ambiguous | Measure and report connection profile; require definition |
| Scope creep into post-login product features | Time/complexity | Out of scope per Section 6; only dashboard transition verified |

## 17. Approvals

| Name | Role | Signature / Date |
|------|------|------------------|
| _(QA Owner)_ | QA Lead | ▢ |
| _(Engineering)_ | Engineering Lead | ▢ |
| _(Product)_ | Product Owner | ▢ |

---

## 18. HUMAN REVIEW GATE (mandatory — per repository skill)

- **I assumed:** tooling choices (Playwright/Selenium, k6/JMeter, axe/PA11Y) are
  recommendations aligned with the repo suite — they are not PRD requirements. SOC2
  appears as a user-input context item only (R3), **not** in the PRD.
- **I could not confirm (Gaps):**
  - **G01** Exact error-message text (F12/F44) — not in PRD
  - **G02** Password complexity rules (F15) — not in PRD
  - **G03** Session-timeout duration values (F6) — not in PRD
  - **G04** 2FA methods/delivery (F7) — not in PRD
  - **G05** SSO providers/config (F8, F37) — not in PRD
  - **G06** Browser/OS/devices matrix — not in PRD
  - **G07** Definition of "standard connections" (F31) — not in PRD
  - **G08** API endpoints/schemas — no API documentation
  - **G09** Test-data values / selectors / credentials — not provided
  - **G10** Post-login product features, user management, workspace, tracking dashboard —
    **insufficient information to determine**
- **Open questions blocking sign-off:** G01–G10 above; also — are 2FA/SSO/social login
  enabled in the environment under test?
- ▶ **Approve, or edit, before I finalize the test cases / automation.**
