---
titulo: Gemini Notebook MCP
type:
  - literature
author:
  - Giuliano
source_title: Especificación y despliegue del servidor MCP para Gemini Notebook (NotebookLM)
media_type:
  - especificacion
published: 2026-11-11
creada: 2026-11-11
retocada: 2026-11-11
description: Guía técnica integral sobre la arquitectura, autenticación OAuth 2.0 sin gcloud, despliegue del servidor local MCP stdio e integración en Pi para interactuar con la API oficial de Gemini Notebook.
tags:
  - MCP
status:
  - Procesado
temas:
valor:
  - 5 - Bueno
---

# Gemini Notebook MCP —  Guía de Implementación y Arquitectura

## Síntesis
Documentación técnica del proceso completo de investigación, desarrollo, resolución de autenticación e integración del servidor **Model Context Protocol (MCP)** para la API oficial de **Gemini Notebook (NotebookLM)** de Google en el entorno de desarrollo y en el agente **Pi**.

Aborda la superación de las limitaciones del endpoint HTTP oficial (expiración de tokens a los 60 minutos, dependencia de `gcloud` y ausencia de transporte `stdio`), la implementación de un bridge en Node.js con auto-renovación de credenciales OAuth 2.0 y la resolución de las barreras de permisos en Google Cloud Platform.

---

## 1. Contexto y Arquitectura General

### 1.1 El Endpoint Oficial de Google
Google provee acceso a la API de Gemini Notebook a través de un servidor MCP remoto sobre **Streamable HTTP (JSON-RPC 2.0)**:
* **URL:** `https://notebook.googleapis.com/mcp`
* **Transporte:** HTTP POST con `Content-Type: application/json`
* **Versión de protocolo:** `2026-07-28`
* **Scope OAuth 2.0:** `https://www.googleapis.com/auth/notebook`
* **Headers requeridos:**
  * `Authorization: Bearer <ACCESS_TOKEN>`
  * `x-goog-user-project: <PROJECT_ID>`
  * `MCP-Protocol-Version: 2026-07-28`
  * `Mcp-Method: tools/call | tools/list`
  * `Mcp-Name: <tool_name>` (al invocar herramientas)

### 1.2 Limitaciones del uso directo
1. **Expiración de tokens:** Los tokens de acceso generados mediante Google Cloud expiran indefectiblemente cada 60 minutos. En configuraciones estáticas de clientes MCP (como Cursor o Claude Code vía HTTP), esto obliga a renovar y reescribir cabeceras constantemente.
2. **Dependencia de `gcloud`:** La guía de inicio rápido asume la presencia del CLI `gcloud` y Application Default Credentials (ADC), herramientas pesadas que no siempre están instaladas o deseadas en el sistema.
3. **Compatibilidad:** Muchos clientes y agentes MCP operan prioritariamente mediante transporte local estándar `stdio` (entrada y salida estándar por procesos hijos).

### 1.3 Solución implementada: Bridge Local Stdio
Se construyó un servidor MCP local en Node.js ubicado en:
`~/Projects/gemini-notebook-mcp/`

```
┌────────────────────────┐         stdio          ┌───────────────────────────┐
│        Cliente         │ ◄────────────────────► │    gemini-notebook-mcp    │
│  (Pi / Cursor / etc.)  │      (JSON-RPC)        │       (Bridge Local)      │
└────────────────────────┘                        └─────────────┬─────────────┘
                                                                │
                                                         Streamable HTTP
                                                      (Auto-refresh OAuth)
                                                                │
                                                                ▼
                                                  ┌───────────────────────────┐
                                                  │   Google Gemini Notebook  │
                                                  │   notebook.googleapis.com │
                                                  └───────────────────────────┘
```

---

## 2. Catálogo de Herramientas (7 Tools)

El servidor expone el conjunto completo de métodos de la API de Gemini Notebook:

| Herramienta | Propósito | Argumentos Obligatorios | Argumentos Opcionales |
|---|---|---|---|
| `create_notebook` | Crea un cuaderno de notas en Gemini Notebook. | `notebook: { displayName }` | --- |
| `get_notebook` | Obtiene metadatos de un cuaderno existente. | `name: "notebooks/{id}"` | --- |
| `list_notebooks` | Lista los cuadernos accesibles por el usuario. | --- | `pageSize`, `pageToken`, `filter` (`notebook.is_owner = true`) |
| `update_notebook` | Modifica el nombre de un cuaderno. | `notebook: { name, displayName }` | `updateMask` (`displayName`) |
| `create_source` | Ingesta fuentes web, videos de YouTube o texto/Markdown. | `parent: "notebooks/{id}"`, `source: { displayName, ... }` | `webContent: { url }`, `textContent: { content }`, `mimeType` |
| `get_source` | Consulta metadatos y estado (`PROCESSING`, `ACTIVE`, `FAILED`). | `name: "notebooks/{id}/sources/{src}"` | --- |
| `list_sources` | Lista todas las fuentes ingestadas en un cuaderno. | `parent: "notebooks/{id}"` | `pageSize`, `pageToken` |

---

## 3. Autenticación y Despliegue sin `gcloud`

La API de Gemini Notebook no admite API Keys estáticas porque opera sobre los cuadernos personales del usuario. Requiere un flujo OAuth 2.0 completo.

### 3.1 Flujo OAuth 2.0 Integrado (`--login`)
Para prescindir de `gcloud`, el servidor `gemini-notebook-mcp` incorpora un asistente de autorización interactivo:

```bash
node /home/giuliano/Projects/gemini-notebook-mcp/index.js --login
```

**Mecanismo interno:**
1. Solicita el `Project ID`, `Client ID` y `Client Secret` (creados en Google Cloud Console como *Aplicación de escritorio*).
2. Levanta un servidor HTTP local efímero en el puerto `8085` (`http://127.0.0.1:8085/callback`).
3. Abre el navegador con la URL de consentimiento de Google solicitando los scopes:
   * `https://www.googleapis.com/auth/cloud-platform`
   * `https://www.googleapis.com/auth/notebook`
   * Parámetro `access_type=offline` (fundamental para obtener el `refresh_token`).
4. Al recibir la redirección, intercambia el código de autorización por `access_token` y `refresh_token` contra `https://oauth2.googleapis.com/token`.
5. Almacena las credenciales de forma segura con permisos restringidos (`0600`) en:
   `~/.config/gemini-notebook/credentials.json`
6. En cada llamada, si el `access_token` está próximo a expirar (margen de 60 segundos), el servidor solicita un nuevo token silenciosamente usando el `refresh_token`.

### 3.2 Resolución de Obstáculos Técnicos

#### Obstáculo 1: Error 403 `access_denied` ("solo los verificadores aprobados...")
* **Causa:** Toda aplicación en Google Cloud recién creada se encuentra en estado **"En prueba" (Testing)**. Bajo este modo, Google bloquea a cualquier usuario que no esté explícitamente añadido a la lista de prueba.
* **Solución:** En *Google Cloud Console* → *Google Auth Platform* → *Audiencia* / *Pantalla de consentimiento*, agregar la cuenta de correo personal en la sección **Usuarios de prueba (Test users)**. En la pantalla de advertencia del navegador, avanzar con *Avanzado* → *Ir a la aplicación (no seguro)*.

#### Obstáculo 2: Error 403 `API disabled`
* **Causa:** La API `notebook.googleapis.com` no estaba habilitada en el proyecto `proyecto-giuliano`.
* **Solución:** Habilitar el servicio desde la consola:  
  `https://console.developers.google.com/apis/api/notebook.googleapis.com/overview?project=proyecto-giuliano`

---

## 4. Configuración en Pi

Pi administra sus servidores MCP a través de `~/.pi/agent/mcp.json`.

### 4.1 Registro del servidor
Se registró el servidor con exposición directa (`exposure: "direct"`):

```json
{
  "mcpServers": {
    "gemini-notebook": {
      "command": "node",
      "args": [
        "/home/giuliano/Projects/gemini-notebook-mcp/index.js"
      ],
      "exposure": "direct",
      "description": "Gemini Notebook API tools for creating notebooks and ingesting sources"
    }
  }
}
```

### 4.2 Verificación
Ejecutando `pi mcp list`:
```text
gemini-notebook: connected, 7 tools (direct, global)
  node /home/giuliano/Projects/gemini-notebook-mcp/index.js
  tools: create_notebook, get_notebook, list_notebooks, update_notebook, create_source, get_source, list_sources
```

---

## 5. Resumen de Archivos del Sistema

| Ruta | Propósito |
|---|---|
| `~/Projects/gemini-notebook-mcp/index.js` | Código fuente del servidor MCP stdio, lógica de auto-refresh y login. |
| `~/Projects/gemini-notebook-mcp/package.json` | Definición de paquete Node.js y dependencias (`@modelcontextprotocol/sdk`). |
| `~/Projects/gemini-notebook-mcp/README.md` | Documentación técnica de uso y configuración. |
| `~/.config/gemini-notebook/credentials.json` | Credenciales OAuth persistentes (`access_token`, `refresh_token`, IDs). |
| `~/.pi/agent/mcp.json` | Configuración global de servidores MCP en Pi. |

---

## Reflexiones
> "La API no acepta API keys estáticas porque accede a los cuadernos personales del usuario. La solución no es intentar saltarse OAuth, sino encapsular el flujo de renovación dentro del propio bridge local."

* **Reflexión de arquitectura:** El patrón de diseño más efectivo para integrar APIs en la nube con agentes locales de IA consiste en desacoplar la capa de transporte: el agente interactúa con un proceso local estándar (`stdio`), mientras que el proceso local se encarga de la complejidad de la red (HTTP, reintentos, autenticación por tokens y rotación de claves). De este modo, el agente se mantiene enfocado en su tarea cognitiva sin interrupciones por expiración de sesión.

---

## Referencias
* [Google Notebook MCP Documentation](https://developers.google.com/notebook/docs/mcp)
* [Google Gemini Notebook API Authentication](https://developers.google.com/notebook/docs/authentication)
* [Model Context Protocol Specification](https://modelcontextprotocol.io)
* [[Solución Audio Omarchy]]
* [[Guia de git]]
