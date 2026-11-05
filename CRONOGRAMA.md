# Cronograma de Atividades — Power BI e DAX para Manutenção

| | |
|---|---|
| **Instituição** | CEDTEC Guarapari |
| **Público** | Funcionários da Samarco |
| **Período** | 09 a 13 de novembro de 2026 (segunda a sexta) |
| **Carga horária** | 40 horas — 8 h por dia (08:00 às 16:00) |
| **Horário** | Aulas das 08:00 às 12:00 e das 13:00 às 15:45 · fechamento das 15:45 às 16:00 · almoço das 12:00 às 13:00 |
| **Turma A** | Professor(a): Renzo Fraga — abordagem **fazer → entender** (laboratório primeiro) |
| **Turma B** | Professor(a): Israel Magalhães — abordagem **entender → fazer** (sala primeiro) |
| **Material** | Apostila *Power BI e DAX para Manutenção* (PBD-261025-A), capítulos 1 a 5 |

---

## Revezamento do laboratório

Há um único laboratório de informática. As turmas trocam de espaço na virada de cada bloco, de modo que **o laboratório nunca recebe as duas turmas ao mesmo tempo**. As duas turmas estudam o mesmo conteúdo no mesmo dia, em ordem invertida. Não há intervalos: a troca de espaço acontece no horário de início do bloco seguinte.

| Bloco | Horário | Turma A | Turma B |
|---|---|---|---|
| B1 | 08:00 – 10:00 | 🖥️ **Laboratório** — Parte 1 | 📖 Sala — Parte 1 |
| B2 | 10:00 – 12:00 | 📖 Sala — Parte 1 | 🖥️ **Laboratório** — Parte 1 |
| — | 12:00 – 13:00 | Almoço | Almoço |
| B3 | 13:00 – 14:20 | 🖥️ **Laboratório** — Parte 2 | 📖 Sala — Parte 2 |
| B4 | 14:20 – 15:45 | 📖 Sala — Parte 2 | 🖥️ **Laboratório** — Parte 2 |
| — | 15:45 – 16:00 | Fechamento: checkpoint, dúvida do dia, prévia do próximo dia | Fechamento: checkpoint, dúvida do dia, prévia do próximo dia |

A tarde é mais curta que a manhã, por isso a Parte 2 de cada dia é mais leve.

**Na sala (sem computador):** slides de apoio do dia, pergunta da manutenção, conceito, contas à mão com tabelas pequenas de OS, testes rápidos com exercícios selecionados da apostila.

**No laboratório:** cada aluno constrói o próprio **Painel de Manutenção.pbix**, que cresce todos os dias, com roteiro passo a passo na tela (slides "Como fazer"). São usadas apenas as bases de dados fictícias do curso.

**Checkpoint:** ao fim de cada dia é distribuído um arquivo .pbix com o projeto no ponto esperado, para quem ficou para trás recomeçar dele no dia seguinte.

---

## Dia 1 — Segunda, 09/11 — Do sistema para o Power BI

**Parte 1:** 1.1 Inteligência de negócio nos dados da manutenção · 1.2 Componentes do Power BI e arquivo PBIX · 1.3 Interface do Power BI Desktop · 1.4 Ordem de serviço como linha de tabela
**Parte 2:** 1.5 Importação de planilhas e arquivos CSV · 1.6 Origens de dados e modos de conexão · 2.1 Editor do Power Query e etapas aplicadas · 2.2 Tipos de dados das colunas

| Horário | Turma A | Turma B |
|---|---|---|
| 08:00 – 09:00 | 🖥️ Laboratório — **Prova inicial impressa** (10 questões) | 📖 Sala — **Prova inicial impressa** (10 questões) |
| 09:00 – 10:30 | 🖥️ Laboratório — Abertura; tour guiado pela interface; primeiro gráfico e primeiro .pbix salvo | 📖 Sala — Abertura; 1.1 as seis OS resolvidas de cabeça; 1.2 Desktop, Serviço e Mobile; 1.3 e 1.4 |
| 10:30 – 12:00 | 📖 Sala — 1.1 a 1.4: BI, PBIX, modos de exibição, dado tabular e granularidade | 🖥️ Laboratório — Tour pela interface; primeiro gráfico; conferência da base de OS |
| 13:00 – 14:20 | 🖥️ Laboratório — Importar Excel, CSV e pasta; abrir o Power Query e corrigir tipos | 📖 Sala — 1.5 e 1.6: conectores, Importar × DirectQuery; 2.1 e 2.2: etapas aplicadas e tipos |
| 14:20 – 15:45 | 📖 Sala — 1.5 a 2.2: conectores, modos de conexão, etapas aplicadas e tipos | 🖥️ Laboratório — Importar Excel, CSV e pasta; corrigir tipos no Power Query |
| 15:45 – 16:00 | Fechamento | Fechamento |

- **Desafio:** gráfico de colunas "horas paradas por mês" com agregação implícita.
- **Checkpoint:** base de OS importada e com os tipos de dados corretos.

---

## Dia 2 — Terça, 10/11 — Dado limpo, modelo certo

**Parte 1:** Teoria de conjuntos (40 min, no bloco de sala) · 2.3 Remoção de linhas e colunas indesejadas · 2.4 Padronização de textos e códigos de equipamento · 2.5 Colunas personalizadas e condicionais · 2.6 Mesclagem e acréscimo de consultas
**Parte 2:** 2.7 Conversão de colunas em linhas · 2.8 Perfil e qualidade das colunas · 3.1 Tabelas fato e dimensão no esquema estrela

| Horário | Turma A | Turma B |
|---|---|---|
| 08:00 – 10:00 | 🖥️ Laboratório — Limpeza da exportação suja: remover linhas, padronizar TAG, coluna condicional, mesclar com o cadastro | 📖 Sala — **Teoria de conjuntos** (união, interseção, diferença, Venn); 2.3 a 2.6: limpeza, chave, junções como operações de conjuntos |
| 10:00 – 12:00 | 📖 Sala — **Teoria de conjuntos** (união, interseção, diferença, Venn); 2.3 a 2.6 formalizados; junções como operações de conjuntos | 🖥️ Laboratório — Limpeza da exportação suja: remover linhas, padronizar TAG, coluna condicional, mesclar |
| 13:00 – 14:20 | 🖥️ Laboratório — Despivotar a planilha mensal; perfil de dados; separar fato e dimensões | 📖 Sala — 2.7 e 2.8; 3.1 fato × dimensão (analogia da agenda do celular) e esquema estrela |
| 14:20 – 15:45 | 📖 Sala — 2.7, 2.8 e 3.1: colunas em linhas, perfil de dados, fato × dimensão | 🖥️ Laboratório — Despivotar; perfil de dados; separar fato e dimensões |
| 15:45 – 16:00 | Fechamento | Fechamento |

- **Atividade:** "Caça ao erro" individual — achar e corrigir os problemas de uma base suja, com correção coletiva.
- **Checkpoint:** tabelas limpas, com fato e dimensões separadas.

---

## Dia 3 — Quarta, 11/11 — O painel que responde perguntas

**Parte 1:** 3.2 Relacionamentos e direção do filtro · 3.3 Hierarquias e organização dos campos · 3.4 Escolha do visual pela pergunta da manutenção · 3.5 Gráficos de barras e de linhas · 3.6 Cartão e matriz com formatação condicional
**Parte 2:** 3.7 Segmentação de dados e interação entre visuais · 3.8 Publicação no serviço do Power BI · 3.9 Casos integrados de painel de manutenção

| Horário | Turma A | Turma B |
|---|---|---|
| 08:00 – 10:00 | 🖥️ Laboratório — Relacionamentos no modelo; hierarquia Área > Equipamento; colunas, barras, linhas; cartões e matriz com cor | 📖 Sala — Relacionamento e cardinalidade (retomando conjuntos); jogo "pergunta → visual"; empilhado × clusterizado; formatação condicional |
| 10:00 – 12:00 | 📖 Sala — 3.2 a 3.6 formalizados; jogo "pergunta → visual" | 🖥️ Laboratório — Relacionamentos; hierarquia; gráficos, cartões e matriz com cor |
| 13:00 – 14:20 | 🖥️ Laboratório — Segmentações; editar interações; demonstração da publicação pelo professor | 📖 Sala — 3.7 segmentação e interação; 3.8 publicação e espaços de trabalho; 3.9 caso das horas em dobro |
| 14:20 – 15:45 | 📖 Sala — 3.7 a 3.9: segmentação, publicação, caso das horas em dobro | 🖥️ Laboratório — Segmentações; editar interações; demonstração da publicação pelo professor |
| 15:45 – 16:00 | Fechamento | Fechamento |

- **Desafio:** página "Visão Geral" que responde em 30 segundos três perguntas do gerente de manutenção.
- **Publicação:** demonstrada pelo professor; na Samarco, conta, espaço de trabalho e licença vêm da TI da empresa.
- **Checkpoint:** relatório com duas páginas (visão geral e por equipamento).

---

## Dia 4 — Quinta, 12/11 — Falando DAX

**Parte 1:** 4.1 Modelo de dados visto pelo DAX · 4.2 Colunas calculadas e medidas · 4.3 Sintaxe e funções de agregação · 4.4 Divisão segura e funções lógicas · 4.5 Variáveis e formatação das fórmulas
**Parte 2:** 4.6 Contexto de linha e contexto de filtro · 4.7 Iteradores SUMX e AVERAGEX · 5.1 CALCULATE e a troca de filtros · 5.2 Percentual do total com ALL e REMOVEFILTERS

| Horário | Turma A | Turma B |
|---|---|---|
| 08:00 – 10:00 | 🖥️ Laboratório — Primeiras medidas: total de horas, contagem de OS, custo por hora com DIVIDE, medida com VAR | 📖 Sala — Medida × coluna calculada; sintaxe; agregações conferidas à mão; DIVIDE e BLANK; IF e SWITCH; VAR/RETURN |
| 10:00 – 12:00 | 📖 Sala — 4.1 a 4.5 formalizados; agregações conferidas à mão | 🖥️ Laboratório — Primeiras medidas: total de horas, contagem de OS, custo por hora com DIVIDE, medida com VAR |
| 13:00 – 14:20 | 🖥️ Laboratório — SUMX com RELATED; CALCULATE; percentual do total | 📖 Sala — Contexto de filtro como subconjunto de linhas; contexto de linha; iteradores; CALCULATE; REMOVEFILTERS e ALL |
| 14:20 – 15:45 | 📖 Sala — 4.6 a 5.2 formalizados; "Adivinhe o número" | 🖥️ Laboratório — SUMX com RELATED; CALCULATE; percentual do total |
| 15:45 – 16:00 | Fechamento | Fechamento |

- **Atividade:** "Adivinhe o número" individual — antes de rodar cada medida, cada aluno prevê o resultado.
- **Material extra:** cola de DAX em uma página.
- **Checkpoint:** tabela de medidas organizada.

---

## Dia 5 — Sexta, 13/11 — Indicadores da manutenção

**Parte 1:** 5.3 Tabela de datas para inteligência de tempo · 5.4 Relacionamento inativo com USERELATIONSHIP · 5.5 Acumulado no ano e comparação entre períodos · 5.6 MTBF e MTTR em medidas DAX
**Parte 2:** 5.7 Backlog e aderência ao plano · 5.8 Custo e proporção de preventivas · 5.9 Ranking e Pareto das falhas com RANKX · 5.10 Casos integrados (discussão curta) · Prova final

| Horário | Turma A | Turma B |
|---|---|---|
| 08:00 – 10:00 | 🖥️ Laboratório — Tabela de datas; OS por data de encerramento; acumulado no ano; MTBF, MTTR e disponibilidade | 📖 Sala — Tabela de datas; ativo × inativo; YTD e ano anterior; MTBF, MTTR e disponibilidade calculados à mão |
| 10:00 – 12:00 | 📖 Sala — 5.3 a 5.6 formalizados; indicadores conferidos à mão | 🖥️ Laboratório — Tabela de datas; USERELATIONSHIP; acumulado no ano; MTBF, MTTR e disponibilidade |
| 13:00 – 13:50 | 🖥️ Laboratório — Backlog, % de preventiva, RANKX e Pareto; galeria de painéis | 📖 Sala — 5.7 a 5.9: backlog, aderência, % preventiva, Pareto e ABC; caso 5.10 |
| 13:50 – 14:40 | 📖 Sala — 5.7 a 5.9 formalizados; caso 5.10 | 🖥️ Laboratório — Backlog, % de preventiva, RANKX e Pareto; galeria de painéis |
| 14:45 – 15:45 | 📖 Sala — **Prova final impressa** (10 questões) | 🖥️ Laboratório — **Prova final impressa** (10 questões) |
| 15:45 – 16:00 | Encerramento | Encerramento |

- **Galeria de painéis:** 3 a 4 painéis de alunos mostrados no projetor (10 minutos).
- **Pergunta de encerramento:** "Qual equipamento pede atenção primeiro, e por quê?"

---

## Avaliação

| | Prova inicial | Prova final |
|---|---|---|
| **Data e horário** | Segunda, 09/11 — 08:00 às 09:00 | Sexta, 13/11 — 14:45 às 15:45 |
| **Formato** | Impressa, modelo de avaliação do CEDTEC | Impressa, modelo de avaliação do CEDTEC |
| **Questões** | 10 (5 objetivas e 5 discursivas, 2 por capítulo), 1,0 ponto cada | 10 (mesmos temas e mesma dificuldade), 1,0 ponto cada |
| **Valor** | 10,0 | 10,0 |
| **Local** | Turma A no laboratório, Turma B na sala | Turma A na sala, Turma B no laboratório |
| **Objetivo** | Diagnóstico do ponto de partida | Medir o aprendizado e comparar com a prova inicial |

A comparação entre as duas provas mostra o ganho de cada aluno e de cada turma, e também permite comparar as abordagens "fazer → entender" (Turma A) e "entender → fazer" (Turma B).

---

## Checklist antes do curso (até 06/11)

- [ ] Power BI Desktop atualizado em todas as máquinas do laboratório do CEDTEC (Windows 64 bits).
- [ ] Bases de dados fictícias do curso copiadas em pastas separadas por turma.
- [ ] Demonstração de publicação no serviço do Power BI testada pelo professor.
- [ ] Provas inicial e final impressas para as duas turmas.
- [ ] Arquivos de checkpoint dos dias 1 a 4 preparados.
- [ ] Projetor testado na sala e no laboratório.
- [ ] Slides do dia abertos no Google Slides em sala e no laboratório.
