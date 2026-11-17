# Vía360 — Backend XML: protocolos de conservación

Este repositorio simula un sistema legado de protocolos de conservación de carreteras.
Incluye **12 documentos XML estáticos**, uno por cada combinación de `tipo` y
`prioridad` de las 12 incidencias del repertorio Vía360. Son datos **ficticios**
para demostración; los tiempos e instrucciones no son normativa real.

## Publicación en GitHub Pages

1. Crea un repositorio público, por ejemplo `via360-protocolos`.
2. Sube **el contenido de esta carpeta a la raíz del repositorio** (`main`).
3. En `Settings > Pages`, selecciona `Deploy from a branch`, rama `main`, directorio `/ (root)`.
4. Comprueba que el enlace al XML responde con el contenido XML, por ejemplo:

   `https://USUARIO.github.io/via360-protocolos/BARRERA_CRITICA.xml`

Sustituye `USUARIO` y `via360-protocolos` por los valores reales. GitHub Pages
sirve estos ficheros por HTTPS; **no ejecuta un backend dinámico ni exige login**.
No publiques secretos ni datos de clientes reales.

## Contrato de integración previsto en Foundry

- Recurso: `GET /{tipo}_{prioridad}.xml` (ruta estática, sin parámetros de consulta).
- Formato de respuesta: XML, documento raíz `<protocolo>`.
- Campos: `codigo`, `tipoIncidencia`, `prioridad`, `especialidad`,
  `plazoMaximoMinutos`, `requiereCorteCarril`, `instrucciones`.
- Tipos: elementos XML de texto; en el BFF convertir `plazoMaximoMinutos` a
  número y `requiereCorteCarril` a booleano.
- XPath para el adaptador XML de Foundry:
  `/protocolo/codigo`, `/protocolo/tipoIncidencia`,
  `/protocolo/prioridad`, `/protocolo/especialidad`,
  `/protocolo/plazoMaximoMinutos`, `/protocolo/requiereCorteCarril`,
  `/protocolo/instrucciones`.
- El futuro BFF debe validar la combinación tipo/prioridad con una lista de
  valores admitidos antes de construir la ruta, y tratar un HTTP 404 como
  «protocolo no disponible».

## Ejemplo de la demo

`INC-0002` -> `tipo=BARRERA`, `prioridad=CRITICA` ->
`BARRERA_CRITICA.xml` -> especialidad `SEGURIDAD_VIAL`, plazo 60 minutos.

`BR-02` (tramo `TR-001`) está disponible y tiene esa especialidad. De ese modo,
la orquestación de Foundry puede verificar la brigada y crear la orden de trabajo.

El archivo `catalogo_incidencias.csv` documenta qué XML corresponde a cada
incidencia del repertorio existente. Solo los `.xml` son necesarios para publicar
el catálogo.
