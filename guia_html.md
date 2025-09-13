title: "Guia AxView — Padrões Definitivos para HTMLs Corporativos"
version: 3.0.0
owner: "Guilherme"
scope: "Padrões unificados de design e desenvolvimento para HTMLs corporativos e institucionais das marcas AxiaCare®, MedValor®, Unihealth e Unimed"
last_compiled: 2025-09-13
tags: [AxSets, AxView, VBHC, Compliance, Propostas, Educação Executiva]
Sumário executivo

Este guia consolida e atualiza os padrões de HTML corporativo e institucional para AxiaCare, MedValor, Unihealth e Unimed, unindo as diretrizes do Documento Mestre anterior, a marca AxView™ e as especificações oficiais da GT Corporation. Os objetivos principais são:

Padronizar e escalar o desenvolvimento de páginas e documentos HTML (políticas, autorizações, propostas, relatórios, landing pages, dashboards) de forma consistente, responsiva, acessível e elegante.

Harmonizar a identidade visual, incorporando os tokens de cores e tipografia da marca (retirados do kit de marca no Canva) e garantindo que as diferenças entre AxiaCare (consultoria/gestão) e MedValor (educação executiva) se manifestem apenas na cor de acento
canva.com
.

Garantir governança e segurança, com uso de hash SHA‑256 + data/hora como carimbo de autoridade, placeholders claros para personalização e integração de webhooks e mailto para interações.

Promover modularidade e reuso: bibliotecas de componentes, templates completos e comandos (gen_html, assess_html, refine_html) que podem ser utilizados em DevMode, GPT‑Custom e API para geração automatizada de documentos.

Reforçar a cultura AxControl™, assegurando que cada documento seja mais que uma página: um ambiente inteligente ligado ao Método (AxWay), Visual (AxView) e Inteligência (AxIntel), conforme as fórmulas do GTCorp.

1. Paleta de cores e tokens visuais

A paleta oficial da marca foi obtida no Kit de Marca Axia | MedValor no Canva. Para padronizar, adote os seguintes grupos de cores e use tokens semânticos. Cada marca (AxiaCare®, MedValor®, Unihealth, Unimed) escolhe um accent (cor de acento) diferente, mantendo as demais.

Token	Hex	Uso principal
--color-primary	#196396	Azul escuro — base para cabeçalhos, títulos e elementos de marca.
--color-secondary-1	#a6d8c0	Verde pastel — detalhes, cartões e fundos claros.
--color-secondary-2	#a6fb90	Verde‑limão — realces sutis, gradientes e gráficos.
--color-dark	#2d3445	Azul‑marinho quase preto — texto, ícones escuros, contrastes.
--color-light	#f8f5f0	Off‑white — fundo de páginas e cartões.
--color-white	#ffffff	Branco puro — fundo de seções e elementos transparentes.
--color-accent	#2dbf7f (AxiaCare/Unihealth) / #ff8f00 (MedValor)	Cor de destaque para botões, chips, links e CTAs; altere conforme a marca.

Nota: O Documento Mestre original usava #196396 e #2DBF7F como primária e acento. O kit de marca adiciona o laranja #ff8f00 (usado apenas na MedValor) e tons pastéis. Use os tokens acima para todos os elementos estilizados. Aplique clamp() em tamanhos de fonte para responsividade e garanta contraste AA em combinações de cores
canva.com
.

2. Tipografia e hierarquia de títulos

O kit de marca define Grifter como fonte oficial para títulos e subtítulos. Aplique:

Nível	Fonte	Tamanho base (desktop)	Estilo	Uso
Título	Grifter	42px	Bold	Títulos de páginas e seções principais.
Subtítulo	Grifter	36px	Regular	Subtítulos e cabeçalhos de blocos.
Cabeçalho de seção	System UI / Sans-serif (e.g., Inter, Segoe UI)	24–32px via clamp()	Regular	Títulos internos, colunas de tabelas.
Corpo	System UI / Sans-serif	16–20px via clamp()	Regular	Parágrafos, listas, notas e tabelas.
Legenda	System UI / Sans-serif	14px	Italic	Figuras, gráficos e observações.

Para compatibilidade universal (web e PDF), utilize fontes nativas do sistema no corpo de texto. Evite fontes customizadas externas ou carregamentos adicionais.

3. Layout e responsividade

Grid fluido: Base de 8px. Use clamp() para espaçamentos e fontes. Empilhe seções verticalmente em telas menores e use colunas (até 4) em desktop.

Cabeçalho e navegação: Cabeçalho sticky com altura entre 46–56 px; inclui logo da marca à esquerda, chip “Compliance” ou menu de acessos rápidos à direita. Evite barras largas.

Tabelas responsivas: Envolva tabelas em <div class="table-wrap" role="region" tabindex="0">; use position: sticky; top: 0; em <thead> para cabeçalhos fixos; evite rolagem horizontal duplicada. Aplique .matrix ou .pricing para tabelas de matriz e precificação.

Mobile first: Priorize layouts de largura única (100 %) e ajuste margens internas (padding) para 16 px em mobile. Evite colunas múltiplas; use carrosséis ou acordes se necessário.

Impressão em PDF: Defina mídia @media print em CSS para ajustar margens, ocultar elementos interativos (botões) e garantir legibilidade. Use -webkit-print-color-adjust: exact para preservar cores principais.

4. Acessibilidade e governança

Contraste e leitura: Garanta contraste mínimo de 4,5:1 em texto e 3:1 em ícones. Utilize aria-label e aria-live em elementos interativos para tornar a navegação por teclado intuitiva.

Foco e navegação: Exiba outline visível e diferenciado para elementos focáveis. Ordem de tabulação deve seguir a estrutura visual.

Hash e carimbo: Em documentos formais (autorização, políticas, propostas), gere um hash SHA‑256 do escopo e exiba com data/hora (ISO 8601 ou padrão pt-BR) como “Carimbo de autoridade”; isso assegura autenticidade e versionamento.

Placeholders claros: Todo campo personalizável (nomes, datas, valores) deve estar marcado com comentários <!-- TODO: placeholder --> e textos entre chaves (ex.: {{nome_do_cliente}}). Não altere conteúdo jurídico aprovado.

Integridade do conteúdo: Use mail para envios simples (assunto e corpo); para registros, utilize webhooks (POST JSON) com endpoints configuráveis. Não inclua JS externo ou bibliotecas de terceiros sem avaliação de segurança.

5. Biblioteca de componentes
5.1 Cabeçalho (Header)

Estrutura: <header><div class="wrap"><a class="brand"><img src="[logo]" alt="[Marca]"><span class="sr-only">Marca</span></a><nav>…</nav></div></header>

Chip “Compliance” / Acesso rápido: Use <a class="chip" href="[link]">Compliance</a> ou substitua por “Cursos”, “Unidades” conforme a página. Ajuste bordas e cores por marca.

Menu suspenso: Opção para contatos (mailto:) ou redes sociais; use role="menu" e aria-haspopup.

5.2 Cards institucionais e chips de status

Cards: <section class="card"><h2>Título</h2><p>Conteúdo…</p></section>; aplique borda suave (--radius-md) e sombra leve (--shadow-sm).

Chips: <a class="chip" href="#">Texto</a>; utilize cores de acento para identificar seções ou ações; opte por bordas arredondadas.

5.3 Tabelas (matriz, precificação, indicadores)

Use <table> com classes matrix ou pricing; cabeçalho (<th>) com fundo claro e texto escuro; células (<td>) com padding generoso e linhas alternadas.

Para comparações ou precificação 3×3, defina <colgroup> para controlar larguras das colunas e alinhar textos.

5.4 Timeline / Diagramas

Estrutura em <div class="timeline"><div class="t-step"><div class="t-title">Fase</div><div class="t-desc">Descrição…</div></div>…</div>; use linhas e ícones circulares para representar etapas. Ajuste com flexbox.

5.5 Formulários e interações (modais e toasts)

Utilize <dialog> para modais de feedback ou captura de dados mínimos; inclua <form method="dialog"> para permitir encerramento. Para envios via webhook, use botões com onclick que chamam funções definidas em script inline.

Crie <div class="toast" role="status" aria-live="polite">Mensagem…</div>; o toast deve aparecer e desaparecer com transições CSS.

5.6 Rodapé (Footer)

Inclua logo em tamanho reduzido, uma descrição curta da marca e links para sites (AxiaCare, MedValor, GuiThome), redes sociais e canal de contato. Ajuste cores de texto para alto contraste.

Adicione direitos autorais e menção à GTCorp.

6. Templates completos

Os templates a seguir são referências para criação rápida de páginas. Eles devem ser parametrizados via JSON/YAML para permitir personalização por DevMode ou API.

6.1 Políticas (Privacidade, Cookies, Anticorrupção, Brindes, Terceiros, Cláusulas)

Estrutura base: Header → Hero (título e data) → Sumário com âncoras → Seções com headings claros → Tabela (se houver) → Canal de Compliance (links) → Footer.
Preserve texto jurídico; use tabelas com wrapper e thead sticky; adicione seções “Atualizações” e “Contato do DPO”.

6.2 Central de Compliance

Página hub com seções separadas para Essenciais (Privacidade, Termos, Cookies, Autorização) e Integridade (demais políticas). Menu de âncoras no topo. Utilize botões sem mostrar URLs no rótulo.

6.3 Autorização de Parceria (hash + datetime + webhook + mailto)

Página interativa para autorizar uso de marca ou parceria:

Termo legal introdutório e campos de entrada (nome, CNPJ, razão social, finalidade).

Geração de hash SHA‑256 e carimbo de data/hora (mostrados em pill com cor de acento).

Botões para enviar via webhook (POST JSON) e via mailto, ambos incluindo hash e carimbo no corpo.

Botão “Salvar PDF” (impressão nativa) e status de envio via toast.

6.4 Proposta Comercial AxView | Pages

Divida a página em seções: Apresentação (hero, logo do cliente, frase de impacto), Impressões (três colunas de diferencial com ícone/imagem), Matriz de Valor (tabela 3×3 com entregas por eixo), Precificação & Timeline (tabela de preços + linha do tempo visual) e Vamos ao trabalho (contato, feedback).
Inclua menu de âncoras fixo no topo e rodapé padrão AxView.

6.5 Relatórios de Linhas de Cuidado e Dashboards (Ex.: Pareto de suturas – Unihealth)

Campos de filtro contextual no topo; tabelas com indicadores assistenciais e gráficos simples (bar, line); seções de observações e conclusões.

Use colunas responsivas e aplique cores da marca correspondente (Unihealth/Unimed).

6.6 Landing pages educativas (MedValor)

Hero com título motivador e call‑to‑action; seções curtas explicando a missão, público-alvo e oferta de cursos/eventos; CTAs com mailto ou WhatsApp.

Paleta com laranja #ff8f00 como acento e azul #196396 como base.

7. Formatos de saída e automação

HTML padrão: Código sem dependências externas; use CSS inline e JS mínimo.

PDF: Geração via impressão do navegador; garanta que as cores fiquem legíveis e que tabelas não quebrem em páginas.

E‑mails (mailto): Defina assunto e corpo dinâmico com dados do documento; informe limitações de anexos e encoraje uso de PDF via impressão quando necessário.

Webhooks: Estruture JSON com campos obrigatórios (hash, timestamp, identificadores) e opcionais (observações, anexos). Invoque endpoints via fetch no script inline.

8. Dúvidas e pontos em evolução

Banner de cookies e consentimento: Avaliar ferramenta ou script de preferências para coletar consentimentos (já previsto na Política de Cookies).

Rodapé unificado: Consolidar elementos de AxiaCare, MedValor e GTCorp em um único rodapé adaptável.

Multilíngue: Definir padrões para páginas em inglês e espanhol; utilizar arquivos de tradução para textos fixos.

Integrações de backend: Explorar endpoints para logs, autenticação e anexos além de webhooks simples.

Catalogação completa de compliance: Continuar mapeando documentos legais faltantes para inclusão no hub.

9. Aplicações práticas

Portais institucionais: Hub de compliance, página “Sobre” e landing pages de linhas de cuidado.

Propostas comerciais: Repositório no GitHub com um HTML por cliente; branch para revisão e histórico de mudanças (carimbo SHA‑256).

Educação executiva: Materiais didáticos, cronogramas de cursos e páginas de captura; uso intensivo do acento laranja na MedValor.

Relatórios e dashboards: Comparações de indicadores (OPME, paretos, dimensionamentos) publicados via GitHub Pages ou Notion e acessíveis para gestores; exportáveis em PDF.

Automação: Envio de autorizações, registros de eventos e feedbacks via webhooks; integração com CRM e Notion para histórico.

10. Riscos e limitações

Clientes de e-mail: mailto não suporta anexos automáticos; instruir usuários a baixar o PDF antes de enviar.

Desvio visual: Múltiplas cópias de CSS/JS podem causar deriva; centralize tokens em arquivos assets/brand.css e assets/brand.js e importe via <link> e <script> quando necessário.

Hash/JS: Cálculo de hash e data/hora depende do JavaScript do navegador; para persistência e verificação externa, registre no webhook.

Dependências ocultas: Sites como axcare.com.br e medvalor.med.br podem ter estilos próprios; alinhe gradualmente com este guia, evitando rupturas bruscas.

11. Organização e versionamento

(A seção 11 permanece similar à anterior, mas com as correções sobre domínios e repositórios introduzidas anteriormente.)

12. Anexo – Comandos e DevMode

(A seção 12 permanece idêntica à versão 2.0.0.)

Conclusão: Esta versão 3.0.0 do guia inclui o fluxo completo para integração com repositórios GitHub (control, comercial, hub) e o manual (guia_html.md), reforçando a governança e modularidade da AxView. Use-o como referência definitiva para o GPT Custom AxIntel™ | HTML AxView™, para DevMode e para qualquer automação via API.
