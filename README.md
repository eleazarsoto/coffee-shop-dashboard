# ☕ Coffee Shop Sales Dashboard — Looker Studio

Dashboard interactivo de 3 páginas construido en Looker Studio sobre datos reales de transacciones de una cafetería (3,547 ventas, marzo 2024 - marzo 2025). Conecta el flujo completo **Google Sheets → Looker Studio**, con auditoría de tipos de datos, KPIs, series temporales, desglose por producto/horario/día, y filtros interactivos por año y por producto.

🔗 **[Ver el dashboard en vivo](https://datastudio.google.com/reporting/c49187c3-9076-48ff-a9c9-00e9f98079b0/page/okB4F)**

*Parte de mi [Data Analytics Portfolio](https://github.com/eleazarsoto/data-analytics-portfolio) · Crédito: [Oráculo Analytics](https://oraculoanalytics.com)*

---

## 📋 Resumen del proyecto

- **Fuente:** Google Sheets conectado vía el conector nativo de Looker Studio, tabla "Coffee Sales" (copia de los datos en `data/Coffee_sales.csv`)
- **Volumen:** 3,547 pedidos totales, 8 productos distintos, periodo marzo 2024 - marzo 2025
- **Facturación total:** $112,245.58 USD
- **Horario cubierto:** 17 horas al día (06:00 a 22:00)
- **Filtros globales:** por año (`Date`) y por producto (`coffee_name`), aplicados a las 3 páginas

## 🛠️ Proceso de construcción

**1. Conexión de la fuente.** Se vinculó la pestaña "Coffee Sales" de Google Sheets directo al informe, usando el conector nativo — sin exportar ni duplicar el archivo.

**2. Auditoría de tipos de datos (metadata).** Antes de construir cualquier gráfica, se revisó cómo interpretó Looker Studio cada columna. El campo `money` venía mapeado como número simple y se corrigió a formato de moneda (USD) para que las tarjetas de resultado mostraran cifras financieras correctas en vez de números crudos.

**3. Scorecards (tarjetas de resultado).** Cuatro KPIs a la vista en la página 1: Facturación Total, Cantidad de productos distintos vendidos (`COUNT DISTINCT(coffee_name)` en vez del *Record Count* automático, que contaría transacciones, no productos únicos), Pedidos Totales, y Horas por día de operación.

**4. Serie temporal con granularidad ajustada.** El dato original está capturado día por día, lo que saturaba el gráfico de líneas. Se ajustó la granularidad del eje X a año-mes **directamente en la configuración del gráfico**, sin modificar la hoja de origen.

**5. Gráfico de barras con orden cronológico.** Por defecto, Looker Studio ordena las barras de mayor a menor por valor financiero — lo cual desordena las horas del día. Se forzó el ordenamiento por la dimensión del eje X (hora) en vez del valor, para que el gráfico de "Facturación por hora" se lea de 06:00 a 22:00 como el horario real de operación.

**6. Gráfico circular por facturación, no por volumen.** Al desglosar la participación de cada café (página 2), se mapeó la dimensión de desglose sobre `money` en vez de `Record Count` — un café más caro pero menos vendido puede representar más ingreso real que uno barato y popular.

**7. Desglose por producto en el tiempo (página 3).** Serie temporal con una línea por cada uno de los 8 productos simultáneamente, para comparar la evolución individual de cada café mes a mes, no solo el total agregado.

**8. Controladores interactivos.** Filtros desplegables por año y por producto en las 3 páginas, con filtrado cruzado nativo: dar clic en cualquier barra o punto de una gráfica filtra el resto del dashboard por esa selección.

## 💡 Hallazgos

1. **Octubre 2024 y febrero 2025 son los meses pico**, con $13,891 y $13,215 de facturación respectivamente — más del doble que el mes más bajo (enero 2025, $6,399).
2. **Martes es el día de mayor facturación** ($18.17K), y domingo el más bajo ($13.34K) — una diferencia de 36% entre el mejor y el peor día de la semana.
3. **La facturación está sorprendentemente pareja entre momentos del día**: Noche ($38.19K), Tarde ($38.13K) y Mañana ($35.93K) están a menos de 7% de diferencia entre sí — no hay un "momento estrella" claro.
4. **Latte y Americano with Milk concentran el 46% de la facturación total** (23.9% y 22.1% respectivamente) entre los 8 productos del menú.
5. **Octubre 2024 destaca también en el desglose por producto**: Latte alcanza su pico individual más alto de todo el año ese mes (~$4,291), coincidiendo con el pico general de facturación de la página 1.

## 📊 Vistas del dashboard

| Página | Captura | Contenido |
|---|---|---|
| 1 — Resumen y KPIs | `screenshots/pagina1_resumen_kpis.png` | 4 tarjetas de resultado + facturación por mes + facturación por hora |
| 2 — Día, momento y producto | `screenshots/pagina2_dia_semana_producto.png` | Facturación por día de la semana, por momento del día, y circular por producto |
| 3 — Tendencia por producto | `screenshots/pagina3_tendencia_por_producto.png` | Serie temporal con las 8 líneas de producto simultáneas |

> Capturas tomadas directamente del dashboard en Looker Studio — no son reproducciones.

## 🗂️ Estructura del repositorio

```
coffee-shop-dashboard/
├── data/
│   └── Coffee_sales.csv        # datos fuente (misma tabla conectada en Looker Studio)
├── screenshots/                # 3 capturas reales del dashboard, una por página
└── README.md
```
