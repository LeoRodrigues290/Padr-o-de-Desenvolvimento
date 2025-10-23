# 🧠 Diretrizes IA - Firebase Studio
## Guia de Desenvolvimento para Inteligência Artificial

**Versão:** 1.0 - Especificação para Gemini 2.5 e Built-in Models  
**Propósito:** Diretrizes técnicas para geração de código de alta qualidade no Firebase Studio

---

## 🎯 OBJETIVOS PRIMÁRIOS DA IA

### 1.1. Princípios Fundamentais
```typescript
// SEMPRE SEGUIR ESTES PRINCÍPIOS:
interface IADirectives {
  codeQuality: "HIGH";
  maintainability: "MAXIMUM";
  performance: "OPTIMIZED";
  structure: "MODULAR";
  styling: "RESPONSIVE";
}
```

### 1.2. Comportamento Esperado
- **CRIAR** componentes modulares (< 150 linhas)
- **EVITAR** posicionamento absoluto para layouts
- **PREFERIR** Flexbox/Grid para estruturação
- **VALIDAR** responsividade em todos os breakpoints
- **MODULARIZAR** código em arquivos especializados

### 1.3. Renderização, Mídia e Loading States
- **Server Components por padrão:** execute data fetching intensivo, autenticação e composição de layout em componentes Server. Migre para Client Components apenas quando precisar de interatividade (eventos, estado local complexo ou APIs do navegador).
- **Client Components com parcimônia:** use `'use client'` somente em árvores que realmente dependam de hooks de efeito, estado ou bibliotecas third-party que exigem o navegador.
- **Imagens otimizadas com `next/image`:** habilite `fill` ou `sizes` adequados, defina `priority` para herói acima da dobra, utilize `placeholder="blur"` para imagens críticas e hospede assets preferencialmente em domínios confiáveis registrados em `next.config.js`.
- **Loading states consistentes:** padronize skeletons e suspense boundaries para evitar layout shifts.

```typescript
interface LoadingStates {
  skeleton: "PREFER over spinners";
  suspense: "USE for data fetching";
  streaming: "ENABLE for large datasets";
}
```

---

## 🏗️ ARQUITETURA E ESTRUTURA

### 2.1. Padrão de Estrutura de Pastas
```
app/
├── (route-groups)/
│   ├── component-specific/
│   │   └── components/
├── layout.tsx                  // MAX: 100 linhas
├── page.tsx                    // MAX: 150 linhas
└── loading.tsx                 // MAX: 50 linhas

components/
├── ui/                         // Componentes base
├── layout/                     // Componentes estruturais  
└── shared/                     // Componentes reutilizáveis
```

### 2.2. Regras de Modularização
```typescript
// ✅ CORRETO - Componente modular
interface ComponentRules {
  maxLines: 150;
  singleResponsibility: true;
  propTypes: "TypeScript";
  dependencyCount: "MINIMAL";
}

// ❌ EVITAR - Componente monolítico
interface AntiPattern {
  lines: ">500",
  responsibilities: "MULTIPLE",
  styling: "INLINE_COMPLEX"
}
```

---

## 🎨 ESTILOS E LAYOUT

### 3.1. Diretrizes de Posicionamento

**🚫 PROIBIDO - Uso indiscriminado de absolute:**
```tsx
// ❌ NUNCA FAZER ISSO:
<div className="absolute top-0 left-0 right-0 bottom-0">
  <div className="absolute top-10 left-5">...</div>
  <div className="absolute top-20 left-10">...</div>
</div>
```

**✅ PREFERIR - Layouts flexíveis:**
```tsx
// ✅ SEMPRE PREFERIR:
<div className="flex flex-col space-y-4">
  <div className="flex items-center justify-between">
    <div>Header</div>
    <div>Actions</div>
  </div>
  <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
    <div>Content</div>
    <div>Sidebar</div>
  </div>
</div>
```

### 3.2. Sistema de Layout Responsivo

```tsx
// ✅ PADRÃO RESPONSIVO OBRIGATÓRIO
const ResponsiveLayout = () => (
  <div className="container mx-auto px-4">
    {/* Mobile First */}
    <div className="flex flex-col space-y-4">
      
      {/* Tablet */}
      <div className="md:flex md:space-x-4 md:space-y-0">
        <div className="md:flex-1">Main</div>
        <div className="md:w-64">Sidebar</div>
      </div>
      
      {/* Desktop */}
      <div className="lg:grid lg:grid-cols-3 lg:gap-6">
        <div className="lg:col-span-2">Content</div>
        <div>Panel</div>
      </div>
    </div>
  </div>
);
```

```tsx
// ✅ GRID COMPLEXO E RESPONSIVO
const ResponsiveGrid = () => (
  <div
    className="grid grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-6 2xl:grid-cols-8"
  >
    {/* Items dinâmicos */}
  </div>
);
```

```tsx
// ✅ GRID COM ÁREAS NOMEADAS (CSS customizado)
const NamedAreasLayout = () => (
  <div className="grid-layout">
    <header className="area-header">Header</header>
    <nav className="area-nav">Nav</nav>
    <main className="area-main">Main</main>
    <aside className="area-aside">Aside</aside>
    <footer className="area-footer">Footer</footer>
  </div>
);
```

```css
.grid-layout {
  display: grid;
  gap: 1rem;
  grid-template-areas:
    'header'
    'nav'
    'main'
    'aside'
    'footer';
}

@media (min-width: 768px) {
  .grid-layout {
    grid-template-columns: 240px 1fr;
    grid-template-areas:
      'header header'
      'nav main'
      'aside main'
      'footer footer';
  }
}

@media (min-width: 1280px) {
  .grid-layout {
    grid-template-columns: 240px 1fr 320px;
    grid-template-areas:
      'header header header'
      'nav main aside'
      'footer footer footer';
  }
}
```

### 3.3. Classes Tailwind Organizadas
```tsx
// ✅ VARIANTES COM `cva`
import { cva } from 'class-variance-authority';

export const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-lg font-medium transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500',
  {
    variants: {
      variant: {
        primary: 'bg-blue-600 text-white hover:bg-blue-700',
        secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300',
        ghost: 'hover:bg-gray-100 text-gray-900'
      },
      size: {
        sm: 'px-3 py-1.5 text-sm',
        md: 'px-4 py-2 text-base',
        lg: 'px-6 py-3 text-lg'
      }
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md'
    }
  }
);
```

---

## ⚛️ PADRÕES REACT/NEXT.JS

### 4.1. Server vs Client Components

**✅ SERVER COMPONENTS (Padrão):**
```tsx
// app/page.tsx - SERVER COMPONENT
import { getData } from '@/lib/actions'

export default async function Page() {
  const data = await getData() // ✅ Direto no servidor
  
  return (
    <div>
      <StaticContent data={data} />
      <InteractiveComponent />
    </div>
  )
}
```

**✅ CLIENT COMPONENTS (Apenas quando necessário):**
```tsx
'use client' // ✅ Apenas quando precisa de interatividade

import { useState, useEffect } from 'react'

export function InteractiveComponent() {
  const [state, setState] = useState()
  
  return <div>Interactive</div>
}
```

### 4.2. Gestão de Estado
```tsx
'use client'

// ✅ Estado local simples
export function FormComponent() {
  const [formData, setFormData] = useState({
    name: '',
    email: ''
  })
  
  // ✅ Custom Hook para lógica complexa
  const { data, loading } = useCustomHook()
  
  return <form>...</form>
}
```

### 4.3. Composition Pattern
```tsx
// ✅ COMPONENTES COMPOSTOS
interface CardProps {
  children: React.ReactNode;
}

export function Card({ children }: CardProps) {
  return <div className="card">{children}</div>;
}

Card.Header = function CardHeader({ children }: CardProps) {
  return <div className="card-header">{children}</div>;
};

Card.Body = function CardBody({ children }: CardProps) {
  return <div className="card-body">{children}</div>;
};

// Uso
<Card>
  <Card.Header>Title</Card.Header>
  <Card.Body>Content</Card.Body>
</Card>;
```

---

## 🔥 FIREBASE DATA CONNECT

### 5.1. Padrão de Queries GraphQL
```graphql
# ✅ Schema bem estruturado
type Project {
  id: ID!
  name: String!
  description: String
  status: ProjectStatus!
  createdAt: DateTime!
}

type Query {
  getProjects(
    status: ProjectStatus
    limit: Int = 10
  ): [Project!]!
}

input CreateProjectInput {
  name: String!
  description: String
}
```

### 5.2. Hooks com React Query
```tsx
'use client'

import { useQuery } from '@tanstack/react-query';
import { executeQuery } from '@firebase/data-connect';

// ✅ Hook customizado para Data Connect
export function useProjects(status?: ProjectStatus) {
  return useQuery({
    queryKey: ['projects', status],
    queryFn: async () => {
      const result = await executeQuery(dataConnect, {
        query: GET_PROJECTS,
        variables: { status }
      });

      return result.data.projects;
    },
    staleTime: 5 * 60 * 1000
  });
}
```

### 5.3. Mutations com Invalidação Inteligente
```tsx
'use client'

import { useMutation, useQueryClient } from '@tanstack/react-query';
import { executeMutation } from '@firebase/data-connect';

export function useCreateProject() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async (input: CreateProjectInput) => {
      const result = await executeMutation(dataConnect, {
        mutation: CREATE_PROJECT,
        variables: { input }
      });

      return result.data.createProject;
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['projects'] });
    }
  });
}
```

---

## 📱 RESPONSIVIDADE E ACESSIBILIDADE

### 6.1. Breakpoints Obrigatórios
```tsx
// ✅ MOBILE FIRST - Sempre começar do mobile
const ResponsiveComponent = () => (
  <div>
    {/* Mobile */}
    <div className="flex flex-col space-y-4 sm:space-y-6">
      
      {/* Tablet */}
      <div className="md:flex md:space-x-6">
        <div className="md:flex-1">Content</div>
      </div>
      
      {/* Desktop */}
      <div className="lg:grid lg:grid-cols-4 xl:gap-8">
        <div className="lg:col-span-3">Main</div>
        <div className="lg:col-span-1">Side</div>
      </div>
    </div>
  </div>
)
```

### 6.2. Acessibilidade
```tsx
// ✅ SEMPRE INCLUIR ACESSIBILIDADE
const AccessibleComponent = () => (
  <>
    {/* Labels semânticos */}
    <label htmlFor="search" className="sr-only">
      Buscar
    </label>
    
    {/* ARIA attributes */}
    <button
      aria-label="Fechar menu"
      aria-expanded={isOpen}
    >
      <CloseIcon />
    </button>
    
    {/* Focus management */}
    <div className="focus:ring-2 focus:ring-blue-500">
      Conteúdo
    </div>
  </>
)
```

### 6.3. Container Queries
```tsx
// ✅ ADAPTAR CONTEÚDO AO CONTÊINER
const ContainerAwareGrid = () => (
  <div className="@container">
    <div className="grid grid-cols-1 gap-4 @sm:grid-cols-2 @md:grid-cols-3 @lg:grid-cols-4">
      {/* Items dinâmicos */}
    </div>
  </div>
);
```

---

## 🧩 PADRÕES DE COMPONENTES

### 7.1. Componente Base
```tsx
// ✅ Componente bem estruturado com loading e ícones
import { buttonVariants } from '@/components/ui/button-variants';

interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  loading?: boolean;
  disabled?: boolean;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
  children: React.ReactNode;
  onClick?: () => void;
}

export function Button({
  variant = 'primary',
  size = 'md',
  loading,
  disabled,
  leftIcon,
  rightIcon,
  children,
  ...rest
}: ButtonProps) {
  return (
    <button
      {...rest}
      disabled={disabled || loading}
      className={buttonVariants({ variant, size })}
    >
      {loading ? <Spinner className="mr-2 h-4 w-4" /> : leftIcon}
      <span className="mx-1">{children}</span>
      {!loading && rightIcon}
    </button>
  );
}
```

### 7.2. Componente de Layout
```tsx
// ✅ Layout responsivo e limpo
interface CardProps {
  title: string
  children: React.ReactNode
  actions?: React.ReactNode
}

export function Card({ title, children, actions }: CardProps) {
  return (
    <div className="bg-white rounded-lg shadow-sm border border-gray-200">
      {/* Header */}
      <div className="px-4 py-5 sm:px-6 border-b border-gray-200">
        <div className="flex items-center justify-between">
          <h3 className="text-lg font-semibold text-gray-900">
            {title}
          </h3>
          {actions && (
            <div className="flex space-x-2">
              {actions}
            </div>
          )}
        </div>
      </div>
      
      {/* Content */}
      <div className="px-4 py-5 sm:p-6">
        {children}
      </div>
    </div>
  )
}
```

---

## 🚫 ANTI-PATTERNS PROIBIDOS

### 8.1. Estilos Problemáticos
```tsx
// ❌ PROIBIDO - Posicionamento absoluto complexo
<div className="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2">
  <div className="absolute top-10 left-5">...</div>
</div>

// ❌ PROIBIDO - Muitas classes inline complexas
<div className="flex justify-between items-center p-4 bg-white rounded-lg shadow-md border border-gray-200 hover:shadow-lg transition-shadow duration-200">
  ...
</div>

// ❌ PROIBIDO - Componentes muito grandes
// Arquivos com mais de 200 linhas devem ser divididos
```

### 8.2. Estruturas Complexas
```tsx
// ❌ PROIBIDO - Aninhamento profundo
<div>
  <div>
    <div>
      <div> {/* MUITO ANINHADO */}
        ...
      </div>
    </div>
  </div>
</div>

// ✅ PREFERIR - Estrutura plana
<div className="space-y-4">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>
```

### 8.3. Fluxos de Estado Problemáticos
```tsx
// ❌ PROIBIDO - Prop drilling excessivo
function Parent() {
  const [state, setState] = useState();
  return <Child state={state} setState={setState} />;
}

function Child({ state, setState }) {
  return <GrandChild state={state} setState={setState} />;
}

// ✅ PREFERIR - Context ou Store
const StateContext = createContext();

function Parent() {
  const [state, setState] = useState();
  return (
    <StateContext.Provider value={{ state, setState }}>
      <Child />
    </StateContext.Provider>
  );
}

// ❌ PROIBIDO - useEffect desnecessário
useEffect(() => {
  setFilteredData(data.filter((item) => item.active));
}, [data]);

// ✅ PREFERIR - Computação direta
const filteredData = useMemo(
  () => data.filter((item) => item.active),
  [data]
);
```

---

## ✅ CHECKLIST DE VALIDAÇÃO

### 9.1. Antes de Gerar Código
- [ ] **Modularidade**: Componente tem < 150 linhas?
- [ ] **Responsividade**: Testado em mobile/tablet/desktop?
- [ ] **Posicionamento**: Usando Flexbox/Grid em vez de absolute?
- [ ] **Acessibilidade**: Incluído ARIA labels e focus management?
- [ ] **Performance**: Evitado re-renders desnecessários?
- [ ] **Tipagem**: TypeScript interfaces definidas?
- [ ] **Manutenibilidade**: Código é fácil de entender?
- [ ] **Imagens**: Componentes críticos usam `next/image` com otimização?
- [ ] **Fonts**: `next/font` configurado para reduzir layout shift?
- [ ] **SEO**: Meta tags, Open Graph e dados estruturados revisados?
- [ ] **Analytics**: Eventos essenciais registrados no Logger/Analytics?
- [ ] **Error/Suspense Boundaries**: Presentes em componentes críticos?

### 9.2. Padrões de Qualidade
```typescript
interface QualityChecklist {
  structure: {
    modular: boolean;
    singleResponsibility: boolean;
    maxLines: number; // <= 150
  };
  styling: {
    responsive: boolean;
    noAbsoluteLayout: boolean;
    tailwindOrganized: boolean;
  };
  accessibility: {
    semanticHTML: boolean;
    ariaLabels: boolean;
    keyboardNavigation: boolean;
  };
  performance: {
    noLargeBundles: boolean;
    optimizedRenders: boolean;
    cleanImports: boolean;
  };
}
```

---

## 🔧 Troubleshooting Expandido

| Problema | Sinais | Solução Recomendada |
|----------|--------|---------------------|
| **Data Connect timeout** | Queries acima de 10s, respostas vazias | Ative `streaming` no React Server Component, reduza payload com `limit`, utilize `executeQuery` com `signal` abortable e monitore métricas no Firebase Studio. |
| **Cache invalidation não funciona** | Dados obsoletos após deploy | Garanta chaves previsíveis, use `queryClient.invalidateQueries` após mutações e configure `CacheService.setMany` com tags coerentes. |
| **Rate limiting agressivo** | Usuários legítimos bloqueados | Ajuste o `TokenBucket` para limites por rota, adicione whitelists para tarefas internas e registre métricas de consumo. |
| **Build falha no Firebase Hosting** | `npm run build` quebra no deploy | Certifique `firebase experiments:enable webframeworks`, valide `next.config.js` e limpe `node_modules` com `firebase hosting:channel:deploy --only hosting`. |
| **CORS no Cloudflare Worker** | Requisições 403/blocked | Revise lista de origens, ative `OPTIONS` preflight e aplique headers dinâmicos conforme ambiente. |

## 🔄 Guias de Migração

- **De Vercel para Firebase Studio:** configure `firebase init hosting`, habilite `webframeworks`, atualize DNS e mova secrets para `firebase env`.
- **De REST para GraphQL (Data Connect):** converta endpoints em resolvers, gere tipos com `__generated__/graphql.ts` e substitua `fetch` por `executeQuery`/`executeMutation`.
- **De Redux para Zustand:** identifique slices críticos, migre para stores com middlewares (`devtools`, `persist`) e use selectors memorizados.
- **De styled-components para Tailwind:** traduza tokens de design para classes utilitárias, aplique `cva`/`clsx` para variantes e remova estilos globais não utilizados.

## 📚 Glossário Essencial

- **Data Connect:** camada GraphQL do Firebase com tipagem e execução serverless gerenciada.
- **Durable Objects:** instâncias consistentes na edge Cloudflare para coordenação de estado.
- **KV Namespace:** armazenamento chave-valor distribuído utilizado para cache e configuração.
- **Edge Computing:** execução de código em regiões próximas ao usuário para reduzir latência.
- **Hydration:** processo de anexar handlers client-side a markup gerada no servidor.

## 💰 Estimativas de Custos

| Serviço | Faixa Gratuita | Custo Estimado Mensal* |
|---------|----------------|------------------------|
| Firebase Firestore | 1 GiB storage / 50K reads | USD 25-80 (workloads médias) |
| Firebase Hosting | 10 GB transferência | USD 0-20 conforme tráfego |
| Firebase Auth | 10K verificações | USD 0-15 dependendo de MFA |
| Cloudflare Workers | 100K execuções | USD 5-50 (plan Worker Paid) |
| Cloudflare KV | 1 GB storage | USD 5-30 conforme operações |

> *Valores aproximados, monitorar via dashboards oficiais.

## 🛣️ Roadmap de Evolução

1. **Suporte a i18n** com `next-intl` integrado ao Firebase Studio.
2. **PWA capabilities** incluindo service worker customizado e caching offline.
3. **Offline-first** para Firestore com sincronização incremental.
4. **Real-time collaboration** com Presence e CRDTs hospedados em Durable Objects.

## 🎯 Prioridades de Implementação

### Alta Prioridade
- Corrigir exemplos de Data Connect com React Query e invalidation.
- Reforçar segurança JWT no Worker com verificação dedicada.
- Adicionar error boundaries e loading states consistentes.
- Implementar logging estruturado com sampling.
- Configurar testes integrados utilizando Firebase Emulators.

### Média Prioridade
- Adicionar container queries em layouts críticos.
- Implementar cache `getCachedWithSWR` para dados estratégicos.
- Aperfeiçoar rate limiting com Token Bucket configurável.
- Rodar bundle analysis no CI/CD (`@next/bundle-analyzer`).
- Criar componentes reutilizáveis adicionais (e.g. `AnimatedLayout`).

### Baixa Prioridade
- Documentar APIs públicas e contratos GraphQL.
- Expandir exemplos avançados para charts/visualizações.
- Automatizar governança de custos com alertas.

## 🎯 RESUMO DAS DIRETRIZES

### 10.1. REGRAS DE OURO
1. **MOBILE FIRST** - Sempre começar do mobile
2. **MODULARIDADE** - Componentes pequenos e especializados  
3. **FLEXBOX/GRID** - Evitar position: absolute para layouts
4. **RESPONSIVIDADE** - Testar todos os breakpoints
5. **ACESSIBILIDADE** - Incluir semântica e ARIA
6. **PERFORMANCE** - Otimizar re-renders e bundles
7. **MANUTENIBILIDADE** - Código limpo e compreensível

### 10.2. MÉTRICAS DE QUALIDADE
```typescript
const QualityMetrics = {
  componentSize: "MAX 150 lines",
  complexity: "LOW cognitive load", 
  styling: "RESPONSIVE and clean",
  performance: "FAST rendering",
  accessibility: "FULL compliance"
}
```

---

**ESTE DOCUMENTO DEVE SER CONSULTADO EM TODAS AS GERAÇÕES DE CÓDIGO NO FIREBASE STUDIO**

*Última atualização: Outubro 2025*  
*Compatível com: Gemini 2.5, Built-in Models, Firebase Studio IDE*