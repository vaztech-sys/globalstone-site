# globalstone-site

Tema Shopify da **Global Stone Florida** (globalstoneflorida.com), versionado em Git.

A loja já está no ar e continua no ar. Este repositório é o lugar onde o tema é
editado com histórico, revisão e possibilidade de reverter — em vez do editor de
código do admin, onde toda alteração é definitiva e invisível.

## Começando

```bash
npm install
cp .env.example .env     # preencha SHOPIFY_FLAG_STORE
npm run theme:list       # autentica no navegador e lista os temas da loja
```

Passo a passo completo em **[docs/conectar-shopify.md](docs/conectar-shopify.md)**.

## Comandos

| Comando | O que faz |
|---|---|
| `npm run theme:list` | Lista os temas da loja com seus IDs |
| `npm run theme:backup` | Duplica o tema publicado (rede de segurança) |
| `npm run theme:pull` | Baixa o tema da loja para `theme/` |
| `npm run theme:dev` | Preview local com hot reload em `127.0.0.1:9292` |
| `npm run theme:check` | Lint do Liquid — rode antes de todo push |
| `npm run theme:push:preview` | Sobe como tema **não publicado** (seguro) |
| `npm run theme:push:live` | Sobrescreve o tema publicado |

## Estrutura

```
theme/          o tema Shopify (preenchido pelo theme:pull)
docs/           guia de conexão e roadmap de personalização
.env.example    variáveis necessárias; copie para .env
```

## Fluxo de trabalho

1. `git checkout -b minha-alteracao`
2. `npm run theme:dev` e edite os arquivos em `theme/`
3. `npm run theme:check`
4. `npm run theme:push:preview` → valide na URL de preview
5. Commit, push, Pull Request
6. Depois do merge: publique pelo admin ou `npm run theme:push:live`

## Cuidados

- `.env` e tokens **nunca** vão para o Git (já bloqueados no `.gitignore`)
- Não edite o tema publicado pelo editor de código do admin — o próximo push apaga
- Se a equipe mexe no editor de temas, dê `theme:pull` do `config/settings_data.json`
  antes de subir, senão o trabalho deles é sobrescrito

## Próximos passos

Ver **[docs/roadmap-personalizacao.md](docs/roadmap-personalizacao.md)**.
