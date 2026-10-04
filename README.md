# YXZADA

Painel web responsivo com tema preto e magenta, PWA e navegação superior. Mantém as funções de autenticação de keys, gerador de sensibilidade, histórico/favoritos, Auxílio, Sobre e painel administrativo. O Auxílio foi redesenhado em módulos compactos.

## Desenvolver e validar

```bash
pnpm install --frozen-lockfile
pnpm dev
```

```bash
pnpm check
pnpm test
pnpm build
```

## Deploy na Vercel

Importe este repositório na Vercel e configure estas variáveis de ambiente para Production (e Preview/Development se usar esses ambientes):

- `RBXIS_ADMIN_KEY`: defina uma chave privada e exclusiva no painel da Vercel; não a inclua no código ou no repositório.
- `RBXIS_SESSION_SECRET`: segredo aleatório longo, diferente deste exemplo. Gere um valor novo com `openssl rand -hex 32`. Não compartilhe nem o commite.
- `MOCKAPI_KEYS_URL`: `https://6ac2d1c03f4ae78f69453b4b.mockapi.io/usuarios` (opcional; este é o padrão do projeto)

O login administrativo só funciona com `RBXIS_ADMIN_KEY`; o login de usuários e o Admin falham de forma segura se `RBXIS_SESSION_SECRET` não estiver definido, e o Admin também falha de forma segura se sua própria variável estiver ausente. Segredos não são embutidos no código nem devolvidos como nome de usuário.

## MockAPI

A coleção configurada é `https://6ac2d1c03f4ae78f69453b4b.mockapi.io/usuarios`. O código não cria coleções automaticamente. Se o schema da collection estiver restritivo, inclua os seguintes campos para compatibilidade com as funções do painel:

| Campo | Tipo esperado | Uso |
|---|---|---|
| `key` | String | credencial gerada |
| `username` | String | nome associado à key |
| `used` | Boolean | indica ativação |
| `device` | String | HWID vinculado |
| `expire` | Number | duração em dias ou horas conforme `type` |
| `type` | String | `daily`, `weekly`, `monthly`, `yearly`, `hourly` ou `perm` |
| `createdAt` | Number | criação (Unix seconds) |
| `activatedAt` | Number | primeira ativação (Unix seconds; `0` enquanto pendente) |
| `expiresAt` | Number | expiração (Unix seconds; calculada na ativação) |
| `status` | String | `active`, `revoked` ou `blocked` |
| `onlineAt` | Number | última atividade (Unix seconds) |
| `history` | Array/Object | histórico de sensibilidades e favoritos |

O campo `id` é criado pela MockAPI. Não é necessário apagar a collection nem modificar registros existentes. A opção de 1 hora mantém `type: hourly`, `expire: 1` e inicia a contagem na primeira ativação.

## PWA

O manifesto, service worker, ícone e metadados de instalação estão em `client/public/` e `client/index.html`. Para ver a versão mais recente no iPhone depois de um deploy, feche e reabra o PWA; se o cache antigo persistir, remova o atalho da tela inicial e adicione-o novamente pelo Safari.
