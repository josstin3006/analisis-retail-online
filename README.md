# Análisis de Retail Online — ¿Quién compra en esta tienda y quién se nos escapa?

Limpieza y análisis exploratorio de 1+ millón de transacciones reales de un retailer británico (2009-2011), con foco en un hallazgo de negocio concreto: **una porción significativa del ingreso viene de clientes que la tienda no puede identificar**.

Dataset: [Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii) (UCI Machine Learning Repository).

## 📌 El hallazgo principal

**15.15% del ingreso** (excluyendo cancelaciones) viene de ventas sin `Customer ID` registrado. No es un error de captura de datos: el porcentaje se mantiene estable entre 14% y 37% mes a mes durante los 2 años del dataset, y el ticket promedio de esas compras es 41% más bajo que el de clientes identificados — compatible con checkout de invitado, no con datos faltantes al azar.

**Implicación de negocio**: esa porción del ingreso es invisible para cualquier campaña de marketing o programa de fidelización — no hay forma de recontactar a ese cliente. Vale la pena revisar el proceso de checkout.

## 🔍 Otros hallazgos

- **Concentración geográfica**: Reino Unido representa ~85% del ingreso total.
- **Estacionalidad**: pico de ventas claro en noviembre, ambos años — temporada de compras de fin de año.
- **Cancelaciones**: 23.6% de los pedidos en USA se cancelan (la tasa más alta entre países con volumen confiable), vs. tasas mucho menores en el resto.
- **Segmentación RFM**: el 25% de los clientes identificados ("Campeones" — compran seguido y recién) genera el 69% del ingreso identificado. El segmento "En riesgo" (compraban seguido, dejaron de volver) es el de mejor costo/beneficio para una campaña de reactivación.

## 🛠 Metodología

1. **Limpieza** (`Limpieza.ipynb`): carga y concatenación de las 2 hojas del Excel (1,067,371 filas), identificación de cancelaciones por prefijo de factura, exclusión de 6,207 filas de ajustes contables (`Price <= 0`, no son ventas), eliminación de 34,147 duplicados exactos. Resultado: 1,027,017 filas limpias.
   - Decisión clave: **no se imputa `Customer ID` faltante ni se recortan montos altos como outliers** — ambos destruirían las dos señales de negocio más valiosas del dataset (ver hallazgos arriba).
2. **Análisis exploratorio**: estadística descriptiva, distribución de ingreso por transacción, serie de tiempo mensual, top productos/países, tasa de cancelación por país (filtrando países con muestra insuficiente para evitar ruido estadístico).
3. **Segmentación RFM** (Recency, Frequency, Monetary): scoring por quintiles y segmentación por reglas de negocio explícitas (no clustering no supervisado) — prioriza explicabilidad frente a stakeholders no técnicos.

## 📂 Estructura

```
Analisis_Retail/
├── Limpieza.ipynb       # notebook completo: limpieza + análisis + RFM, ejecutado de punta a punta
├── README.md
└── .gitignore           # excluye el dataset crudo y el CSV limpio (pesan demasiado para git)
```

El dataset crudo (`online_retail_II.xlsx`) y la salida limpia (`retail_clean.csv`) no están en el repo por tamaño — se descargan/regeneran corriendo el notebook completo desde la fuente de UCI enlazada arriba.

## 🚀 Cómo correrlo

```bash
pip install pandas numpy matplotlib seaborn openpyxl
```

Descargar `online_retail_II.xlsx` del link de UCI de arriba, colocarlo en la misma carpeta que el notebook, y correr `Limpieza.ipynb` de punta a punta (Restart & Run All).

## 🔮 Próximos pasos

- Consultas SQL sobre el dataset limpio (window functions, CTEs).
- Dashboard en Power BI.

---

*Proyecto de portafolio — parte de mi camino hacia analista de datos.*
