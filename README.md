# clone-tabnews

Projeto de exemplo que implementa partes do TabNews como exercício do [curso.dev](https://curso.dev/). Usa Next.js para a camada web/API e Postgres para persistência. Fornece infraestrutura em Docker Compose, migrações com `node-pg-migrate` e testes de integração com Jest.

**Pré-requisitos**

- Node.js (18+ recomendado)
- Docker e Docker Compose
- `npm`

**Configuração de ambiente**

Copie ou ajuste o arquivo de ambiente presente em [`.env.development`](.env.development). Variáveis importantes:

- `POSTGRES_HOST`, `POSTGRES_PORT`, `POSTGRES_USER`, `POSTGRES_DB`, `POSTGRES_PASSWORD`
- `DATABASE_URL`

**Instalação e execução local**

1. Instale dependências:

```bash
npm install
```

2. Inicie a aplicação em modo de desenvolvimento:

```bash
npm run dev
```

**Scripts úteis**

- `npm run dev`: Sobe os serviços, aplica migrações e inicia o Next.js em dev.
- `npm run services:up|stop|down`: Controla os serviços Docker definidos em [infra/compose.yaml](infra/compose.yaml).
- `npm run test`: Sobe serviços e executa a suíte Jest de integração.
- `npm run test:watch`: Executa Jest em modo watch.
- `npm run migration:create`: Cria uma nova migração (usa `node-pg-migrate`).
- `npm run migration:up`: Aplica migrações em `infra/migrations`.
- `npm run wait-for-postgres`: Script helper para aguardar o banco estar disponível.

**Endpoints relevantes**

- `GET /api/v1/status` — implementado em [pages/api/v1/status/index.js](pages/api/v1/status/index.js). Retorna informações do banco (versão, conexões) e `updated_at`.
- `GET /api/v1/migrations` — lista migrações pendentes (ver [pages/api/v1/migrations/index.js](pages/api/v1/migrations/index.js)).
- `POST /api/v1/migrations` — aplica migrações via `node-pg-migrate`.

Os testes usam esses endpoints para orquestrar e validar o comportamento da aplicação (veja [tests/orchestrator.js](tests/orchestrator.js)).

**Estrutura principal (detalhada)**
**Estrutura principal**

- [infra/compose.yaml](infra/compose.yaml) — definição dos serviços usados em dev/test.
- [infra/migrations](infra/migrations) — migrações do banco (node-pg-migrate).
- [infra/scripts/wait-for-postgres.js](infra/scripts/wait-for-postgres.js) — utilitário para sincronizar startup.
- [pages/index.js](pages/index.js) — página principal Next.js.
- [api/v1/status/index.js](api/v1/status/index.js) — endpoint de status usado nos testes de integração.
- [api/v1/migrations/index.js](api/v1/migrations/index.js) — endpoints de exemplo para migrações (API interna de teste).
- [tests](tests) e [tests/integration](tests/integration) — testes e orquestração.

- [tests/orchestrator.js](tests/orchestrator.js) — utilitário que aguarda `GET /api/v1/status` antes de rodar testes.

**Como rodar os testes**

Os testes de integração sobem os serviços e executam o Jest:

```bash
npm run test
```

Para desenvolvimento iterativo dos testes:

```bash
npm run test:watch
```

**Notas sobre ambiente**

- As migrações usam variáveis de ambiente (veja `migration:up` que aponta para `.env.development`). Garanta as variáveis de conexão com o Postgres configuradas antes de rodar as migrações.
- Se preferir rodar o Postgres local sem Docker, ajuste `DATABASE_URL` adequadamente e pule os passos de `services:up`.

**Licença**

MIT
