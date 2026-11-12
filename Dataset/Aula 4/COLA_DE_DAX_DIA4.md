# Cola de DAX em Uma Página — Dia 4 (Falando DAX)
**Curso:** Power BI e DAX para Manutenção (CEDTEC Guarapari · Samarco)  
**Referência Rápida:** Seções 4.1 a 5.2 · Mantenha ao lado do teclado no laboratório!

---

## 1. Regra de Ouro: Medida × Coluna Calculada (Seções 4.1 e 4.2)

| Critério | **Medida** *(Usar em 95% dos casos)* | **Coluna Calculada** |
| :--- | :--- | :--- |
| **Quando calcula?** | Na hora em que o visual pede (em tempo real). | Na carga/atualização da tabela (linha a linha). |
| **Contexto padrão** | **Contexto de Filtro** (enxerga a célula da matriz/segmentação). | **Contexto de Linha** (enxerga só a própria linha). |
| **Quando usar?** | Totais, médias, razões (`R$/h`, `MTTR`, `MTBF`), `% do total`. | Categorias fixas para usar em filtro/eixo (ex: `Faixa de Custo`). |
| **Como chamar em outra fórmula?** | Sempre apenas entre colchetes: `[Total de Horas Paradas]` | Sempre com o nome da tabela antes: `OS[Horas Paradas]` |

---

## 2. Sintaxe e As 5 Agregações Básicas (Seção 4.3)

```dax
// 1. Soma de uma coluna numérica
Total de Horas Paradas = SUM( OS[Horas Paradas] )

// 2. Soma do custo
Custo Total = SUM( OS[Custo (R$)] )

// 3. Média simples por linha preenchida
Média de Horas por OS = AVERAGE( OS[Horas Paradas] )

// 4. Contagem de linhas da tabela (RECOMENDADO para contar OS!)
Quantidade de OS = COUNTROWS( OS )

// 5. Quantidade de equipamentos distintos que tiveram OS
Equipamentos Atendidos = DISTINCTCOUNT( OS[Equipamento] )
```
> [!WARNING]
> **Pegadinha (`COUNT` × `COUNTROWS` — Slide 10):** `COUNT(OS[Horas Paradas])` pula as OS que estão com horas em branco (`BLANK`). Para contar o total de ordens de serviço da tabela, use sempre **`COUNTROWS(OS)`**!

---

## 3. Divisão Segura, Lógica e Variáveis (Seções 4.4 e 4.5)

```dax
// DIVIDE: Evita erro de divisão por zero (devolve BLANK automaticamente)
Custo por Hora = 
DIVIDE( [Custo Total], [Total de Horas Paradas] )

// SWITCH(TRUE()): Classifica faixas sem aninhar vários IF (Coluna Calculada)
Faixa de Custo = 
SWITCH(
    TRUE(),
    OS[Custo (R$)] <= 1000, "Baixo",
    OS[Custo (R$)] <= 5000, "Médio",
    "Alto"
)

// VAR e RETURN: Dá nome às partes da conta, calcula 1 vez só e facilita leitura
Horas Médias por OS = 
VAR Horas = SUM( OS[Horas Paradas] )
VAR Quantidade = COUNTROWS( OS )
RETURN
    DIVIDE( Horas, Quantidade )
```

---

## 4. Iteradores (`SUMX`) e Busca em Outra Tabela (`RELATED`) (Seção 4.7)

Quando você precisa **multiplicar duas colunas linha a linha** antes de somar (e uma delas mora na Dimensão `Equipamentos`):

```dax
// Percorre a tabela OS linha a linha, busca o Valor Hora Parada no cadastro e soma os produtos
Custo de Parada Indireto = 
SUMX(
    OS,
    OS[Horas Paradas] * RELATED( Equipamentos[Valor Hora Parada (R$)] )
)
```

---

## 5. `CALCULATE` e Percentual do Total com `REMOVEFILTERS` (Seções 5.1 e 5.2)

```dax
// CALCULATE: Troca ou fixa um filtro antes de calcular a medida
Horas Corretivas = 
CALCULATE(
    [Total de Horas Paradas],
    OS[Tipo de Manutenção] = "Corretiva"
)

// Percentual de Corretivas sobre o total de horas daquela célula
% de Horas Corretivas = 
DIVIDE( [Horas Corretivas], [Total de Horas Paradas] )

// Percentual da Área sobre o Total Geral (tira o filtro de Área só no denominador!)
% Horas da Área = 
DIVIDE(
    [Total de Horas Paradas],
    CALCULATE(
        [Total de Horas Paradas],
        REMOVEFILTERS( Equipamentos[Área] )
    )
)
```
> [!TIP]
> **Atalhos de Teclado na Barra de Fórmulas DAX:**  
> - `Shift + Enter`: Quebra de linha dentro da fórmula.  
> - `Tab`: Autocompleta o nome da função, tabela ou coluna.  
> - `Ctrl + Scroll do Mouse`: Aumenta o tamanho da letra da fórmula no telão/monitor.
