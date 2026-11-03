# Agenta 2.0 no Coolify

Compose adaptado do Agenta 2.0 OSS para rodar como um **Docker Compose resource** no Coolify, sem o Traefik interno e sem os serviços de mobile/Composio.

## O que tem aqui

- `docker-compose.yml`: web, api, services, runner, workers, cron, alembic, Postgres, Redis (volátil e durável), SeaweedFS, SuperTokens e nginx.
- `nginx/nginx.conf`: roteia `/api/`, `/services/` e `/` para os containers internos.
- `init-db-oss.sql`: cria os bancos do Agenta no primeiro start do Postgres.
- `.env.example`: variáveis para colar no Coolify.

## Deploy no Coolify

1. Crie um projeto `agenta` no Coolify.
2. Adicione um recurso **Docker Compose** apontando para este repositório, branch `main`, arquivo `docker-compose.yml`.
3. Em **Environment Variables**, cole e preencha o `.env.example`. Gere os segredos com `openssl rand -hex 32`.
4. No serviço `nginx`, adicione um domínio:
   - Domínio: `agenta.sinceracompliance.com.br`
   - Porta do container: `80`
   - HTTPS: ativado
5. Crie o DNS `A` de `agenta` para o IP da VM.
6. Faça o deploy.

## Primeiro acesso

- Abra `https://agenta.sinceracompliance.com.br`.
- Crie a conta com e-mail `@sinceracompliance.com.br` (o cadastro é restrito a esse domínio).
- Conecte a chave da Anthropic em **Settings > Model providers**.
- Crie o agente Barney e conecte as ferramentas (MCP/Composio).

## Observações

- O runner usa **sandbox local**. Ele não isola uma execução da outra; use com time de confiança. Para isolamento por execução, configure Daytona.
- O runner precisa de `/dev/fuse` e `SYS_ADMIN`, e roda com `security_opt: label=disable` por causa do SELinux do Oracle Linux.
- O compose foi adaptado do repositório oficial do Agenta. Ao atualizar o Agenta, compare com `hosting/docker-compose/oss/docker-compose.gh.yml` e traga as mudanças.
- Não comite `.env` com segredos. O Coolify gera o `.env` a partir das variáveis do recurso.
