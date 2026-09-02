# Roadmap de personalização

Escrito **depois** de auditar o catálogo real via Admin API. Ver
[auditoria-catalogo.md](auditoria-catalogo.md) para os números.

A loja vende **ferramentas e insumos para fabricadores de pedra** — discos
diamantados, brocas, pads, cubas, torneiras, EPI. Não vende chapas. O cliente
principal é profissional, sabe o que quer e compra por especificação técnica:
diâmetro, material de corte, tipo de máquina, molhado ou seco.

Isso define a prioridade: **encontrabilidade por especificação**. Não é um site
de descoberta visual, é um catálogo técnico onde o cliente precisa chegar no item
certo em poucos cliques e repetir a compra.

## Por que tema customizado e não headless

| | Tema Liquid (recomendado) | Headless (Hydrogen/Next.js) |
|---|---|---|
| Esforço inicial | Baixo — parte do que existe | Alto — reconstrói tudo |
| Editor de temas para a equipe | Mantido | Perdido |
| Apps da Shopify | Funcionam | A maioria quebra |
| Checkout e pagamentos | Intactos | Reintegrar |
| SEO já indexado | Preservado | Risco de perder |
| Filtros por tag/metafield | Nativos (Search & Discovery) | Reimplementar do zero |

No plano Basic com 78 SKUs e um time pequeno, headless é custo sem retorno.
**Recomendação: tema Liquid.**

## Fase 0 — Limpar o dado (antes de qualquer código)

Esta fase não toca no tema e é a que mais move a agulha. Detalhe e ordem em
[auditoria-catalogo.md](auditoria-catalogo.md#ordem-de-execução-sugerida).

- [ ] Padronizar `productType` e preencher os 11 vazios
- [ ] Aplicar tags de aplicação, corte, máquina, diâmetro e marca
- [ ] Reescrever as regras das coleções `Blades` e `Sinks`, hoje baseadas em
      texto do título — elas deixam 15 cubas de fora e colocam lâmina de estilete
      junto com disco diamantado
- [ ] Criar as 5 coleções ausentes (core bits, pads, abrasivos, elétricas, EPI)
- [ ] Corrigir typo "Dakora MOSARA 3018", handles com `™`, handle
      `positive-inventory`
- [ ] Preencher SKU nos produtos de março

Enquanto isso não estiver feito, filtro na vitrine não tem em que se apoiar:
com zero tags em 78 produtos, não há o que filtrar.

## Fase 1 — Navegação e filtros

- [ ] Instalar **Search & Discovery** (app gratuito da Shopify) e configurar os
      filtros sobre as tags da Fase 0
- [ ] Menu principal refletindo a nova taxonomia — hoje 44% do catálogo não está
      em nenhuma coleção de categoria
- [ ] Imagem e texto de categoria em cada coleção: hoje as 6 estão sem as duas
      coisas, e a página de categoria é o que ranqueia para "diamond blade granite"
- [ ] Trocar a ordenação `best selling` nas coleções, que hoje empurra os itens
      baratos para o topo

## Fase 2 — Página de produto para comprador técnico

- [ ] **Metafields de especificação** por tipo: diâmetro, furo/arbor, RPM máx.,
      altura de segmento, material de aplicação, molhado/seco
- [ ] Tabela de especificação renderizada a partir dos metafields, no lugar do
      paredão de texto atual
- [ ] Encurtar títulos para 60–70 caracteres; a especificação sai do título e vai
      para a tabela
- [ ] Guia de compatibilidade: qual disco serve para qual máquina e material
- [ ] "Compre junto": disco + flange, cuba + clips + âncoras

## Fase 3 — Recompra e volume

O cliente é profissional e recompra consumível. É onde está o ticket recorrente.

- [ ] Recompra em um clique a partir do histórico de pedidos
- [ ] Preço por caixa/case explícito — vários produtos já vêm em
      "100 por caixa, 50 caixas por case", mas isso está solto na descrição
- [ ] Desconto por quantidade para fabricadores
- [ ] Cadastro de conta comercial / lista de preço para revenda
- [ ] Alerta de reposição para consumíveis

## Fase 4 — Confiança e SEO

- [ ] Schema.org `Product` com marca, SKU, preço e disponibilidade
- [ ] Schema `LocalBusiness` — a loja atende a Flórida e existe busca local
- [ ] Página de marca para STINGER™, SPIDER™ e ZAK™: são marcas próprias e hoje
      não têm página nenhuma
- [ ] Política de frete e devolução visível na página de produto
- [ ] Avaliações de produto

## Convenções ao mexer no tema

- Uma **section** nova por funcionalidade, em `theme/sections/`, com
  `{% schema %}` — assim a equipe configura pelo editor sem pedir dev
- Lógica repetida vira **snippet** em `theme/snippets/`
- Nada de texto fixo no Liquid: use `{{ 'chave' | t }}` e os arquivos de
  `theme/locales/` — há público em inglês e espanhol na Flórida
- CSS novo em arquivo próprio dentro de `theme/assets/`, não empilhado no
  arquivo do tema base — facilita atualizar o tema depois
