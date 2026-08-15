# Test Cases: VWO Login Dashboard (app.vwo.com)

Generated under the **Anti-Hallucination Rules** — every assertion is traceable to the PRD
(`Product Requirements Document_ VWO Login Dashboard.pdf`). Nothing is invented; anything
not defined in the PRD is listed under *Missing / Unknown Information* or labeled
`Inference (low confidence)`.

---

## 1. Verified Facts (extracted from PRD)

| ID | Fact | PRD Source |
|----|------|------------|
| F1 | Login form contains email address and password input fields | Existing Features — Standard Authentication Fields |
| F2 | "Remember Me" checkbox option enables persistent login sessions | Existing Features — Remember Me Functionality |
| F3 | Account registration link provides direct path to free trial signup | Existing Features — Account Registration Link |
| F4 | Product announcement banner highlights new UI launch with Light and Dark Mode options | Existing Features — Product Announcements |
| F5 | Primary authentication is email + password with secure validation | Login Process |
| F6 | Session handling is secure with configurable timeout periods | Session Management |
| F7 | Optional 2FA support for enhanced security | Multi-Factor Authentication |
| F8 | Enterprise SSO integration for organizational accounts | Single Sign-On (SSO) |
| F9 | Field validation occurs on blur (real-time feedback) | Real-time Validation |
| F10 | Automatic email format validation | Email Format Verification |
| F11 | Visual password strength/requirements feedback | Password Strength Indicators |
| F12 | Clear, actionable error messages for failed authentication | Error Handling |
| F13 | Forgot password flow uses secure token generation | Forgot Password Flow |
| F14 | Multiple recovery options including email-based reset | Password Recovery |
| F15 | Enforced security standards for password complexity | Password Requirements |
| F16 | Mobile-optimized responsive interface with touch-friendly controls | Responsive Design |
| F17 | Auto-focus on the first input field | Auto-focus |
| F18 | Clickable form labels for accessibility | Clickable Labels |
| F19 | Loading state shown during authentication processing | Loading States |
| F20 | ARIA labels and screen-reader compatibility | Screen Reader Support |
| F21 | High contrast mode option for visually impaired users | High Contrast Mode |
| F22 | Full keyboard accessibility for all interactive elements | Keyboard Navigation |
| F23 | Visual design aligned with VWO design system and color scheme | Brand Consistency |
| F24 | Light and Dark Mode theme support | Theme Support |
| F25 | End-to-end encryption for authentication data transmission | Encryption |
| F26 | Encrypted password storage using industry-standard hashing | Secure Storage |
| F27 | Secure session token generation and management | Session Security |
| F28 | SSL/TLS (HTTPS) enforced for all login communications | HTTPS Enforcement |
| F29 | GDPR compliance for user data handling | Compliance Standards |
| F30 | Rate limiting / request throttling against brute force attacks | Rate Limiting |
| F31 | Login page loads within 2 seconds on standard connections | Page Load Speed |
| F32 | 99.9% uptime / high availability | High Availability |
| F33 | Supports thousands of simultaneous login attempts | Concurrent Users |
| F34 | Seamless transition to main dashboard after successful authentication | VWO Core Platform Integration |
| F35 | Login success/failure tracking for analytics | Analytics Integration |
| F36 | Integration with support systems for login assistance | Customer Support |
| F37 | Support for SAML, OAuth, and other enterprise authentication protocols | Third-Party Services |
| F38 | Optional social login with Google, Microsoft, and other identity providers | Third-Party Services |
| F39 | New-user registration path (free trial signup CTA) | Registration Path |
| F40 | Guided onboarding for new users post-registration | Onboarding |
| F41 | Returning-user quick access with remembered credentials | Quick Access |
| F42 | Immediate access to personalized dashboard after login | Dashboard Transition |
| F43 | Context preservation from previous sessions (recent activity) | Recent Activity |
| F44 | Clear messaging for authentication failures | Error Recovery — Error Identification |
| F45 | Multiple account recovery / support paths | Error Recovery — Recovery Options |
| F46 | Clear indication of successful login | Error Recovery — Success Confirmation |
| F47 | Login success rate target: 95%+ | Success Metrics |
| F48 | WCAG 2.1 AA accessibility compliance | Accessibility Standards |

---

## 2. Missing / Unknown Information

The following are **not defined in the PRD**. Test steps that depend on them must be
completed using approved test data / design specs before execution. They are **not assumed**:

- Valid / invalid email and password test data (specific values)
- Exact error message text for failed authentication
- Exact password complexity rules (minimum length, character classes)
- Session timeout duration values
- 2FA delivery methods (SMS, TOTP, authenticator app, etc.)
- SSO identity providers and configuration
- UI element selectors (IDs, names, data attributes)
- Definition of "standard connections" for the 2-second load test
- Supported browser/OS/devices matrix
- Localization / language requirements
- API endpoints and API documentation
- Cookie / token names and formats
- Exact behavior of Light vs Dark mode (e.g., which palette, toggle placement)

---

## 3. Generated Output — Test Cases

> Legend: **Optional** = capability marked optional in PRD. Tests for optional features
> apply only if the feature is enabled in the environment under test.

### 3.1 Login Form — UI Elements (Existing Features)

**TC-01: Email and password fields displayed**
- **Preconditions:** Navigate to app.vwo.com login page.
- **Steps:** Observe the login form.
- **Expected:** An email address input field and a password input field are displayed (F1).
- **Traceability:** F1

**TC-02: "Remember Me" checkbox displayed**
- **Preconditions:** Login page loaded.
- **Steps:** Observe the login form for a Remember Me option.
- **Expected:** A "Remember Me" checkbox option is present (F2).
- **Traceability:** F2

**TC-03: Registration link to free trial signup displayed**
- **Preconditions:** Login page loaded.
- **Steps:** Observe the page for a registration/signup link.
- **Expected:** A direct path (link) to free trial signup is present (F3).
- **Traceability:** F3

**TC-04: Product announcement banner with Light/Dark mode options displayed**
- **Preconditions:** Login page loaded.
- **Steps:** Observe the announcement banner.
- **Expected:** A banner highlighting the new UI launch, with Light and Dark Mode options, is displayed (F4).
- **Traceability:** F4

**TC-05: Brand consistency of login page**
- **Preconditions:** Login page loaded.
- **Steps:** Compare page colors, logo, and typography against VWO design system reference.
- **Expected:** Visual design aligns with VWO's design system and color scheme (F23).
- **Traceability:** F23

### 3.2 Authentication — Login Flow

**TC-06: Successful login with valid email and password**
- **Preconditions:** Valid credentials supplied by test data owner (see Missing/Unknown).
- **Steps:**
  1. Enter valid email.
  2. Enter valid password.
  3. Submit the form.
- **Expected:** Authentication succeeds; user is transitioned to the main VWO dashboard (F5, F34, F46).
- **Traceability:** F5, F34, F46

**TC-07: Failed login displays clear, actionable error message**
- **Preconditions:** Known-invalid credentials supplied.
- **Steps:**
  1. Enter invalid email or password.
  2. Submit the form.
- **Expected:** A clear, actionable error message for the failed authentication attempt is displayed (F12, F44). *(Exact message text is not defined in the PRD — confirm with design spec.)*
- **Traceability:** F12, F44

**TC-08: Loading state during authentication processing**
- **Preconditions:** Valid credentials ready; network latency normal.
- **Steps:** Submit the form; observe UI while request is in progress.
- **Expected:** Clear loading feedback is shown during authentication processing (F19).
- **Traceability:** F19

**TC-09: Success indicator after login**
- **Preconditions:** Valid credentials ready.
- **Steps:** Log in successfully.
- **Expected:** A clear indication of successful login is provided (F46).
- **Traceability:** F46

### 3.3 User Input Validation

**TC-10: Validation occurs on blur**
- **Preconditions:** Login page loaded.
- **Steps:**
  1. Type an invalid value in the email field.
  2. Tab out of the field (trigger blur).
- **Expected:** Field validation feedback appears immediately on blur (F9).
- **Traceability:** F9

**TC-11: Email format validation rejects malformed email**
- **Preconditions:** Login page loaded.
- **Steps:** Enter a malformed email (e.g., missing "@" — value supplied by test data owner) and trigger blur.
- **Expected:** Automatic email format validation flags the malformed email (F10).
- **Traceability:** F10

**TC-12: Password strength indicator provides visual feedback**
- **Preconditions:** Password field with strength meter present.
- **Steps:** Type a weak password, then a strong password; observe indicators.
- **Expected:** Visual feedback for password requirements/strength is provided (F11). *(Strength criteria not defined in PRD — confirm with design spec.)*
- **Traceability:** F11

**TC-13: Password complexity requirements enforced**
- **Preconditions:** Password rules defined in product/security spec (not in PRD).
- **Steps:** Attempt to submit with a password that violates the documented complexity rules.
- **Expected:** Submission is blocked/flagged per the enforced complexity standards (F15). *(Exact rules not defined in PRD — confirm before executing.)*
- **Traceability:** F15

### 3.4 Password Management

**TC-14: Forgot password flow initiates reset with secure token**
- **Preconditions:** User with a registered email; forgot-password link present.
- **Steps:**
  1. Click the Forgot Password link.
  2. Follow the flow to the email-based reset.
- **Expected:** Password reset process initiates with secure token generation (F13, F14).
- **Traceability:** F13, F14

**TC-15: Email-based password recovery option available**
- **Preconditions:** Forgot-password flow reachable.
- **Steps:** Review recovery options presented.
- **Expected:** At least an email-based reset option is available (F14).
- **Traceability:** F14

### 3.5 Session & Remember Me

**TC-16: Remember Me keeps session persistent**
- **Preconditions:** Valid credentials; Remember Me checkbox available.
- **Steps:**
  1. Check Remember Me.
  2. Log in successfully.
  3. Close the browser and reopen it.
- **Expected:** Session persists (persistent login session) per Remember Me selection (F2, F41). *(Persistence duration not defined in PRD — confirm.)*
- **Traceability:** F2, F41

**TC-17: Session respects configured timeout**
- **Preconditions:** Session timeout period configured by admin (value not in PRD).
- **Steps:** Log in; remain idle beyond the configured timeout.
- **Expected:** Session terminates per the configured timeout period (F6).
- **Traceability:** F6

**TC-18: Quick access for returning user with remembered credentials**
- **Preconditions:** Returning user with previously remembered credentials.
- **Steps:** Return to login page and use remembered credentials.
- **Expected:** Streamlined quick-access login is provided (F41).
- **Traceability:** F41

### 3.6 Optional Authentication Methods

**TC-19: Optional 2FA flow (if enabled)**
- **Preconditions:** 2FA enabled for the test account; 2FA method defined by product (not in PRD).
- **Steps:** Log in with credentials; complete the 2FA challenge.
- **Expected:** Optional 2FA is supported and required when enabled (F7).
- **Traceability:** F7

**TC-20: Enterprise SSO login (if enabled)**
- **Preconditions:** SSO configured for an organizational account (SAML/OAuth).
- **Steps:** Log in via the SSO path.
- **Expected:** SSO login succeeds for organizational accounts (F8, F37).
- **Traceability:** F8, F37

**TC-21: Social login with identity providers (if enabled)**
- **Preconditions:** Social login enabled; Google/Microsoft account available.
- **Steps:** Log in via a social identity provider option.
- **Expected:** Social login succeeds via the identity provider (F38).
- **Traceability:** F38

### 3.7 UX — Interface Design

**TC-22: Auto-focus on first input field**
- **Preconditions:** Login page loaded.
- **Steps:** Observe focus position on page load.
- **Expected:** Focus is automatically placed on the first input field (F17).
- **Traceability:** F17

**TC-23: Form labels are clickable**
- **Preconditions:** Login page loaded.
- **Steps:** Click the email label, then the password label.
- **Expected:** Clicking each label focuses/activates its associated field (F18).
- **Traceability:** F18

**TC-24: Responsive layout on mobile with touch-friendly controls**
- **Preconditions:** Mobile viewport / device available.
- **Steps:** Load login page on a mobile viewport; interact with all controls.
- **Expected:** Interface is mobile-optimized and controls are touch-friendly (F16).
- **Traceability:** F16

**TC-25: Light and Dark mode switching**
- **Preconditions:** Theme toggle available on the announcement banner.
- **Steps:** Switch between Light and Dark modes; reload page.
- **Expected:** Theme selection applies and persists across reloads (F4, F24).
- **Traceability:** F4, F24

### 3.8 Accessibility (WCAG 2.1 AA)

**TC-26: ARIA labels present for screen readers**
- **Preconditions:** Screen reader (e.g., NVDA, VoiceOver) available.
- **Steps:** Navigate the login form with the screen reader.
- **Expected:** ARIA labels are announced; form is screen-reader compatible (F20).
- **Traceability:** F20, F48

**TC-27: Full keyboard navigation**
- **Preconditions:** Login page loaded; no mouse.
- **Steps:** Use Tab/Enter/Escape to operate every interactive element.
- **Expected:** All interactive elements are reachable and operable via keyboard (F22).
- **Traceability:** F22, F48

**TC-28: High contrast mode (if available)**
- **Preconditions:** High contrast option present (accessibility options).
- **Steps:** Enable high contrast mode and review the page.
- **Expected:** High contrast option is available for visually impaired users (F21).
- **Traceability:** F21

**TC-29: WCAG 2.1 AA compliance audit**
- **Preconditions:** Accessibility audit tooling available.
- **Steps:** Run automated + manual WCAG 2.1 AA checks against the login page.
- **Expected:** Login page complies with WCAG 2.1 AA (F48).
- **Traceability:** F48

### 3.9 Security

**TC-30: HTTPS enforced on login**
- **Preconditions:** Network proxy/browser devtools available.
- **Steps:** Load login page; submit credentials; inspect transport.
- **Expected:** All login communications use SSL/TLS (HTTPS); no plaintext transmission (F28).
- **Traceability:** F28

**TC-31: Brute force protection via rate limiting**
- **Preconditions:** Throttling policy configured (threshold values not in PRD).
- **Steps:** Send repeated failed login attempts beyond the configured threshold.
- **Expected:** Requests are throttled/blocked to prevent brute force (F30).
- **Traceability:** F30

**TC-32: Session token security**
- **Preconditions:** Valid login; browser devtools/security tools.
- **Steps:** Log in; inspect the session token attributes (per security team's checklist).
- **Expected:** Session tokens are securely generated and managed (F27). *(Token format/attributes not in PRD — verify against security spec.)*
- **Traceability:** F27

**TC-33: Password storage encryption (back-end check)**
- **Preconditions:** DB access or audit evidence provided by engineering.
- **Steps:** Verify stored credentials.
- **Expected:** Passwords are stored encrypted with industry-standard hashing (F26).
- **Traceability:** F26

**TC-34: GDPR compliance check**
- **Preconditions:** Privacy/compliance checklist provided.
- **Steps:** Review login data handling against GDPR requirements.
- **Expected:** User data handling adheres to GDPR (F29).
- **Traceability:** F29

### 3.10 Performance & Scale

**TC-35: Login page loads within 2 seconds**
- **Preconditions:** Standard connection as defined by performance spec (definition not in PRD).
- **Steps:** Load login page; measure load time.
- **Expected:** Page loads within 2 seconds (F31, page load KPI).
- **Traceability:** F31

**TC-36: Concurrent login attempts — thousands of users**
- **Preconditions:** Load testing tool; load model from engineering.
- **Steps:** Execute load test with thousands of simultaneous login attempts.
- **Expected:** System handles thousands of simultaneous login attempts without failure (F33).
- **Traceability:** F33

**TC-37: High availability (99.9% uptime)**
- **Preconditions:** Monitoring/uptime data available.
- **Steps:** Review uptime metrics over the reporting period.
- **Expected:** 99.9% uptime maintained (F32).
- **Traceability:** F32

### 3.11 User Journeys

**TC-38: New user registration path**
- **Preconditions:** Login page loaded; no account.
- **Steps:** Click the free trial signup link from the login page.
- **Expected:** User is directed to free trial signup with minimal friction (F3, F39).
- **Traceability:** F3, F39

**TC-39: Onboarding after registration**
- **Preconditions:** Newly registered account.
- **Steps:** Complete registration and proceed.
- **Expected:** Guided introduction to VWO capabilities is provided post-registration (F40).
- **Traceability:** F40

**TC-40: Dashboard transition after login**
- **Preconditions:** Valid credentials.
- **Steps:** Log in successfully.
- **Expected:** User reaches the personalized VWO dashboard immediately after successful authentication (F34, F42).
- **Traceability:** F34, F42

**TC-41: Recent activity context preserved**
- **Preconditions:** Returning user with prior session activity.
- **Steps:** Log in and check the dashboard context.
- **Expected:** Context from previous sessions is preserved (F43). *(Nature of preserved context not defined in PRD — confirm.)*
- **Traceability:** F43

**TC-42: Error recovery — multiple recovery paths**
- **Preconditions:** Failed authentication experienced by user.
- **Steps:** Review available recovery options after failure.
- **Expected:** Multiple account recovery and support paths are offered (F45, F36).
- **Traceability:** F45, F36

### 3.12 Analytics & KPI Monitoring (non-functional observations)

**TC-43: Login success/failure analytics tracking**
- **Preconditions:** Analytics backend access (per engineering).
- **Steps:** Perform successful and failed logins; check analytics events.
- **Expected:** Login success and failure events are tracked (F35).
- **Traceability:** F35

**TC-44: Login success rate ≥ 95% (KPI monitoring)**
- **Preconditions:** Analytics/telemetry data over a defined period.
- **Steps:** Review login success rate metric.
- **Expected:** Login success rate meets the 95%+ target (F47).
- **Traceability:** F47

---

## 4. Self-Validation Check

- [x] **No invented features:** every test case maps to a PRD fact (F1–F48). No APIs, error codes, UI elements, or behaviors were added beyond the PRD.
- [x] **No assumed defaults:** all unspecified values (credentials, message text, password rules, timeouts, throttle thresholds, "standard connection") are listed in Section 2 and flagged inline in the affected test cases.
- [x] **Inferences labeled:** tests whose expected outcome depends on an undefined value carry an inline `*(confirm with ...)*` note — none are asserted as fact.
- [x] **Deterministic and repeatable:** each test case has explicit preconditions, steps, expected result, and traceability.
- [x] **Optional features handled:** 2FA, SSO, and social login tests are explicitly conditional ("if enabled").
- [x] **Contradiction check:** no test contradicts another; KPI targets (F47, F31) are written as monitoring checks, not functional assertions.
