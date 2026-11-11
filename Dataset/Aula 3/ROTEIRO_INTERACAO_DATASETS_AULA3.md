# Roteiro Prático: Interação com os Datasets da Aula 3 no Power BI Desktop
**Curso:** Power BI e DAX para Manutenção (CEDTEC Guarapari · Samarco)  
**Apresentação de Referência:** [`Slides/Power BI e DAX para Manutenção — Dia 3 · O painel que responde perguntas.pptx`](../../Slides/Power%20BI%20e%20DAX%20para%20Manuten%C3%A7%C3%A3o%20%E2%80%94%20Dia%203%20%C2%B7%20O%20painel%20que%20responde%20perguntas.pptx) *(Slides 1 a 26)*  
**Arquivos de Dados na Pasta `Dataset/Aula 3/`:**
- [`Dataset_PowerBI_Manutencao.xlsx`](./Dataset_PowerBI_Manutencao.xlsx) *(Modelo com `OS`, `Equipamentos` — incluindo `RA-01`, `EM-01`, `EM-02`, `CN-01`, `SL-02` — e `Calendario`)*
- [`OS_Marco_Auditoria.csv`](./OS_Marco_Auditoria.csv) *(48 OS de março somando 92,0 h — para o Caso Integrado dos Slides 22 a 24)*
- [`Alocacao_Tecnicos_OS.csv`](./Alocacao_Tecnicos_OS.csv) *(63 linhas de alocação com duplas de técnicos — para o Caso das "Horas em Dobro" dos Slides 22 a 24)*

---

## Mapa Prático Alinhado aos 26 Slides da Aula 3

| Bloco dos Slides | Seções | O que fazer no Power BI Desktop |
| :--- | :---: | :--- |
| **Slides 4 a 8** | **3.2** | Ligar `Equipamentos (1) ➔ (*) OS` com direção **Único** e testar segmentações como interseção ($\cap$) e união ($\cup$). |
| **Slides 9 a 12** | **3.3 · 3.4** | Criar a hierarquia `Área › Equipamento` (abrindo `Pátio` em `EM-01`, `EM-02` e `CN-01`) e escolher os visuais para as 4 perguntas do Teste Rápido. |
| **Slides 13 a 17** | **3.5 · 3.6** | Montar colunas empilhadas vs clusterizadas, corrigir a pegadinha do `Número da OS` (Contagem) e aplicar o semáforo na Matriz (`≤15h` verde, `15–30h` amarelo, `>30h` vermelho). |
| **Slides 18 a 21** | **3.7 · 3.8** | Configurar segmentações, `Formato › Editar interações`, Painel Filtros em 3 níveis e demonstrar a publicação no Espaço de Trabalho. |
| **Slides 22 a 26** | **3.9** | Reproduzir e corrigir o caso das **131,5 h vs 92,0 h** e fechar o **Checkpoint** (relatório com 2 páginas: *Visão Geral* e *Por Equipamento*). |

---

# BLOCO 1: Relacionamentos e Direção do Filtro (Slides 4 a 8 · Seção 3.2)

### Passo 1.1: Importar as Tabelas e Ver a Situação das "Barras da Mesma Altura" (Slide 5)
1. Abra o **Power BI Desktop** e clique em **Página Inicial** ➔ **Pasta de Trabalho do Excel**.
2. Selecione [`Dataset_PowerBI_Manutencao.xlsx`](./Dataset_PowerBI_Manutencao.xlsx) e marque `OS`, `Equipamentos` e `Calendario`. Clique em **Carregar**.
3. Vá até a **Exibição de Modelo** (3º ícone à esquerda). Se o Power BI tiver criado alguma linha automática entre `Equipamentos` e `OS`, clique com o botão direito nela e escolha **Excluir**.
4. Volte para a **Exibição de Relatório** e crie um **Gráfico de Colunas**:
   - **Eixo X:** `Equipamentos[Área]`
   - **Eixo Y:** `OS[Horas Paradas]`
5. **Conferência com o Slide 5:** Sem ligação entre `OS` e `Equipamentos`, a soma das OS não sabe separar por área — **todas as barras ficam exatamente da mesma altura**, mostrando o total geral repetido!

---

### Passo 1.2: Criar o Relacionamento `*:1` com Direção `Único` (Slides 6 e 7)
1. Volte para a **Exibição de Modelo**.

![Esquema Estrela](./imagens/fig_3_03_esquema_estrela.png)
*Figura 1: Tabelas organizadas no Esquema Estrela (OS no centro, Equipamentos e Calendário ao redor).*

2. Siga um dos dois caminhos do **Slide 6**:
   - **Caminho 1 (Arrastar):** Arraste a coluna `Equipamento` da tabela `Equipamentos` (cadastro) e solte exatamente sobre a coluna `Equipamento` da tabela `OS`.
   - **Caminho 2 (Menu):** Guia **Modelagem** ➔ **Gerenciar relações** ➔ **Novo**.
3. Dê duplo clique na linha criada para conferir a leitura do **Slide 6 e Slide 7**:
   - **Cardinalidade:** `Muitos para um (*:1)` — muitas OS para um único equipamento.
   - **Direção do filtro cruzado:** `Único` (a área filtra as OS).
   - > [!WARNING]
     > **Pegadinha do Slide 7:** Nunca mude para `Ambas` sem necessidade! Com `Único` (o padrão), escolher um mês nas OS não encurta a lista de equipamentos cadastrados. Com `Ambas`, o filtro vai nos dois sentidos e o resultado fica difícil de prever.

![Editar Relacionamento](./imagens/fig_3_04_editar_relacionamento.png)
*Figura 2: Janela Editar Relacionamento confirmando cardinalidade *:1 e direção Único.*

4. Repita o processo arrastando `Calendario[Data]` sobre `OS[Data de Abertura]`.
5. No painel **Dados**, clique com o botão direito em `OS[Equipamento]` (na tabela fato) e escolha **Ocultar da visualização de relatório** para que apenas a coluna do cadastro seja usada nos gráficos.

![Ocultar Chaves](./imagens/fig_3_08_ocultar_renomear.png)
*Figura 3: Ocultando a chave estrangeira na tabela fato.*

---

### Passo 1.3: Testar Segmentações como Interseção e União (Slide 8)
1. Na **Exibição de Relatório**, adicione duas **Segmentações de dados**:
   - Segmentação 1: `Equipamentos[Área]`
   - Segmentação 2: `Calendario[Nome do Mês]`
2. **Teste de Interseção ($\cap$) do Slide 8:**
   - Clique em `Área = Pátio` na primeira segmentação **E** em `Mês = março` na segunda.
   - O gráfico mostra apenas as OS que estão **nos dois conjuntos ao mesmo tempo**!
3. **Teste de União ($\cup$) do Slide 8:**
   - Na mesma segmentação de `Área`, segure a tecla `Ctrl` e clique em `Pátio` **OU** `Pelotização` (ou `Forno`).
   - Selecionar várias opções dentro da mesma segmentação **junta os conjuntos (união)**!

---

# BLOCO 2: Hierarquias e Escolha do Visual (Slides 9 a 12 · Seções 3.3 e 3.4)

### Passo 2.1: Criar a Hierarquia `Área › Equipamento` (Slide 10)
1. No painel **Dados** (à direita), expanda a tabela `Equipamentos`.
2. Clique com o botão direito sobre **Área** ➔ selecione **Criar hierarquia**.
3. Clique com o botão direito sobre **Equipamento** ➔ **Adicionar à hierarquia** ➔ **Hierarquia Área**.

![Hierarquia](./imagens/fig_3_07_hierarquia.png)
*Figura 4: A hierarquia Área › Equipamento e a hierarquia automática de Datas (Ano › Trimestre › Mês › Dia).*

4. Arraste a `Hierarquia Área` para o **Eixo X** do seu gráfico de colunas.
5. No cabeçalho superior direito do gráfico, clique na **seta para baixo** (*Ativar o Drill Down*) e clique sobre a coluna da área **Pátio**:
   - **Conferência com o Slide 10:** A área **Pátio** se abre imediatamente mostrando seus equipamentos: **`EM-01`**, **`EM-02`** e **`CN-01`**!
6. Clique na **seta para cima** (*Subir nível*) para retornar à visão por Área.

![Drill Down](./imagens/fig_3_23_drill_down.png)
*Figura 5: As setas no cabeçalho do visual que descem e sobem os níveis da hierarquia.*

---

### Passo 2.2: Prática do Teste Rápido — Construindo o Visual de Cada Pergunta (Slides 11 e 12)
Monte na sua página os 4 visuais que respondem às 4 perguntas do **Slide 12**:

| # no Slide 12 | Pergunta da Manutenção | Família (Slide 11) | Como Montar no Power BI |
| :-: | :--- | :--- | :--- |
| **1** | **Total de horas paradas de março** | *Mostrar um número* | Visual **Cartão** com `OS[Horas Paradas]` filtrado em `março`. |
| **2** | **Qual equipamento da Moagem parou mais** | *Comparar* | **Gráfico de Barras/Colunas** com `Área › Equipamento` e drill-down na `Moagem`. |
| **3** | **As corretivas estão aumentando desde janeiro?** | *Acompanhar no tempo* | **Gráfico de Linhas** com `Mês` no Eixo X e `Horas Paradas` no Eixo Y. |
| **4** | **Lista das OS abertas do silo `SL-02`** | *Detalhar* | Visual **Tabela** com `Número da OS`, `Equipamento`, `Status` e `Descrição`, filtrado para `SL-02` e `Status = Aberta` (exibe as OS `51908` e `51909`). |

![Layout da Página](./imagens/fig_3_11_layout_painel.png)
*Figura 6: Disposição dos visuais na página seguindo a ordem natural de leitura.*

---

# BLOCO 3: Gráficos, Cartões e Matriz com Semáforo (Slides 13 a 17 · Seções 3.5 e 3.6)

### Passo 3.1: Comparar Colunas Empilhadas × Colunas Clusterizadas (Slide 14)
1. Selecione o gráfico de colunas de horas por área.
2. Arraste `OS[Tipo de Manutenção]` para o campo **Legenda** do visual.
3. No painel **Visualizações**, alterne entre os dois ícones lado a lado para observar a diferença do **Slide 14**:
   - **Gráfico de colunas empilhadas:** Cada coluna mostra o **total da área**, e as cores mostram quanto veio de Corretiva, Preventiva e Preditiva (*Responde: quanto a área parou no total?*).
   - **Gráfico de colunas clusterizadas:** Os tipos ficam **lado a lado** em cada área (*Responde: qual tipo pesa mais dentro de cada área?*).

![Colunas Empilhadas](./imagens/fig_3_12_colunas_empilhadas.png)
*Figura 7: Gráfico de colunas empilhadas dividindo cada área por Tipo de Manutenção na Legenda.*

---

### Passo 3.2: Corrigir a Pegadinha do `Número da OS` no Cartão (Slide 15)
1. Insira um novo **Cartão** no topo da página.
2. Arraste o campo `OS[Número da OS]` para o Cartão.
3. **Observe a Pegadinha do Slide 15:** Como `Número da OS` é um código numérico (`51020`, `51021`...), o Power BI tenta **somar** os códigos, exibindo um número na casa dos milhões (`43,89 Mi`) que não significa nada!
4. **Como corrigir:**
   - Clique na setinha ao lado de `Soma de Número da OS` no painel **Visualizações**.
   - Troque o resumo do campo de *Soma* para **Contagem** (ou *Contagem distinta*).
   - Agora o cartão mostra exatamente quantas OS existem!

![Lista de Resumos](./imagens/fig_3_15_lista_resumos.png)
*Figura 8: Trocando a agregação padrão de Soma para Contagem no painel Visualizações.*

---

### Passo 3.3: Como Fazer — Matriz com Semáforo (Slides 16 e 17)
Siga exatamente os 3 passos do **Slide 17**:
1. **Passo 1:** Em **Visualizações**, escolha **Matriz** e preencha:
   - **Linhas:** `Equipamentos[Área]`
   - **Colunas:** `OS[Tipo de Manutenção]`
   - **Valores:** `OS[Horas Paradas]`
2. **Passo 2:** No painel Visualizações, clique com o botão direito (ou na setinha) em `Horas Paradas` (dentro de *Valores*) ➔ **Formatação condicional** ➔ **Cor da tela de fundo**.
3. **Passo 3 (Regras do Semáforo do Slide 17):**
   - Na janela que abrir, mude *Estilo do formato* de Gradiente para **Regras**.
   - Configure as 3 faixas exatas do slide (trocando `%` para `Número` em todas):
     - Regra 1: `Se o valor for >= 0` (Número) `e <= 15` (Número) ➔ **Cor Verde** (`#8CC63F` ou verde suave).
     - Regra 2: `Se o valor for > 15` (Número) `e <= 30` (Número) ➔ **Cor Amarela** (`#F2C80F` ou amarelo suave).
     - Regra 3: `Se o valor for > 30` (Número) `e <= Máximo` (Número) ➔ **Cor Vermelha** (`#E66C37` ou vermelho alerta).
   - Clique em **OK**.

![Matriz com Semáforo](./imagens/fig_3_19_barras_icones.png)
*Figura 9: A formatação condicional destaca imediatamente na matriz onde estão as maiores concentrações de horas.*

---

# BLOCO 4: Filtros, Interações e Publicação (Slides 18 a 21 · Seções 3.7 e 3.8)

### Passo 4.1: Segmentações, Editar Interações e Painel Filtros (Slide 19)
1. **Segmentação para o leitor:** Posicione à esquerda as segmentações de `Área` (estilo Lista ou Mosaico) e `Mês` (estilo Suspensa ou Mosaico).
2. **Editar Interações (`Guia Formato`):**
   - Clique sobre o gráfico de colunas por Área.
   - Vá na guia superior **Formato** ➔ clique em **Editar interações**.
   - Nos outros visuais da página (como o Gráfico de Linhas), clique no ícone de **Funil (Filtrar)** no lugar de *Realçar*.

![Editar Interações](./imagens/fig_3_21_editar_interacoes.png)
*Figura 10: Escolhendo o que o clique em um visual faz nos demais (Filtrar, Realçar ou Nenhum).*

3. **Responder à Pergunta do Dia (Slide 1) em 10 segundos:**
   - Filtre `Mês = março` ou clique no equipamento **`RA-01`**:
   - Veja no Cartão que o **`RA-01` parou `16,5 horas` em março** (`10,5 h` de corretiva + `6,0 h` de preventiva).
   - Veja no Gráfico de Linhas que as horas do `RA-01` estão **subindo** (`6,5 h` em janeiro ➔ `11,0 h` em fevereiro ➔ `16,5 h` em março)!
4. **Painel Filtros em 3 Níveis:**
   - Abra o painel **Filtros** à direita e observe os três alcances do **Slide 19**: *Filtros neste visual*, *Filtros nesta página* e *Filtros em todas as páginas*.

![Painel Filtros](./imagens/fig_3_21_painel_filtros.png)
*Figura 11: Os três níveis de alcance do painel Filtros.*

---

### Passo 4.2: Criar a Segunda Página ("Por Equipamento") e Sincronizar (Slide 26)
1. Renomeie a primeira aba para **Visão Geral**.
2. Clique com o botão direito na aba **Visão Geral** ➔ **Duplicar página** e renomeie a nova aba para **Por Equipamento**.
3. Na página **Por Equipamento**, troque o eixo de Área por `Equipamentos[Equipamento]` e adicione uma **Tabela** detalhada das OS na base.
4. Na guia **Exibição** ➔ marque **Sincronizar segmentações** para que a escolha de `Área` e `Mês` funcione integrada nas duas páginas.

---

### Passo 4.3: Demonstração da Publicação no Serviço (Slides 20 e 21)
1. Salve o arquivo `.pbix` no computador.
2. Na guia **Página Inicial**, localize o botão **Publicar** (conforme demonstrado pelo professor no projetor, seguindo a orientação do **Slide 21**).

![Publicar no Serviço](./imagens/fig_3_26_publicar.png)
*Figura 12: Publicando o relatório para um Espaço de Trabalho no Serviço do Power BI.*

---

# BLOCO 5: Caso Integrado — "Horas em Dobro" (Slides 22 a 24 · Seção 3.9)

Vamos reproduzir no Power BI a situação exata dos **Slides 22, 23 e 24** usando os arquivos [`OS_Marco_Auditoria.csv`](./OS_Marco_Auditoria.csv) e [`Alocacao_Tecnicos_OS.csv`](./Alocacao_Tecnicos_OS.csv):

### Passo 5.1: Conferir o Valor Original da Exportação (`92,0 h`)
1. Na guia **Página Inicial** ➔ **Texto/CSV**, importe [`OS_Marco_Auditoria.csv`](./OS_Marco_Auditoria.csv) e [`Alocacao_Tecnicos_OS.csv`](./Alocacao_Tecnicos_OS.csv).
2. Coloque um **Cartão** na tela com `OS_Marco_Auditoria[Horas Paradas]`:
   - **Conferência (Slide 23):** O cartão mostra **`92,0 h`** (de 48 OS).

### Passo 5.2: Reproduzir o Erro da Mesclagem (`131,5 h`)
1. Abra o **Power Query** (**Página Inicial** ➔ **Transformar dados**).
2. Selecione `OS_Marco_Auditoria` ➔ **Mesclar Consultas** ➔ ligue pelo `Número da OS` com a tabela `Alocacao_Tecnicos_OS` e clique em **OK**.
3. Expanda a coluna `Técnico` e clique em **Fechar e Aplicar**.
4. **Conferência com o Slide 23:**
   - O cartão salta exatamente de **`92,0 h`** para **`131,5 h`**!
   - As empilhadeiras `EM-01` e `EM-02` saltam de **`23,0 h`** para **`41,5 h`**!

### Passo 5.3: Aplicar a Correção do Slide 24
1. Volte ao **Power Query** e remova as duas etapas da mesclagem em `OS_Marco_Auditoria` (voltando para **48 linhas**, uma linha por OS).
2. Clique em **Fechar e Aplicar**.
3. Na **Exibição de Modelo**, ligue `OS_Marco_Auditoria[Número da OS]` (`1`) à tabela separada `Alocacao_Tecnicos_OS[Número da OS]` (`*`).
4. **Conferência final:** O cartão volta para **`92,0 h`**, batendo 100% com a exportação de origem!
