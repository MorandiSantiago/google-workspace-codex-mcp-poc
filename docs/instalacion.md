# Instalación y configuración documentada

Esta guía reconstruye los pasos respaldados por las notas locales de la integración e incorpora las operaciones que el autor confirmó haber realizado y comprobado. Distingue esa experiencia de las sesiones de validación local registradas. No incluye comandos de instalación o autenticación cuyo texto exacto no quedó confirmado. Todos los nombres y rutas de ejemplo son ficticios.

## 1. Preparar Google Cloud

1. Crear o seleccionar un proyecto de integración en Google Cloud Console.
2. Habilitar **Google Sheets API**, **Google Apps Script API** y **Google Drive API**. Drive también aparece como dependencia de los servicios seleccionados.
3. Configurar Google Auth Platform con audiencia **External** y una pantalla de consentimiento. En esta experiencia se utilizó el modo **Testing** y se agregó la cuenta autorizada como usuario de prueba.
4. Crear un cliente OAuth de tipo **Desktop app** y guardar el JSON de cliente fuera del repositorio. El archivo privado nunca debe publicarse.
5. Habilitar el acceso a la Apps Script API en la configuración de usuario de Apps Script.

La autorización inicial pidió permisos para los servicios configurados. Un error 403 asociado al acceso de la aplicación en modo de prueba se resolvió para las lecturas posteriores al completar la configuración de consentimiento y usuario de prueba. El registro disponible no conserva el detalle exacto de cada intento, por lo que no se atribuye el 403 a un parámetro único.

## 2. Instalar y registrar el servidor MCP

Se usó [Google Workspace MCP, desarrollado por taylorwilsdon](https://github.com/taylorwilsdon/google_workspace_mcp). La instalación local registrada fue la versión `workspace-mcp==2.2.1`, con Python 3.12 en un entorno aislado gestionado por `uv`. No consta en las notas el comando exacto de instalación, por lo que no se propone uno como reproducción histórica.

El servidor se registró globalmente en Codex con transporte `stdio`. Los parámetros observados fueron `--transport stdio`, `--tools sheets appscript` y `--tool-tier complete` para la fase inicial. También se configuraron directorios privados para el JSON OAuth, las credenciales y los logs. El archivo [config.toml.example](../examples/config.toml.example) ilustra la configuración posterior con Drive, usando valores ficticios y la lista de herramientas de Drive restringida.

Después de configurar el servidor, se recargó VS Code con **Developer: Reload Window** y se inició una conversación nueva de Codex. Una comprobación de comunicación local encontró 27 herramientas de Sheets y Apps Script. Esta comprobación solo verificó el servidor y su inventario; no consultó Google.

## 3. Autenticar y comprobar operaciones

Se inició el flujo de autorización OAuth en el navegador, se eligió la cuenta de prueba autorizada y se concedieron los permisos solicitados. La redirección registrada utilizó un callback local de loopback. Los tokens permanecieron fuera del proyecto y de Git.

La validación autenticada registrada se hizo en modo de lectura:

- Confirmó la cuenta autorizada sin imprimir credenciales.
- Leyó mediante MCP un proyecto de Apps Script, sus archivos y un archivo fuente completo.
- Consultó directamente metadatos de Script API para resolver el spreadsheet vinculado.
- Leyó mediante MCP la estructura del spreadsheet y realizó solicitudes GET complementarias con Sheets API.

Esa validación concreta no ejecutó funciones de Apps Script ni métodos de escritura. Durante la integración también se leyeron y modificaron datos de Sheets y se leyó y modificó código de Apps Script; estas operaciones fueron realizadas y comprobadas por el autor. Para las lecturas indicadas arriba consta la vía MCP o API directa. Para las modificaciones, operación verificada; herramienta de ejecución no identificada con certeza. No se atribuyen por defecto a MCP, `clasp`, Python ni a una API directa. Los scripts locales de validación y sus rutas son específicos de aquella instalación y no se incluyen en este repositorio.

## 4. Ampliar a Google Drive

En la etapa posterior se añadió `drive` a los servicios cargados. La configuración limitó Drive a `search_drive_files`, `list_drive_items` y `copy_drive_file`. Se deshabilitaron explícitamente las herramientas de Drive restantes que exponía la instalación y se aplicó además una lista de herramientas permitidas en Codex. La verificación local posterior encontró 30 herramientas, es decir, las 27 anteriores más estas tres.

Durante la integración se accedió a archivos de Drive, se crearon carpetas y se copió el CRM a una carpeta de destino para pruebas, llamada aquí **Carpeta de pruebas**. Son operaciones verificadas; herramienta de ejecución no identificada con certeza en el registro disponible. La validación local descrita arriba solo comprobó el inventario MCP y no ejecutó la copia. La configuración MCP examinada deshabilitaba crear carpetas, así que esa creación no se atribuye a dicho servidor. Copiar un archivo y crear una carpeta requieren permisos y controles adicionales a los necesarios para buscarlo o leerlo.

## 5. Herramientas complementarias y alcance

`clasp` se autenticó con Google para trabajar con Apps Script fuera de MCP. En una operación inicial se informó que no se encontraban archivos de script; el acceso posterior al proyecto mediante MCP sí quedó comprobado. Las notas disponibles no documentan los comandos exactos de `clasp`. La modificación de Apps Script fue verificada; herramienta de ejecución no identificada con certeza. No se adjudica la edición a `clasp` ni a MCP sin más trazabilidad.

`gcloud`, Python y las APIs de Google fueron herramientas complementarias en la experiencia. Las lecturas GET directas de Sheets API y la consulta de metadatos de Script API se distinguen de las llamadas MCP. No se incluyen instrucciones inventadas para operaciones que no quedaron registradas.

## 6. Cuidados para repetir la configuración

Cada servicio de Google requiere scopes y permisos adecuados. Antes de una escritura sobre información real, debe identificarse el recurso afectado, obtenerse una copia de seguridad completa y verificable, revisar el cambio concreto y contar con autorización expresa. El autor confirmó escrituras en Sheets y Apps Script, acceso a Drive, creación de carpetas y copia del CRM durante la integración. El registro local de validación examinado no identifica con certeza la herramienta de ejecución de esas operaciones ni documenta ejecución de funciones de Apps Script. Por ello, la guía no propone comandos para reproducirlas.

No deben incorporarse al repositorio clientes OAuth privados, tokens, configuraciones originales, logs, identificadores reales, datos del CRM ni código privado de Apps Script.

