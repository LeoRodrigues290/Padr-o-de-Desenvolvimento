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

### 3.3. Classes Tailwind Organizadas
```tsx
// ✅ ESTRUTURA CORRETA DE CLASSES
const Button = () => (
  <button
    className={`
      // Layout
      inline-flex items-center justify-center
      
      // Spacing  
      px-4 py-2
      
      // Typography
      text-sm font-medium
      
      // Colors
      text-white bg-blue-600
      
      // States
      hover:bg-blue-700 focus:outline-none focus:ring-2
      
      // Responsive
      md:px-6 md:text-base
    `}
  >
    Action
  </button>
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

### 5.2. Hooks para Data Connect
```tsx
'use client'

// ✅ Hook customizado para Data Connect
export function useProjects() {
  const { data, loading, error } = useQuery(GetProjectsQuery, {
    variables: { status: 'ACTIVE' }
  })

  const [createProject] = useMutation(CreateProjectMutation)

  return {
    projects: data?.getProjects || [],
    loading,
    createProject
  }
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

---

## 🧩 PADRÕES DE COMPONENTES

### 7.1. Componente Base
```tsx
// ✅ Componente bem estruturado
interface ButtonProps {
  variant?: 'primary' | 'secondary'
  size?: 'sm' | 'md' | 'lg'
  children: React.ReactNode
  onClick?: () => void
}

export function Button({ 
  variant = 'primary',
  size = 'md',
  children,
  onClick 
}: ButtonProps) {
  const baseStyles = 'inline-flex items-center justify-center font-medium'
  
  const variants = {
    primary: 'bg-blue-600 text-white hover:bg-blue-700',
    secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300'
  }
  
  const sizes = {
    sm: 'px-3 py-1.5 text-sm',
    md: 'px-4 py-2 text-base',
    lg: 'px-6 py-3 text-lg'
  }

  return (
    <button
      className={`
        ${baseStyles}
        ${variants[variant]}
        ${sizes[size]}
        rounded-lg transition-colors
        focus:outline-none focus:ring-2 focus:ring-blue-500
      `}
      onClick={onClick}
    >
      {children}
    </button>
  )
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