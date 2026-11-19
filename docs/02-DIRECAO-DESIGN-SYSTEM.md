# 02 — Direção visual e regras do Design System

**Natureza:** especificação de direção, não inventário completo de tokens/componentes já implementados.

## 1. Princípios

1. **Clareza antes de ornamentação:** a prioridade operacional fica evidente em segundos.
2. **Identidade premium sem excesso:** visual autoral, com luz, contraste, equilíbrio e textura controlada.
3. **Social-first sem imitar rede social:** previews reais, status de publicação e formato de conteúdo ajudam a operar; não transformar o sistema em feed recreativo.
4. **Informação progressiva:** mostrar o necessário, revelar detalhes sob demanda.
5. **Comportamentos verdadeiros:** qualquer número, gráfico ou estado deve corresponder a uma fonte real.
6. **Consistência operacional:** mudanças visuais não podem eliminar a capacidade de completar tarefas.

## 2. Paleta e superfícies

**Preferência visual discutida:** sidebar vinho profundo, marca vermelho AC, canvas quase branco/cinza muito claro, superfícies brancas/off-white e sombras finas. Equilibrar vermelho e neutros para manter legibilidade e diminuir fadiga.

Valores discutidos em mockups, **provisórios e sujeitos a validação com identidade oficial**:
- Vinho profundo exploratório: `#570D22`.
- Vermelho/acento exploratório: `#C21835`.
- Canvas exploratório: `#F5F6F9`.
- Superfícies: branco e off-white.

Não incorporar esses valores como verdade oficial sem comparar com ativos reais da marca. Criar tokens semânticos para marca, fundo, superfície, texto, borda, seleção, sucesso, informação, atenção e erro; não usar vermelho de marca automaticamente para erro.

## 3. Shell e navegação

- Navegação forte e reconhecível, com sidebar vinho em desktop e estrutura mobile própria.
- Menu organizado por fluxo de trabalho, não por lista arbitrária de módulos.
- Seleção atual perceptível com mais do que cor; iconografia estável e rótulos claros.
- Busca global/ações rápidas somente se existirem rotas ou lógica reais.
- Não adicionar banner autopromocional, foguete ou cards decorativos fixos na sidebar.
- Perfil e notificações em local coerente; reduzir sobreposição com o dashboard.

## 4. Composição do Dashboard

**A mudança esperada é arquitetural, não cosmética.** Evitar template fixo “quatro KPI + duas colunas de listas”. Uma possibilidade a estudar:

- Header conciso com identificação da página, período e ações úteis.
- Uma **área principal de trabalho** com os itens prioritários do usuário, formatos/miniaturas reais quando disponíveis, estados e próximas ações.
- Uma **área secundária compacta** para agenda e alertas que exijam intervenção.
- Indicadores selecionados por utilidade, não por preencher uma fileira.
- Recortes discretos de publicações e acesso à Central; não duplicar sua timeline completa.
- Minuto de Sabedoria preservado, visualmente distinto e sem competir com alertas críticos.
- Atividades recentes secundárias, recolhíveis quando adequado.

A quantidade exata de blocos deve ser decidida a partir dos dados e da largura real da janela. **Menos módulos visíveis não significa remover funcionalidades.**

## 5. Conteúdo como linguagem visual

- Miniaturas de posts, carrosséis e Reels **somente quando a mídia realmente estiver disponível**.
- Fallbacks elegantes para conteúdos sem mídia, sem thumbnails inventadas.
- Status pequenos, legíveis e consistentes (Produzindo, Revisão, Aprovado, Agendado, Postado, Falhou, Incerto).
- Agenda com tempos e datas reais e timezone coerente.
- Prioridades legíveis, com alerta justificado por lógica de negócio.

## 6. Tipografia e componentes

- Família sans moderna e bem legível; títulos com presença editorial, sem excesso de peso.
- Escala tipográfica reduzida a níveis realmente necessários: título de página, título de seção, corpo, metadados.
- Tamanhos mínimos de leitura em notebook; evitar copiar microtextos da imagem gerada.
- Ícones padronizados e proporcionais, sem misturar várias bibliotecas/estéticas.
- Cards com bordas e sombras sutis; evitar arredondamentos exagerados em todo componente.
- Botões primários reservados à ação mais importante da área; secundários discretos.
- Filtros e busca compactos; não ocupar mais área que a lista principal.

## 7. Motion, efeitos e 3D

Microinterações são permitidas quando comunicam mudança de estado, foco, abertura, carregamento ou transição. Respeitar `prefers-reduced-motion`. Evitar pulso contínuo meramente ornamental, gráficos animados sem dados e ilustrações 3D grandes em toda seção. 3D pode ser usado em momentos raros de branding ou vazio, não como decoração operacional diária.

## 8. Responsividade e acessibilidade

Projetar de verdade para 1440px, **1366×768**, tablet e ~390px. No notebook, priorizar conteúdo acima da dobra, sem estreitar textos para “caber” quatro colunas. No mobile, preservar ações e contexto sem miniaturizar o desktop.

Garantir navegação por teclado, foco visível, contraste, estados hover/pressed/disabled, textos alternativos, feedback de erro, empty/loading e não depender exclusivamente de cor.

## 9. O que uma imagem NÃO especifica

Mockups têm valores e imagens **ilustrativos**. Não inferir métricas reais, performance, suporte a novos canais, integração, gráfico, automação ou calendário a partir do desenho. Para cada componente, a implementação deve mapear:
- qual dado real o alimenta;
- qual ação executa;
- seu estado vazio, erro e carregando;
- permissões;
- comportamento no mobile.

Sem backend real, mostrar fallback honesto em vez de inventar valores.

## 10. Critério para aprovação

Um usuário deve abrir o Dashboard e reconhecer um **produto visualmente novo**, sem reaproveitamento óbvio do painel administrativo antigo; deve conseguir localizar prioridades, ações e agenda com menos esforço; e deve continuar concluindo fluxos existentes sem passos extras.
