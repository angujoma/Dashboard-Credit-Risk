# Dashboard-Credit-Risk

### Documentación: Campos Calculados en Looker Studio para Riesgo de Crédito

**1. Saldo Capital:**
```text
SUM(SaldoCapital)
```
**Descripción:** Calcula la suma total del saldo de capital en la cartera. Es útil para obtener la exposición general de la cartera de crédito.

---

**2. Variación mensual del saldo en mora:**
```text
SUM(IF(DPD > 0, SaldoCapital, 0))
```
**Descripción:** Calcula el saldo vencido del mes actual. Para visualizar la variación respecto al mes anterior, se utiliza una tabla pivot con la dimensión del mes (por ejemplo, `FORMAT_DATE("%Y-%m", FechaCierre)`) para comparar valores de diferentes periodos.

---

**3. %PAR01 (Proporción de cartera vencida de 1 a 30 días):**
```text
SUM(IF(DPD BETWEEN 1 AND 30, SaldoCapital, 0)) / SUM(SaldoCapital) * 100
```
**Descripción:** Calcula el porcentaje de la cartera cuyo saldo vencido tiene entre 1 y 30 días de mora. Es útil para monitorear la mora temprana.

---

**4. %PAR02 (Proporción de cartera vencida de 31 a 60 días):**
```text
SUM(IF(DPD BETWEEN 31 AND 60, SaldoCapital, 0)) / SUM(SaldoCapital) * 100
```
**Descripción:** Calcula el porcentaje de la cartera cuyo saldo vencido tiene entre 31 y 60 días de mora. Esto ayuda a evaluar mora intermedia.

---

**5. IMOR (Índice de Morosidad):**
```text
SUM(IF(DPD > 0, SaldoCapital, 0)) / SUM(SaldoCapital) * 100
```
**Descripción:** Representa el porcentaje de la cartera total que se encuentra en mora. Es un indicador clave para la salud general de la cartera.

---

**6. % de Pérdida Esperada (ECL):**
```text
SUM(PD * LGD * EAD)
```
**Descripción:** Calcula la pérdida esperada (Expected Credit Loss) combinando la probabilidad de incumplimiento (PD), la pérdida dado el incumplimiento (LGD) y la exposición al vencimiento (EAD). Es útil para analizar riesgos a nivel sucursal o segmento.

---

**7. Dimensión del mes:**
```text
FORMAT_DATE("%Y-%m", FechaCierre)
```
**Descripción:** Convierte las fechas a un formato de año y mes para facilitar comparaciones temporales, como variaciones mensuales o tendencias.

---

**8. Saldo Vencido Actual:**
```text
SUM(IF(DPD > 0, SaldoCapital, 0))
```
**Descripción:** Suma el saldo total en mora para el periodo actual. Es un valor base para calcular métricas como la variación mensual o el índice de morosidad.

---

**Notas Adicionales:**
- **DPD (Días en Mora):** Campo que identifica los días de retraso en los pagos. Es fundamental para segmentar la cartera según el nivel de mora.
- **Tablas Pivot:** En Looker Studio, se recomienda usar tablas pivot para visualizar las comparaciones entre periodos (meses o años) cuando no se dispone de funciones como `LAG()`.
- **Filtros Dinámicos:** Configura filtros interactivos para permitir análisis por sucursal, región, segmento de cliente, entre otros.

Este conjunto de campos calculados puede servir como base para la creación de dashboards avanzados en riesgo de crédito, facilitando el análisis y la toma de decisiones estratégicas.
