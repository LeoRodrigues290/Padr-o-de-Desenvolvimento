# 🚀 Guia Definitivo: Desenvolvimento Enterprise
## Firebase Studio + Next.js + Cloudflare Workers  
**Versão: 3.0 - Outubro 2025**

**Objetivo:** Construir sistemas robustos, performáticos e seguros com redução de até 80% em leituras/escritas do Firebase usando Firebase Studio como ambiente de desenvolvimento principal

---

## 📚 Índice

1. [🗺️ Visão Geral da Arquitetura](#1-visão-geral)
2. [🚀 Stack Tecnológica Padrão](#2-stack)
3. [⚙️ Setup Inicial no Firebase Studio](#3-setup)
4. [📁 Estrutura de Pastas Otimizada](#4-estrutura)
5. [⚛️ Padrões de Código Next.js + Firebase](#5-nextjs)
6. [✨ Animações e UX com Framer Motion](#6-framer-motion)
7. [☁️ Arquitetura Edge (Cloudflare)](#7-cloudflare)
8. [⚡ Configuração de Cache e Durable Objects](#8-cache-durable-objects)
9. [🔥 Integração Firebase & Data Connect](#9-firebase)
10. [🔒 Segurança Avançada](#10-segurança)
11. [🧾 Validação e Formulários](#11-formularios)
12. [⚛️ Gerenciamento de Estado Global](#12-estado-global)
13. [🧪 Testes Automatizados](#13-testes)
14. [📊 Logging e Monitoramento](#14-logging)
15. [⚙️ Variáveis de Ambiente](#15-variaveis-ambiente)
16. [🚀 CI/CD & Deploy Firebase](#16-ci-cd)
17. [💅 Padronização de Código](#17-padronizacao)
18. [🛡️ Ferramentas de Segurança](#18-seguranca)
19. [⚡ Performance & Caching](#19-performance)
20. [📈 Auditoria e Observabilidade](#20-auditoria)
21. [🧾 Checklist Diário de Desenvolvimento](#21-checklist)
22. [🐞 Troubleshooting & Debugging](#22-troubleshooting)

---

## 1. 🗺️ Visão Geral da Arquitetura {#1-visão-geral}

### 1.1. Arquitetura Híbrida: Firebase Studio + Cloudflare

```mermaid
graph TB
    User[👤 Usuário] --> CF[🌐 Cloudflare Worker<br/>Edge Layer]
    
    CF --> Cache{Cache HIT?}
    Cache -->|SIM| Response[⚡ Resposta Rápida<br/>~10-50ms]
    Cache -->|NÃO| Firebase[🔥 Firebase Studio<br/>Core Layer]
    
    Firebase --> DataConnect[Data Connect GraphQL]
    Firebase --> Auth[Authentication]
    Firebase --> Firestore[Firestore Database]
    Firebase --> Hosting[App Hosting]
    
    CF --> RateLimit[Rate Limiting]
    CF --> JWT[JWT Validation]
    CF --> Transform[Data Transform]
    
    Studio[🛠️ Firebase Studio IDE] --> Firebase
    Studio --> CF
    
    style Firebase fill:#ff9800,stroke:#333
    style CF fill:#f96,stroke:#333
    style Cache fill:#4caf50,stroke:#333
    style Studio fill:#4285f4,stroke:#333
```

### 1.2. Princípios Fundamentais

- **Firebase Studio:** Ambiente de desenvolvimento unificado com IDE integrado
- **Data Connect:** Camada GraphQL tipada para operações de dados
- **Cloudflare Workers:** Gateway inteligente com cache e segurança
- **Firebase Hosting:** Deploy automatizado com CDN global
- **Redução de Custos:** 70-80% menos leituras via cache KV

### 1.3. Fluxo de Desenvolvimento no Firebase Studio

```typescript
// 1. Desenvolvimento no Firebase Studio IDE
Firebase Studio (VSCode integrado)
  ↓ [Desenvolvimento com IA assistente]
  ↓ [Prototipagem com Gemini]
  
// 2. Build e Deploy
Firebase CLI → Build Next.js
  ↓
Firebase App Hosting (Deploy automático)
  ↓
Cloudflare Workers (Configuração automática)

// 3. Runtime
Usuário → Cloudflare (Cache/Segurança) → Firebase Services
```

---

## 2. 🚀 Stack Tecnológica Padrão {#2-stack}

### 2.1. Dependências Core (Otimizadas para Firebase Studio)

```json
{
  "dependencies": {
    "next": "15.0.0",
    "react": "19.0.0",
    "typescript": "5.5.0",
    
    "firebase": "11.0.0",
    "firebase-admin": "12.0.0",
    "@firebase/data-connect": "latest",
    
    "@tanstack/react-query": "5.40.0",
    "zustand": "4.5.0",
    
    "react-hook-form": "7.51.0",
    "@hookform/resolvers": "3.3.4",
    "zod": "3.23.0",
    
    "framer-motion": "11.0.0",
    
    "graphql": "16.8.0",
    "@apollo/client": "3.8.0",
    
    "@radix-ui/react-*": "latest",
    "tailwindcss": "3.4.0",
    "class-variance-authority": "0.7.0",
    "clsx": "2.1.0"
  },
  "devDependencies": {
    "vitest": "1.6.0",
    "@testing-library/react": "15.0.0",
    "firebase-tools": "13.0.0",
    
    "husky": "9.0.0",
    "lint-staged": "15.2.0",
    "@typescript-eslint/eslint-plugin": "7.0.0",
    "@typescript-eslint/parser": "7.0.0",
    
    "prettier": "3.0.0",
    "stylelint": "16.0.0",
    "stylelint-config-standard": "36.0.0",
    
    "wrangler": "3.57.0"
  }
}
```

### 2.2. Justificativa das Bibliotecas Principais

| Biblioteca | Por Que Usar | Integração Firebase Studio |
|------------|--------------|----------------------------|
| Firebase Data Connect | GraphQL tipado nativo | Integração direta no IDE |
| TanStack Query | Cache client-side inteligente | Complementa cache do Workers |
| Zustand | Estado global leve | Funciona com atualizações em tempo real |
| React Hook Form + Zod | Forms performáticos | Validação unificada client/server |
| Framer Motion | Animações fluidas | Zero conflito com arquitetura |
| Apollo Client | GraphQL client | Otimizado para Data Connect |

---

## 3. ⚙️ Setup Inicial no Firebase Studio {#3-setup}

### 3.1. Criação do Projeto no Firebase Studio

```bash
# 1. Acessar Firebase Studio
# https://firebase.studio/

# 2. Criar novo projeto
firebase studio:create meu-projeto-enterprise

# 3. Configurar Data Connect
firebase data-connect:init
? Database type: PostgreSQL
? Schema name: enterprise_app

# 4. Inicializar Next.js com templates do Studio
firebase studio:template nextjs-enterprise
```

### 3.2. Estrutura Inicial Gerada pelo Firebase Studio

```
meu-projeto/
├── firebase/
│   ├── data-connect/
│   │   ├── connector/
│   │   │   ├── connector.yaml
│   │   │   └── schema.graphql
│   │   └── generated/
│   ├── firestore.rules
│   ├── firestore.indexes.json
│   └── firebase.json
├── app/                    # Next.js App Router
├── components/             # Componentes compartilhados
├── lib/                    # Configurações e utilitários
└── workers/                # Cloudflare Workers
```

### 3.3. Configuração do Firebase Studio Workspace

**.firebaserc**
```json
{
  "projects": {
    "default": "meu-projeto-enterprise"
  },
  "targets": {
    "meu-projeto-enterprise": {
      "hosting": {
        "app": ["meu-projeto-enterprise"]
      },
      "data-connect": {
        "connectorId": "enterprise-connector"
      }
    }
  }
}
```

**firebase.json**
```json
{
  "hosting": {
    "source": ".",
    "ignore": ["firebase-debug.log", "firestore-debug.log"],
    "frameworksBackend": {
      "region": "us-central1"
    }
  },
  "dataConnect": {
    "source": "firebase/data-connect",
    "connectorId": "enterprise-connector",
    "location": "us-central1"
  },
  "firestore": {
    "rules": "firebase/firestore.rules",
    "indexes": "firebase/firestore.indexes.json"
  }
}
```

### 3.4. Configuração Cloudflare Worker

**workers/api-gateway/wrangler.toml**
```toml
name = "api-gateway"
main = "src/index.ts"
compatibility_date = "2024-01-01"

[[kv_namespaces]]
binding = "CACHE_KV"
id = "cache-namespace-id"

[durable_objects.bindings]
name = "RATE_LIMITER"
class_name = "RateLimiter"

[vars]
FIREBASE_PROJECT_ID = "meu-projeto-enterprise"
DATA_CONNECT_ENDPOINT = "https://dataconnect.firebase.com/your-project"
```

---

## 4. 📁 Estrutura de Pastas Otimizada {#4-estrutura}

```
meu-projeto-enterprise/
├── app/                          # Next.js App Router
│   ├── (auth)/
│   │   ├── login/
│   │   │   └── page.tsx
│   │   └── layout.tsx
│   ├── (dashboard)/
│   │   ├── projects/
│   │   │   ├── page.tsx
│   │   │   ├── [id]/
│   │   │   │   └── page.tsx
│   │   │   └── components/
│   │   │       ├── ProjectCard.tsx
│   │   │       └── ProjectForm.tsx
│   │   └── layout.tsx
│   ├── api/                      # API Routes para webhooks
│   │   └── webhooks/
│   │       └── route.ts
│   └── layout.tsx
│
├── components/
│   ├── ui/                       # ShadCN UI components
│   ├── layout/
│   └── shared/
│
├── lib/
│   ├── firebase/
│   │   ├── config.ts             # Configuração cliente
│   │   ├── data-connect.ts       # Cliente GraphQL
│   │   └── admin.ts              # Admin SDK (server)
│   ├── schemas/                  # Zod schemas
│   │   ├── project.ts
│   │   ├── user.ts
│   │   └── data-connect.ts       # Tipos do Data Connect
│   └── utils/
│       ├── cn.ts
│       └── format.ts
│
├── hooks/                        # Custom hooks
│   ├── useAuth.ts
│   ├── useDataConnect.ts         # Hook para GraphQL
│   └── useProjects.ts
│
├── types/
│   ├── models/
│   │   ├── User.ts
│   │   └── Project.ts
│   └── graphql.ts                # Tipos GraphQL gerados
│
├── firebase/                     # Configurações Firebase
│   ├── data-connect/
│   │   ├── connector/
│   │   │   ├── connector.yaml
│   │   │   └── schema.graphql
│   │   └── generated/
│   ├── firestore.rules
│   └── firebase.json
│
├── workers/                      # Cloudflare Workers
│   └── api-gateway/
│       ├── src/
│       │   ├── index.ts
│       │   ├── routes/
│       │   │   ├── projects.ts
│       │   │   └── auth.ts
│       │   ├── middleware/
│       │   │   ├── jwt.ts
│       │   │   └── rateLimit.ts
│       │   └── utils/
│       │       └── cache.ts
│       ├── wrangler.toml
│       └── package.json
│
├── tests/
├── public/
└── package.json
```

### 4.1. Convenções de Nomenclatura no Firebase Studio

| Tipo | Convenção | Exemplo |
|------|-----------|---------|
| Data Connect Entities | PascalCase | Project, User, Client |
| GraphQL Queries | camelCase | getProjects, createUser |
| Firestore Collections | snake_case | user_profiles, project_tasks |
| Components | PascalCase | ProjectCard, UserProfile |

---

## 5. ⚛️ Padrões de Código Next.js + Firebase {#5-nextjs}

### 5.1. Data Fetching com Firebase Data Connect

**Schema GraphQL (firebase/data-connect/connector/schema.graphql)**
```graphql
type Project {
  id: ID!
  name: String!
  description: String
  status: ProjectStatus!
  createdAt: DateTime!
  updatedAt: DateTime!
  createdBy: User!
}

enum ProjectStatus {
  ACTIVE
  COMPLETED
  ARCHIVED
}

type Query {
  getProjects(status: ProjectStatus): [Project!]!
  getProject(id: ID!): Project
}

type Mutation {
  createProject(input: CreateProjectInput!): Project!
  updateProject(id: ID!, input: UpdateProjectInput!): Project!
}

input CreateProjectInput {
  name: String!
  description: String
  status: ProjectStatus = ACTIVE
}
```

### 5.2. Cliente Data Connect

**lib/firebase/data-connect.ts**
```typescript
import { DataConnect } from '@firebase/data-connect';
import { getApp } from 'firebase/app';
import { 
  CreateProjectMutation, 
  GetProjectsQuery,
  Project 
} from '@/types/graphql';

export const dataConnect = new DataConnect(getApp(), {
  connector: 'enterprise-connector',
  location: 'us-central1'
});

// Hook personalizado para Data Connect
export const useDataConnect = () => {
  const { data, loading, error } = dataConnect.useQuery(GetProjectsQuery, {
    variables: { status: 'ACTIVE' }
  });

  const [createProject] = dataConnect.useMutation(CreateProjectMutation);

  return {
    projects: data?.getProjects || [],
    loading,
    error,
    createProject
  };
};
```

### 5.3. Server Components com Data Connect

**app/(dashboard)/projects/page.tsx**
```typescript
import { dataConnect } from '@/lib/firebase/data-connect';
import { GetProjectsQuery } from '@/lib/graphql/queries';
import { ProjectCard } from './components/ProjectCard';

export default async function ProjectsPage() {
  // Server Component busca dados via Data Connect
  const { data } = await dataConnect.execute(GetProjectsQuery, {
    variables: { status: 'ACTIVE' }
  });

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-6">Projetos Ativos</h1>
      <div className="grid gap-4">
        {data?.getProjects.map(project => (
          <ProjectCard key={project.id} project={project} />
        ))}
      </div>
    </div>
  );
}
```

### 5.4. Client Components com Cache Híbrido

**app/(dashboard)/projects/components/ProjectCard.tsx**
```typescript
"use client";

import { useQuery } from '@tanstack/react-query';
import { motion } from 'framer-motion';
import { useDataConnect } from '@/hooks/useDataConnect';

export function ProjectCard({ project }: { project: Project }) {
  const { updateProject } = useDataConnect();

  // Cache client-side para atualizações em tempo real
  const { data: projectData } = useQuery({
    queryKey: ['project', project.id],
    queryFn: () => fetchProjectFromWorker(project.id),
    initialData: project,
    refetchInterval: 30000, // Atualiza a cada 30s
  });

  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      className="p-4 border rounded-lg hover:shadow-lg transition-shadow"
    >
      <h3 className="font-semibold">{projectData.name}</h3>
      <p className="text-gray-600">{projectData.description}</p>
    </motion.div>
  );
}
```

---

## 6. ✨ Animações e UX com Framer Motion {#6-framer-motion}

### 6.1. Componentes de Animação Reutilizáveis

**components/shared/animations/FadeIn.tsx**
```tsx
"use client";

import { motion } from 'framer-motion';
import { ReactNode } from 'react';

interface FadeInProps {
  children: ReactNode;
  delay?: number;
  duration?: number;
}

export const FadeIn = ({ 
  children, 
  delay = 0, 
  duration = 0.3 
}: FadeInProps) => (
  <motion.div
    initial={{ opacity: 0, y: 20 }}
    animate={{ opacity: 1, y: 0 }}
    transition={{ 
      duration, 
      delay,
      ease: "easeOut" 
    }}
  >
    {children}
  </motion.div>
);
```

### 6.2. Animações de Lista Otimizadas

**app/(dashboard)/projects/components/ProjectList.tsx**
```tsx
"use client";

import { motion, AnimatePresence } from 'framer-motion';
import { ProjectCard } from './ProjectCard';
import type { Project } from '@/types/models/Project';

interface ProjectListProps {
  projects: Project[];
}

export function ProjectList({ projects }: ProjectListProps) {
  return (
    <div className="space-y-4">
      <AnimatePresence>
        {projects.map((project, index) => (
          <motion.div
            key={project.id}
            initial={{ opacity: 0, x: -20 }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: 20 }}
            transition={{ 
              duration: 0.3,
              delay: index * 0.1 
            }}
          >
            <ProjectCard project={project} />
          </motion.div>
        ))}
      </AnimatePresence>
    </div>
  );
}
```

---

## 7. ☁️ Arquitetura Edge (Cloudflare) {#7-cloudflare}

### 7.1. Worker Principal com Cache Inteligente

**workers/api-gateway/src/index.ts**
```typescript
import { Hono } from 'hono';
import { cors } from 'hono/cors';
import { verifyJWT } from './middleware/jwt';
import { rateLimit } from './middleware/rateLimit';
import { projectsRoute } from './routes/projects';

type Bindings = {
  CACHE_KV: KVNamespace;
  RATE_LIMITER: DurableObjectNamespace;
  FIREBASE_PROJECT_ID: string;
  DATA_CONNECT_ENDPOINT: string;
};

const app = new Hono<{ Bindings: Bindings }>();

// Middleware global
app.use('*', cors());
app.use('/api/*', verifyJWT);
app.use('/api/*', rateLimit);

// Health check
app.get('/health', (c) => c.json({ status: 'ok', timestamp: Date.now() }));

// Rotas de API
app.route('/api/projects', projectsRoute);

// Fallback para Data Connect
app.all('/data-connect/*', async (c) => {
  return handleDataConnectProxy(c);
});

export default app;
```

### 7.2. Middleware de Autenticação JWT

**workers/api-gateway/src/middleware/jwt.ts**
```typescript
import { jwtVerify } from 'jose';
import type { Context, Next } from 'hono';

export async function verifyJWT(c: Context, next: Next) {
  const authHeader = c.req.header('Authorization');
  
  if (!authHeader?.startsWith('Bearer ')) {
    return c.json({ error: 'Token não fornecido' }, 401);
  }
  
  const token = authHeader.substring(7);
  
  try {
    // Verificar token do Firebase
    const response = await fetch(
      `https://identitytoolkit.googleapis.com/v1/accounts:lookup?key=${c.env.FIREBASE_PROJECT_ID}`,
      {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ idToken: token })
      }
    );
    
    if (!response.ok) {
      throw new Error('Token inválido');
    }
    
    const { users } = await response.json();
    c.set('user', users[0]);
    
    await next();
  } catch (error) {
    return c.json({ error: 'Token inválido' }, 401);
  }
}
```

---

## 8. ⚡ Configuração de Cache e Durable Objects {#8-cache-durable-objects}

### 8.1. Sistema de Cache com KV Namespace

**workers/api-gateway/src/utils/cache.ts**
```typescript
interface CacheOptions {
  ttl?: number; // seconds
  tags?: string[];
}

export class CacheService {
  constructor(private kv: KVNamespace) {}

  async get<T>(key: string): Promise<T | null> {
    const value = await this.kv.get(key, 'json');
    return value as T;
  }

  async set<T>(key: string, value: T, options: CacheOptions = {}): Promise<void> {
    const { ttl = 300, tags = [] } = options;
    
    await this.kv.put(key, JSON.stringify(value), {
      expirationTtl: ttl,
      metadata: { tags, createdAt: Date.now() }
    });
  }

  async invalidate(tag: string): Promise<void> {
    // Implementação de invalidação por tags
    const list = await this.kv.list({ prefix: tag });
    
    for (const key of list.keys) {
      await this.kv.delete(key.name);
    }
  }
}
```

### 8.2. Durable Object para Rate Limiting

**workers/api-gateway/src/durableObjects/RateLimiter.ts**
```typescript
export class RateLimiter {
  private state: DurableObjectState;

  constructor(state: DurableObjectState) {
    this.state = state;
  }

  async fetch(request: Request) {
    const userId = request.headers.get('user-id');
    const now = Date.now();
    const windowMs = 60 * 1000; // 1 minute
    const maxRequests = 100;

    if (!userId) {
      return new Response('Unauthorized', { status: 401 });
    }

    const key = `rate_limit:${userId}`;
    let data = await this.state.storage.get<{ count: number; resetTime: number }>(key);

    if (!data || now > data.resetTime) {
      data = { count: 0, resetTime: now + windowMs };
    }

    if (data.count >= maxRequests) {
      return new Response('Rate limit exceeded', { 
        status: 429,
        headers: { 'Retry-After': Math.ceil((data.resetTime - now) / 1000).toString() }
      });
    }

    data.count++;
    await this.state.storage.put(key, data);

    return new Response(JSON.stringify({ 
      count: data.count, 
      resetTime: data.resetTime 
    }), {
      headers: { 'Content-Type': 'application/json' }
    });
  }
}
```

---

## 9. 🔥 Integração Firebase & Data Connect {#9-firebase}

### 9.1. Configuração do Data Connect

**firebase/data-connect/connector/connector.yaml**
```yaml
connectorId: enterprise-connector
location: us-central1
generate:
  graphql:
    schema: schema.graphql
    output:
      typescript: ../../lib/graphql/generated/
    operations:
      - operation: getProjects
        query: queries/getProjects.graphql
      - operation: createProject  
        mutation: mutations/createProject.graphql
```

### 9.2. Operações GraphQL

**firebase/data-connect/connector/queries/getProjects.graphql**
```graphql
query GetProjects($status: ProjectStatus) {
  getProjects(status: $status) {
    id
    name
    description
    status
    createdAt
    updatedAt
    createdBy {
      id
      name
      email
    }
  }
}
```

### 9.3. Hook Personalizado para Data Connect

**hooks/useDataConnect.ts**
```typescript
import { useQuery, useMutation } from '@tanstack/react-query';
import { dataConnect } from '@/lib/firebase/data-connect';

export const useProjects = (status?: string) => {
  return useQuery({
    queryKey: ['projects', status],
    queryFn: async () => {
      const result = await dataConnect.execute(GetProjectsQuery, {
        variables: { status }
      });
      return result.data?.getProjects || [];
    },
    staleTime: 5 * 60 * 1000, // 5 minutos
  });
};

export const useCreateProject = () => {
  return useMutation({
    mutationFn: async (input: CreateProjectInput) => {
      const result = await dataConnect.execute(CreateProjectMutation, {
        variables: { input }
      });
      return result.data?.createProject;
    },
    onSuccess: () => {
      // Invalidar cache relevante
      queryClient.invalidateQueries({ queryKey: ['projects'] });
    },
  });
};
```

---

## 10. 🔒 Segurança Avançada {#10-segurança}

### 10.1. Regras de Segurança Firestore

**firebase/firestore.rules**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only access their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Projects: team-based access
    match /projects/{projectId} {
      allow read, write: if request.auth != null && 
        exists(/databases/$(database)/documents/projects/$(projectId)/team/$(request.auth.uid));
      
      allow create: if request.auth != null &&
        request.auth.token.firebase.sign_in_provider != 'anonymous';
    }
    
    // Admin access
    match /admin/{document} {
      allow read, write: if request.auth != null && 
        request.auth.token.admin == true;
    }
  }
}
```

### 10.2. Validação de Dados com Zod

**lib/schemas/project.ts**
```typescript
import { z } from 'zod';

export const projectSchema = z.object({
  name: z.string().min(1).max(100),
  description: z.string().max(500).optional(),
  status: z.enum(['ACTIVE', 'COMPLETED', 'ARCHIVED']),
  deadline: z.date().min(new Date()),
  budget: z.number().min(0).optional(),
});

export const createProjectSchema = projectSchema.extend({
  teamMembers: z.array(z.string()).min(1, 'Projeto precisa de pelo menos 1 membro'),
});

export type ProjectFormData = z.infer<typeof createProjectSchema>;
```

---

## 11. 🧾 Validação e Formulários {#11-formularios}

### 11.1. Formulário com React Hook Form + Zod

**app/(dashboard)/projects/components/CreateProjectForm.tsx**
```tsx
"use client";

import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { createProjectSchema, type ProjectFormData } from '@/lib/schemas/project';
import { useCreateProject } from '@/hooks/useDataConnect';

export function CreateProjectForm() {
  const createProject = useCreateProject();
  
  const {
    register,
    handleSubmit,
    formState: { errors },
    reset
  } = useForm<ProjectFormData>({
    resolver: zodResolver(createProjectSchema),
    defaultValues: {
      status: 'ACTIVE'
    }
  });

  const onSubmit = async (data: ProjectFormData) => {
    try {
      await createProject.mutateAsync(data);
      reset();
      // Feedback de sucesso
    } catch (error) {
      // Tratamento de erro
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div>
        <label htmlFor="name" className="block text-sm font-medium">
          Nome do Projeto
        </label>
        <input
          {...register('name')}
          className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2"
        />
        {errors.name && (
          <p className="text-red-500 text-sm mt-1">{errors.name.message}</p>
        )}
      </div>

      <div>
        <label htmlFor="description" className="block text-sm font-medium">
          Descrição
        </label>
        <textarea
          {...register('description')}
          rows={3}
          className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2"
        />
      </div>

      <button
        type="submit"
        disabled={createProject.isPending}
        className="bg-blue-500 text-white px-4 py-2 rounded-md disabled:opacity-50"
      >
        {createProject.isPending ? 'Criando...' : 'Criar Projeto'}
      </button>
    </form>
  );
}
```

---

## 12. ⚛️ Gerenciamento de Estado Global {#12-estado-global}

### 12.1. Store com Zustand

**store/uiStore.ts**
```typescript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface UIStore {
  // Sidebar state
  sidebarOpen: boolean;
  toggleSidebar: () => void;
  
  // Theme
  theme: 'light' | 'dark';
  setTheme: (theme: 'light' | 'dark') => void;
  
  // Notifications
  notifications: Notification[];
  addNotification: (notification: Omit<Notification, 'id' | 'timestamp'>) => void;
  removeNotification: (id: string) => void;
}

export const useUIStore = create<UIStore>()(
  persist(
    (set, get) => ({
      sidebarOpen: true,
      toggleSidebar: () => set({ sidebarOpen: !get().sidebarOpen }),
      
      theme: 'light',
      setTheme: (theme) => set({ theme }),
      
      notifications: [],
      addNotification: (notification) => set((state) => ({
        notifications: [
          ...state.notifications,
          {
            ...notification,
            id: Math.random().toString(36),
            timestamp: Date.now()
          }
        ]
      })),
      removeNotification: (id) => set((state) => ({
        notifications: state.notifications.filter(n => n.id !== id)
      }))
    }),
    {
      name: 'ui-storage'
    }
  )
);
```

---

## 13. 🧪 Testes Automatizados {#13-testes}

### 13.1. Testes de Componentes

**tests/integration/ProjectForm.test.tsx**
```typescript
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { CreateProjectForm } from '@/app/(dashboard)/projects/components/CreateProjectForm';

// Mock do hook
jest.mock('@/hooks/useDataConnect', () => ({
  useCreateProject: () => ({
    mutateAsync: jest.fn().mockResolvedValue({ id: '123' }),
    isPending: false
  })
}));

describe('CreateProjectForm', () => {
  it('submits form with valid data', async () => {
    render(<CreateProjectForm />);
    
    fireEvent.input(screen.getByLabelText(/nome do projeto/i), {
      target: { value: 'Novo Projeto' }
    });
    
    fireEvent.click(screen.getByText(/criar projeto/i));
    
    await waitFor(() => {
      expect(screen.getByText(/projeto criado com sucesso/i)).toBeInTheDocument();
    });
  });
});
```

---

## 14. 📊 Logging e Monitoramento {#14-logging}

### 14.1. Configuração de Logs Estruturados

**lib/utils/logger.ts**
```typescript
interface LogEntry {
  level: 'info' | 'warn' | 'error';
  message: string;
  timestamp: string;
  userId?: string;
  projectId?: string;
  metadata?: Record<string, any>;
}

export class Logger {
  static info(message: string, metadata?: Record<string, any>) {
    this.log('info', message, metadata);
  }

  static error(message: string, error?: Error, metadata?: Record<string, any>) {
    this.log('error', message, { ...metadata, error: error?.message, stack: error?.stack });
  }

  private static log(level: LogEntry['level'], message: string, metadata?: Record<string, any>) {
    const entry: LogEntry = {
      level,
      message,
      timestamp: new Date().toISOString(),
      metadata
    };

    // Enviar para serviço de logging
    if (process.env.NODE_ENV === 'production') {
      this.sendToLogService(entry);
    } else {
      console[level](entry);
    }
  }

  private static sendToLogService(entry: LogEntry) {
    // Integração com Firebase Crashlytics ou serviço similar
    fetch('/api/logs', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(entry)
    });
  }
}
```

---

## 15. ⚙️ Variáveis de Ambiente {#15-variaveis-ambiente}

### 15.1. Configuração para Firebase Studio

**.env.example**
```bash
# Firebase
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your-project-id

# Data Connect
NEXT_PUBLIC_DATA_CONNECT_ENDPOINT=https://dataconnect.firebase.com/your-project
DATA_CONNECT_CONNECTOR_ID=enterprise-connector

# Cloudflare
CF_ACCOUNT_ID=your_account_id
CF_KV_NAMESPACE_ID=your_kv_namespace_id

# Aplicação
NEXT_PUBLIC_APP_URL=https://your-app.web.app
```

### 15.2. Configuração no Firebase Studio

No Firebase Studio, as variáveis são gerenciadas através do painel de configurações do projeto:

1. **Firebase Console** → Seu projeto → Configurações do projeto
2. **App Hosting** → Variáveis de ambiente
3. **Data Connect** → Configurações do conector

---

## 16. 🚀 CI/CD & Deploy Firebase {#16-ci-cd}

### 16.1. GitHub Actions para Deploy Automatizado

**.github/workflows/deploy.yml**
```yaml
name: Deploy to Firebase

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '20'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm run test:unit
    
    - name: Build application
      run: npm run build
    
    - name: Deploy to Firebase
      uses: FirebaseExtended/action-hosting-deploy@v0
      with:
        repoToken: '${{ secrets.GITHUB_TOKEN }}'
        firebaseServiceAccount: '${{ secrets.FIREBASE_SERVICE_ACCOUNT }}'
        projectId: '${{ secrets.FIREBASE_PROJECT_ID }}'
        channelId: live
    
    - name: Deploy Cloudflare Worker
      run: |
        cd workers/api-gateway
        npx wrangler deploy
      env:
        CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}
        CLOUDFLARE_ACCOUNT_ID: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
```

### 16.2. Deploy Manual via Firebase Studio

```bash
# Deploy do App Hosting
firebase deploy --only hosting

# Deploy das regras de segurança
firebase deploy --only firestore:rules

# Deploy do Data Connect
firebase data-connect:deploy

# Deploy completo
firebase deploy
```

---

## 17. 💅 Padronização de Código {#17-padronizacao}

### 17.1. Configuração ESLint para Firebase Studio

**.eslintrc.json**
```json
{
  "extends": [
    "next/core-web-vitals",
    "@typescript-eslint/recommended"
  ],
  "rules": {
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "warn",
    "prefer-const": "error",
    "no-console": ["warn", { "allow": ["warn", "error"] }]
  }
}
```

---

## 18. 🛡️ Ferramentas de Segurança {#18-seguranca}

### 18.1. Auditoria de Dependências

```bash
# Verificação de vulnerabilidades
npm audit

# Auditoria com Snyk
npx snyk test

# Atualização segura de dependências
npm update
```

---

## 19. ⚡ Performance & Caching {#19-performance}

### 19.1. Estratégia de Cache Híbrido

```typescript
// Exemplo de camadas de cache
export const getProjectsWithCache = async (userId: string) => {
  // 1. Cache do Worker (Edge - ~10ms)
  const cached = await cache.get(`projects:${userId}`);
  if (cached) return cached;

  // 2. Data Connect (Firebase - ~200ms)
  const projects = await dataConnect.execute(GetProjectsQuery);
  
  // 3. Salvar no cache
  await cache.set(`projects:${userId}`, projects, { ttl: 300 });
  
  return projects;
};
```

---

## 20. 📈 Auditoria e Observabilidade {#20-auditoria}

### 20.1. Métricas de Performance

**lib/utils/metrics.ts**
```typescript
export const trackPerformance = async (metric: string, duration: number) => {
  // Enviar métricas para serviço de monitoramento
  await fetch('/api/metrics', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      metric,
      duration,
      timestamp: Date.now(),
      userId: getCurrentUserId()
    })
  });
};
```

---

## 21. 🧾 Checklist Diário de Desenvolvimento {#21-checklist}

### Antes de Iniciar
- [ ] Firebase Studio configurado e acessível
- [ ] Data Connect schema atualizado
- [ ] Variáveis de ambiente configuradas

### Durante o Desenvolvimento
- [ ] Typescript sem erros de tipo
- [ ] Schema Zod para todos os formulários
- [ ] Cache implementado para queries frequentes

### Antes do Commit
- [ ] `npm run lint` sem erros
- [ ] `npm run test:unit` passando
- [ ] Build funcionando localmente

### Antes do Deploy
- [ ] Todos os testes passando
- [ ] Data Connect deployado
- [ ] Regras de segurança validadas

---

## 22. 🐞 Troubleshooting & Debugging {#22-troubleshooting}

### Problemas Comuns e Soluções

| Problema | Causa | Solução |
|----------|-------|---------|
| Data Connect não responde | Schema inválido | Verificar `connector.yaml` |
| Cache não funcionando | TTL muito curto | Aumentar TTL ou verificar chaves |
| Worker com timeout | Query muito lenta | Otimizar query ou adicionar cache |

---

Este guia foi completamente atualizado para refletir o fluxo de trabalho com **Firebase Studio** como ambiente principal de desenvolvimento, removendo todas as referências desnecessárias ao Vercel e otimizando a arquitetura para a stack Firebase + Cloudflare.

As principais vantagens desta abordagem:

- ✅ **Desenvolvimento unificado** no Firebase Studio
- ✅ **Deploy simplificado** com Firebase Hosting  
- ✅ **Performance máxima** com cache em edge
- ✅ **Custos otimizados** com redução de leituras
- ✅ **Segurança robusta** com validação em múltiplas camadas
- ✅ **Developer experience** excelente com IDE integrado