# Caderno de Atividades, Conferência de Números e Gabarito Comentado — Aulas 2 e 3
**Curso:** Power BI e DAX para Manutenção (CEDTEC Guarapari · Samarco)  
**Público:** Instrutor e Alunos (Apoio de Sala e Laboratório)  
**Escopo:**
1. Atividade **"Caça ao Erro"** da Aula 2 (`Exportacao_OS_Lote2.csv` e `complemento_2-7.csv`) com gabarito de conferência.
2. Gabarito Comentado dos **Exercícios do Capítulo 2** da Apostila (Página 53).
3. Desafio dos **30 Segundos** e Conferência Numérica da Aula 3 (`Dataset_PowerBI_Manutencao.xlsx`, `OS_Marco_Auditoria.csv` e `Alocacao_Tecnicos_OS.csv`).
4. Gabarito Comentado dos **Exercícios do Capítulo 3** da Apostila (Página 86).

---

# 1. Atividade "Caça ao Erro" — Aula 2 (Gabarito do Professor)

### A. Auditoria do Arquivo [`Dataset/Aula 2/Exportacao_OS_Lote2.csv`](../Dataset/Aula%202/Exportacao_OS_Lote2.csv)
O arquivo bruto tem **26 linhas** no bloco de notas, mas apenas **19 ordens de serviço reais e únicas**. Abaixo estão os **7 erros plantados** que os alunos devem encontrar e corrigir no Power Query:

| # | Erro Escondido no Arquivo | Onde Está | Impacto se NÃO Corrigir | Como Corrigir no Power Query |
| :-: | :--- | :--- | :--- | :--- |
| **1** | **Cabeçalho corporativo do SIGMA** | Linhas 1 a 4 | As colunas ficam sem nome (`Column1`, `Column2`...) e os tipos viram Texto. | `Remover Linhas Principais (4)` + `Usar a Primeira Linha como Cabeçalho` |
| **2** | **Linha de `Total Geral` no rodapé** | Linha 26 (`66,92 h` e `R$ 86.271,90`) | Dobra o total de horas e de custo do lote nos cartões do relatório. | `Remover Linhas Inferiores (1)` ou filtrar `Número da OS <> "Total Geral"` |
| **3** | **OS Duplicada na exportação** | Linhas 14 e 15 (OS `51878` - `PR-02`) | Conta 2 falhas no guincho `PR-02` e soma `3,5 h` e `R$ 4.320,00` duas vezes. | Selecionar coluna `Número da OS` ➔ Botão direito ➔ `Remover Duplicadas` |
| **4** | **TAG sem hífen (`TR 01`)** | Linha 7 (OS `51871`) | Não encontra correspondência no cadastro (`TR-01`) na hora de mesclar ou relacionar. | `Transformar` ➔ `Substituir Valores` (`TR 01` por `TR-01`) |
| **5** | **TAG com espaço no fim (`TR-02 `)** | Linha 17 (OS `51880`) | Cria uma barra duplicada no gráfico para a mesma máquina por causa do espaço invisível. | Botão direito na coluna `Equipamento` ➔ `Transformar` ➔ `Cortar` (*Trim*) |
| **6** | **TAG com descrição (`MO-02 - Moinho...`)** | Linha 22 (OS `51885`) | Impede o relacionamento com o código limpo `MO-02` da tabela `Equipamentos`. | `Transformar` ➔ `Extrair` ➔ `Texto Antes do Delimitador` (`" - "`) |
| **7** | **Texto na coluna numérica (`5,5 h`)** | Linha 9 (OS `51873` - `FO-01`) | Gera célula com `Error` (vermelho) ao converter `Horas Paradas` para Número Decimal. | `Substituir Valores` (trocar `" h"` por vazio) antes de converter o tipo |

#### Números Finais Esperados após a Limpeza de `Exportacao_OS_Lote2.csv`:
- **Total de linhas (OS únicas):** `19 ordens de serviço` (OS `51870` até `51888`).
- **Soma de Horas Paradas (limpa, sem a duplicata da 51878 e com os 5,5 h do FO-01):** **`68,92 horas`** *(Nota: o rodapé sujo marcava 66,92 porque somava a duplicata de 3,5h, mas ignorava o texto `"5,5 h"`! $66,92 - 3,5 + 5,5 = 68,92\text{ h}$).*
- **Soma de Horas Previstas (limpa):** **`83,0 horas`**.
- **Soma de Custo (limpa):** **`R$ 96.151,90`**.

---

### B. Auditoria do Arquivo [`Dataset/Aula 2/complemento_2-7.csv`](../Dataset/Aula%202/complemento_2-7.csv)
- **Linhas na matriz original:** `6 equipamentos` (`BR-01`, `BR-02`, `MO-01`, `MO-02`, `FO-01`, `PE-01`).
- **Erro estrutural 1:** Meses (`Jan`, `Fev`, `Mar`, `Abr`) estão no cabeçalho das colunas.
- **Erro estrutural 2 (Armadilha):** Coluna `Total` à direita (`51.0`, `27.0`, `69.5`, `65.0`, `43.0`, `46.0`).
- **Resultado esperado após remover `Total` e aplicar *Transformar outras colunas em linhas*:**
  - **Total de linhas:** **`24 linhas`** ($6\text{ equipamentos} \times 4\text{ meses}$).
  - **Soma total de Horas Paradas no quadrimestre:** **`301,5 horas`** *(Se o aluno esquecer de apagar a coluna `Total`, a tabela ficará com 30 linhas e a soma dobrará para `603,0 horas`!).*

---

# 2. Gabarito Comentado — Exercícios do Capítulo 2 da Apostila (Página 53)

### Exercício 1 — Etapas Aplicadas na Atualização
> *O que acontece com as etapas aplicadas de uma consulta quando o arquivo de exportação do mês seguinte substitui o anterior e o botão Atualizar é acionado?*
- ✅ **Alternativa Correta: B)** *As etapas são executadas de novo, na mesma ordem, sobre o arquivo novo, sem alterar o arquivo de origem.*
- **Por que explicar assim na sala:** O Power Query é uma "receita gravada" que apenas **lê** o Excel/CSV. Ele nunca modifica a planilha original na pasta da rede (o que elimina a C), não apaga o trabalho feito (elimina a A) e não exige abrir código M manualmente (elimina a D).

### Exercício 2 — Cálculo de Duração entre Datas/Horas
> *Uma OS tem Início em `10/04/2026 22:30` e Fim em `11/04/2026 03:15`. Aplicado `Fim - Início` e `Duração > Total de Horas`, qual é o valor obtido?*
- ✅ **Alternativa Correta: A) `4,75`**
- **Conta no quadro branco:**
  - Das `22:30` até `00:00` (meia-noite) = `1 hora e 30 minutos`.
  - Da `00:00` até `03:15` = `3 horas e 15 minutos`.
  - Soma = `4 horas e 45 minutos`.
  - **Pegadinha clássica da manutenção:** `45 minutos` em decimal **não é `,45`**! Como $45 \div 60 = 0,75$, o Total de Horas em número decimal é **`4,75 horas`** (a alternativa C `4,45` é a pegadinha de quem confunde minuto no relógio com decimal!).

### Exercício 3 — Ordem das Cláusulas na Coluna Condicional
> *Uma coluna condicional `Faixa de Duração` tem a 1ª regra `"Horas Paradas < 8 ➔ Média"`, a 2ª regra `"Horas Paradas < 2 ➔ Curta"` e `Senão ➔ Longa`. Qual é o resultado para uma OS com `1,5 hora`, e por quê?*
- ✅ **Alternativa Correta: B)** *Média, porque as cláusulas são testadas de cima para baixo e a primeira verdadeira encerra o teste.*
- **Explicação:** Como `1,5` é menor que `8`, a primeira condição dá verdadeiro na hora e o Power Query nem chega a ler a segunda linha! Para funcionar, a menor faixa (`< 2`) precisa vir primeiro.

### Exercício 4 — Despivotar Preparado para Novos Meses
> *Uma planilha tem `Equipamento` e uma coluna por mês, ganhando coluna nova a cada mês. Qual a forma certa de transformar as colunas de mês em linhas para continuar correta quando abril chegar?*
- ✅ **Alternativa Correta: C)** *Selecionar a coluna `Equipamento` e escolher **Transformar outras colunas em linhas**, que alcança as colunas de mês que ainda não existem.*

---

# 3. Conferência Numérica da Aula 3 — Valores Exatos dos Painéis

### A. Números Oficiais do Modelo Principal ([`Dataset_PowerBI_Manutencao.xlsx`](../Dataset/Aula%203/Dataset_PowerBI_Manutencao.xlsx))
Use esta tabela para conferir se o painel dos alunos bateu exatamente com o esperado:

#### Cartões de Topo (Sem Filtros — Base Completa vs Sem Canceladas)
| Indicador (Cartão) | Base Completa (Todas as OS) | Filtrando sem `Cancelada` (`Encerrada` + `Aberta`) |
| :--- | :---: | :---: |
| **Total de OS (Contagem)** | **850 OS** *(707 Encerradas, 96 Abertas, 47 Canceladas)* | **803 OS** |
| **Total de Horas Paradas (Soma)** | **4.002,24 h** *(OS abertas e canceladas têm 0 h)* | **4.002,24 h** |
| **Custo Total de Manutenção (Soma)** | **R$ 4.074.608,64** | — |

#### Matriz: Horas Paradas por `Área` × `Tipo de Manutenção`
| Área da Planta | Corretiva (h) | Preventiva (h) | Preditiva (h) | **Total da Área (h)** |
| :--- | :---: | :---: | :---: | :---: |
| **1. Britagem** | **709,09** 🔴 | 380,45 | 134,80 | **1.224,34 h** |
| **2. Beneficiamento** | **456,53** 🔴 | 325,85 | 109,41 | **891,79 h** |
| **3. Moagem** | 320,78 🟡 | 134,78 | 47,56 | **503,12 h** |
| **4. Filtragem** | 250,32 🟡 | 133,96 | 46,86 | **431,14 h** |
| **5. Concentração** | 191,50 🟢 | 156,87 | 35,08 | **383,45 h** |
| **6. Pelotização** | 160,31 🟢 | 172,91 | 38,46 | **371,68 h** |
| **7. Utilidades** | 104,92 🟢 | 67,18 | 24,62 | **196,72 h** |
| **Total por Tipo** | **2.193,45 h (54,8%)** | **1.372,00 h (34,3%)** | **436,79 h (10,9%)** | **4.002,24 h (100%)** |

#### Top 5 Equipamentos Vilões de Indisponibilidade (Soma de Horas Paradas)
1. **`TR-02`** (Transportador de correia 2 — Britagem): **`287,69 h`**
2. **`BM-01`** (Bomba de polpa 1 — Beneficiamento): **`269,63 h`**
3. **`ES-01`** (Espessador 1 — Concentração): **`268,36 h`**
4. **`SM-02`** (Separador magnético 2 — Beneficiamento): **`235,14 h`**
5. **`PN-01`** (Peneira vibratória 1 — Britagem): **`225,64 h`**

---

### B. Números Oficiais do Caso 3.9 — "Horas em Dobro" (`OS_Marco_Auditoria.csv` + `Alocacao_Tecnicos_OS.csv`)

| Métrica Conferida | Antes da Mesclagem (Correto / Modelo `1:*`) | Após Mesclar e Expandir `Técnico` na Fato (Erro) | Diferença Fantasma |
| :--- | :---: | :---: | :---: |
| **Linhas na Tabela `OS_Marco_Auditoria`** | **48 linhas** | **63 linhas** | $+15\text{ linhas duplicadas}$ |
| **Cartão de Horas Paradas** | **92,0 horas** | **131,5 horas** | $+39,5\text{ horas infladas}$ |
| **Soma das Empilhadeiras (`EM-01` + `EM-02`)** | **23,0 horas** (`12,0` + `11,0`) | **41,5 horas** (`21,5` + `20,0`) | $+18,5\text{ horas infladas}$ |

---

# 4. Gabarito Comentado — Exercícios do Capítulo 3 da Apostila (Página 86)

### Exercício 1 — Identificação da Tabela Fato
> *Num modelo de painel de manutenção, qual tabela é a tabela fato e por quê?*
- ✅ **Alternativa Correta: B)** *A tabela de ordens de serviço, porque registra eventos e tem colunas numéricas que fazem sentido somadas.*
- **Revisão rápida:** `Equipamentos`, `Calendário` e `Áreas` são todas **Dimensões** (cadastros usados para filtrar e agrupar).

### Exercício 2 — Agregações Média e Contagem no Cartão
> *O dosador `DO-03` teve cinco ordens de serviço no mês, com `1,5`, `4,0`, `2,5`, `6,0` e `1,0` horas paradas. Num cartão com a coluna Horas Paradas, qual valor aparece com a agregação Média, e qual aparece com Contagem?*
- ✅ **Alternativa Correta: C) `Média 3,0 e Contagem 5`.**
- **Conta:**
  - Soma das 5 OS = $1,5 + 4,0 + 2,5 + 6,0 + 1,0 = 15,0\text{ horas}$.
  - Quantidade de linhas (Contagem) = **`5`**.
  - Média = $15,0 \div 5 =$ **`3,0 horas`**.

### Exercício 3 — Segmentação Valendo em Duas Páginas
> *Uma segmentação de área criada na página de visão geral não altera a página de detalhe por equipamento. Qual recurso faz a mesma seleção valer nas duas páginas?*
- ✅ **Alternativa Correta: B)** *A opção **Sincronizar segmentações**, na guia Exibição, marcando as duas páginas na coluna Sincronizar.*

### Exercício 4 — Barra "(Em branco)" no Gráfico por Área
> *Depois de ligar a tabela de equipamentos à tabela de ordens de serviço, o gráfico de horas por área mostra uma barra sem nome, com 9,0 horas. O que essa barra indica e onde a correção deve ser feita?*
- ✅ **Alternativa Correta: D)** *Há OS com código de equipamento que não existe no cadastro; a correção é na origem ou na limpeza, antes do modelo.*
- **Conexão com Teoria de Conjuntos (Dia 2):** É o conjunto diferença $B - A$ (uma OS apontando para um TAG órfão ou digitado errado que não tem par na dimensão `Equipamentos`).

### Exercício 5 — Atualização Agendada Parada há Cinco Dias
> *Um painel publicado com atualização agendada às seis da manhã mostra a data parada há cinco dias. O modelo aponta para um gateway em modo pessoal, instalado no notebook de quem montou o painel, que está desligado. Qual é a causa e a solução?*
- ✅ **Alternativa Correta: A)** *O gateway pessoal exige a máquina ligada no horário; a solução é um gateway em modo padrão num servidor da empresa.*
