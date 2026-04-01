# Job Board

Plataforma fullstack de vagas de emprego com dois perfis de usuário: employer e candidate.

## Arquitetura

Monolito modular — módulos com fronteiras bem definidas (auth, jobs, applications, users)
rodando em um único processo, compartilhando o mesmo banco de dados.

Cada módulo possui seu próprio controller, service e routes. As fronteiras estão desenhadas
para permitir extração futura para serviços independentes sem refatoração estrutural.

## Stack

**Backend**

- Node.js + Express + TypeScript
- Prisma ORM + PostgreSQL (Docker)
- JWT (access token de curta duração) + Refresh Token (httpOnly cookie)
- Zod para validação de schemas
- Multer + Cloudinary para upload de arquivos

**Frontend**

- Next.js 14 (App Router)
- React + TypeScript
- Tailwind CSS + shadcn/ui
- NextAuth.js para autenticação

## Estrutura

```
job-board/
├── apps/
│   ├── api/                  ← backend REST
│   │   └── src/
│   │       ├── modules/
│   │       │   ├── auth/
│   │       │   ├── jobs/
│   │       │   ├── applications/
│   │       │   └── users/
│   │       ├── shared/
│   │       │   ├── middlewares/
│   │       │   ├── errors/
│   │       │   └── utils/
│   │       ├── app.ts
│   │       └── server.ts
│   └── web/                  ← frontend Next.js
├── .gitignore
└── README.md
```

## Decisões de arquitetura

**Por que monolito modular e não microserviços?**
O nível de complexidade operacional de microserviços (filas, service discovery, deploy
independente) não se justifica para o escopo atual. O monolito modular resolve o problema
com menos partes móveis e mantém a opção de extração aberta.

**Por que Express e não NestJS?**
Express tem maior adoção no mercado para projetos Node.js puros. A estrutura modular
aplicada manualmente aqui replica os princípios do NestJS sem a dependência do framework.

**Por que JWT com refresh token em httpOnly cookie?**
O access token de curta duração (15min) limita a janela de exposição em caso de vazamento.
O refresh token em cookie httpOnly não é acessível via JavaScript — elimina a superfície
de ataque de XSS que existe em tokens armazenados no localStorage.

**Por que Prisma?**
Type safety end-to-end entre banco e aplicação. O schema é a fonte da verdade — models,
migrations e o client TypeScript são gerados a partir dele.

## Rodando localmente

> Em breve

## Deploy

- Frontend: Vercel
- Backend + DB: Railway
