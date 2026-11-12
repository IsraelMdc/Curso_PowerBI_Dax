# Cola Rápida de DAX — Dia 5: Inteligência de Tempo e Indicadores de Manutenção
**Curso:** Power BI e DAX para Manutenção (CEDTEC Guarapari · Samarco)  
*Folha de consulta rápida (Seções 5.3 a 5.9)*

---

## 1. Tabela de Datas Contínua (`5.3`)
Criar em **Modelagem › Nova tabela**, renomear `Date` para `Data`, ordenar `Nome do Mês` pela coluna `Mês` e **Marcar como tabela de datas**:
```dax
Calendário = 
ADDCOLUMNS(
    CALENDAR( DATE(2024, 1, 1), DATE(2026, 12, 31) ),
    "Ano", YEAR ( [Date] ),
    "Mês", MONTH ( [Date] ),
    "Nome do Mês", FORMAT ( [Date], "MMMM" ),
    "Ano-Mês", FORMAT ( [Date], "YYYY-MM" )
)
```

---

## 2. Relacionamento Inativo com `USERELATIONSHIP` (`5.4`)
Ativa temporariamente uma linha tracejada (`Data de Encerramento` ou `Data Programada`) sem alterar o restante da página:
```dax
OS Encerradas = 
CALCULATE(
    COUNT( OS[Data de Encerramento] ),
    USERELATIONSHIP( OS[Data de Encerramento], 'Calendário'[Data] )
)
```

---

## 3. Inteligência de Tempo com Trava da Última OS (`5.5`)
| Indicador | Fórmula DAX |
| :--- | :--- |
| **Acumulado no Ano (`YTD`)** | `Horas YTD = IF( MIN('Calendário'[Data]) <= CALCULATE(MAX(OS[Data de Abertura]), REMOVEFILTERS()), TOTALYTD([Total de Horas Paradas], 'Calendário'[Data]) )` |
| **Mesmo Período do Ano Anterior** | `Horas Ano Anterior = IF( MIN('Calendário'[Data]) <= CALCULATE(MAX(OS[Data de Abertura]), REMOVEFILTERS()), CALCULATE([Total de Horas Paradas], SAMEPERIODLASTYEAR('Calendário'[Data])) )` |
| **Variação Anual (`YoY %`)** | `Variação Anual = DIVIDE( [Total de Horas Paradas] - [Horas Ano Anterior], [Horas Ano Anterior] )` |
| **Mês Anterior** | `Horas Mês Anterior = CALCULATE( [Total de Horas Paradas], DATEADD('Calendário'[Data], -1, MONTH) )` |
| **Média Móvel 30 Dias** | `Média Móvel 30 Dias = CALCULATE( [Total de Horas Paradas], DATESINPERIOD('Calendário'[Data], MAX('Calendário'[Data]), -30, DAY) ) / 30` |

---

## 4. Confiabilidade: `MTBF`, `MTTR` e `Disponibilidade` (`5.6`)
```dax
Horas Calendário = COUNTROWS( 'Calendário' ) * 24 * DISTINCTCOUNT( Equipamentos[Equipamento] )

Número de Falhas = CALCULATE( COUNTROWS( OS ), OS[Tipo de Manutenção] = "Corretiva" )

Tempo de Operação = [Horas Calendário] - CALCULATE( [Total de Horas Paradas], REMOVEFILTERS( OS[Tipo de Manutenção] ) )

MTBF = DIVIDE( [Tempo de Operação], [Número de Falhas] )

MTTR = DIVIDE( [Horas Corretivas], [Número de Falhas] )

Disponibilidade = DIVIDE( [MTBF], [MTBF] + [MTTR] )
```

---

## 5. Backlog, Carteira por Data de Corte e Aderência ao Plano (`5.7`)
```dax
-- Backlog Atual (Fotografia de Hoje)
OS Abertas = CALCULATE( COUNTROWS( OS ), OS[Status] = "Aberta", REMOVEFILTERS( 'Calendário' ) )
Backlog HH = CALCULATE( SUM( OS[Horas Previstas] ), OS[Status] = "Aberta", REMOVEFILTERS( 'Calendário' ) )

-- Evolução Histórica da Carteira na Data de Corte
OS em Carteira = 
VAR Corte = MAX( 'Calendário'[Data] )
VAR Base = CALCULATETABLE( OS, REMOVEFILTERS( 'Calendário' ), REMOVEFILTERS( OS[Status] ) )
VAR Abertas = FILTER( Base, OS[Data de Abertura] <= Corte && ( OS[Data de Encerramento] > Corte || ISBLANK( OS[Data de Encerramento] ) ) )
RETURN COUNTROWS( FILTER( Abertas, OS[Data de Cancelamento] > Corte || ISBLANK( OS[Data de Cancelamento] ) ) )

-- Aderência ao Plano de Preventiva (pela Data Programada)
OS Programadas = CALCULATE( COUNT( OS[Data Programada] ), OS[Tipo de Manutenção] = "Preventiva", USERELATIONSHIP( OS[Data Programada], 'Calendário'[Data] ) )
OS no Prazo = CALCULATE( COUNTROWS( OS ), OS[Tipo de Manutenção] = "Preventiva", NOT ISBLANK( OS[Data de Encerramento] ) && OS[Data de Encerramento] <= OS[Data Programada], USERELATIONSHIP( OS[Data Programada], 'Calendário'[Data] ) )
Aderência = DIVIDE( [OS no Prazo], [OS Programadas] )
```

---

## 6. Custos, Ranking (`RANKX`) e Curva de Pareto ABC (`5.8` e `5.9`)
```dax
% Preventiva = DIVIDE( [Custo Preventiva], [Custo Total] )
% do Tipo = DIVIDE( [Custo Total], CALCULATE( [Custo Total], REMOVEFILTERS( OS[Tipo de Manutenção] ) ) )

Ranking Horas = IF( ISINSCOPE( Equipamentos[Equipamento] ), RANKX( ALL( Equipamentos[Equipamento] ), [Total de Horas Paradas] ) )

Horas Top 3 = CALCULATE( [Total de Horas Paradas], TOPN( 3, ALL( Equipamentos[Equipamento] ), [Total de Horas Paradas] ) )

% Acumulado = 
VAR Atual = [Total de Horas Paradas]
RETURN
    IF(
        ISINSCOPE( Equipamentos[Equipamento] ),
        DIVIDE(
            SUMX( FILTER( ALL( Equipamentos[Equipamento] ), [Total de Horas Paradas] >= Atual ), [Total de Horas Paradas] ),
            CALCULATE( [Total de Horas Paradas], ALL( Equipamentos[Equipamento] ) )
        )
    )

Classe ABC = IF( NOT ISBLANK( [% Acumulado] ), SWITCH( TRUE(), [% Acumulado] <= 0.80, "A", [% Acumulado] <= 0.95, "B", "C" ) )
```
