# Roadmap de personalização

Prioridades pensadas para um **fornecedor de pedras naturais** (granito, quartzo,
mármore, quartzito) que vende para fabricadores, construtores e consumidor final.
O funil aqui é *catálogo → orçamento*, não *carrinho → checkout*.

## Por que tema customizado e não headless

| | Tema Liquid (recomendado) | Headless (Hydrogen/Next.js) |
|---|---|---|
| Esforço inicial | Baixo — parte do que existe | Alto — reconstrói tudo |
| Editor de temas para a equipe | Mantido | Perdido |
| Apps da Shopify | Funcionam | A maioria quebra |
| Checkout e pagamentos | Intactos | Reintegrar |
| SEO já indexado | Preservado | Risco de perder |
| Controle de design | Total (é HTML/CSS) | Total |

Headless só se paga quando o front precisa de algo que o Liquid não entrega —
não é o caso de um catálogo de chapas. **Recomendação: tema Liquid.**

## Fase 1 — Fundação (fazer primeiro)

- [ ] Baixar o tema live e commitar como baseline (`docs/conectar-shopify.md`)
- [ ] Auditar performance: pedra é negócio de foto pesada. Rodar PageSpeed e
      converter imagens para WebP com `image_url` + `srcset` do Liquid
- [ ] Revisar responsividade no mobile — a maioria das buscas por "granite
      countertops near me" vem de celular

## Fase 2 — Catálogo de chapas

O ponto onde o site de pedra ganha ou perde o cliente.

- [ ] **Metafields de produto** para as specs reais da chapa:
      material, cor predominante, acabamento (polido/leathered/honed),
      espessura (2cm/3cm), dimensão da chapa, origem, lote/bundle
- [ ] **Filtros de coleção** por esses metafields (Search & Discovery, app
      gratuito da Shopify) — "quartzo branco 3cm polido" tem que ser 3 cliques
- [ ] **Galeria de chapa em alta resolução** com zoom: o cliente compra pelo
      veio da pedra, a foto pequena não vende
- [ ] Seção "peças similares" por material/cor

## Fase 3 — Conversão por orçamento

- [ ] Substituir/complementar "Adicionar ao carrinho" por **"Solicitar orçamento"**
      no que é vendido por m², levando material, cor e metragem para o formulário
- [ ] Formulário de orçamento com upload de planta ou foto do ambiente
- [ ] Calculadora de metragem (bancada: comprimento × profundidade + ilha)
- [ ] Agendamento de visita ao showroom

## Fase 4 — Confiança e SEO local

- [ ] Galeria de projetos antes/depois (a prova social que fecha venda de pedra)
- [ ] Página de showroom com mapa, horário e área de atendimento
- [ ] Schema.org `LocalBusiness` + `Product` no `theme.liquid` — é o que faz
      aparecer no mapa do Google
- [ ] Páginas por cidade/região atendida, se houver mais de um showroom
- [ ] Depoimentos e credenciais de fabricação

## Convenções ao mexer no tema

- Uma **section** nova por funcionalidade, em `theme/sections/`, com
  `{% schema %}` — assim a equipe configura pelo editor sem pedir dev
- Lógica repetida vira **snippet** em `theme/snippets/`
- Nada de texto fixo no Liquid: use `{{ 'chave' | t }}` e os arquivos de
  `theme/locales/` — o site atende público em inglês e espanhol na Flórida
- CSS novo em arquivo próprio dentro de `theme/assets/`, não empilhado no
  arquivo do tema base — facilita atualizar o tema depois
