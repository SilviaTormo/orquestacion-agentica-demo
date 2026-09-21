# Auditoría de enlaces y rutas de compra
Fecha: 21/09/2026 · Autor: agente de orquestación (ST Innovation) · Método: petición HTTP real a cada URL (`curl -L`, código y URL final) + revisión del panel.

## 1 · Conclusión

**No existe ningún 404 ni dominio caducado.** El fallo que reportaste tiene dos causas concretas y ya está corregido:

1. **TikTok de frikienlos90**: no había URL (nunca me diste el @handle) y el botón del panel apuntaba a la home genérica de TikTok → enlace inútil. **Corregido**: ahora ese botón **no se muestra** hasta que exista el handle real (no hay enlace falso).
2. **Etsy**: el botón apuntaba a la página de *alta de vendedor* (`/sell`), no a una tienda (que aún no existe). **Corregido**: se etiqueta como "en alta" y se mantiene como acceso al proceso de apertura, no como tienda.

Además, dos enlaces abren **solo con tu sesión** (no están rotos): el panel de negocio de Whop y FastMoss. Se han etiquetado como "requiere tu sesión" para que el equipo no los confunda con errores.

## 2 · Inventario antes / después

| # | Enlace | HTTP real | Diagnóstico | Antes | Después |
|---|---|---|---|---|---|
| 1 | https://www.tiktok.com/@bellezanbienestarbebe | **200** | correcto | abierto | abierto ✅ |
| 2 | TikTok · frikienlos90 | — | **sin dato**: falta el @handle | botón a la home de TikTok (inútil) | **sin botón** + estado "pendiente" |
| 3 | https://www.etsy.com/sell | **200** | es el alta, no una tienda | como si fuera tienda | etiquetado "en alta" ✅ |
| 4 | https://whop.com/dashboard/biz_DKnr550HGJtKJa/ | 200 → redirige a login | requiere sesión | parecía roto | "requiere tu sesión" ✅ |
| 5 | https://www.fastmoss.com/es/account/center | 429 | protección anti-bot (abre en navegador) | parecía roto | "requiere sesión" ✅ |
| 6 | https://developers.fastmoss.com/ | 567 | protección anti-bot | — | documentado ✅ |
| 7 | https://partner.tiktokshop.com/ | **200** | correcto | — | abierto ✅ |
| 8 | https://docs.whop.com/ | **200** | correcto | — | abierto ✅ |
| 9 | https://developer.etsy.com/ | **200** | correcto | — | abierto ✅ |
| 10 | https://linear.app/stnnvtn/project/fastmoss-10k-gmv-850915664337 | **200** | correcto | — | abierto ✅ |
| 11 | http://127.0.0.1:8787/ (app local) | **200** | correcto | — | servida ✅ |
| 12 | http://127.0.0.1:8787/data.json | **200** | correcto | — | servida ✅ |

**Resumen numérico:** 12 URLs comprobadas · 9 responden 200 · 2 requieren sesión (Whop, FastMoss) · 2 bloquean bots (429/567) · **1 sin URL (frikienlos90)** · 0 errores 404.

## 3 · Correcciones aplicadas (archivos)

- `index.html`: tabla de **Enlaces verificados** con código HTTP, diagnóstico y estado; los enlaces sin URL válida no muestran botón; los de sesión se etiquetan.
- `scripts/refresh_data.py`: lista `ENLACES` con el resultado de esta auditoría (se regenera con cada refresco de datos).
- `connectors.json`: misma información en formato máquina, con la nota de la API de FastMoss.

## 4 · Lo que falta para cerrar al 100 % (solo tú puedes darlo)

| Dato | Para qué | Impacto si falta |
|---|---|---|
| **@handle de TikTok de frikienlos90** | activar su enlace y su exploración de nicho | su enlace queda vacío (honesto, no roto) |
| **URL de la tienda de Etsy** cuando la abras | enlazar la tienda real | el botón sigue llevando al alta de vendedor |
| Sesión iniciada en Whop/FastMoss y WhatsApp vinculado | que los agentes lean y reporten sin ti | los informes automáticos no llegan |

## 5 · Rutas críticas de compra (estado)

| Ruta | Estado | Evidencia |
|---|---|---|
| Web → ficha de producto → pago | **no verificable aún**: no hay tienda publicada con catálogo | falta la tienda (TikTok Shop seller / Etsy) |
| Pago → registro de pedido → factura | **verificada en el sistema**: pedido P-0001 y factura F-2026-0001 creados y conciliados | `registro.db`, `data-comercial.json`, conciliación "cuadra: true" |

> Cuando exista la primera tienda, la verificación de compra extremo a extremo se hace con una compra de prueba (o en modo test de la pasarela) siguiendo `MANUAL-OPERACION.md` §5.
