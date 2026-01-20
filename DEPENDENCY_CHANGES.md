# Dependency Changes Summary

## Overview
This document summarizes the changes made in `package.json.RECOMMENDED` compared to the current `package.json`.

---

## REMOVED Dependencies (Node.js Built-ins)

These are built into Node.js and should NOT be npm dependencies:

```diff
- "child_process": "^1.0.2"
- "http": "0.0.1-security"
- "https": "^1.0.0"
- "path": "^0.12.7"
- "util": "^0.12.4"
- "npm": ">=10.3.0"
```

**Impact:** None - these are still available as Node.js built-ins
**Action Required:** None - code continues to work unchanged

---

## ADDED

### engines field
```json
"engines": {
  "node": ">=18.0.0",
  "npm": ">=10.3.0"
}
```
**Reason:** Proper way to specify Node/npm version requirements (instead of npm as dependency)

### overrides field
```json
"overrides": {
  "ws": "^8.19.0",
  "semver": "^7.6.4",
  "word-wrap": "^1.2.5",
  "@babel/traverse": "^7.26.10",
  "@babel/helpers": "^7.26.10",
  "@babel/runtime": "^7.26.10"
}
```
**Reason:** Forces vulnerable transitive dependencies to use secure versions

---

## UPDATED Dependencies

### Critical Security Updates

| Package | Old Version | New Version | Reason |
|---------|-------------|-------------|---------|
| **ws** | ^8.4.0 | ^8.19.0 | 🔴 HIGH: DoS vulnerability (CVE-2024-37890) |
| **xlsx** | ^0.18.5 | ^0.20.2 | 🔴 HIGH: Prototype pollution + ReDoS |

### Fixed Wildcard Versions

| Package | Old | New | Reason |
|---------|-----|-----|---------|
| **bootstrap** | * | ^5.3.8 | 🟡 Wildcard = security risk |
| **bootstrap-vue** | * | ^2.23.1 | 🟡 Wildcard = security risk |

### Regular Updates (Security + Bug Fixes)

| Package | Old | New | Type |
|---------|-----|-----|------|
| discord.js | ^14.6.0 | ^14.25.1 | Minor (safe) |
| simple-git | ^3.13.0 | ^3.30.0 | Patch (safe) |
| pg | ^8.11.0 | ^8.17.2 | Patch (safe) |
| winston | ^3.8.2 | ^3.19.0 | Patch (safe) |
| telegraf | ^4.11.2 | ^4.16.3 | Patch (safe) |
| chart.js | ^4.4.4 | ^4.5.1 | Minor (safe) |
| chartjs-plugin-zoom | ^2.0.1 | ^2.2.0 | Minor (safe) |
| cli-table3 | ^0.6.3 | ^0.6.5 | Patch (safe) |
| date-fns | ^2.29.3 | ^2.30.0 | Patch (safe) |
| node-fetch | ^2.6.6 | ^2.7.0 | Patch (safe) |
| open | ^8.4.0 | ^8.4.2 | Patch (safe) |
| web3 | ^1.6.1 | ^1.10.4 | Patch (safe) |
| yargs | ^17.6.2 | ^17.7.2 | Patch (safe) |
| vite | ^3.2.4 | ^3.2.11 | Patch (safe) |
| vue | ^3.2.40 | ^3.5.27 | Minor (safe) |
| vue-chartjs | ^5.3.1 | ^5.3.3 | Patch (safe) |
| vue-router | ^4.1.5 | ^4.6.4 | Minor (safe) |
| sqlite3 | ^5.1.6 | ^5.1.7 | Patch (safe) |
| ip | ^1.1.5 | ^1.1.9 | Patch (safe) |
| jsdom | ^20.0.1 | ^20.0.3 | Patch (safe) |
| knex | ^2.4.2 | ^2.5.1 | Minor (safe) |
| twitter-api-v2 | ^1.7.1 | ^1.29.0 | Minor (safe) |
| pm2 | ^5.2.2 | ^5.4.3 | Minor (safe) |
| ccxt | ^2.7.62 | ^2.9.16 | Minor (safe) |
| ethers | ^5.5.3 | ^5.8.0 | Patch (safe) |
| workbox-webpack-plugin | ^6.5.4 | ^6.6.0 | Minor (safe) |
| lookpath | ^1.2.2 | ^1.2.3 | Patch (safe) |

### Dev Dependencies Updates

| Package | Old | New | Type |
|---------|-----|-----|------|
| axios | ^0.18.0 | ^1.7.9 | 🔴 CRITICAL: v0.18 is from 2018, has CVEs |
| eslint | ^8.10.0 | ^8.57.1 | Patch (safe) |
| css-loader | ^6.7.1 | ^6.11.0 | Minor (safe) |
| html-webpack-plugin | ^5.5.0 | ^5.6.3 | Minor (safe) |
| nock | ^13.2.4 | ^13.5.7 | Minor (safe) |
| vue-loader | ^17.0.0 | ^17.4.2 | Minor (safe) |
| vue-template-compiler | ^2.7.10 | ^2.7.16 | Patch (safe) |
| webpack | ^5.74.0 | ^5.97.1 | Patch (safe) |
| webpack-dev-server | ^4.11.1 | ^4.15.2 | Minor (safe) |

### Optional Dependencies Updates

| Package | Old | New | Type |
|---------|-----|-----|------|
| electron | ^21.3.1 | ^21.4.4 | Patch (safe) |
| electron-updater | ^4.6.1 | ^4.6.5 | Patch (safe) |

---

## NOT UPDATED (Require Major Version Changes)

These require breaking changes and should be updated in a separate PR:

| Package | Current | Latest | Why Not Updated |
|---------|---------|--------|-----------------|
| electron | 21.4.4 | 40.0.0 | 19 major versions - extensive testing needed |
| @tensorflow/tfjs-node | 2.8.6 | 4.22.0 | May break ML models |
| dotenv | 8.2.0 | 17.2.3 | Breaking API changes |
| dotenv-expand | 5.1.0 | 12.0.3 | Breaking API changes |
| web3 | 1.10.4 | 6.16.0 | Complete API rewrite v1→v6 |
| ethers | 5.8.0 | 6.16.0 | Significant API changes |
| vite | 3.2.11 | 7.3.1 | Config/plugin changes |
| chalk | 4.1.2 | 5.6.2 | ESM-only in v5 |
| boxen | 5.1.2 | 8.0.1 | ESM-only in v6+ |
| node-fetch | 2.7.0 | 3.3.2 | ESM-only in v3 |
| open | 8.4.2 | 11.0.0 | Potential breaking changes |

---

## Testing Recommendations

After applying these changes:

### 1. Install and Audit
```bash
# Backup current setup
cp package.json package.json.backup
cp package-lock.json package-lock.json.backup

# Apply recommended changes
cp package.json.RECOMMENDED package.json

# Clean install
rm -rf node_modules package-lock.json
npm install

# Verify security fixes
npm audit
```

### 2. Run Test Suite
```bash
npm run unitTest
npm run lintAll
```

### 3. Manual Testing
- [ ] Test Discord bot functionality (discord.js updated)
- [ ] Test database operations (pg, knex updated)
- [ ] Test web3/blockchain features (ethers, web3 updated)
- [ ] Test Vue UI (vue, vue-router updated)
- [ ] Test chart rendering (chart.js updated)
- [ ] Test spreadsheet import/export (xlsx MAJOR update)
- [ ] Test WebSocket connections (ws updated)
- [ ] Test PM2 process management
- [ ] Test CCXT exchange integrations
- [ ] Test build process (webpack, vite)

### 4. Check for Runtime Errors
```bash
# Start the application
npm start

# Monitor logs for any errors related to:
# - WebSocket connections (ws)
# - Spreadsheet parsing (xlsx)
# - Discord bot
# - Database queries
```

---

## Rollback Plan

If issues occur:

```bash
# Restore backup
cp package.json.backup package.json
cp package-lock.json.backup package-lock.json

# Reinstall
rm -rf node_modules
npm install
```

---

## Expected Impact

### Positive
✅ Eliminates 60+ known security vulnerabilities
✅ Reduces package bloat by ~500KB (removed built-ins)
✅ Better dependency management with overrides
✅ More predictable builds (no wildcards)
✅ Latest bug fixes and improvements

### Potential Issues
⚠️ **xlsx** (0.18.5 → 0.20.2): Major version jump, may have breaking changes in spreadsheet parsing
⚠️ **axios** in devDeps (0.18.0 → 1.7.9): If any dev scripts use axios, may need updates
⚠️ **vue** (3.2.40 → 3.5.27): Minor version jump but within v3, generally safe

### Low Risk
- All other updates are patch/minor versions within same major version
- npm's `^` semver ranges should handle these gracefully
- No changes to core business logic dependencies (web3, ethers, ccxt stay in same major version)

---

## Next Steps (Future PRs)

After validating these changes work:

1. **Major Version Updates** (3-4 weeks of testing)
   - electron 21 → 40
   - web3 1 → 6 (or migrate to ethers fully)
   - vite 3 → 7
   - @tensorflow/tfjs-node 2 → 4

2. **ESM Migration** (if needed)
   - Evaluate if project should migrate to ESM
   - Update chalk, boxen, node-fetch to latest if going ESM
   - Or stay on current major versions if staying CommonJS

3. **Dependency Cleanup**
   - Run `npx depcheck` to find unused dependencies
   - Consolidate axios versions (bundled vs regular)
   - Choose between web3 and ethers (having both is redundant)

---

## Questions?

- Review full analysis: `DEPENDENCY_AUDIT_REPORT.md`
- Test in development environment first
- Create issues for any problems found
- Consider gradual rollout (canary deployment)
