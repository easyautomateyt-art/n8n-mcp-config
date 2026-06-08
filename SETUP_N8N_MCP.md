# Conectar el MCP de n8n (self-hosted) a Claude Code on the web

Instancia: `https://app-reservas-n8n.cuku3d.easypanel.host`

## Por qué no funciona "en caliente" en la sesión actual
- El proxy de seguridad del entorno bloquea dominios no permitidos (`Host not in allowlist`).
- `.mcp.json` se carga al **clonar el repo al inicio de la sesión**, no a mitad de sesión.
- Por eso los cambios requieren **iniciar una sesión nueva**.

## Pasos (una sola vez)

### 1. Commitear `.mcp.json` al repo
El archivo `.mcp.json` ya está creado en la raíz. Debe vivir en el repositorio
desde el que lanzas las sesiones de Claude Code on the web.

```bash
git add .mcp.json
git commit -m "Add n8n MCP server config"
git push
```

### 2. Añadir la variable de entorno (NO la pegues en el repo)
Icono de la nube -> editar entorno -> campo **Environment variables** (formato .env,
sin comillas):

```
N8N_API_KEY=tu_api_key_de_n8n
```

`.mcp.json` la referencia como `${N8N_API_KEY}`, así el secreto no queda committeado.

> La API key se genera en n8n: Settings -> n8n API -> Create an API key.
> AVISO: este entorno aun no tiene un almacen de secretos dedicado; las env vars
> son visibles para quien pueda editar el entorno.

### 3. Permitir el dominio en la red
Editar entorno -> **Network access** -> **Custom** -> Allowed domains:

```
app-reservas-n8n.cuku3d.easypanel.host
```

(npm ya esta permitido por defecto en el nivel Trusted, asi que `npx n8n-mcp`
descarga sin problema.)

### 4. Iniciar una sesión NUEVA
Las herramientas `mcp__n8n__*` quedaran disponibles. Verifica pidiendo:
"lista mis workflows de n8n".

## Seguridad
- Rota la API key que compartiste en el chat (queda en el transcript).
- Maneja la nueva key solo como variable de entorno.

## Herramientas que habilita n8n-mcp (resumen)
- Gestion de workflows (crear/actualizar/validar/eliminar)
- Gestion de ejecuciones (probar workflows y recuperar historial de ejecuciones) <- para tu reporte
- Busqueda de nodos y plantillas, health checks y auditoria de seguridad
