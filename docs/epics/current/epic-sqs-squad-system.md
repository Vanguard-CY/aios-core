# Epic: Squad System Enhancement (SQS)

**Epic ID:** SQS
**Status:** APPROVED - Architecture Validated
**Created:** 2025-12-18
**PO:** Pax (Balancer)
**Owner:** Architect (Aria) + Dev (Dex) + DevOps (Gage)
**Target Sprint:** Sprint 7-8 (post OSR-10)
**Total Stories:** 10 stories (SQS-0 to SQS-9)
**Total Effort:** ~58-78 hours
**Repository:** https://github.com/SynkraAI/aios-squads (PUBLIC, EXISTS)

---

## Executive Summary

Completar o sistema de Squads do AIOS, transformando o framework de templates existente em um ecossistema completo de extensibilidade seguindo a **Task-First Architecture**. Este epic habilita:

1. **Squad Creator Agent** - Agente dedicado com tasks para criar, validar e publicar squads
2. **Task-First Squad Structure** - Squads seguem a mesma arquitetura do aios-core
3. **Validação robusta** de squad.yaml contra JSON Schema
4. **Distribuição em 3 níveis** - Local (privado), aios-squads (público), Synkra (API)
5. **Integração com core-config** - coding-standards, tech-stack, source-tree por squad
6. **Sinergia com aios-core** - Squads trabalham como extensões naturais do framework

---

## Strategic Context

### Problema de Negócio

Desenvolvedores usando AIOS em seus projetos precisam:
- Criar agentes especializados para seus domínios
- Reutilizar workflows entre projetos
- Compartilhar soluções com a comunidade
- Manter squads privados para código proprietário

### Solução Proposta

Sistema completo de Squads via **Agent + Tasks** (não CLI separado):

```
.aios-core/development/
├── agents/
│   └── squad-creator.md          # Agente criador de squads
└── tasks/
    ├── squad-creator-create.md       # *create-squad
    ├── squad-creator-validate.md     # *validate-squad
    ├── squad-creator-publish.md      # *publish-squad (→ aios-squads)
    ├── squad-creator-download.md     # *download-squad
    └── squad-creator-sync-synkra.md  # *sync-squad-synkra
```

### Modelo de Distribuição

```
┌─────────────────────────────────────────────────────────────────┐
│                    SQUAD DISTRIBUTION MODEL                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. LOCAL (Privado)                                             │
│     └─ ./squads/meu-squad/                                      │
│     └─ Apenas no projeto do usuário                             │
│                                                                  │
│  2. AIOS-SQUADS (Público)                                       │
│     └─ github.com/SynkraAI/aios-squads                          │
│     └─ Todos podem baixar via *download-squad                   │
│                                                                  │
│  3. SYNKRA API (Marketplace)                                    │
│     └─ api.synkra.dev/squads                                    │
│     └─ Uso via API, discovery, analytics                        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Task-First Squad Architecture

### Squad Structure (Espelhando aios-core)

```
my-squad/
├── squad.yaml                    # Manifest (similar a core-config)
├── README.md                     # Documentação
├── config/                       # Configurações do squad
│   ├── coding-standards.md      # Padrões específicos (extends core)
│   ├── tech-stack.md            # Tecnologias do squad
│   └── source-tree.md           # Estrutura documentada
├── agents/
│   └── {agent-id}.md            # Definições de agentes
├── tasks/
│   └── {agent-id}-{task}.md     # Tasks (task-first!)
├── checklists/
│   └── {checklist-name}.md      # Checklists de validação
├── workflows/
│   └── {workflow-name}.md       # Workflows multi-step
├── templates/
│   └── {template-name}.md       # Templates de documentos
├── tools/
│   └── {tool-name}.js           # Ferramentas utilitárias
├── scripts/
│   └── {script-name}.js         # Scripts de automação
└── data/
    └── {data-file}.yaml         # Base de conhecimento
```

### Integração com core-config

Squads lidos devem se integrar com o sistema de configuração do projeto:

```yaml
# core-config.yaml (aios-core)
squads:
  templateLocation: templates/squad
  autoLoad: true                    # ← Atualizado para carregar squads
  localPath: ./squads               # Squads locais do projeto

  # Squads ativos neste projeto
  active:
    - etl-squad
    - my-custom-squad

  # Override de config por squad
  overrides:
    etl-squad:
      coding-standards: extend      # Estende padrões do core
    my-custom-squad:
      coding-standards: override    # Substitui padrões
```

### Herança de Configurações

```
aios-core (base)
├── docs/framework/coding-standards.md    # Padrões base
├── docs/framework/tech-stack.md          # Stack base
└── docs/framework/source-tree.md         # Estrutura base
         │
         ├─── my-squad/ (extends)
         │    └── config/coding-standards.md   # Adiciona regras
         │
         └─── another-squad/ (override)
              └── config/coding-standards.md   # Substitui regras
```

---

## Architecture Decisions (ADR-SQS-001)

> **STATUS:** ✅ APPROVED by @architect (Aria) on 2025-12-18

### Fatos Estabelecidos

| Fato | Status | Detalhe |
|------|--------|---------|
| Repositório aios-squads | ✅ Existe | https://github.com/SynkraAI/aios-squads (PUBLIC) |
| Estrutura monorepo | ✅ Definida | npm workspaces em `packages/*` |
| Squads existentes | ✅ Production | etl-squad v2.0.0, creator-squad v1.0.0 |
| Manifest format | ✅ Decidido | Suporta ambos, padrão `squad.yaml` |
| peerDependency | ✅ Atualizado | `@aios-fullstack/core: >=2.0.0` |

### Q1: Manifest Format ✅ DECIDED

**Decisão:** Suportar ambos os formatos, mas padronizar em `squad.yaml`

**Implementação:**
```javascript
// Squad Loader resolution order
const MANIFEST_FILES = ['squad.yaml', 'config.yaml'];

async function loadManifest(squadPath) {
  for (const filename of MANIFEST_FILES) {
    const manifestPath = path.join(squadPath, filename);
    if (await exists(manifestPath)) {
      return { path: manifestPath, data: await parseYaml(manifestPath) };
    }
  }
  throw new Error(`No squad manifest found in ${squadPath}`);
}
```

**Rationale:**
- Prioriza `squad.yaml` (padrão do aios-core templates)
- Mantém compatibilidade com `config.yaml` existentes
- Migração gradual sem breaking changes
- Console warning para `config.yaml` (deprecated)

**Migration Path:**
1. v2.2: Suporte a ambos, warning para config.yaml
2. v3.0: Apenas squad.yaml (breaking change documentado)

### Q2: Loading Strategy ✅ DECIDED

**Decisão:** Hybrid Loading Strategy

**Implementação:**
```javascript
const loaderConfig = {
  strategy: 'hybrid',
  eagerLoad: ['core-squads'],      // Carrega no startup
  lazyLoad: ['community-squads'],  // Carrega sob demanda
  preloadHints: true               // Pre-fetch metadata
};
```

**Rationale:**
- Core squads (etl, creator) carregados no startup para disponibilidade imediata
- Community squads carregados sob demanda para performance
- Metadata preloaded para UX (autocomplete, discovery)

### Q3: Synkra Integration ✅ DECIDED

**Decisão:** Option A - Novo `squadSyncService.js`

**Arquitetura:**
```
synkra/api/src/services/
├── taskSyncService.js     # Existing (tasks)
├── squadSyncService.js    # NEW (squads)
└── syncServiceBase.js     # Shared utilities (extract from existing)
```

**Rationale:**
- Separação de responsabilidades clara
- Não afeta estabilidade do taskSyncService existente
- Permite evolução independente
- Reutiliza padrões via classe base

### Q4: Resolution Chain ✅ DECIDED

**Decisão:** 4-Level Resolution Chain

```
Priority Order:
┌─────────────────────────────────────────────────────┐
│ 1. LOCAL      ./squads/{squad-name}/squad.yaml     │ ← Highest
├─────────────────────────────────────────────────────┤
│ 2. PROJECT    node_modules/@aios-squads/{squad}/   │
├─────────────────────────────────────────────────────┤
│ 3. WORKSPACE  ../node_modules/@aios-squads/{squad}/│
├─────────────────────────────────────────────────────┤
│ 4. REGISTRY   github.com/SynkraAI/aios-squads/     │ ← Lowest
└─────────────────────────────────────────────────────┘
```

**Rationale:**
- Local sempre vence (desenvolvimento/customização)
- Project npm para dependências do projeto
- Workspace para monorepos
- Registry como fallback para discovery

### Q5: Package Namespace ✅ DECIDED

**Decisão:** Option A - `@aios-squads/*`

**Exemplos:**
```bash
npm install @aios-squads/etl
npm install @aios-squads/creator
npm install @aios-squads/community-{name}
```

**Rationale:**
- Namespace limpo e dedicado
- Separação clara de `@aios-fullstack/core`
- Permite community contributions com namespacing
- Fácil discovery no npm registry

---

## Existing Assets (Real Inventory)

### GitHub Repository (ALREADY EXISTS)

| Property | Value |
|----------|-------|
| **URL** | https://github.com/SynkraAI/aios-squads |
| **Visibility** | PUBLIC |
| **Created** | 2025-12-09 |
| **Description** | "AIOS Squads: Equipes de AI agents trabalhando com você - ETL, Creator, MMOS" |
| **Package** | `@aios/squads` v2.1.0 |
| **Structure** | Monorepo com npm workspaces |

### Local Project

**Path:** `C:\Users\AllFluence-User\Workspaces\AIOS\SynkraAI\aios-squads`

```
aios-squads/
├── package.json           # @aios/squads v2.1.0 (monorepo root)
├── README.md              # ⚠️ Needs SynkraAI branding update
├── LICENSE                # MIT
├── .github/
│   ├── CODEOWNERS
│   └── workflows/ci.yml
└── packages/
    ├── etl-squad/         # ✅ Production Ready (v2.0.0)
    │   ├── config.yaml    # Squad manifest
    │   ├── agents/        # 6 agents (some deprecated)
    │   ├── scripts/       # Active utilities
    │   └── deprecated/    # Legacy code
    └── creator-squad/     # ✅ Production Ready (v1.0.0)
        ├── config.yaml    # Squad manifest
        ├── agents/        # 1 agent (expansion-creator)
        ├── tasks/         # 5 tasks
        └── templates/     # 5 templates
```

### Squad Manifest Format (config.yaml - NOT squad.yaml)

O formato atual usa `config.yaml`:

```yaml
# packages/etl-squad/config.yaml
name: etl
version: 2.0.0
short-title: ETL - Blog Collection Utilities
description: >-
  Lightweight blog collection utilities...
author: Academia Lendar[IA] (Alan Nicolas)
slashPrefix: etl

tags: [blog-collection, web-scraping]

requires:
  node: ">=18.0.0"
  aios: ">=2.0.0"

scripts:
  discovery: [...]
  collection: [...]
  transformers: [...]

dependencies:
  node: ["@mozilla/readability", "cheerio", ...]
  python: []

mcps:
  recommended_alternatives: {...}

integration:
  mmos: {...}
  standalone: {...}
```

### Existing Squads Inventory

| Squad | Version | Status | Agents | Scripts | Description |
|-------|---------|--------|--------|---------|-------------|
| **etl-squad** | 2.0.0 | Production | 6 (4 deprecated) | 5 active | Blog collection utilities, 100% success rate |
| **creator-squad** | 1.0.0 | Production | 1 | 5 tasks | Expansion pack creator tool |

### aios-core Assets

| Asset | Location | Status |
|-------|----------|--------|
| Squad Template | `templates/squad/` (10 arquivos) | Production Ready |
| Squad Guide | `docs/guides/squads-guide.md` (293 linhas) | Production Ready |
| Squad Examples | `docs/guides/squad-examples/` | Production Ready |
| Sync Service | `synkra/api/src/services/taskSyncService.js` | Production Ready |

### Issues Identified

1. **README.md** - References old `allfluence` org instead of `SynkraAI`
2. **package.json** - Repository URL points to `allfluence/aios-squads`
3. **Format Mismatch** - aios-core templates use `squad.yaml`, repo uses `config.yaml`
4. **No Squad Loader** - Code mentions `loadSquad()` but not implemented
5. **peerDependency** - Uses `@aios/core` but package is `@aios-fullstack/core`

---

## Stories Breakdown

### Sprint 7 - Foundation (5 stories, ~30-40h)

| ID | Story | Tipo | Priority | Effort | Status | File |
|----|-------|------|----------|--------|--------|------|
| **SQS-0** | aios-squads Repo Cleanup (Quick Win) | Tech Debt | Critical | 2h | ✅ DONE | N/A |
| **SQS-1** | Architecture Validation Session | Investigation | Critical | 4h | ✅ DONE | ADR-SQS-001 |
| **SQS-2** | Squad Loader (Simplified Utility) | Feature | Critical | 8-10h | 📋 Revising | [story-sqs-2](../../stories/v2.1/sprint-7/story-sqs-2-squad-loader.md) |
| **SQS-3** | Squad Validator + JSON Schema | Feature | High | 6-8h | 📋 Revising | [story-sqs-3](../../stories/v2.1/sprint-7/story-sqs-3-schema-validator.md) |
| **SQS-4** | Squad Creator Agent + Tasks | Feature | Critical | 10-14h | 📋 Revising | [story-sqs-4](../../stories/v2.1/sprint-7/story-sqs-4-squad-creator-agent.md) |

**Mudanças na Arquitetura:**
- SQS-2: Simplificado para utility de resolução (não serviço complexo)
- SQS-3: Mantido, com integração ao squad-creator
- SQS-4: **MUDANÇA MAIOR** - De CLI npm para Agent + Tasks no aios-core

### Sprint 8 - Integration (5 stories, ~54-71h)

| ID | Story | Tipo | Priority | Effort | Deps |
|----|-------|------|----------|--------|------|
| **SQS-5** | SquadSyncService for Synkra API | Feature | High | 10-12h | SQS-4 |
| **SQS-6** | Download & Publish Tasks | Feature | High | 8-10h | SQS-4 |
| **SQS-7** | Migration Tool (expansion-packs → squads) | Feature | Medium | 6-8h | SQS-3 |
| **SQS-8** | Documentation & Examples Update | Enhancement | Medium | 4-6h | All |
| **SQS-9** | Squad Designer - Guided Creation from Docs | Feature | High | 26-35h | SQS-4 |

---

## Story Details

### SQS-0: aios-squads Repo Cleanup (Quick Win) ✅ COMPLETE

**Objective:** Fix branding e referências no repositório aios-squads existente.

**Status:** ✅ COMPLETE (commit fc3d97c, pushed 2025-12-18)

**Repository:** https://github.com/SynkraAI/aios-squads

**Local Path:** `C:\Users\AllFluence-User\Workspaces\AIOS\SynkraAI\aios-squads`

**Checklist:**
- [x] `README.md` - Update all `allfluence` references to `SynkraAI`
- [x] `package.json` - Fix repository URL to `SynkraAI/aios-squads`
- [x] `package.json` - Update peerDependency from `@aios/core` to `@aios-fullstack/core`
- [x] `packages/README.md` - Update any old references
- [x] Create `registry.json` with current squads inventory
- [x] Commit and push changes (commit fc3d97c)

**Files to Update:**
```
aios-squads/
├── README.md                    # Fix allfluence → SynkraAI
├── package.json                 # Fix repository URL, peerDep
└── packages/
    └── README.md                # Fix any old references
```

**Effort:** 2 hours
**Dependencies:** None (can start immediately)
**Output:** Clean repository ready for architecture decisions

---

### SQS-1: Architecture Validation Session ✅ COMPLETE

**Objective:** Validar decisões arquiteturais com @architect antes de implementação.

**Status:** ✅ COMPLETE (ADR-SQS-001 approved 2025-12-18)

**Checklist:**
- [x] Revisar Q1-Q5 com @architect
- [x] Documentar decisões em ADR-SQS-001 (inline neste epic)
- [x] Validar estrutura do aios-squads repo
- [x] Definir interface entre squads e aios-core (Resolution Chain)
- [x] Definir estratégia de integração Synkra (squadSyncService.js)
- [x] Aprovar formato manifest (squad.yaml padrão, config.yaml deprecated)

**Output:** ADR-SQS-001 documentado na seção "Architecture Decisions" deste epic

**Dependencies:** SQS-0 ✅

---

### SQS-2: Squad Loader (Simplified Utility)

**Objective:** Utility simples para resolver e carregar manifests de squads.

**Escopo Revisado:**
- Não é um serviço complexo com estratégias de loading
- É uma utility que resolve paths e carrega squad.yaml
- Usado internamente pelo squad-creator agent

**Scope:**
```javascript
// .aios-core/development/scripts/squad/squad-loader.js
export class SquadLoader {
  // Resolve squad path following priority chain
  async resolve(squadName) {
    // 1. Local: ./squads/{squadName}/squad.yaml
    // 2. Template: templates/squad/ (for new squads)
    return { path, manifest };
  }

  // Load and parse squad manifest
  async loadManifest(squadPath) {
    // Try squad.yaml first, then config.yaml (deprecated)
    return parseYaml(manifestPath);
  }

  // List all available squads in project
  async listLocal() {
    return glob('./squads/*/squad.yaml');
  }
}
```

**Deliverables:**
- [ ] `.aios-core/development/scripts/squad/squad-loader.js`
- [ ] Resolution chain: Local → Template
- [ ] Support both `squad.yaml` and `config.yaml` (deprecated warning)
- [ ] Unit tests (80%+ coverage)

**Success Criteria:**
- Resolve squad from local `./squads/` directory
- Load and parse squad manifest
- List available local squads
- Provide deprecation warning for `config.yaml`

---

### SQS-3: Squad Validator + JSON Schema

**Objective:** Validação robusta de `squad.yaml` seguindo TASK-FORMAT-SPECIFICATION-V1.

**JSON Schema (Atualizado para Task-First):**
```yaml
# .aios-core/schemas/squad-schema.json
$schema: http://json-schema.org/draft-07/schema#
type: object
required: [name, version, aios]
properties:
  name:
    type: string
    pattern: "^[a-z0-9-]+$"
  version:
    type: string
    pattern: "^\\d+\\.\\d+\\.\\d+$"
  description:
    type: string
    maxLength: 500
  author:
    type: string
  license:
    type: string
    enum: [MIT, Apache-2.0, ISC, GPL-3.0, UNLICENSED]
  aios:
    type: object
    required: [minVersion, type]
    properties:
      minVersion:
        type: string
        pattern: "^\\d+\\.\\d+\\.\\d+$"
      type:
        type: string
        enum: [squad]

  # Task-First Structure
  components:
    type: object
    properties:
      tasks:           # Tasks são primários (task-first!)
        type: array
        items: { type: string }
      agents:
        type: array
        items: { type: string }
      workflows:
        type: array
        items: { type: string }
      checklists:
        type: array
        items: { type: string }
      templates:
        type: array
        items: { type: string }
      tools:
        type: array
        items: { type: string }
      scripts:
        type: array
        items: { type: string }

  # Config inheritance
  config:
    type: object
    properties:
      extends:         # extend | override | none
        type: string
        enum: [extend, override, none]
        default: extend
      coding-standards:
        type: string
      tech-stack:
        type: string
      source-tree:
        type: string
```

**Deliverables:**
- [ ] `.aios-core/schemas/squad-schema.json`
- [ ] `.aios-core/development/scripts/squad/squad-validator.js`
- [ ] Validação de estrutura de diretórios
- [ ] Validação de task format (TASK-FORMAT-SPECIFICATION-V1)
- [ ] Mensagens de erro claras com sugestões de fix
- [ ] Integração com squad-creator agent

---

### SQS-4: Squad Creator Agent + Tasks (MAJOR CHANGE)

**Objective:** Agente dedicado no aios-core para criar, validar e gerenciar squads.

**MUDANÇA ARQUITETURAL:**
- ❌ Antes: CLI npm package (`create-aios-squad`)
- ✅ Agora: Agent + Tasks em `.aios-core/development/`

**Agent Definition:**
```yaml
# .aios-core/development/agents/squad-creator.md
agent:
  name: Craft
  id: squad-creator
  title: Squad Creator
  icon: 🏗️
  whenToUse: Use to create, validate, publish and manage squads

persona:
  role: Squad Architect & Builder
  style: Systematic, task-first, follows AIOS standards
  focus: Create well-structured squads that work in synergy with aios-core

commands:
  - name: create-squad
    description: Create new squad following task-first architecture
  - name: validate-squad
    description: Validate squad against JSON Schema and standards
  - name: publish-squad
    description: Publish squad to aios-squads repository (creates PR)
  - name: download-squad
    description: Download public squad from aios-squads
  - name: sync-squad-synkra
    description: Sync squad to Synkra API for marketplace

dependencies:
  tasks:
    - squad-creator-create.md
    - squad-creator-validate.md
    - squad-creator-publish.md
    - squad-creator-download.md
    - squad-creator-sync-synkra.md
  scripts:
    - squad/squad-loader.js
    - squad/squad-validator.js
    - squad/squad-generator.js
```

**Tasks Deliverables:**

```
.aios-core/development/tasks/
├── squad-creator-create.md       # *create-squad
│   └── Elicitation: name, description, author, components
│   └── Generates: ./squads/{name}/ structure
│   └── Validates: squad.yaml against schema
│
├── squad-creator-validate.md     # *validate-squad
│   └── Input: squad path
│   └── Validates: schema, task format, directory structure
│   └── Output: validation report with fixes
│
├── squad-creator-publish.md      # *publish-squad (Sprint 8)
│   └── Input: squad path
│   └── Creates: PR to aios-squads repo
│   └── Validates: before publish
│
├── squad-creator-download.md     # *download-squad (Sprint 8)
│   └── Input: squad name from registry
│   └── Downloads: to ./squads/{name}/
│   └── Validates: after download
│
└── squad-creator-sync-synkra.md  # *sync-squad-synkra (Sprint 8)
    └── Input: squad path
    └── Syncs: to Synkra API
    └── Returns: squad URL in marketplace
```

**Scripts Deliverables:**

```
.aios-core/development/scripts/squad/
├── squad-generator.js     # Generates squad structure from template
├── squad-validator.js     # Validates squad against schema
├── squad-loader.js        # Resolves and loads squad manifests
└── squad-publisher.js     # Helpers for publishing to aios-squads
```

**Interactive Flow (*create-squad):**

```
@squad-creator

*create-squad

? Squad name: meu-dominio-squad
? Description: Squad para automação de X
? Author: Seu Nome
? License: MIT
? Include example agent? Yes
? Include example task? Yes
? Include example workflow? No
? Minimum AIOS version: 2.1.0
? Config inheritance: extend (extends core standards)

Creating squad structure...
├── ./squads/meu-dominio-squad/
│   ├── squad.yaml ✅
│   ├── config/
│   │   ├── coding-standards.md ✅ (extends core)
│   │   └── source-tree.md ✅
│   ├── agents/
│   │   └── example-agent.md ✅
│   ├── tasks/
│   │   └── example-agent-task.md ✅
│   └── README.md ✅

✅ Squad created successfully!

📋 Next steps:
   1. cd squads/meu-dominio-squad
   2. Customize agents and tasks
   3. *validate-squad ./squads/meu-dominio-squad
```

**Success Criteria:**
- Agent ativável via @squad-creator
- *create-squad gera estrutura completa seguindo task-first
- *validate-squad valida contra schema e standards
- Squads criados trabalham em sinergia com aios-core
- Config inheritance funciona (extend/override)

---

### SQS-5: SquadSyncService for Synkra API

**Objective:** Serviço de sincronização de squads para o projeto Synkra.

**Based on:** `taskSyncService.js` patterns

**Scope:**
```javascript
// squadSyncService.js
export class SquadSyncService {
  // Sync all squads from directories
  async syncAll(options) { }

  // Sync single squad
  async syncSquad(squadPath, options) { }

  // Watch for squad changes
  watchDirectory(dirPath, callback) { }

  // Validate squad
  validateSquad(squadData) { }

  // Get checksum
  getChecksum(squadPath) { }
}
```

**Database Schema:**
```sql
CREATE TABLE synkra_squads (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  squad_id VARCHAR(100) UNIQUE NOT NULL,
  name VARCHAR(200) NOT NULL,
  version VARCHAR(20) NOT NULL,
  description TEXT,
  author VARCHAR(200),
  license VARCHAR(50),
  min_aios_version VARCHAR(20),
  components JSONB,
  dependencies JSONB,
  keywords TEXT[],
  source_path TEXT,
  checksum VARCHAR(64),
  is_public BOOLEAN DEFAULT false,
  is_official BOOLEAN DEFAULT false,
  workspace_id UUID REFERENCES workspaces(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  last_synced_at TIMESTAMPTZ
);

CREATE INDEX idx_squads_workspace ON synkra_squads(workspace_id);
CREATE INDEX idx_squads_public ON synkra_squads(is_public) WHERE is_public = true;
```

**Deliverables:**
- [ ] `synkra/api/src/services/squadSyncService.js`
- [ ] Database migration for `synkra_squads` table
- [ ] REST API endpoints: `POST /api/squads/sync`, `GET /api/squads`
- [ ] Integration with existing taskSyncService patterns
- [ ] Unit tests

---

### SQS-6: Registry Integration & Repo Cleanup

**Objective:** Organizar o repositório existente e habilitar publicação de squads.

**Repositório Existente:** https://github.com/SynkraAI/aios-squads

**Cleanup Tasks:**
```bash
# 1. Fix branding (allfluence → SynkraAI)
# README.md, package.json repository URLs

# 2. Update peerDependency
# @aios/core → @aios-fullstack/core

# 3. Add registry.json
# Index of available squads
```

**Registry Format (registry.json):**
```json
{
  "version": "1.0.0",
  "updated": "2025-12-18T00:00:00Z",
  "squads": {
    "official": [
      {
        "name": "etl-squad",
        "version": "2.0.0",
        "description": "Blog collection utilities with 100% success rate",
        "path": "packages/etl-squad",
        "status": "production"
      },
      {
        "name": "creator-squad",
        "version": "1.0.0",
        "description": "Expansion pack creator tool",
        "path": "packages/creator-squad",
        "status": "production"
      }
    ],
    "community": []
  }
}
```

**npm Publishing Setup:**
```bash
# Individual squad publishing
cd packages/etl-squad
npm publish --access public

# Install squad
npm install @aios-squads/etl
```

**Deliverables:**
- [ ] Fix README.md branding (allfluence → SynkraAI)
- [ ] Fix package.json repository URLs
- [ ] Update peerDependency to `@aios-fullstack/core`
- [ ] Create `registry.json` with existing squads
- [ ] `aios squad submit` command (creates PR)
- [ ] `aios squad search` command (searches registry)
- [ ] GitHub Action for registry validation on PR

---

### SQS-7: Migration Tool

**Objective:** Converter expansion-packs existentes para formato Squad.

**Command:**
```bash
aios squad migrate ./expansion-packs/my-pack ./squads/my-squad
```

**Migration Steps:**
1. Parse `expansion-pack.yaml` → `squad.yaml`
2. Move agents to `agents/` directory
3. Move tasks to `tasks/` directory
4. Update internal references
5. Generate `package.json`
6. Validate migrated squad
7. Report issues and manual fixes needed

**Deliverables:**
- [ ] `src/cli/commands/squad-migrate.js`
- [ ] Mapping rules for expansion-pack → squad
- [ ] Migration report generator
- [ ] Test with `hybrid-ops` expansion pack

---

### SQS-8: Documentation & Examples Update

**Objective:** Atualizar documentação existente com novos recursos.

**Updates Required:**
- [ ] `docs/guides/squads-guide.md` - Add new commands and workflows
- [ ] `docs/guides/squad-examples/` - Add real-world examples
- [ ] `README.md` - Update Squads section
- [ ] `CONTRIBUTING.md` - Add squad contribution guide
- [ ] Create video tutorial outline

**New Documentation:**
- [ ] `docs/guides/squad-loader-usage.md`
- [ ] `docs/guides/squad-publishing.md`
- [ ] `docs/guides/squad-migration.md`

---

### SQS-9: Squad Designer - Guided Creation from Documentation

**Objective:** Assistente inteligente que analisa documentacao do usuario e guia a criacao de squads com recomendacoes.

**Problem:**
- `*create-squad` foca em COMO criar, nao O QUE criar
- Usuario precisa saber previamente quais agents/tasks precisa
- Templates sao genericos, nao domain-aware

**Solution:**
Nova task `*design-squad` que:
1. Recebe documentacao (PRD, specs, texto)
2. Analisa dominio e extrai conceitos
3. Gera recomendacoes de agents e tasks
4. Permite refinamento interativo
5. Gera blueprint (`squad-design.yaml`)
6. Integra com `*create-squad --from-design`

**Deliverables:**
- [ ] `.aios-core/development/tasks/squad-creator-design.md`
- [ ] `.aios-core/development/scripts/squad/squad-designer.js`
- [ ] `.aios-core/schemas/squad-design-schema.json`
- [ ] Update squad-generator.js for `--from-design`
- [ ] Update squad-creator.md agent
- [ ] Unit tests (90%+ coverage)
- [ ] Integration tests

**Effort:** 26-35 hours

**Story File:** [story-sqs-9-squad-designer.md](../../stories/v2.1/sprint-8/story-sqs-9-squad-designer.md)

---

## Dependency Flow

```
Sprint 7 (Foundation)
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  SQS-0 (Repo Cleanup) ──► SQS-1 (Architecture)             │
│                                  │                          │
│                                  ├──────► SQS-2 (Loader)   │
│                                  │                          │
│                                  ├──────► SQS-3 (Validator)│
│                                  │              │           │
│                                  │              ▼           │
│                                  └─────────► SQS-4 (Agent) │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
Sprint 8 (Integration)
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  SQS-2 ────► SQS-5 (Synkra Integration)                    │
│                                                             │
│  SQS-4 ────► SQS-6 (Download & Publish)                    │
│       │                                                     │
│       └────► SQS-9 (Squad Designer) ◄─── NEW               │
│                                                             │
│  SQS-3 ────► SQS-7 (Migration)                             │
│                                                             │
│  ALL ──────► SQS-8 (Documentation)                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Success Criteria

### Technical

- [ ] Squad Loader supports npm, local, and repo sources
- [ ] 100% JSON Schema validation coverage
- [ ] CLI creates valid squad in < 30 seconds
- [ ] SquadSyncService matches taskSyncService patterns
- [ ] Migration tool handles 90%+ of expansion-packs automatically

### Community

- [ ] aios-squads repository created and public
- [ ] At least 2 official squads published (etl, creator)
- [ ] Squad submission process documented
- [ ] Community can create and share squads

### Integration

- [ ] Synkra API can discover and load squads
- [ ] Squads sync to database for marketplace readiness
- [ ] Private squads remain in user projects only
- [ ] Public squads visible in registry

---

## Risks & Mitigation

### Risk 1: Architecture complexity delays implementation

- **Probability:** MEDIUM
- **Impact:** HIGH (blocks all stories)
- **Mitigation:** SQS-1 is time-boxed to 4h, decisions documented immediately

### Risk 2: npm namespace conflicts

- **Probability:** LOW
- **Impact:** MEDIUM
- **Mitigation:** Use `@aios-squads/` namespace, claim early

### Risk 3: Breaking changes to existing squads

- **Probability:** MEDIUM
- **Impact:** MEDIUM
- **Mitigation:** Migration tool, deprecation period, version guards

### Risk 4: Synkra integration complexity

- **Probability:** MEDIUM
- **Impact:** MEDIUM
- **Mitigation:** Follow existing taskSyncService patterns exactly

---

## Related Resources

### From Backlog

- Investigation: Squads System Enhancement (ID: 1733400000001)
- Investigation: hybrid-ops Squad refactor (ID: 1733400000002)

### Dependencies

- **Upstream:** Epic OSR (OSR-8 provides foundation)
- **Downstream:** Epic WIS (Workflow Intelligence may use squad patterns)

### External References

- `synkra/api/src/services/taskSyncService.js` - Pattern reference
- `templates/squad/` - Existing template
- `docs/guides/squads-guide.md` - Current documentation

---

## Next Steps

1. ~~**BLOCKER:** @architect (Aria) validate architecture questions (Q1-Q5)~~ ✅ DONE
2. ~~Create aios-squads repository structure~~ ✅ EXISTS
3. ~~SQS-0: aios-squads repo cleanup~~ ✅ DONE (commit fc3d97c)
4. **NEXT:** Create SQS-1 story file (Architecture documentation)
5. **NEXT:** Begin SQS-2 implementation (Squad Loader)
6. Schedule Sprint 7 planning

---

## Decision Log

| Date | Decision | Stakeholder |
|------|----------|-------------|
| 2025-12-18 | Epic created from backlog item 1733400000001 | @po (Pax) |
| 2025-12-18 | Scope expanded to include Synkra integration | Stakeholder |
| 2025-12-18 | Sprint 7 target confirmed | @po (Pax) |
| 2025-12-18 | SQS-0 completed - aios-squads repo rebranded to SynkraAI | @po (Pax) |
| 2025-12-18 | Q1: Manifest format - Support both, standardize on `squad.yaml` | @architect (Aria) |
| 2025-12-18 | Q2: Loading strategy - Hybrid (core eager, extensions lazy) | @architect (Aria) |
| 2025-12-18 | Q3: Synkra integration - New squadSyncService.js | @architect (Aria) |
| 2025-12-18 | Q4: Resolution chain - 4-level (Local → npm → Workspace → Registry) | @architect (Aria) |
| 2025-12-18 | Q5: Package namespace - `@aios-squads/*` | @architect (Aria) |
| 2025-12-18 | Architecture validated - ADR-SQS-001 approved | @architect (Aria) |
| 2025-12-18 | SQS-9 created - Squad Designer for guided squad creation | @architect (Aria) |

---

**Created by:** Pax (PO)
**Date:** 2025-12-18
**Architecture Approved:** 2025-12-18 by @architect (Aria)
**Status:** APPROVED - Ready for Implementation

---

*Epic SQS: Transformando Squads em um ecossistema completo de extensibilidade*
