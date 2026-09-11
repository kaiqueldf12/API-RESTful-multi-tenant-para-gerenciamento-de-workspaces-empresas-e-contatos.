# CRM API (.NET 8 / ASP.NET Core)

API RESTful multi-tenant (M2M) para o CRM: **Workspace → Company → Contact**.

## Como rodar

```bash
dotnet restore
dotnet run
```

A API sobe com Swagger em `/swagger` (ambiente Development).

## Endpoints principais

- `POST   /api/workspaces` — cria um workspace (tenant)
- `GET    /api/workspaces` — lista workspaces
- `POST   /api/workspaces/{workspaceId}/companies` — cria empresa dentro do workspace
- `GET    /api/workspaces/{workspaceId}/companies` — lista empresas do workspace
- `POST   /api/workspaces/{workspaceId}/contacts` — cria contato (opcionalmente vinculado a uma company)
- `GET    /api/workspaces/{workspaceId}/contacts?companyId=...` — lista/filtra contatos

## Notas de arquitetura

- Isolamento multi-tenant feito por `WorkspaceId` em cada entidade + rotas aninhadas
  (`/api/workspaces/{workspaceId}/...`), evitando vazamento de dados entre tenants.
- Banco atual: `EntityFrameworkCore.InMemory`, só para prototipagem. Troque por
  `UseSqlServer(...)` ou `UseNpgsql(...)` em `Program.cs` para produção, usando a
  connection string em `appsettings.json`.
- Próximos passos sugeridos: autenticação/JWT com claim de `workspaceId`, migrations
  do EF Core, paginação nas listagens, e camada de permissões (papéis por usuário/workspace).
