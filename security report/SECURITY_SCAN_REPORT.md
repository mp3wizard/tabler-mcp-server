# Automated Security Scan Report

**Target:** `/Users/mp3wizard/Public/ICONs MCP/tabler-mcp-server`  
**Scanned at:** 2026-07-20 18:21 ICT  
**Git HEAD:** `f1d02fb`  
**Standard:** OWASP APTS-aligned (Scope Enforcement · Auditability · Manipulation Resistance · Reporting)

## Scope Record

```text
Scan target: /Users/mp3wizard/Public/ICONs MCP/tabler-mcp-server
Git HEAD:    f1d02fb
Include:     all supported source, dependency, MCP-manifest, and Git-history files
Exclude:     .git internals; credential file contents were not read
```

## Remediated Findings

| Severity | Area | Remediation |
|---|---|---|
| High | Local file read | Reject icon names containing path delimiters/traversal before resolving the bundled SVG path. |
| High | Generated SVG/JSX | Validate `color` before substituting it into SVG or generated React/Vue/Svelte markup, preventing attribute injection. |
| Medium | Docs network access | Require HTTPS and disable redirects after the hostname allowlist check, preventing redirect-based SSRF. |

## Coverage Disclosure (APTS § Reporting)

| Tool | Status | Version | Result / coverage |
|---|---|---|---|
| Gitleaks | OK | 8.30.1 | 8 Git commits and working tree; no leaks found. |
| TruffleHog | OK | 3.95.9 | 0 verified and 0 unverified secrets. Its temporary-artifact cleanup was blocked by the sandbox. |
| Trivy | OK with limitation | 0.72.0 | `package-lock.json` scanned; 0 vulnerabilities. Embedded policy fallback used after an OCI credential lookup failure. |
| OSV-Scanner | OK | 2.4.0 | 195 packages from `package-lock.json`; no issues found after permitted-network retry. |
| npm audit | OK | npm | Production dependencies: 0 vulnerabilities. |
| Semgrep | SKIPPED | — | Not installed in the current environment. |
| Bandit | N/A | — | No Python source files. |
| CodeQL | N/A | — | No CodeQL workflow found. |
| mcps-audit | PARTIAL | npx | Invoked for the MCP manifest; no streamed finding output was returned. |
| mcp-exfil-scan | PARTIAL | bundled | Streamed result: 0/100, clean. Its report-file write failed because plugin storage is read-only. |
| skill-security-auditor / SkillSpector | N/A | — | No `.skill`, `SKILL.md`, or `AGENTS.md` artifacts in scope. |
| mcp-scan | OPT-IN / not run | — | Not run because it can send data to invariantlabs.ai. |

## Cross-Tool Observations

- Gitleaks and TruffleHog both found no credentials.
- Trivy, OSV-Scanner, and npm audit all reported no known dependency vulnerabilities.
- The source-level fixes protect the only discovered local filesystem, generated-markup, and network allowlist boundaries.

## Verification

```text
npm test       PASS (24 tests, 0 failures)
npm run lint   PASS
npm run build  PASS
npm audit --omit=dev  PASS (0 vulnerabilities)
git diff --check  PASS
```

## Coverage Gaps

- Semgrep was unavailable.
- No dynamic MCP-client integration test or runtime authorization test was performed.
- `mcp-scan` was intentionally not run because it is external-data opt-in.

### APTS Audit Log

Audit events are recorded at `/tmp/css-scan-20260720T111817Z.jsonl` for this session. The log contains measured exit codes and durations for wrapped scanner invocations.
