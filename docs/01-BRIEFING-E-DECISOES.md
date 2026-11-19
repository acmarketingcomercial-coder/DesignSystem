# 01 — Briefing e histórico de decisões

## Objetivo

O AC Marketing OS 2.0 deve parecer um **produto SaaS maduro**, moderno, elegante, claro, tecnológico e com personalidade de agência de marketing. O usuário rejeitou repetidas versões que apenas mudavam ícones, bordas, chips e espaçamento sobre a mesma estrutura antiga.

**A mudança visual precisa ser estrutural e perceptível.** Não preservar a composição só porque a view atual já existe ou os testes de apresentação esperam aquele DOM.

## Contexto real de uso

Sistema de trabalho diário da agência, incluindo clientes, calendário e planejamento editorial, produção de posts/Reels/carrosséis, captações, tarefas, aprovação, publicações manuais e agendadas via Instagram/Meta, tráfego pago e equipe. A UX deve permitir que qualquer colaborador compreenda rapidamente onde está, o que precisa fazer e o próximo passo.

A tela inicial é uma **visão da operação**, não substitui módulos completos. A Central de Publicações é o espaço dedicado à agenda/publicação de todos os clientes.

## Preferências expressas nesta rodada

- O usuário gostou do **conceito dark inicial**, mas pediu explicitamente uma **versão clara**.
- Gostou da versão clara com **cores AC (vinho/vermelho)**.
- Quando uma iteração acrescentou muitos cards, textos, gráficos, alvo, foguete, notebook e elementos ilustrados, considerou-a **excessivamente cheia**.
- A orientação mais recente foi **reduzir texto, desenhos e ruído**, conservando marca e sofisticação.
- A imagem de referência final **minimalista ainda não foi gerada/aprovada**. Não tratar o último mockup disponível como a decisão final de layout.

## Preservar no produto real

- Funcionalidades existentes e a lógica dos workflows.
- Permissões e diferenças entre visão de gestor e visão de colaborador.
- Dados reais, contagens e estados confiáveis; falhas de publicação devem permanecer visíveis.
- Rotas e navegação operacional sem duplicação gratuita.
- O **Minuto de Sabedoria**, patrimônio cultural da agência.
- Marcação manual como **Postado em um clique**, sem prova obrigatória ou formulários extras.
- Estado `uncertain` do Instagram sem afirmar publicação concluída.
- Acessibilidade, mobile e desempenho.

## Liberado para redesenhar

Shell visual, sidebar, header, navegação, composição, tamanhos, disposição das áreas, cards, tipografia, microinterações, densidade e hierarquia. O antigo arranjo de **quatro KPIs no topo + duas colunas de listas** não é uma estrutura a ser preservada.

## Evitar

- Sidebar administrativa genérica e cards brancos repetidos em toda a tela.
- Muitos subtítulos descritivos e CTAs redundantes.
- Ver todos os dados de todos os módulos simultaneamente.
- Gradientes, ícones 3D, foguetes e ilustrações como decoração obrigatória.
- Vermelho aplicado indiscriminadamente, inclusive a informação neutra.
- Métricas, comparações percentuais, atividade ou imagem de conteúdo inventadas.
- Badge `Live` quando não existe atualização realmente ao vivo.
- Cópia literal das referências de terceiros.
- Auditorias repetidas e refatorações técnicas fora da mudança de UX/UI.

## Sequência de trabalho aprovada

1. Definir e **aprovar um mockup mais minimalista** de Dashboard com a identidade AC.
2. Usá-lo como referência visual oficial, combinado com especificações funcionais e estados.
3. Implementar primeiro a shell/Dashboard em branch isolada.
4. Validar visualmente no LAB mediante autorização separada.
5. Só depois estender a identidade a Central de Publicações, Editor, Produção, Calendário, Clientes e Tarefas.

O objetivo é **qualidade e eficiência**, com testes proporcionais ao risco e sem transformar cada entrega em uma auditoria do sistema.
