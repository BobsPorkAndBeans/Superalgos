# Dependency Audit Report for Superalgos

**Generated:** 2026-01-20
**Project Version:** 1.6.1
**Total Dependencies:** 2,106 (1,418 prod, 372 dev, 312 optional)

---

## Executive Summary

This audit reveals **critical security concerns** requiring immediate action:

- **137 Security Vulnerabilities** (15 Critical, 77 High, 22 Moderate, 23 Low)
- **60+ Outdated Packages** requiring major version updates
- **Unnecessary bloat** from Node.js built-in packages listed as dependencies
- **Breaking changes** expected in major dependency updates

**Recommended Priority:** CRITICAL - Address security vulnerabilities immediately

---

## 1. CRITICAL Security Vulnerabilities

### 1.1 Highest Priority (Critical Severity)

| Package | Issue | CVSS Score | Fix Available |
|---------|-------|------------|---------------|
| **@babel/traverse** | Arbitrary code execution when compiling malicious code | 9.4 | ✅ Yes |
| **elliptic** | Multiple critical vulnerabilities in cryptographic operations | 9.8 | ✅ Yes |
| **@ethersproject/signing-key** | Critical vulnerability via elliptic dependency | Critical | ✅ Yes |

**Immediate Action Required:**
```bash
npm audit fix --force
```

### 1.2 High Severity (77 vulnerabilities)

Key packages requiring immediate updates:

| Package | Issue | Current | Fix |
|---------|-------|---------|-----|
| **ws** | DoS vulnerability (multiple CVEs) | 8.4.0 | 8.19.0+ |
| **xlsx** | Prototype pollution + ReDoS | 0.18.5 | 0.20.2+ |
| **web3** | Multiple transitive vulnerabilities | 1.6.1 | 6.16.0 |
| **webpack-dev-server** | Exposure of sensitive information | 4.11.1 | 5.2.3+ |
| **semver** | Regular Expression DoS | Various | Latest |
| **got** | SSRF vulnerability | Via deps | Update parents |
| **tar** | Arbitrary file overwrite | Via deps | Update parents |

**xlsx Note:** Version 0.18.5 has NO fix available in the 0.18.x line. Must upgrade to 0.20.2+

---

## 2. Outdated Dependencies Analysis

### 2.1 Major Version Updates Required

These packages are **significantly outdated** and require major version upgrades:

| Package | Current | Latest | Versions Behind |
|---------|---------|--------|-----------------|
| **electron** | 21.3.1 | 40.0.0 | 19 major |
| **@tensorflow/tfjs-node** | 2.8.6 | 4.22.0 | 2 major |
| **dotenv** | 8.2.0 | 17.2.3 | 9 major |
| **dotenv-expand** | 5.1.0 | 12.0.3 | 7 major |
| **web3** | 1.6.1 | 6.16.0 | 5 major |
| **ethers** | 5.5.3 | 6.16.0 | 1 major |
| **jsdom** | 20.0.1 | 27.4.0 | 7 major |
| **vite** | 3.2.4 | 7.3.1 | 4 major |
| **open** | 8.4.0 | 11.0.0 | 3 major |
| **electron-builder** | 23.6.0 | 26.4.0 | 3 major |
| **electron-updater** | 4.6.1 | 6.7.3 | 2 major |
| **@reduxjs/toolkit** | 1.6.2 | 2.11.2 | 1 major |
| **ccxt** | 2.7.62 | 4.5.34 | 2 major |
| **knex** | 2.4.2 | 3.1.0 | 1 major |
| **pm2** | 5.2.2 | 6.0.14 | 1 major |
| **boxen** | 5.1.2 | 8.0.1 | 3 major |
| **chalk** | 4.1.2 | 5.6.2 | 1 major (ESM only) |
| **date-fns** | 2.29.3 | 4.1.0 | 2 major |
| **yargs** | 17.6.2 | 18.0.0 | 1 major |
| **node-fetch** | 2.6.6 | 3.3.2 | 1 major (ESM only) |
| **winston-daily-rotate-file** | 4.7.1 | 5.0.0 | 1 major |
| **workbox-webpack-plugin** | 6.5.4 | 7.4.0 | 1 major |
| **ip** | 1.1.5 | 2.0.1 | 1 major |

**Breaking Changes Warning:** Many of these (chalk, boxen, node-fetch) have moved to ESM-only, requiring code changes.

### 2.2 Minor/Patch Updates Available

Safer updates with backwards compatibility:

| Package | Current | Latest |
|---------|---------|--------|
| discord.js | 14.6.0 | 14.25.1 |
| telegraf | 4.11.2 | 4.16.3 |
| simple-git | 3.13.0 | 3.30.0 |
| pg | 8.11.0 | 8.17.2 |
| winston | 3.8.2 | 3.19.0 |
| chart.js | 4.4.4 | 4.5.1 |

---

## 3. Unnecessary Bloat & Anti-Patterns

### 3.1 Node.js Built-in Packages (REMOVE THESE)

These packages are **Node.js built-ins** and should NOT be in dependencies:

```json
// ❌ REMOVE - These are built into Node.js
"child_process": "^1.0.2",
"http": "0.0.1-security",
"https": "^1.0.0",
"path": "^0.12.7",
"util": "^0.12.4"
```

**Impact:**
- Adds ~500KB of unnecessary code
- Security risk (some are marked as security placeholders)
- Can cause module resolution conflicts

**Fix:** Remove from package.json and use Node.js built-ins directly:
```javascript
// Instead of: const path = require('path')  // from package
// Use: const path = require('path')  // Node.js built-in (no change in code, just remove from package.json)
```

### 3.2 Wildcard Dependencies (SECURITY RISK)

```json
"bootstrap": "*",        // ❌ Pins to ANY version
"bootstrap-vue": "*"     // ❌ Pins to ANY version
```

**Risk:** Auto-updates to ANY version, including breaking changes
**Fix:** Pin to specific versions:
```json
"bootstrap": "^5.3.8",
"bootstrap-vue": "^2.23.1"
```

### 3.3 Redundant Dependencies

**Issue:** Both axios packages:
- `@bundled-es-modules/axios: ^0.27.2` (production)
- `axios: ^0.18.0` (dev)

**Recommendation:** Consolidate to one version. The dev version is extremely old (2018) and has known vulnerabilities.

### 3.4 npm as Dependency

```json
"npm": ">=10.3.0"
```

**Issue:** npm should not be a package dependency. It's a package manager.
**Fix:** Remove this line. Use `.nvmrc` or `engines` field instead:
```json
"engines": {
  "node": ">=18.0.0",
  "npm": ">=10.3.0"
}
```

---

## 4. Large Dependencies Impact

### 4.1 Heaviest Packages (Estimated)

| Package | Size | Notes |
|---------|------|-------|
| **electron** | ~150MB | Optional - Good |
| **@tensorflow/tfjs-node** | ~50MB | Optional - Good |
| **web3** | ~15MB | Consider lighter alternatives |
| **ccxt** | ~10MB | Crypto exchange library |
| **webpack** | ~5MB | Dev dependency |
| **vite** | ~4MB | Build tool |

**Recommendation:** The large packages are appropriately marked as optional (electron, tensorflow). Consider if web3 and ccxt are both necessary.

---

## 5. Detailed Recommendations

### 5.1 IMMEDIATE (Within 24-48 hours)

**Priority 1: Critical Security Fixes**

```bash
# 1. Update critical vulnerabilities
npm audit fix --force

# 2. Manual fixes for packages without automatic fixes
npm install ws@latest
npm install xlsx@latest  # Requires code review for breaking changes
```

**Priority 2: Remove Unnecessary Dependencies**

Edit `package.json` and remove:
```diff
- "child_process": "^1.0.2",
- "http": "0.0.1-security",
- "https": "^1.0.0",
- "path": "^0.12.7",
- "util": "^0.12.4",
- "npm": ">=10.3.0"
```

**Priority 3: Fix Wildcard Dependencies**

```diff
- "bootstrap": "*",
- "bootstrap-vue": "*",
+ "bootstrap": "^5.3.8",
+ "bootstrap-vue": "^2.23.1"
```

### 5.2 SHORT-TERM (1-2 weeks)

**Update High-Impact Packages with Breaking Changes**

Test thoroughly in development environment first:

```bash
# Web3 ecosystem (Breaking changes expected)
npm install ethers@^6.16.0
npm install web3@^4.16.0  # Major breaking changes from v1 -> v4

# Build tools
npm install vite@^7.3.1
npm install webpack@^5.latest
npm install webpack-dev-server@^5.2.3

# Electron (if used)
npm install electron@^40.0.0 --save-optional
npm install electron-builder@^26.4.0 --save-optional
npm install electron-updater@^6.7.3 --save-optional
```

### 5.3 MEDIUM-TERM (1 month)

**Systematic Dependency Updates**

Create a migration plan for:

1. **ESM-only packages** (chalk, boxen, node-fetch)
   - Requires converting to ESM imports or staying on older versions
   - Consider: Does the project need to migrate to ESM?

2. **Discord.js** - Update to 14.25.1
   ```bash
   npm install discord.js@^14.25.1
   npm install discord-api-types@^0.38.37
   ```

3. **Database packages**
   ```bash
   npm install knex@^3.1.0
   npm install pg@^8.17.2
   ```

4. **Development tools**
   ```bash
   npm install eslint@^8.latest
   npm install jest@^29.latest
   npm install husky@^9.latest
   ```

### 5.4 LONG-TERM (Ongoing)

1. **Dependency Management Strategy**
   - Set up automated dependency updates (Dependabot, Renovate)
   - Monthly security audits
   - Quarterly major version reviews

2. **Consider Alternatives**
   - **web3** → ethers.js (already have both, pick one)
   - **winston** → pino (lighter, faster)
   - **axios** → native fetch (Node 18+)

3. **Code Quality**
   - Remove unused dependencies (run `depcheck`)
   - Add `overrides` for transitive vulnerabilities
   - Document why each major dependency exists

---

## 6. Migration Risks & Considerations

### 6.1 High-Risk Updates (Test Extensively)

| Package | Risk Level | Reason |
|---------|------------|--------|
| web3 | 🔴 HIGH | v1→v4 complete API rewrite |
| ethers | 🟡 MEDIUM | v5→v6 significant changes |
| electron | 🔴 HIGH | 19 major versions behind |
| vite | 🟡 MEDIUM | 4 major versions, config changes |
| chalk/boxen | 🟡 MEDIUM | ESM-only, requires code changes |
| @tensorflow/tfjs-node | 🟡 MEDIUM | ML model compatibility |
| ccxt | 🟡 MEDIUM | Exchange API changes |

### 6.2 Breaking Change Summary

**web3 v1 → v4:**
- Complete API redesign
- Different contract interaction methods
- New provider system
- Estimate: 40-80 hours for migration

**ethers v5 → v6:**
- Import path changes
- TypeScript improvements
- New BigNumber handling
- Estimate: 20-40 hours

**electron v21 → v40:**
- Multiple Node.js version jumps
- Deprecated API removals
- Security model changes
- Estimate: 60-100 hours

**Vite v3 → v7:**
- Plugin API changes
- Config structure updates
- Rollup version changes
- Estimate: 8-16 hours

---

## 7. Recommended Action Plan

### Week 1: Critical Security
- [ ] Run `npm audit fix`
- [ ] Manually update `ws` to 8.19.0+
- [ ] Update `xlsx` to 0.20.2+ (test spreadsheet functionality)
- [ ] Remove Node.js built-in dependencies
- [ ] Fix wildcard dependencies
- [ ] Run full test suite

### Week 2-3: High-Priority Updates
- [ ] Update discord.js ecosystem
- [ ] Update database packages (knex, pg)
- [ ] Update chart.js and related
- [ ] Update winston and logging
- [ ] Update CLI tools (yargs, chalk if staying on v4)

### Month 2: Major Version Updates
- [ ] Plan web3/ethers migration (choose one)
- [ ] Update vite to v7
- [ ] Update webpack-dev-server to v5
- [ ] Update build tooling

### Month 3+: Long-term Modernization
- [ ] Evaluate electron update feasibility
- [ ] Consider ESM migration
- [ ] Set up automated dependency management
- [ ] Review and remove unused dependencies

---

## 8. Quick Reference Commands

### Immediate Security Fix
```bash
npm audit fix --force
npm install ws@latest xlsx@latest
npm test  # Verify nothing broke
```

### Remove Bloat
Edit `package.json` manually, then:
```bash
npm install  # Reinstall with cleaned dependencies
```

### Update Specific Package
```bash
npm install <package>@latest
npm test  # Always test after updates
```

### Check for Unused Dependencies
```bash
npx depcheck
```

### Lock File Cleanup
```bash
rm -rf node_modules package-lock.json
npm install
```

---

## 9. Cost-Benefit Analysis

### Immediate Fixes (Week 1)
- **Time:** 4-8 hours
- **Benefit:** Eliminate critical security vulnerabilities, reduce attack surface
- **Risk:** Low (mostly security patches)

### Short-term Updates (Month 1)
- **Time:** 16-24 hours
- **Benefit:** Modern dependencies, better performance, more bug fixes
- **Risk:** Medium (some breaking changes)

### Long-term Migration (Months 2-4)
- **Time:** 100-200 hours
- **Benefit:** Future-proof codebase, latest features, active maintenance
- **Risk:** High (major breaking changes, extensive testing required)

---

## 10. Conclusion

The Superalgos project has **significant technical debt** in its dependency management:

✅ **Good Practices:**
- Optional dependencies properly marked (electron, tensorflow)
- Package-lock.json committed
- Using modern tools (Vite, Vue 3)

❌ **Issues:**
- Critical security vulnerabilities
- Very outdated dependencies
- Unnecessary bloat from Node.js built-ins
- Wildcard version specifications

**Recommended Approach:**
1. **Immediate:** Fix critical security issues (1-2 days)
2. **Short-term:** Update medium-risk packages (2-3 weeks)
3. **Long-term:** Plan major migrations with dedicated sprints (3-4 months)

**Estimated Total Effort:** 150-250 hours over 4 months for complete modernization.

---

*Generated by Claude Code Dependency Audit*
*For questions or issues, create a GitHub issue or consult the Superalgos team*
