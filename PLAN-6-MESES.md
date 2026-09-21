# Plan de 6 meses hacia 100.000 € de facturación
Ventana: **21/09/2026 → 22/03/2027** (26 semanas) · Base: facturación bruta acumulada, IVA incluido, contada en el registro local (`registro.db` → `data-comercial.json` → panel).

## 1 · La matemática del objetivo

| Concepto | Valor |
|---|---|
| Objetivo acumulado | **100.000 €** |
| Ritmo medio necesario | **3.846 €/semana** |
| Ticket medio asumido (mezcla físico/digital) | 25–35 € |
| Pedidos necesarios | ≈ **3.000–4.000** en 6 meses |
| Sesiones/visitas necesarias (conversión 2–3 %) | ≈ **110.000–180.000** |
| Vídeos necesarios (a 3–5/semana) | **78–130** + directos |

> Estas cifras son un modelo de trabajo, no una promesa: sirven para saber cada semana si vas por delante o por detrás.

## 2 · Objetivo por mes y comprobación

| Mes | Objetivo | Acumulado | Acciones principales | Cómo se comprueba |
|---|---|---|---|---|
| **Mes 1** (oct) | 4.000 € | 4.000 € | Abrir las **3 tiendas** (TikTok Shop 3B, TikTok Shop friki, Etsy), 6 productos publicados, 12 vídeos, primer directo, bio+escaparate de ambas cuentas | 6 fichas activas + 12 vídeos publicados + 1 directo + primer pedido real en `registro.db` |
| **Mes 2** (nov) | 9.000 € | 13.000 € | Repetir ganadores, 2º proveedor, 20 listings en Etsy, escaparate 3B completo | GMV del mes ≥ 9.000 € en el panel + 20 listings |
| **Mes 3** (dic) | 16.000 € | 29.000 € | Navidad, directos 3/semana, primer **producto digital en Whop** | GMV ≥ 16.000 € + ≥ 1 venta digital registrada |
| **Mes 4** (ene) | 20.000 € | 49.000 € | Rebajas, UGC a marcas, captación de **creators afiliados** (comisión 10–15 %) | GMV ≥ 20.000 € + ≥ 3 creators activos |
| **Mes 5** (feb) | 24.000 € | 73.000 € | Escalar catálogo ganador, pauta pequeña **solo con tu OK**, optimizar fichas | GMV ≥ 24.000 € + ROAS ≥ 2 |
| **Mes 6** (mar) | 27.000 € | **100.000 €** | Bundles, fidelización, revisión de precios | Acumulado ≥ 100.000 € en el panel |

## 3 · Dependencias y responsables

| Pieza | Depende de | Responsable |
|---|---|---|
| Enlaces de tienda y fichas | @handle friki + apertura Etsy | **Silvia** (datos) / agentes (fichas) |
| Exploración de producto | sesión de FastMoss en el navegador de AutoClaw | Silvia (login) / Exploradores (trabajo) |
| Contenido 3–5/semana | guiones (Guionista) + montaje/publicación | agentes + 30-60 min/día de Silvia |
| Medición y conciliación | pedidos registrados con `registro.py` | agentes (rutina) / Silvia (revisión) |
| Pauta de pago | **confirmación explícita de Silvia** | Silvia decide, agentes ejecutan |

## 4 · Revisiones y desviaciones

- **Semanal (viernes):** el panel compara facturación real vs. objetivo del mes; si vas por debajo del 80 % del ritmo, se apunta la desviación y una acción correctiva (más volumen de contenido, subir ticket con bundles, acelerar Etsy).
- **Mensual:** cierre con conciliación (`registro.py recon --mes AAAA-MM`) antes de dar el mes por bueno.
- **Regla de honestidad:** no se maquillan métricas ni pedidos; los pedidos de prueba quedan marcados `es_prueba=1` y **nunca** cuentan para el objetivo.

## 5 · Requisitos de inversión (nada se compromete sin tu OK)

| Concepto | Coste estimado | Cuándo | Nota |
|---|---|---|---|
| Alta de tiendas (TikTok Shop, Etsy) | 0 € de alta; Etsy cobra por listing (~0,20 $) | mes 1 | listing fee es gasto real → lo confirmas tú |
| Muestras de producto para grabar | 0–80 €/producto | mes 1–2 | opcional; hay proveedores que envían muestra gratis |
| Pauta publicitaria | 5–15 €/día (sugerido) | mes 4 en adelante | **requiere confirmación explícita** antes de gastar |
| Herramientas | 0 € (todo con planes gratuitos o lo que ya tienes) | — | FastMoss API y Whop pueden requerir plan: se revisa antes |
