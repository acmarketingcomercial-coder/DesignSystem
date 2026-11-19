# Auditoria consolidada de travas visuais e dependências legadas — AC Marketing OS 2.0

**Data:** 19/11/2026  
**Sistema auditado:** [acmarketingcomercial-coder/ac-marketing-os](https://github.com/acmarketingcomercial-coder/ac-marketing-os)  
**Branch de trabalho:** `feat/v2-redesign-v2`  
**SHA de referência:** `479f7d6b4de05673be3d6ef9401e69d106e78cba`  
**Natureza:** auditoria **estática** de impedimentos de UX/UI; não é prova de homologação, de aprovação visual, nem de execução de testes locais. **Nenhum arquivo do aplicativo foi modificado nesta auditoria.**

## 1. Escopo e método

- A árvore Git de referência contém 1.260 entradas e 1.084 arquivos rastreados; não estava truncada.
- Foi feita varredura dirigida dos **90 templates Blade** do diretório `resources/views`, dos **158 testes Feature** de `tests/Feature`, dos **sete testes Node em tests/js**, CSS principal e CSS da integração Instagram; inspecionados componentes base, recursos JavaScript relevantes, controladores de interface, ESLint/Vite, documentação atual e instruções ativas de agentes.
- Testes de negócio foram pesquisados por padrões de acoplamento visual e os arquivos que apresentaram evidências foram inspecionados em maior detalhe. **Não constitui leitura semântica linha a linha dos 1.084 arquivos**, nem substitui análise dinâmica em navegador ou validação com PHP/Node executados no ambiente de desenvolvimento.
- Registros históricos sob `.agents/handoffs` e migrações não são, por si, regras visuais ativas. Não se deve removê-los.
- Prioridade: identificar todas as fontes relevantes de interferência **num só plano**, sem confundir boas proteções funcionais com restrições estéticas ultrapassadas.

## 2. Achados confirmados de prioridade alta

### P0-01 — Contratos de teste exigem a estrutura antiga do Dashboard
**Arquivo:** `tests/Feature/DashboardOperationalTest.php`, especialmente linhas 603–632 e 642–800.

O teste procura quatro `section` filhas diretas de um `div.dashboard-workspaces`, exige ordem `productions → tasks → agenda → attention`, exige a classe `dashboard-workspace-card` e proíbe determinada geometria anterior. Outros testes exigem as variáveis `productionWide`, `taskWide`, `agendaWide` e `attentionWide` no contexto da view.

**Impacto:** reprova composições melhores com containers aninhados, zonas editoriais assimétricas ou diferentes posições, apesar de manter dados/ações. A fixação de **quatro colunas** não é imposta diretamente por esse teste; ela está no Blade. Não atribuir erroneamente toda a causa aos testes.

**Correção:** substituir exigências de parent/child, ordem e classe por contratos semânticos: seções identificáveis, dados corretos, ordenação de prioridades quando exigida pelo negócio, links funcionais, acessibilidade, visibilidade por papel, estados vazios e sem perda de itens.

### P0-02 — Geometria obrigatória no Blade do Dashboard
**Arquivo:** `resources/views/dashboard.blade.php`, principalmente linhas 7, 45 e 104.

Há `lg:grid-cols-12`, KPIs em `sm:grid-cols-4` e matriz `grid grid-cols-1 md:grid-cols-2 xl:grid-cols-4 gap-5 items-stretch`. Cards compartilham `flex flex-col justify-between` e geometria homogênea. Isso cria painéis vazios esticados para a altura da maior fila.

**Impacto:** visual de quatro caixas administrativas, mesmo com dados desequilibrados. Isto é **uma escolha de implementação ainda presente**, não uma barreira obrigatória da linguagem/framework.

**Correção:** reconstruir composição com grid intencional, spans/áreas responsivas e compactação de estados vazios, sem esticar painéis por padrão. Adaptar a dados escassos e abundantes.

### P0-03 — Editor de Conteúdo tem testes que congelam classes exatas
**Arquivos:** `tests/Feature/Task193ReviewLayoutTest.php`, linhas ~99–124; `tests/Feature/ContentMobileLayoutTest.php`, linhas ~33–35.

Testes exigem literalmente `xl:grid-cols-[minmax(0,1fr)_380px] xl:items-start`, `contents xl:block xl:space-y-4`, várias classes `panel order-*` e até a sequência de classes em `x-show`. O Editor contém atualmente `xl:grid-cols-[minmax(0,1fr)_380px]` em `resources/views/contents/edit.blade.php`, linha ~130.

**Impacto:** qualquer Editor V2 que mude a proporção de colunas, agrupamentos, ordem visual ou nomenclatura CSS quebra testes independentemente de funcionar.

**Correção:** preservar campos, dados, ordem de leitura acessível, fluxos de revisão e upload, mas trocar testes de classes literais por testes semânticos, responsivos e de navegação.

### P0-04 — Testes de identidade visual foram afrouxados demais e outros mantêm premissas antigas
**Arquivos:** `tests/Feature/DesignSystemComponentsTest.php` e `tests/js/design-tokens.test.js`.

- Há teste que apenas faz `expect(true)->toBeTrue()`, sem verificar qualquer requisito.
- Há teste da navegação ativa que verifica só a existência de `.nav-link-active` no CSS, sem validar o novo contrato.
- O teste de tokens ainda se chama `semantic tokens alias the legacy palette (no visual change)` e fixa variáveis/códigos antigos.
- Outros testes de componentes ainda esperam visualização grafite para `x-ui.metric` e `x-ui.tabs`.
- O linter de cor atualizado exclui o Dashboard da procura por utilitários `rose/red`, mas essa exceção não documenta as novas regras semânticas.

**Impacto:** coexistem dois problemas: falsas aprovações sem cobertura e requisitos cosméticos ultrapassados.

**Correção:** criar testes positivos para paleta/token oficial, contraste e semântica de estado; não validar tonalidade exata em todas as superfícies. Eliminar `expect(true)`, preservar verificação de danger/attention, foco e `aria-current`. Preferir tokens a hexadecimais dispersos.

### P0-05 — Duas fontes de verdade em conflito
**Arquivos:** `docs/design-system/README.md`, `tokens.md`, `components.md`, `AGENTS.md`, `CLAUDE.md` e `.agents/protocol.md`.

Os documentos locais ainda declaram, em termos normativos, “vermelho somente para ação”, “seleção é grafite” e componente obrigatório para toda página. Paralelamente, o novo DS v1.1 e o repositório [DesignSystem](https://github.com/acmarketingcomercial-coder/DesignSystem) aprovaram sidebar vinho e seleção carmim.

`AGENTS.md` e o protocolo ainda tratam Claude Code como ambiente exclusivo e Antigravity como identidade histórica, apesar de autorização posterior do usuário para trabalho isolado no Antigravity.

**Impacto:** um agente pode seguir instruções ativas contraditórias e voltar ao design antigo; confusão de governança.

**Correção:** registrar formalmente precedência do DS v1.1 e os mockups **expressamente aprovados**. Atualizar as instruções dos agentes para reconhecer Antigravity como ambiente autorizado, **sem** reduzir regras de branch, revisão, backup, CI local, autorização independente de merge/LAB/produção ou proibição de GitHub Actions. Não reescrever/apagar handoffs históricos.

## 3. Achados de prioridade média: interferência distribuída entre módulos

### P1-01 — Componentes globais com desenho único hardcoded
**Arquivos:** `resources/css/app.css` (~linhas 186–190, 240–282), `resources/views/components/ui/card.blade.php`, `metric.blade.php`, `tabs.blade.php`, `page-header.blade.php`, `empty-state.blade.php`.

`.panel`, `.work-row`, `.metric`, `.page-title` e `.eyebrow` têm aparência fixa. `x-ui.card` impõe `rounded-2xl border border-slate-200/80 bg-white shadow-xs`; `x-ui.metric` usa `rounded-2xl border p-5`; `x-ui.tabs` usa seleção global de token grafite. `x-ui.page-header` tem um layout padrão de `header` e classe `page-title`.

**Impacto:** consistência genérica involuntária: mudar os tokens sem alterar arquitetura dos componentes tende a produzir o “mesmo painel com outras cores”.

**Correção:** variantes explícitas e semânticas de componentes; tokens de tamanho, densidade e superfície; composições específicas por módulo quando necessárias. **Não remover componentes compartilhados nem mudar seu padrão global sem verificar todas as telas consumidoras**.

### P1-02 — Tokens e estilo ativos contraditórios
**Arquivo:** `resources/css/app.css`, especialmente tokens ~78–149 e navegação ~194–236.

- `--ac-bg-sidebar` continua com azul-marinho `#0F172A` apesar da sidebar vinho.
- `--ac-selected-bg` global continua grafite, enquanto `[data-sidebar]` o redefine como branco.
- Alguns elementos da navegação usam vinho/carmim hardcoded enquanto componentes genéricos usam outros valores.
- `.btn-danger` permanece com classes rose diretas, apesar de existirem tokens semânticos danger.
- `.dashboard-expanded-scroll` aparece em duas definições.

**Correção:** normalizar tokens por papel e **escopo** (sidebar, superfície, seleção, foco, danger), sem transformar toda seleção em vermelho ou confundir marca com erro. Remover duplicações com evidência de não impacto.

### P1-03 — CSS inline e estilos locais dificultam mudança transversal
A varredura identificou estilos inline, blocos `<style>` e geometrias próprias em várias telas, especialmente:

| Arquivo | Evidência |
|---|---|
| `resources/views/clients/planning.blade.php` | bloco `<style>`, muitos `style="..."` (aprox. 51 ocorrências) e `!important` |
| `resources/views/clients/show.blade.php` | aprox. 16 estilos inline |
| `resources/views/clients/import.blade.php` | bloco `<style>` e aprox. 17 estilos inline |
| `resources/views/clients/import-review.blade.php` | bloco `<style>` e estilos inline |
| `resources/views/clients/intelligence.blade.php` | bloco `<style>` e cores locais |
| `resources/views/clients/profile-edit.blade.php` | bloco `<style>` |
| `resources/views/clients/orders.blade.php` | bloco `<style>` e declarações `!important` |
| `resources/views/captures/_agenda_calendar.blade.php` e `captures/index.blade.php` | grandes blocos de estilo locais |
| `resources/views/clients/profile-reimport*.blade.php` | estilos locais |
| `resources/views/instagram-review.blade.php` | bloco de estilo local |
| `resources/views/contents/edit.blade.php`, `tasks/_detail.blade.php`, `production/index.blade.php` e `traffic/index.blade.php` | grids e dimensões específicas |

**Impacto:** novas variáveis globais não garantem transformação dessas telas. Nem todo `style="width: ...%"` é legado: progresso calculado dinamicamente é válido.

**Correção:** inventariar quais estilos são efetivamente interferentes e migrá-los para CSS de módulo/variantes durante o redesign desse módulo. Não fazer “replace all” em produção.

### P1-04 — Geometrias rígidas nas telas operacionais
**Arquivo:** `resources/css/app.css`, ~linhas 320–387.

`.task-overview-kpis` usa 2/4 colunas, `.task-overview-item` usa colunas específicas em container queries, e `.task-overview-filters` alterna para três colunas. Diversas views têm `xl:grid-cols-[...]` e `lg:grid-cols-[...]` locais.

**Correção:** tratar como **parâmetros atuais**, não como bugs automáticos. Substituir somente quando atrapalharem o redesenho aprovado de Tarefas, Produção, Editor ou Capturas. Garantir comportamentos nas larguras reais, inclusive 1366×768.

### P1-05 — Contratos frágeis fora do Dashboard
- `tests/Feature/CalendarManualContentCreateTest.php` (~84–86): exige strings CSS literais para a grade do calendário. Preservar sete dias e ações, não nomes exatos das classes.
- `tests/Feature/ExternalProductionDemandTest.php` (~366–368): verifica `items-start` e `sm:items-center` em classes.
- `tests/Feature/ProductionStageBadgeVisibilityTest.php` (~90 e 141): compara classes de cor e altura, quando deveria enfatizar etapa, texto legível e sinalização semântica.
- `tests/Feature/NavigationContextStabilizationTest.php` (~163 e 389): contém comparações de HTML com classe e ordem dos atributos rígidas.
- `tests/Feature/Task196AutomaticEditorialProgressTest.php` (~197): exige gradiente literal `from-[#D51A38]`.
- `tests/Feature/OperationalTaskUiTest.php` (~78) e `ContentDeletionTest.php` (~68): verificam padrões de `class` específicos; preservar a presença/semântica das ações, inclusive destrutivas, não depender de uma classe visual particular.
- `tests/Feature/Task208DistinctVisualIdentityTest.php`, `Task203UnifiedOverviewTest.php`, `Task205ProductionOverviewTest.php` e `Task207CapturesOverviewTest.php`: contêm muitos contratos `data-*` para estados/fluxos; verificar a necessidade de cada seletor antes de alterar componentes.

**Correção:** matriz teste-a-teste: “essencial para comportamento/automação” versus “apresentação substituível”; atualizar apenas os últimos. Os atributos `data-*` que suportam ações ou automações **não são automaticamente travas**.

### P1-06 — Código obsoleto e dependências residuais
- `app/Http/Controllers/DashboardController.php` (~74–76, 114–117) ainda calcula/expõe `productionWide`, `taskWide`, `attentionWide` e `agendaWide`; a view atual não usa essas variáveis para spans, mas testes ainda as exigem. São **contratos residuais**, não causa direta do grid atual.
- `resources/js/dashboard-masonry.js` é um stub `Deprecated`, mas `vite.config.js` ainda o inclui nos inputs e testes antigos o referenciam.
- A grade de quatro colunas é fixada pelo Blade atual, não exclusivamente por esse código morto.

**Correção:** remover variáveis e artefatos mortos **em conjunto com os testes que os exigem**, após verificar todos os usos; simplificar build sem deixar referências quebradas.

### P1-07 — JavaScript cria classes fora dos componentes Blade
**Arquivo:** `resources/js/notification-polling.js`.

O polling monta elementos com `element.className` e utilitários literais, inclusive `bg-[var(--ac-selected-bg)]`. Alterar somente o Blade de notificações pode produzir aparência divergente após o primeiro polling.

**Correção:** compartilhar contrato visual/tokens entre marcação server-side e renderizador JS; preservar segurança de URLs, estados e notificações.

## 4. Itens que NÃO devem ser eliminados como “travas”

- Um único `h1` semântico por página e navegação acessível; apenas sua geometria e componente precisam ser flexíveis.
- `aria-current`, foco visível, focus trap, Escape, restauração de foco, scroll-lock e fechamento seguro de modal/drawer.
- Estados de sucesso, perigo, atenção, publicação `failed/uncertain`, visibilidade conforme permissões e isolamento de clientes.
- No Calendário, sete dias por semana e comportamento de reserva/drag-and-drop (pode mudar o CSS).
- Na navegação mobile, preservação dos destinos e acesso à seção “Mais”, mesmo se a apresentação for redesenhada.
- Validação de links, dados, identidade de usuário, contadores, transições e auditoria de workflows.
- Estilos locais necessários ao funcionamento de componentes, como largura percentual de barra de progresso, que não bloqueiam uma nova linguagem visual.
- Os gates de branch, revisão, CI local e autorizações independentes de merge, LAB e produção. **Não usar GitHub Actions**.
- Regras de ESLint (limite de linhas/complexidade) incentivam modularização JavaScript; não foram identificadas como barreira cromática ou de layout.

## 5. Pendências operacionais, sem relação direta com estética

- O Vite compila CSS/JS em bundles com hash; o LAB da Hostinger utiliza o DocumentRoot real em `.../domains/acmarketing.online/public_html/lab/build/`. Houve erro anterior de cópia para diretório alternativo, causando CSS 404. O checklist de deploy deve continuar validando URL/HTTP/MIME dos assets.
- Testes PHP, Node, Pint, ESLint, build e navegador **não foram executados nesta auditoria remota**. Relatórios anteriores de execução são evidência declarada pelo agente, não repetição independente nesta sessão.
- A branch auditada estava à frente da `main` sem commits faltantes na comparação consultada; isso não concede autorização de merge.

## 6. Plano único de resolução — executar uma vez, de forma controlada

### Etapa A — Contrato e governança (não mexer na operação)
1. Fixar SHA-base, branch isolada e inventário dos arquivos afetados.
2. Harmonizar `docs/design-system`, `AGENTS.md`, `CLAUDE.md` e partes realmente conflitantes de `.agents/protocol.md` com o DS v1.1, deixando claro que os **mockups expressamente aprovados** prevalecem sobre explorações. Registrar Antigravity autorizado conforme decisão do usuário, mantendo integralmente as salvaguardas.
3. Publicar um único documento de precedência para que novos agentes não reativem a regra “grafite obrigatório”.

### Etapa B — Substituir testes frágeis por testes efetivos
1. Priorizar `DashboardOperationalTest`, `Task193ReviewLayoutTest`, `ContentMobileLayoutTest`, `DesignSystemComponentsTest`, `design-tokens.test.js`, `NavigationContextStabilizationTest`.
2. Em seguida revisar testes pontuais de Calendário, Produção, Tarefas, Capturas e gradientes.
3. Remover `expect(true)->toBeTrue()` e asserções que só confirmam existência de uma classe; substituir por contratos positivos e verificáveis.
4. Preservar `data-*` necessários, regras de domínio e segurança. **Não reduzir cobertura funcional só para obter verde.**

### Etapa C — Desacoplar geometria e tokens
1. Remover `*Wide` e `dashboard-masonry` apenas depois de verificar usos e adaptar testes.
2. Corrigir os tokens globais contraditórios e introduzir variantes/escopos explícitos para componentes.
3. Eliminar interferências concretas de estilo local, sem fazer migração cega de todos os estilos inline ou redesenhar módulos não aprovados.
4. Reconstruir o Dashboard a partir da composição aprovada, validando estados de muitas tarefas, poucos itens, zero itens, gestoria e colaborador.

### Etapa D — Validação real
1. Executar `git diff --check`, Pint, Pest direcionado e regressão adequada, testes JS, ESLint e Vite, registrando o que realmente rodou.
2. Testar com sessões autenticadas em 1440×900, **1366×768** e 390×844; avaliar desktop/mobile, permissões e conteúdo real ou fixtures controladas.
3. Guardar capturas antes/depois, verificar overflow, densidade, estados vazios, acessibilidade e comportamento de modais.
4. Evidenciar que mudanças no CSS compartilhado não degradaram outras telas; manter matriz de risco por módulo.
5. Entregar um único relatório curto com: arquivos alterados, legados removidos, testes, screenshots, lacunas e riscos. **Nenhum merge/deploy sem autorização separada**.

## 7. Critério de aceite: “destravamento de uma vez”

Considerar concluído somente quando:
- não houver teste ativo cujo objetivo seja exigir a geometria ou cor histórica em desacordo com o novo DS;
- testes de funcionalidade, acessibilidade e permissões continuarem efetivos;
- documentação e governança ativas não se contradisserem;
- tokens/composições compartilhados suportarem o novo visual sem impor o mesmo arranjo a todas as páginas;
- componentes obsoletos comprovados forem eliminados ou isolados;
- o Dashboard reproduzir a direção aprovada com bom comportamento para diferentes quantidades de dados;
- o trabalho for validado em navegador e código no SHA exato.

**Não confundir desbloqueio com redesign simultâneo de todos os módulos.** Esta etapa elimina restrições sistêmicas; a transformação visual das telas segue em entregas controladas para reduzir regressões.

## 8. Handoff direto ao Antigravity

Leia este relatório integralmente e execute **um único plano consolidado** de remoção das restrições visuais legadas confirmadas. Substitua os testes acoplados à estética antiga por verificações reais; preserve segurança, negócio, permissões e acessibilidade. Atualize as fontes de verdade do DS v1.1 e a governança autorizada, remova código morto e desacople componentes compartilhados. Não faça limpeza indiscriminada nem amplie o redesign visual de outros módulos sem necessidade. Depois implemente o Dashboard conforme mockup aprovado e valide com screenshots em estados reais. Sem merge, LAB ou produção sem autorização específica. Reporte achados novos somente se efetivamente demonstrados no código.

---
**Nota de honestidade técnica:** este documento é uma auditoria **estática e abrangente das fontes de bloqueio visual**, não garantia de que não exista nenhum outro problema em runtime ou instrução privada não rastreada no Git. Use-o como base para a correção consolidada e para a validação final por testes e inspeção real.
