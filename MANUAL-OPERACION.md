# Manual de operación — Orquestador ST Innovation
Versión 1.0 · 21/09/2026 · Cómo funciona el sistema, rutinas, qué accesos hacen falta y qué hacer si algo se rompe. **Sin credenciales en este documento** (las claves viven en la configuración del agente, nunca aquí).

## 1 · Qué es el sistema (mapa en 1 minuto)

```
FastMoss (datos de producto) ─┐
AliExpress/CJ (proveedor)  ────┤
Linear (tablero y agentes) ────┼──►  App local de orquestación  ──►  Tiendas
registro.py (pedidos/facturas) ┘         (panel + bandeja)         (TikTok Shop ×2, Etsy, Whop)
```

| Componente | Qué hace | Dónde vive |
|---|---|---|
| **App de orquestación** | Panel: facturación vs 100.000, pedidos, facturas, tiendas, enlaces, agentes, conectores, plan y bandeja humana | `Desktop/repos/orquestacion-agentica/index.html` (servida en `http://127.0.0.1:8787/`) |
| **Registro operativo** | Fuente de verdad de clientes, pedidos y facturas (SQLite) | `.../datos/registro.db` + CLI `datos/registro.py` |
| **Generador de datos** | Lee Linear + el registro y reescribe el panel | `.../scripts/refresh_data.py` |
| **Agentes programados** | 8 tareas automáticas (scrum, exploradores, review, portfolio, refresco) | AutoClaw → panel «定时» / cron |
| **Bandeja humana** | Lo único que necesita tus manos: aprobaciones y encargos | Pestaña «Bandeja humana» de la app |

## 2 · Rutina diaria (30–60 min)

1. **8:45–10:15** — los agentes trabajan solos: standup, exploraciones de las dos marcas, guion del producto del día. Revisa sus mensajes.
2. **Tú** — elige el producto del top 3, grábalo/publica (o lanza el montaje), y marca lo que apruebes en la bandeja.
3. **Al vender** — registra el pedido (ver §4). Es lo único que convierte una venta en dato del objetivo.
4. **Antes de cerrar** — abre la app y comprueba que la facturación del mes se movió.

## 3 · Rutina semanal (viernes, 20–30 min)

1. **Review del equipo** (17:00) y **portfolio sync** (17:30) — revisa los resúmenes que llegan.
2. **Conciliación**: `python3 datos/registro.py recon --mes AAAA-MM` → debe decir `cuadra: true`.
3. **Resumen del objetivo**: `python3 datos/registro.py resumen` → avance %, ritmo semanal necesario.
4. **Refrescar el panel**: `python3 scripts/refresh_data.py` (o pide «refresca la app» al agente).
5. Si vas por debajo del 80 % del objetivo del mes → aplica la acción correctiva del plan (§4 del plan de 6 meses).

## 4 · Registrar una venta (2 comandos)

```powershell
# 1) alta del cliente (una vez por cliente)
python3 datos/registro.py cliente --nombre "Ana Pérez" --email ana@correo.com --canal tiktok-3b

# 2) alta del pedido (y su factura numerada)
python3 datos/registro.py pedido --cliente 1 --concepto "Serum vitamina C" --precio 24.90 `
        --canal tiktok-3b --producto serum-vitc --fecha 2026-09-21 --pagado
python3 datos/registro.py factura --pedido P-0001

# 3) refrescar el panel
python3 scripts/refresh_data.py
```
Canales convenidos: `tiktok-3b`, `tiktok-friki`, `etsy-friki`, `whop-digital`, `directo`.
**Pedidos de prueba**: añade `--prueba` y quedarán marcados; **nunca** cuentan para el objetivo.

## 5 · Comprobar una compra de extremo a extremo (cuando la tienda esté abierta)

1. Abre el enlace de la tienda desde la pestaña **Tiendas** de la app.
2. Compra el producto más barato (o usa el **modo test** de la pasarela si existe).
3. Confirma que llega el correo de confirmación de la plataforma.
4. Registra ese pedido con `registro.py` (sin `--prueba` si es una compra real; con `--prueba` si fue un test autorizado).
5. En la app: aparece en **Pedidos**, en **Libro de facturación** y mueve el marcador de **Facturación**.

## 6 · Accesos necesarios (y dónde van)

| Acceso | Para qué | Estado |
|---|---|---|
| Linear (API key) | tablero y agentes | ✅ guardada en la config del agente |
| FastMoss (sesión de navegador) | exploración de producto | ⚠️ iniciar sesión en el navegador de AutoClaw |
| TikTok (2 cuentas) | publicar y medir | ✅ sesión iniciada; falta el @handle de frikienlos90 |
| Whop (API key del panel) | producto digital | ⚠️ crear la clave cuando quieras conectarlo |
| Etsy (tienda + keystring/OAuth) | vintage friki | ❌ tienda por abrir |
| TikTok Shop (partner/OAuth) | venta y pedidos | ❌ solicitar acceso de partner |
| WhatsApp (cuenta vinculada) | recibir los informes | ⚠️ revincular (QR) |
| Vercel (login o token) | publicar el panel para el equipo | ⚠️ pendiente de tu login/token |

## 7 · Si algo se rompe

| Síntoma | Causa habitual | Qué hacer |
|---|---|---|
| El panel no abre | servidor parado | doble clic en `servir.bat` (o `abrir.bat`) |
| Los datos no cambian | no se ejecutó el generador | `python3 scripts/refresh_data.py` o pídelo al agente |
| «Sin conexión» en la píldora | `data.json` no se lee (¿abriste el archivo sin servidor?) | usa `servir.bat` |
| Un agente avisa «FastMoss pide login» | sesión caducada en el navegador del agente | inicia sesión y responde «listo» |
| Error de certificado/cURL en los scripts | certificado interceptado/caducado en la red | el CLI tiene transporte alternativo automático (curl); si falla, avísame |
| Pedido no aparece en el panel | no se ejecutó `factura`/`exportar` o el refresco | ejecuta `registro.py exportar` y luego el generador |
| Descuadre en la conciliación | pedido sin factura (o al revés) | revisa `listar --tabla pedidos|facturas` y emite la factura que falte |

## 8 · Qué NO hace este sistema (para que no te sorprenda)

- No garantiza la cifra de 100.000 €: garantiza que el sistema funciona, que todo se mide y que hay un plan.
- No hace trámites fiscales/legales: los señala, no los ejecuta.
- No gasta en publicidad sin tu confirmación.
- No guarda credenciales en los documentos ni en la app.
- No opera solo después de los 6 meses: entrega el sistema y el manual.
