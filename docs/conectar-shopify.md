# Conectar este repositório à loja Shopify

> Rode estes comandos **na sua máquina**. O ambiente remoto do Claude não
> alcança os domínios da Shopify, então a autenticação acontece localmente.

## 1. Pré-requisitos

- Node.js 20 ou superior (`node -v`)
- Acesso de **Staff com permissão de temas** no admin da loja
- O domínio `.myshopify.com` da loja

Para achar o domínio `.myshopify.com`:
`admin Shopify > Configurações > Domínios` → é o listado como "Domínio da Shopify".
O `globalstoneflorida.com` é o domínio público, não serve para o CLI.

## 2. Clonar e instalar

```bash
git clone https://github.com/vaztech-sys/globalstone-site.git
cd globalstone-site
npm install
cp .env.example .env      # preencha SHOPIFY_FLAG_STORE
```

## 3. Autenticar

```bash
npx shopify theme list --store SUA-LOJA.myshopify.com
```

Abre o navegador, você faz login e autoriza. A sessão fica salva na máquina —
não é preciso token no `.env` para o trabalho do dia a dia.

A saída lista os temas da loja com **ID**, nome e qual está publicado (`[live]`).
Anote o ID do tema publicado.

## 4. Baixar o tema atual para o Git

```bash
npx shopify theme pull --path theme --theme ID_DO_TEMA_LIVE
```

Isso preenche `theme/` com a estrutura padrão da Shopify:

```
theme/
├── assets/      CSS, JS, imagens do tema
├── config/      settings_schema.json (opções do editor) e settings_data.json
├── layout/      theme.liquid — o esqueleto de toda página
├── locales/     traduções (en.default.json, pt-BR.json…)
├── sections/    blocos que aparecem no editor de temas
├── snippets/    pedaços reutilizáveis de Liquid
└── templates/   product.json, collection.json, page.*.json…
```

Commite esse estado **antes de qualquer alteração** — é o seu ponto de retorno:

```bash
git checkout -b baseline-tema
git add theme/ && git commit -m "Baseline: tema live baixado da loja"
```

## 5. Desenvolver com preview ao vivo

```bash
npm run theme:dev
```

Sobe em `http://127.0.0.1:9292` com hot reload: você edita um `.liquid` e o
navegador atualiza. Usa os **produtos e dados reais** da loja, mas nada do que
você edita localmente afeta a loja publicada.

## 6. Publicar

Sempre passe por um tema de preview antes do ar:

```bash
npm run theme:check          # lint do Liquid, pega erro antes de subir
npm run theme:push:preview   # cria um tema NÃO publicado na loja
```

O comando devolve uma URL de preview. Valide, mande para quem precisa aprovar
e só então publique — pelo admin (`Loja virtual > Temas > Publicar`) ou:

```bash
npm run theme:push:live      # sobrescreve o tema publicado. Cuidado.
```

## Regras de segurança

- **Nunca** commite `.env` ou tokens. O `.gitignore` já bloqueia.
- **Nunca** edite o tema publicado direto pelo editor de código do admin: a
  alteração não passa pelo Git e o próximo `push` a apaga.
- `config/settings_data.json` guarda o conteúdo que a equipe monta no editor de
  temas. Se o marketing mexe no editor, faça `theme:pull` desse arquivo antes de
  dar push, senão você sobrescreve o trabalho deles.
- Duplique o tema live antes de qualquer publicação grande: `npm run theme:backup`.

## Quando você vai precisar de token de API

Só em dois casos:

1. **CI/CD** (deploy automático pelo GitHub Actions) → `SHOPIFY_CLI_THEME_TOKEN`,
   guardado como *secret* do repositório, nunca no código.
2. **Consumir o catálogo fora do tema** (uma landing separada, um app) →
   Storefront API token.

Para gerar: `admin > Configurações > Apps e canais de vendas > Desenvolver apps >
Criar app`, e conceda apenas os escopos necessários (`read_themes`, `write_themes`).
