# Auditoria do catálogo — 02/09/2026

Levantamento feito via Shopify Admin API na loja **Global Stone Supplier LLC**
(`globalstoneflorida.com`, plano Basic, USD, EDT). Todos os números abaixo foram
verificados direto na API, não estimados.

**78 produtos ativos, 6 coleções, 1 fornecedor (a própria loja).**

## O que a loja realmente vende

Ferramentas e insumos para fabricadores de pedra — não chapas. Marcas próprias
STINGER™, SPIDER™, ZAK™, mais Dakota (cubas), MAKITA (elétricas), KRAUS, Satusa.

Discos diamantados · brocas core · pads de polimento · backer pads · lixa ·
cubas · torneiras · insumos de instalação (clips, âncoras, cunhas, parafusos) ·
EPI (óculos, botas) · politrizes e esmerilhadeiras.

## Os 4 problemas que custam venda

### 1. As coleções filtram por texto do título, e erram

`Blades` e `Sinks` são coleções automáticas com a regra `TÍTULO CONTÉM "blade"` /
`"sink"`. A regra não entende o que o produto é — só se a palavra aparece no nome.
Resultado verificado:

| Coleção | Regra | O que dá errado |
|---|---|---|
| `Sinks` (24) | título contém "sink" | Entram 3 itens de instalação (Sink Clips U, Sink Anchors, Sink Clips L) e 2 torneiras. Faltam **15 cubas de verdade**. |
| `Blades` (17) | título contém "blade" | Entra **ZAK™ Razor Blades** — uma caixa de lâmina de estilete de $5,90 no meio de discos diamantados de $270 a $480. |

As 15 cubas ausentes são as linhas **Zylo** (2318, 2718, 3018, 3218, 5050, 6040,
1616, 1915), **Sorelle** (2318, 2718, 3018, 3218) e **Mosara** (2318, 2718, 3018).
Nenhuma tem a palavra "sink" no título, então a regra não as pega. O cliente que
clica em "Sinks" nunca vê essas 15.

Pior: as coleções ordenam por *best selling*, e como os clips/âncoras são baratos
e vendem mais, eles aparecem **primeiro**. Quem abre `/collections/sinks` vê três
caixas de clip antes da primeira cuba.

### 2. Nenhum produto tem uma única tag

**0 de 78.** Sem tags não existe filtro lateral, nem coleção automática confiável,
nem busca por atributo. É a causa raiz do problema 1: a regra caiu no título
porque não havia mais nada em que se apoiar.

### 3. 34 dos 78 produtos (44%) não estão em nenhuma coleção de categoria

Só aparecem em "All Products". Ficam de fora do menu inteiro: todas as brocas
core, os pads de polimento, os backer pads, a lixa, os óculos de segurança, as
botas, as 4 ferramentas MAKITA, o cup wheel, as varetas de fibra e as 15 cubas
do problema 1.

E há um agravante: "All Products" também é automática, com a regra
`ESTOQUE > 0`. Um produto que zera o estoque **some da única coleção que o
continha** — deixa de ser alcançável por navegação, embora a página siga no ar.

### 4. Tipo de produto vazio ou inconsistente

11 produtos com `productType` em branco (incluindo os core bits e os óculos).
Nos preenchidos, a classificação não é confiável:

- `Blade` no singular, `Sinks` e `Faucets` no plural
- As politrizes **MAKITA** estão como `Polishing Pad` — são máquinas, não pads
- A serra e a esmerilhadeira MAKITA estão em branco
- As varetas de fibra de vidro estão como `Adhesive`

## Achados menores

- **Erro de digitação em produto publicado:** "Dako**r**a MOSARA 3018" (handle
  `dakora-mosara-3018`). Está no ar assim.
- **Handles com `™`:** `zak™-razor-blades`, `stinger™-black-10-inch-...`. Caractere
  não-ASCII em URL vira percent-encoding feio e atrapalha compartilhamento e SEO.
- **"All Products" tem handle `positive-inventory`** — a URL é
  `/collections/positive-inventory`, que não diz nada para cliente nem para busca.
- **Coleção "Home page" tem 1 produto só** (o disco STINGER Black Turbo), então a
  vitrine da home destaca um item isolado.
- **Nenhuma coleção tem imagem nem descrição** — páginas de categoria sem banner
  e sem texto indexável.
- **SKU ausente** nos produtos de março; os de setembro já têm. Sem SKU uniforme
  não dá para conferir estoque nem integrar com fornecedor.
- **Títulos no estilo Amazon**, com até 180 caracteres e palavras empilhadas. O
  Google corta em ~60 e o cliente lê um paredão.

## Taxonomia proposta

Trocar as regras de título por regras de **tipo de produto + tags**, e criar as
coleções que faltam:

| Coleção | Produtos | Existe? |
|---|---|---|
| Diamond Blades | 16 | sim, corrigir regra |
| Core Bits & Drill Bits | 4 | **criar** |
| Polishing Pads & Backer Pads | 5 | **criar** |
| Abrasives & Sandpaper | 2 | **criar** |
| Power Tools | 4 (MAKITA) | **criar** |
| Kitchen & Bath Sinks | 34 | sim, corrigir regra |
| Faucets | 3 | sim, corrigir regra |
| Installation Supplies | 7 | sim, já é manual |
| Safety Equipment | 2 | **criar** |

Tags a aplicar, pensadas para virar filtro na vitrine:

- **Aplicação:** `granite`, `quartz`, `quartzite`, `marble`, `porcelain`, `dekton`
- **Corte:** `wet-cut`, `dry-cut`
- **Máquina:** `bridge-saw`, `angle-grinder`, `cnc`, `router`
- **Diâmetro:** `4in`, `5in`, `14in`, `16in`, `18in`, `20in`
- **Marca:** `stinger`, `spider`, `zak`, `dakota`, `makita`, `satusa`, `kraus`

`granite` + `wet-cut` + `14in` é exatamente como um fabricador procura um disco.
Hoje ele não tem como.

## Ordem de execução sugerida

1. Padronizar `productType` nos 78 produtos e preencher os 11 vazios
2. Aplicar tags
3. Reescrever as regras das coleções para usar tipo/tag em vez de título
4. Criar as 5 coleções que faltam
5. Corrigir o typo "Dakora", os handles com `™` e o handle `positive-inventory`
6. Preencher SKU onde falta
7. Encurtar títulos para ~60–70 caracteres, movendo as especificações para a
   descrição e para metafields
8. Só então mexer no tema: com dado limpo, os filtros do Search & Discovery
   passam a funcionar sem código

Os passos 1 a 6 são alteração de dado no admin, não de tema — e valem mais para
a conversão do que qualquer mudança visual feita antes deles.
