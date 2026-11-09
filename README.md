# Preview visual (mockup estático)

Não faz parte da estrutura padrão da skill `reverse-engineering-saas` (que produz só
documentação — `prd.md`, `parity.md`, `implications.md`, `data-model/`). Foi criado a
pedido do Lucas como um checkpoint visual antes de decidir se/como seguir para a fase 5
(Synthesis): algo pra olhar, não só mais markdown.

## O que é

HTML + CSS estático, sem build, sem JS de verdade (só o suficiente pra abrir/fechar
grupo visualmente, se vier a ter). Sem backend, sem banco, sem FastAPI/Jinja — isso é
só a camada visual do Portal PRX real, que mora em `app/` na raiz do projeto e segue as
regras de `CLAUDE.md`.

Dado em todas as tabelas é **100% fictício** (mesmo lote "Google/Apple/Amazon" que o
próprio template do monday usa — não é PII real de ninguém).

## Arquivos

| Arquivo | O que é |
|---|---|
| `tokens.css` | Variáveis CSS extraídas de `../../design/tokens.md` (cor, tipografia, espaçamento, raio, sombra) |
| `shell.css` | Componentes reutilizados nas 7 telas: top bar, sidebar, toolbar, tabela, pills, chips, avatares |
| `icons.svg` | Sprite de referência (standalone) — **não é usado via `<use href="icons.svg#...">`** porque isso quebra no Chrome quando o HTML é aberto por `file://` (bloqueio de segurança a fetch de SVG externo nesse protocolo). Cada página HTML tem o mesmo sprite **inline** no `<body>` |
| `s-001-negociacoes.html` | Board "Negociações" completo — topo, sidebar, toolbar, tabela com 2 grupos, pills de etapa coloridas, barra de cronograma, 10 colunas (as 5 à direita — fechamento/probabilidade/valor previsto/cotações/categoria de previsão — confirmadas em 2026-11-04, tabela com scroll horizontal) |
| `s-002-contatos.html` | Board "Contatos" reaproveitando o shell — grupo ativo com 3 linhas + grupo inativo vazio (vermelho), coluna "Telefone" confirmada (2026-11-04) |
| `s-003-contas.html` | Board "Contas" — grupo único "Empresas", 9 colunas (as 5 à direita — Setor/Descrição/Número de funcionários/Localização da sede/Cronograma de atividades — confirmadas em 2026-11-04, tabela com scroll horizontal; "Cronograma" ainda sem tipo de exibição confirmado, mostra "—") |
| `s-005-projetos-de-clientes.html` | Board "Projetos de clientes" — view "Quadro principal" (Gantt citado mas não reconstruído), barra de ícones extra do board (editar/exportar PDF/comentário — "Automatizar"/"Integrar"/"Fluxo" do monday original cortados em 2026-11-04, ver nota abaixo), prioridade em 3 tons reais de azul (Alta/Média/Baixa, confirmado 2026-11-04), status de Projeto com 4 valores (Não começou/Em andamento/Parado/Feito) |
| `s-006-atividades.html` | Board "Atividades" — grupo único, pills por categoria (chamada/reunião), status Feito/Pendente como pill (não mais texto colorido, confirmado 2026-11-04), coluna "Elemento relacionado" nova (tipo exato do vínculo ainda não confirmado) |
| `s-007-painel-de-vendas.html` | Dashboard "Painel de vendas" — grid de 11 widgets + card de ajuda, estados vazio/erro reproduzidos como observados (o aviso de limite de plano do widget "Metas" foi cortado em 2026-11-04 — ver nota abaixo) |
| `index.html` | Página de navegação entre os previews |

## Ícones

Não são os do monday (sprite/fonte proprietária deles, não copiável — ver
`../barriers.md`). São silhuetas genéricas estilo Feather/Lucide, desenhadas à mão pra
este preview (clean room: mesma função visual, asset diferente).

## Fonte

Figtree de verdade, via Google Fonts — é a mesma fonte que o monday usa, mas é uma
fonte aberta (licença OFL), carregá-la de verdade não é cópia de asset proprietário.

## O que NÃO está aqui (de propósito)

- Vocabulário ainda é o do monday ("Oportunidade", "Negociação") — a tradução pro
  domínio do Portal PRX (`empresa`/`oportunidade`/`solucao`) é decisão de produto, não
  tomada ainda (ver `clone/prd.md`, ainda vazio).
- S-004 Leads não tem preview — o board real tem dado pessoal de terceiros, e por
  cautela nem a estrutura de colunas foi usada pra gerar linhas de exemplo aqui ainda
  (só a estrutura já seguramente documentada, sem nomes/cargos inventados que possam
  parecer reais). Pode ser adicionado com dado claramente fictício depois.
- Interatividade real (abrir item, dropdown de fato funcional, filtros) — é só
  visual/estático por enquanto.
- Tema escuro — tema estudado a fundo foi só o claro (ver `Q-005` em `questions.md`). O
  S-007 real foi capturado em condição de tema escuro (artefato da sessão, não decisão de
  design do monday) — reproduzido aqui em claro mesmo assim, por consistência com as
  outras 6 telas.
- View "Gantt" do S-005 (grade por trimestre/mês, barra de "hoje", lista de tarefas) — só
  a view "Quadro principal" (tabela) foi reconstruída; a aba "Gantt" existe no preview mas
  fica inerte.

## Cortes de produto (2026-11-04)

O monday original pressupõe uma conta multi-time com planos pagos, convites de membro,
marketplace de apps/automação/integração e um assistente de IA proprietário — nenhum
desses conceitos se aplica à PRX Capital (3 sócios + comercial, 8 pessoas na casa, sem
CRM prévio, sem TI). Decidido com o Lucas em 2026-11-04, cortado em todas as 6 telas
(não só numa):

- Pill "Ver planos" (barra superior).
- Ícone de convidar pessoa pro workspace, ícone de apps/marketplace, ícone de inbox
  (barra superior) — ficou só sino, configurações e ajuda.
- Bloco de navegação "Sidekick" / "Agentes" / "Tools" / "Anotador" (sidebar) — IA
  assistente, marketplace de agentes de IA, marketplace de integrações e whiteboard do
  monday.
- Botão "+" de criar novo workspace (só existe um "CRM"), e o item "Mais" no rodapé da
  sidebar (abre admin/apps extras no monday original).
- Item de navegação "Leads" (sidebar) — já tinha ficado decidido que Lead não vira
  página própria (fica fundido em Conta/Contato), então o link era morto; removido por
  consistência com essa decisão, não é uma reabertura dela.
- S-005: ícones "Automatizar" (automação de board), "Integrar" e "Fluxo" (workflow
  builder) — trocados por um botão "Exportar PDF" (ideia do Lucas: gerar um PDF da
  página, ainda sem função real).
- S-007: botão "Convidar" no cabeçalho do dashboard; aviso "This widget is limited to
  500 items" no widget "Metas" (limite de plano pago, não localizado nem aplicável).

Mantido por decisão explícita, mesmo sendo ambíguo quanto a pertencer ao monday-SaaS:
ícone de sino (notificação), ícone de configurações (reservado pra um admin futuro,
provavelmente só os 3 sócios), seção "Favoritos" na sidebar (vazia por ora, função fica
pra depois), ícone de comentários no cabeçalho do board em S-005.

## Navegação entre páginas (2026-11-04)

Os itens da sidebar (`<a class="nav-item">`) agora têm `href` de verdade, levando de
uma tela pra outra entre as 6 páginas — antes eram `<a>` sem destino (visual, mas
mortos).

A sidebar original do monday tinha um item "Página inicial" separado de "Painel de
vendas", mas como não existe uma 7ª página "home" própria nas 6 confirmadas em
`prd.md` (`Q-010`), os dois apontavam pro mesmo arquivo (`s-007-painel-de-vendas.html`)
— redundante (dois botões, mesmo destino), identificado pelo Lucas em 2026-11-04.
Removido o item "Página inicial"; "Painel de vendas" sozinho cobre esse papel de
visão geral/entrada. Ícone do sprite (`icon-home`) ficou órfão no `<symbol>` de cada
página, sem problema (outros ícones não usados em todas as telas já ficavam assim).

Links de abrir um item específico de uma linha (nome de empresa, negociação, etc.)
continuam como placeholder (`href="#"`) — ainda não existe esquema de ID/backend, e
nome de cliente não pode aparecer em URL/log (ver `CLAUDE.md`). Isso é o próximo
patamar de navegação, não feito agora.

## Como abrir

Direto: clique duplo em qualquer `.html` (funciona offline, sem servidor — só a fonte
do Google precisa de internet, com fallback pras fontes do sistema se não tiver).

Ou via servidor local, se preferir: `python3 -m http.server 8001` dentro desta pasta,
depois `http://localhost:8001/`. Tem uma config pronta em `.claude/launch.json`
(`re-monday-preview`) pra isso.
