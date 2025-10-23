# 🚀 Guia Rápido do Desenvolvedor
## Firebase Studio + Cloudflare - Fluxo Otimizado

**Versão:** 1.0 - Outubro 2025  
**Foco:** Ações essenciais do desenvolvedor para maximizar a IA

---

## 📋 CONFIGURAÇÃO INICIAL RÁPIDA

### 1.1. Ordem de Configuração
```bash
# 1. Firebase Studio (Primeiro)
firebase.studio → "New Project" → "Prompt-based Development"

# 2. Cloudflare (Segundo)
Cloudflare Dashboard → Workers & Pages → Create Worker
→ Configurar KV Namespace → Configurar Durable Objects

# 3. Variáveis de Ambiente (Terceiro)
Firebase Studio → Project Settings → Environment Variables
```

### 1.2. Configuração Cloudflare (5 minutos)
```toml
# workers/api-gateway/wrangler.toml
name = "api-gateway"
compatibility_date = "2024-01-01"

[[kv_namespaces]]
binding = "CACHE_KV"
id = "seu-kv-id"

[durable_objects.bindings]
name = "RATE_LIMITER"
class_name = "RateLimiter"

# Configurar secrets via CLI:
wrangler secret put FIREBASE_ADMIN_SDK_JSON
wrangler secret put JWT_SECRET
```

---

## 🎯 PROMPTS ESSENCIAIS PARA IA

### 2.1. Prompt Inicial Obrigatório
```
LEIA E SIGA RIGOROSAMENTE O DOCUMENTO "DIRETRIZES IA - FIREBASE STUDIO"

Principais regras:
- Componentes máx 150 linhas
- ZERO position: absolute para layouts
- Flexbox/Grid obrigatório
- Mobile-first sempre
- TypeScript em tudo
- Acessibilidade completa

Agora crie: [DESCREVA O COMPONENTE/SISTEMA]
```

### 2.2. Prompts por Fase de Desenvolvimento

**Fase 1 - Estrutura Base:**
```
"Configure a estrutura inicial do projeto Next.js + Firebase seguindo:
- App Router com route groups
- Data Connect configurado
- Layout responsivo base
- Sistema de autenticação
- Cache layer structure"
```

**Fase 2 - Componentes:**
```
"Crie o componente [NOME] com:
- Props TypeScript interface
- Styling Tailwind responsivo
- Acessibilidade ARIA
- Máximo 120 linhas
- Error boundaries
- Loading states"
```

**Fase 3 - Integração:**
```
"Integre Firebase Data Connect com:
- GraphQL schema para [ENTIDADE]
- Custom hooks tipados
- Cache management
- Error handling
- Optimistic updates"
```

**Fase 4 - Otimização:**
```
"Otimize performance com:
- Code splitting
- Image optimization
- Bundle analysis
- Cache strategies
- Lazy loading"
```

---

## ⚡ AÇÕES DO DESENVOLVEDOR

### 3.1. Verificações Diárias
```bash
# Scripts de verificação rápida
npm run quality:fast

# quality:fast script (adicione no package.json)
"scripts": {
  "quality:fast": "npm run type-check && npm run lint && npm run build:analyze",
  "build:analyze": "ANALYZE=true npm run build",
  "security:check": "npm audit && npx snyk test"
}
```

### 3.2. Configurações Firebase Studio
```typescript
// firebase.json - Configurações essenciais
{
  "hosting": {
    "source": ".",
    "frameworksBackend": {
      "region": "us-central1"
    }
  },
  "dataConnect": {
    "source": "firebase/data-connect",
    "connectorId": "enterprise-connector",
    "location": "us-central1"
  }
}
```

---

## 🔧 SCRIPTS DE VERIFICAÇÃO

### 4.1. Health Check Completo
```bash
#!/bin/bash
# scripts/health-check.sh

echo "🔍 Iniciando Health Check..."

# 1. TypeScript
echo "📝 Verificando TypeScript..."
npm run type-check

# 2. ESLint
echo "🧹 Verificando código..."
npm run lint

# 3. Build
echo "🏗️ Testando build..."
npm run build

# 4. Security
echo "🛡️ Verificando segurança..."
npm audit
npx snyk test --severity-threshold=high

# 5. Tests
echo "🧪 Rodando testes..."
npm run test:unit

echo "✅ Health Check completo!"
```

### 4.2. Performance Audit
```bash
#!/bin/bash
# scripts/performance-audit.sh

echo "🚀 Auditando Performance..."

# Bundle analysis
npm run build:analyze

# Lighthouse CI
npx lhci autorun

# Core Web Vitals check
curl -s "https://pagespeedonline.googleapis.com/pagespeedonline/v5/runPagespeed?url=SEU_SITE&key=SUA_CHAVE" | jq '.loadingExperience.metrics'

echo "📊 Audit completo!"
```

---

## 🐞 DEBUGGING RÁPIDO

### 5.1. Problemas Comuns e Soluções

**Problema: Build falha**
```bash
# Solução:
npm run type-check          # Identifica erros TypeScript
npm run lint -- --fix       # Corrige problemas ESLint
rm -rf .next && npm run build  # Clean build
```

**Problema: CSS não aplicando**
```bash
# Verificar Tailwind:
npx tailwindcss --input ./src/input.css --output ./src/output.css --watch
# Checar content no tailwind.config.js
```

**Problema: Data Connect não funciona**
```bash
# Solução:
firebase data-connect:deploy
firebase data-connect:generate
# Verificar connector.yaml e schema.graphql
```

### 5.2. Logs e Monitoramento
```typescript
// lib/utils/debug.ts
export const debug = {
  // Log estruturado para desenvolvimento
  log: (component: string, action: string, data?: any) => {
    if (process.env.NODE_ENV === 'development') {
      console.log(`[${component}] ${action}`, data)
    }
  },
  
  // Performance monitoring
  perf: async (name: string, fn: () => Promise<any>) => {
    const start = performance.now()
    const result = await fn()
    const end = performance.now()
    console.log(`⏱️ ${name}: ${(end - start).toFixed(2)}ms`)
    return result
  }
}
```

---

## 🛡️ TESTES DE SEGURANÇA

### 6.1. Checklist de Segurança
```bash
#!/bin/bash
# scripts/security-audit.sh

echo "🛡️ Iniciando Auditoria de Segurança..."

# 1. Dependencies
echo "📦 Verificando dependências..."
npm audit
npx snyk test

# 2. Environment variables
echo "🔐 Verificando variáveis de ambiente..."
[ -z "$NEXT_PUBLIC_FIREBASE_API_KEY" ] && echo "❌ Firebase API Key exposta!"

# 3. Firestore rules
echo "🔥 Testando regras Firestore..."
firebase deploy --only firestore:rules --dry-run

# 4. API security
echo "🌐 Testando endpoints..."
npx playwright test tests/security/ --headed

echo "✅ Auditoria de segurança completa!"
```

### 6.2. Testes Automatizados de Segurança
```typescript
// tests/security/api-security.test.ts
import { test, expect } from '@playwright/test'

test('API rate limiting', async ({ request }) => {
  const promises = Array.from({ length: 110 }, () =>
    request.get('/api/projects')
  )
  
  const responses = await Promise.all(promises)
  const rateLimited = responses.filter(r => r.status() === 429)
  
  expect(rateLimited.length).toBeGreaterThan(0)
})

test('Authentication required', async ({ request }) => {
  const response = await request.get('/api/user-data')
  expect(response.status()).toBe(401)
})
```

---

## 📊 ANÁLISE DE PERFORMANCE

### 7.1. Métricas para Monitorar
```typescript
// lib/utils/performance.ts
export const performanceMetrics = {
  // Core Web Vitals
  trackLCP: () => {
    new PerformanceObserver((entryList) => {
      const entries = entryList.getEntries()
      const lastEntry = entries[entries.length - 1]
      console.log('LCP:', lastEntry.renderTime || lastEntry.loadTime)
    }).observe({ type: 'largest-contentful-paint', buffered: true })
  },

  trackFID: () => {
    new PerformanceObserver((entryList) => {
      const entries = entryList.getEntries()
      entries.forEach(entry => {
        console.log('FID:', entry.processingStart - entry.startTime)
      })
    }).observe({ type: 'first-input', buffered: true })
  }
}
```

### 7.2. Scripts de Otimização
```bash
#!/bin/bash
# scripts/optimize.sh

echo "⚡ Otimizando aplicação..."

# Bundle analysis
ANALYZE=true npm run build

# Image optimization
npx next-image-optimization

# Cache optimization
npx workbox-build

echo "🎯 Otimização completa!"
```

---

## 🚀 DEPLOY E MONITORAMENTO

### 8.1. Pipeline de Deploy
```yaml
# .github/workflows/deploy.yml
name: Deploy
on: [push]

jobs:
  quality-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - run: npm ci
      - run: npm run quality:fast
      - run: npm run security:audit

  deploy:
    needs: quality-check
    runs-on: ubuntu-latest
    steps:
      - run: firebase deploy --only hosting,data-connect
      - run: cd workers/api-gateway && npx wrangler deploy
```

### 8.2. Monitoramento Pós-Deploy
```typescript
// app/layout.tsx - Adicionar monitoring
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <script
          dangerouslySetInnerHTML={{
            __html: `
              // Error tracking
              window.addEventListener('error', (e) => {
                fetch('/api/error-log', {
                  method: 'POST',
                  body: JSON.stringify({
                    error: e.error.toString(),
                    stack: e.error.stack,
                    url: window.location.href
                  })
                })
              })
            `
          }}
        />
      </body>
    </html>
  )
}
```

---

## 🎯 CHECKLIST FINAL

### 9.1. Antes do Commit
```bash
# ✅ Sempre executar:
npm run quality:fast
npm run security:audit
git add .
git commit -m "feat: descrição clara"
```

### 9.2. Antes do Deploy
```bash
# ✅ Verificações finais:
npm run build
npm run test:unit
firebase deploy --dry-run
```

### 9.3. Monitoramento Pós-Deploy
- [ ] Verificar logs no Firebase Console
- [ ] Monitorar métricas no Cloudflare
- [ ] Testar funcionalidades críticas
- [ ] Validar performance Core Web Vitals

---

## 📞 SUPORTE RÁPIDO

### Problemas Imediatos:
1. **Build falhando** → `npm run type-check && npm run lint -- --fix`
2. **CSS não carrega** → Verificar `tailwind.config.js` content paths
3. **Data Connect erro** → `firebase data-connect:deploy`
4. **Worker não deploya** → `wrangler deploy --verbose`

### Logs para Verificar:
- Firebase Hosting logs
- Cloudflare Worker logs
- Browser console errors
- Network tab requests

---

**🎉 PRONTO PARA DESENVOLVER!** 

*Documento otimizado para desenvolvimento rápido - Outubro 2025*