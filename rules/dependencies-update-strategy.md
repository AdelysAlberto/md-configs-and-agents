---
trigger: model_decision
---

# Dependencies Update Strategy

## 🚨 CRITICAL: Update Dependencies at the END of Migration

**Current Date**: March 2026

---

## ❌ Why NOT Update First?

```
If we update React 18.2 → 19.x NOW:
├─ React 19 breaking changes (defaultProps removed, new JSX transform, etc.)
├─ Potential incompatibilities with current ANTD 5.19.1
├─ Current codebase could break in unexpected ways
└─ ❌ WE DON'T KNOW if errors are from:
    - Dependency updates?
    - Our JSX → TSX migration?
    - Our ANTD → Base components migration?
    - Performance optimizations?
```

**Result**: Migration paralysis - can't move forward because everything is broken.

---

## ✅ Correct Strategy: Update at the END

### Phase Order

```
Phase 1-7: Migrate with CURRENT working versions
├─ Phase 1: Setup (tsconfig, biome) - NO dependency updates
├─ Phase 2: Create Base components
├─ Phase 3: Migrate modules (JSX→TSX, ANTD→Base, CSS Modules)
├─ Phase 4: Convert Zustand stores to TypeScript
├─ Phase 5: Implement Result Pattern in services
├─ Phase 6: Mobile-first CSS optimization
├─ Phase 7: Quality Assurance & Testing
└─ Phase 8: Dependencies Update ← ONLY AFTER EVERYTHING ELSE WORKS
```

### Benefits

1. ✅ **Stability**: Work with known, stable versions during migration
2. ✅ **Clear Attribution**: If something breaks, we know it's OUR code, not a dependency
3. ✅ **Focused Work**: One problem at a time
4. ✅ **Rollback Safety**: Can revert our changes without dependency conflicts
5. ✅ **Testing Confidence**: Test migration with stable dependencies first

---

## 📦 Phase 8: Dependencies Update (Final Step)

This phase happens AFTER:
- ✅ All modules migrated to TypeScript
- ✅ All ANTD components replaced with Base components
- ✅ All services use Result Pattern
- ✅ All CSS converted to CSS Modules
- ✅ All tests passing
- ✅ No console errors or warnings
- ✅ Performance metrics validated

### 8.1 Current Versions (March 2026)

**Before updating, verify REAL current versions:**

```bash
# Check current versions in your package.json
cat package.json | grep -A 1 "dependencies"

# Check latest versions available on npm
pnpm outdated

# Or check specific packages
npm view react version
npm view react-dom version
npm view @tanstack/react-query version
npm view zustand version
npm view axios version
npm view vite version
```

### 8.2 Major Updates Expected (March 2026)

**IMPORTANT**: These are estimates. Run commands above to get REAL versions.

#### Production Dependencies

| Package | Current | Target (Est.) | Breaking Changes? |
|---------|---------|---------------|-------------------|
| `react` | 18.2.0 | ~19.2.x | ⚠️ YES (defaultProps, etc.) |
| `react-dom` | 18.2.0 | ~19.2.x | ⚠️ YES |
| `react-router-dom` | 6.23.1 | ~6.26.x | ⚠️ Minor |
| `@tanstack/react-query` | 5.50.1 | ~5.56.x+ | ⚠️ Minor |
| `zustand` | 4.5.4 | ~5.0.x | ⚠️ YES |
| `axios` | 1.7.2 | ~1.7.x | ✅ No |
| `dayjs` | 1.11.11 | ~1.11.x | ✅ No |
| `i18next` | 23.11.5 | ~23.15.x+ | ✅ No |
| `react-i18next` | 14.1.3 | ~15.0.x+ | ⚠️ Minor |

#### Dev Dependencies

| Package | Current | Target (Est.) | Breaking Changes? |
|---------|---------|---------------|-------------------|
| `@biomejs/biome` | 2.2.2 | ~2.x+ | ⚠️ Check changelog |
| `vite` | 5.4.21 | ~5.4.x+ | ✅ No (patch) |
| `@vitejs/plugin-react-swc` | 3.5.0 | ~3.7.x+ | ✅ No |
| `typescript` | N/A (add) | ~5.6.x | ⚠️ New |

**⚠️ WARNING**: DO NOT blindly update to these versions. Always check:
1. Official release notes
2. Breaking changes documentation
3. Migration guides
4. Community feedback

### 8.3 Update Strategy (One at a Time)

**Step 1: Create Update Branch**
```bash
git checkout -b feature/dependencies-update
```

**Step 2: Update TypeScript & Build Tools First**
```bash
# Add TypeScript if not present
pnpm add -D typescript @types/react @types/react-dom @types/node

# Update Vite and plugins
pnpm update vite @vitejs/plugin-react-swc

# Test build
pnpm build
```

**Step 3: Update React Query (Minor Impact)**
```bash
# Check latest version
npm view @tanstack/react-query version

# Update React Query
pnpm update @tanstack/react-query @tanstack/react-query-devtools

# Test queries and mutations
pnpm dev
```

**Step 4: Update Zustand 4 → 5 (Breaking Changes)**
```bash
# Check migration guide first
open https://github.com/pmndrs/zustand/releases

# Update Zustand
pnpm update zustand

# Fix breaking changes in stores
# - Check middleware changes
# - Update store creation patterns
# - Test all Zustand stores

pnpm tsc --noEmit
pnpm dev
```

**Step 5: Update React 18 → 19 (Major Breaking Changes)**

**⚠️ CRITICAL**: This is the riskiest update. Do this LAST.

```bash
# Read React 19 migration guide FIRST
open https://react.dev/blog/2024/04/25/react-19-upgrade-guide

# Backup current working state
git add .
git commit -m "chore: backup before React 19 update"

# Update React
pnpm add react@latest react-dom@latest

# Common breaking changes to fix:
# 1. Remove defaultProps (use default parameters instead)
# 2. Update ref forwarding patterns
# 3. Update Context API usage
# 4. Update Suspense boundaries
# 5. Update error boundaries

# Test EVERYTHING
pnpm tsc --noEmit
pnpm build
pnpm dev
```

**Step 6: Update Other Dependencies**
```bash
# Update remaining dependencies (low risk)
pnpm update axios dayjs i18next react-i18next react-icons

# Final verification
pnpm biome check --write src
pnpm tsc --noEmit
pnpm build
pnpm test
```

### 8.4 Testing After Updates

**Automated Tests**
```bash
# Type checking
pnpm tsc --noEmit

# Linting
pnpm biome check src

# Build
pnpm build

# Run tests
pnpm test
```

**Manual Testing Checklist**
- [ ] Authentication flow works
- [ ] User profile loads
- [ ] Wallet operations work
- [ ] Navigation works
- [ ] Modals open/close
- [ ] Forms submit correctly
- [ ] API calls succeed
- [ ] Zustand stores persist data
- [ ] React Query caches data
- [ ] No console errors
- [ ] No console warnings
- [ ] Performance is same or better

### 8.5 Rollback Plan

If updates break something critical:

```bash
# Revert to previous working state
git reset --hard HEAD~1

# Or revert specific package
pnpm install react@18.2.0 react-dom@18.2.0

# Or revert entire package.json
git checkout HEAD~1 -- package.json pnpm-lock.yaml
pnpm install
```

### 8.6 Post-Update Verification

After successful update:

```bash
# Remove unused packages
pnpm remove antd @ant-design/icons

# Update lockfile
pnpm install

# Verify bundle size decreased
pnpm build
du -sh dist

# Commit changes
git add .
git commit -m "chore: update dependencies to latest versions (March 2026)"
git push origin feature/dependencies-update
```

---

## 🎯 Summary

**DO THIS FIRST (Phases 1-7)**:
1. ✅ Migrate all JSX → TSX with CURRENT React 18.2
2. ✅ Replace all ANTD with Base components
3. ✅ Implement Result Pattern in services
4. ✅ Convert to CSS Modules
5. ✅ Convert Zustand stores to TypeScript
6. ✅ Run all quality checks
7. ✅ Test everything thoroughly

**THEN DO THIS (Phase 8)**:
1. ✅ Verify latest versions with `pnpm outdated`
2. ✅ Update dependencies ONE AT A TIME
3. ✅ Test after each update
4. ✅ Fix breaking changes as they appear
5. ✅ Update React 19 LAST

**Why this order works:**
- ✅ Migration happens with stable, known versions
- ✅ Each problem has one clear cause
- ✅ Can rollback easily if needed
- ✅ Confidence in final result

---

## 📚 Resources

- [React 19 Upgrade Guide](https://react.dev/blog/2024/04/25/react-19-upgrade-guide)
- [Zustand 5 Migration](https://github.com/pmndrs/zustand/releases)
- [TanStack Query Releases](https://github.com/TanStack/query/releases)
- [Vite Releases](https://github.com/vitejs/vite/releases)
- [Biome Releases](https://github.com/biomejs/biome/releases)

---

## ⚠️ REMEMBER

**"Update dependencies AFTER migration, not before."**

This is not a preference - it's a strategy to ensure successful migration without compounding problems.
