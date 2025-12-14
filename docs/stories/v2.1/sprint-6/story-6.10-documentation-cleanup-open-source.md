# Story 6.10: Documentation Cleanup for Open-Source Release

**Epic:** Open-Source Readiness (OSR)
**Story ID:** 6.10
**Sprint:** 6
**Priority:** 🔴 Critical (Blocker for OSR-10)
**Points:** 5
**Effort:** 4-6 hours
**Status:** ⚪ Ready
**Type:** 📝 Documentation / Cleanup

---

## 📊 Status

- [x] Draft
- [x] Validated (PO Review)
- [ ] Approved
- [ ] In Progress
- [ ] Review
- [ ] Done

---

## 📋 User Story

**Como** mantenedor do projeto AIOS/Synkra,
**Quero** que toda a documentação do framework esteja atualizada e consistente com a nova estrutura do repositório,
**Para** garantir que o lançamento open-source (OSR-10) tenha documentação precisa e profissional.

---

## 🎯 Objetivo

Atualizar toda a documentação do framework para refletir:
1. Nova organização: `SynkraAI/aios-core` (não mais `aios/aios-core` ou `AIOS-V4/aios-fullstack`)
2. Sistema de Squads (substitui expansion-packs)
3. Versões atualizadas de dependências (semantic-release v25)
4. Paths corretos no core-config.yaml
5. Remoção de referências obsoletas

---

## 🔍 Contexto

### Problemas Identificados

| Arquivo | Problema | Impacto |
|---------|----------|---------|
| `core-config.yaml` | `expansionPacksLocation: expansion-packs` não existe | Runtime errors |
| `core-config.yaml` | CodeRabbit path aponta para `AIOS-V4/aios-fullstack` | Path incorreto |
| `source-tree.md` | Referencia `expansion-packs/` que não existe | Doc incorreta |
| `source-tree.md` | Migration Notice: `aios/aios-core` | Org errada |
| `tech-stack.md` | `semantic-release: ^22.0.0` | Desatualizado (v25) |
| `coding-standards.md` | Migration Notice errado | Org errada |
| `backlog.md` | Enhancements obsoletos | Confuso |

### Mudança Arquitetural

```
❌ ANTIGO (não existe mais):
expansion-packs/
├── hybrid-ops/
└── expansion-creator/

✅ NOVO (atual):
templates/squad/           # Template para criar Squads
docs/guides/squads-guide.md  # Guia completo (OSR-8)
```

---

## ✅ Acceptance Criteria

### AC1: Framework Documentation Updated
- [ ] `docs/architecture/source-tree.md` atualizado
  - [ ] Removidas referências a `expansion-packs/`
  - [ ] Adicionadas referências a `templates/squad/`
  - [ ] Migration Notice: `SynkraAI/aios-core`
  - [ ] Seção de Squads documentada
- [ ] `docs/architecture/coding-standards.md` atualizado
  - [ ] Migration Notice: `SynkraAI/aios-core`
  - [ ] Seção de Squads/extensões se aplicável
- [ ] `docs/architecture/tech-stack.md` atualizado
  - [ ] semantic-release: `^25.0.2`
  - [ ] Migration Notice: `SynkraAI/aios-core`
  - [ ] Outras dependências verificadas

### AC2: core-config.yaml Updated
- [ ] Removido ou atualizado `expansionPacksLocation`
- [ ] Adicionado `squadsTemplateLocation: templates/squad`
- [ ] CodeRabbit `working_directory` atualizado para `SynkraAI/aios-core`
- [ ] Todos os paths verificados e funcionais

### AC3: Backlog Cleaned
- [ ] Enhancement 1733400000001 transformado em "Investigation Squads"
  - [ ] Scope atualizado para gaps do sistema Squads
  - [ ] Checklist renovado
- [ ] Enhancement 1733400000002 atualizado
  - [ ] Nota de migração para `synkra` project
  - [ ] Status mantido como Open

### AC4: Validation
- [ ] `npm test` passa
- [ ] `npm run lint` passa
- [ ] Nenhuma referência a paths inexistentes
- [ ] Manual review de todos os arquivos atualizados

---

## 📝 Tasks

### Task 1: Update Framework Documentation (2h)

**Files:**
- `docs/architecture/source-tree.md`
- `docs/architecture/coding-standards.md`
- `docs/architecture/tech-stack.md`

**Actions:**
1. [ ] Search and replace `aios/aios-core` → `SynkraAI/aios-core`
2. [ ] Search and replace `AIOS-V4/aios-fullstack` → `SynkraAI/aios-core`
3. [ ] Remove `expansion-packs/` directory references
4. [ ] Add `templates/squad/` references where applicable
5. [ ] Update version numbers (semantic-release, etc.)
6. [ ] Update Last Updated dates

### Task 2: Update core-config.yaml (1h)

**File:** `.aios-core/core-config.yaml`

**Actions:**
1. [ ] Change `expansionPacksLocation` to `squadsTemplateLocation: templates/squad`
2. [ ] Update CodeRabbit `wsl_config.working_directory`
3. [ ] Verify all other paths are correct
4. [ ] Add comment explaining Squads vs Expansion Packs

### Task 3: Transform Enhancement to Investigation Squads (30min)

**File:** `docs/stories/backlog.md`

**Actions:**
1. [ ] Update Enhancement 1733400000001:
   - Title: "Investigation: Squads System Enhancement"
   - New scope focused on gaps:
     - Squad Loader System
     - `npx create-aios-squad` CLI
     - Squad validation system
     - Migration tool from expansion-packs

### Task 4: Update hybrid-ops Enhancement (15min)

**File:** `docs/stories/backlog.md`

**Actions:**
1. [ ] Add migration note to Enhancement 1733400000002
2. [ ] Destination: `C:\Users\AllFluence-User\Workspaces\SynkraAi\synkra`
3. [ ] Keep status as Open but mark as "Migrating to synkra project"

### Task 5: Final Validation (30min)

**Actions:**
1. [ ] Run `npm test`
2. [ ] Run `npm run lint`
3. [ ] Grep for old paths/references
4. [ ] Manual review of changes

---

## 📁 Files to Modify

| File | Action | Priority |
|------|--------|----------|
| `docs/architecture/source-tree.md` | Update | 🔴 High |
| `docs/architecture/coding-standards.md` | Update | 🔴 High |
| `docs/architecture/tech-stack.md` | Update | 🔴 High |
| `.aios-core/core-config.yaml` | Update | 🔴 High |
| `docs/stories/backlog.md` | Update | 🟠 Medium |
| `docs/stories/v2.1/sprint-6/README.md` | Add story | 🟡 Low |

---

## 🔗 Dependencies

| Dependency | Status | Notes |
|------------|--------|-------|
| Story 6.9 | ✅ Done | Mode-aware system implemented |
| OSR-8 | ✅ Done | Squads guide created |
| OSR-9 | ✅ Done | Rebranding to Synkra complete |

---

## ⚠️ Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Breaking existing tests | Low | Medium | Run full test suite after changes |
| Missing references | Medium | Low | Grep search for old paths |
| core-config changes breaking loaders | Low | High | Test mode detection after changes |

---

## 🎯 Definition of Done

- [ ] All framework docs reference `SynkraAI/aios-core`
- [ ] No references to non-existent `expansion-packs/`
- [ ] `tech-stack.md` reflects semantic-release v25
- [ ] `core-config.yaml` paths are valid
- [ ] Backlog enhancements updated
- [ ] All tests pass
- [ ] Lint passes
- [ ] PR merged to main

---

## 📝 Notes

This story is a **BLOCKER** for OSR-10 (Release Checklist). The open-source release cannot proceed with stale documentation that references:
- Non-existent directories
- Wrong organization names
- Outdated dependency versions

---

## 📋 Version History

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 2025-12-14 | 1.0 | Initial story creation | Pax (PO) |

---

*Story created as part of OSR Epic - Open-Source Readiness*
