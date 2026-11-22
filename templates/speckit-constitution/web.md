
<!-- templates v1.0.0: speckit-constitution/web.md (draft: not yet used in a project) -->
## Web Principles

### Web Security (OWASP)

Every feature MUST meet [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/) 5.0 at Level 1 and be checked against the latest [OWASP Top 10](https://owasp.org/www-project-top-ten/). These rules are the ones that most often apply in the browser:

- Output is encoded by the framework. Raw HTML insertion (`innerHTML`, `dangerouslySetInnerHTML`, `v-html`) is not allowed without a sanitizer.
- A Content Security Policy is set and blocks inline scripts.
- Session cookies are `Secure`, `HttpOnly`, and `SameSite`. State-changing requests are protected from CSRF.
- The client bundle is public. It holds no secrets or private API keys.

Rationale: a web page runs untrusted content next to the user's session.

### Accessibility and Layout

- Meet [WCAG 2.2](https://www.w3.org/TR/WCAG22/) level AA: semantic HTML first, ARIA only when no HTML element fits, every function usable by keyboard, visible focus.
- Layout is responsive, mobile first, and works from a 320 px wide window up.
- Every page spec names its loading, error, and empty states.

Rationale: the web is used on every kind of device and by people with every kind of ability.

### Performance

- Pages meet the "good" thresholds of [Core Web Vitals](https://web.dev/articles/vitals) for the 75th percentile of users: LCP at most 2.5 s, INP at most 200 ms, CLS at most 0.1.
- Core content works before JavaScript enhancements load where the framework allows it.

Rationale: slow pages lose users, and search engines rank by these metrics.
