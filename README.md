# Vía360 — Catálogo de protocolos de conservación

## 1. Descripción del catálogo

Este repositorio contiene un **catálogo de 12 protocolos de conservación de carreteras**, expresados como documentos XML independientes. Cada protocolo describe el tratamiento previsto para una combinación determinada de **tipo de incidencia** y **prioridad**.

Cada documento proporciona un código identificador, la especialidad de mantenimiento necesaria, un plazo máximo orientativo de respuesta, la indicación de si requiere un corte de carril y las instrucciones de actuación.

**Alcance:** contenido ficticio para demostración. Los plazos, especialidades, cortes de carril e instrucciones no representan normativa de conservación vial ni procedimientos operativos reales.

## 2. Acceso a los recursos HTTP

**URL base publicada:**

`https://angelmontero-hcl.github.io/via360-protocolos/`

Cada protocolo se consulta mediante una petición HTTP **GET** a un archivo XML concreto:

`GET https://angelmontero-hcl.github.io/via360-protocolos/{TIPO}_{PRIORIDAD}.xml`

Donde `{TIPO}` y `{PRIORIDAD}` son los valores literales del catálogo, escritos en mayúsculas y separados por un guion bajo. Por ejemplo:

`GET https://angelmontero-hcl.github.io/via360-protocolos/BARRERA_CRITICA.xml`

Características de estos recursos:

- **Método:** `GET`.
- **Entrada:** nombre del archivo en la ruta; no hay cuerpo JSON/XML ni parámetros de consulta funcionales.
- **Salida:** documento XML con el elemento raíz `<protocolo>`.
- **Autenticación:** no se exige autenticación; los documentos publicados son de acceso público.
- **Disponibilidad:** solo existen las 12 combinaciones enumeradas abajo. Una ruta correspondiente a otro archivo devuelve normalmente `404 Not Found`.
- **Naturaleza del servicio:** documentos estáticos; la consulta no crea ni modifica registros y no existe una operación de búsqueda dinámica por parámetros.

Ejemplo de consulta desde un cliente HTTP:

```bash
curl -i "https://angelmontero-hcl.github.io/via360-protocolos/BARRERA_CRITICA.xml"
```

Los nombres de archivo y las rutas son sensibles a su escritura y deben emplearse tal como aparecen en el inventario.

## 3. Inventario de protocolos

| Recurso XML | Tipo de incidencia | Prioridad | Especialidad | Plazo máximo (min) | Corte de carril |
|---|---|---|---|---:|---|
| `BARRERA_ALTA.xml` | BARRERA | ALTA | SEGURIDAD_VIAL | 180 | Sí |
| `BARRERA_CRITICA.xml` | BARRERA | CRITICA | SEGURIDAD_VIAL | 60 | Sí |
| `DESPRENDIMIENTO_CRITICA.xml` | DESPRENDIMIENTO | CRITICA | GENERAL | 30 | Sí |
| `DRENAJE_BAJA.xml` | DRENAJE | BAJA | GENERAL | 2880 | No |
| `DRENAJE_MEDIA.xml` | DRENAJE | MEDIA | GENERAL | 480 | No |
| `FIRME_ALTA.xml` | FIRME | ALTA | FIRME | 240 | Sí |
| `FIRME_MEDIA.xml` | FIRME | MEDIA | FIRME | 720 | No |
| `ILUMINACION_MEDIA.xml` | ILUMINACION | MEDIA | ELECTRICIDAD | 480 | No |
| `MARCAS_BAJA.xml` | MARCAS | BAJA | FIRME | 4320 | No |
| `OBSTACULO_CRITICA.xml` | OBSTACULO | CRITICA | GENERAL | 30 | Sí |
| `SENAL_BAJA.xml` | SENAL | BAJA | SEGURIDAD_VIAL | 2880 | No |
| `SENAL_MEDIA.xml` | SENAL | MEDIA | SEGURIDAD_VIAL | 480 | No |

El nombre del recurso resulta de concatenar el tipo y la prioridad con `_` y añadir `.xml`. **No todas las combinaciones posibles de tipos y prioridades tienen un documento asociado.**

## 4. Modelo XML y glosario de metadatos

Todos los documentos presentan una estructura homogénea: un único elemento raíz `<protocolo>` y siete elementos hijos.

| Elemento XML | Significado | Representación y valores |
|---|---|---|
| `protocolo` | Elemento raíz que agrupa los metadatos del procedimiento. | Nodo XML. |
| `codigo` | Identificador único del protocolo. | Texto; formato `PROT-{TIPO}-{PRIORIDAD}`. |
| `tipoIncidencia` | Categoría del problema de conservación al que se aplica. | Texto; uno de los tipos descritos en el glosario de valores. |
| `prioridad` | Nivel de urgencia asignado al tipo de incidencia para seleccionar el protocolo. | Texto; `BAJA`, `MEDIA`, `ALTA` o `CRITICA`. |
| `especialidad` | Competencia profesional que requiere la intervención. | Texto; `FIRME`, `SEGURIDAD_VIAL`, `ELECTRICIDAD` o `GENERAL`. |
| `plazoMaximoMinutos` | Plazo máximo orientativo de respuesta previsto en el protocolo. | Texto XML con contenido numérico entero, expresado en **minutos** (p. ej., `60`). |
| `requiereCorteCarril` | Indica si el protocolo contempla un corte de carril. | Texto XML `true` o `false`. |
| `instrucciones` | Descripción narrativa de las actuaciones previstas. | Texto libre. |

Los elementos de un documento XML contienen texto. Los valores de `plazoMaximoMinutos` y `requiereCorteCarril` representan, respectivamente, una cantidad y un indicador lógico, aunque en el XML aparecen como contenido textual.

### Ejemplo íntegro: `BARRERA_CRITICA.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<protocolo>
  <codigo>PROT-BARRERA-CRITICA</codigo>
  <tipoIncidencia>BARRERA</tipoIncidencia>
  <prioridad>CRITICA</prioridad>
  <especialidad>SEGURIDAD_VIAL</especialidad>
  <plazoMaximoMinutos>60</plazoMaximoMinutos>
  <requiereCorteCarril>true</requiereCorteCarril>
  <instrucciones>Señalizar y balizar la zona. Evaluar la estabilidad de la barrera. Sustituir los elementos dañados y verificar el anclaje antes de restablecer la circulación.</instrucciones>
</protocolo>
```

Para leer un elemento concreto en un documento se puede usar su ruta XML, por ejemplo `/protocolo/especialidad` o `/protocolo/plazoMaximoMinutos`.

## 5. Glosario de valores

### Tipos de incidencia

| Código | Significado |
|---|---|
| `BARRERA` | Daños o alteraciones en barreras y sistemas de contención vial. |
| `DESPRENDIMIENTO` | Caída o desprendimiento de materiales que afectan a la vía. |
| `DRENAJE` | Problemas de evacuación o canalización de agua. |
| `FIRME` | Deterioro de la superficie de rodadura. |
| `ILUMINACION` | Averías o deficiencias en el alumbrado vial. |
| `MARCAS` | Deterioro o necesidad de reposición de marcas viales. |
| `OBSTACULO` | Presencia de objetos u obstáculos en la vía. |
| `SENAL` | Daños o deficiencias en la señalización vial. |

### Prioridades

| Código | Significado funcional |
|---|---|
| `BAJA` | Incidencia de baja urgencia. |
| `MEDIA` | Incidencia que requiere una respuesta programable. |
| `ALTA` | Incidencia que requiere intervención prioritaria. |
| `CRITICA` | Incidencia que requiere respuesta urgente. |

Los tiempos concretos se obtienen de cada protocolo; el catálogo no define un plazo único por prioridad.

### Especialidades

| Código | Ámbito de intervención |
|---|---|
| `FIRME` | Reparación de pavimentos y marcas viales. |
| `SEGURIDAD_VIAL` | Barreras y señalización de seguridad. |
| `ELECTRICIDAD` | Instalaciones eléctricas y alumbrado. |
| `GENERAL` | Trabajos de conservación general, retirada de obstáculos, drenaje y desprendimientos en este catálogo ficticio. |

## 6. Relaciones y correspondencias con incidencias

La **clave funcional de consulta** del catálogo es el par (`tipoIncidencia`, `prioridad`). Ese par identifica un único archivo y, dentro de él, un único `codigo` de protocolo.

Los nombres `tipoIncidencia` y `prioridad` corresponden conceptualmente a los campos `tipo` y `prioridad` de una incidencia. Por ejemplo:

- Incidencia `INC-0002`: `tipo=BARRERA`, `prioridad=CRITICA`.
- Recurso asociado: `BARRERA_CRITICA.xml`.
- Protocolo: `PROT-BARRERA-CRITICA`.
- Especialidad y plazo declarados: `SEGURIDAD_VIAL`, `60` minutos.

El atributo `especialidad` identifica el perfil de equipo adecuado para la intervención. No es una relación física entre repositorios ni contiene el identificador de una brigada concreta.

El archivo auxiliar `catalogo_incidencias.csv` relaciona los identificadores de las **12 incidencias de ejemplo** con sus correspondientes documentos XML y especialidades. Es una tabla de correspondencias para el repertorio de demostración, no un endpoint de consulta dinámica.

## 7. Limitaciones y alcance

- El catálogo contiene exclusivamente los doce procedimientos enumerados.
- Los recursos son de solo lectura y de acceso público.
- Una combinación no publicada no dispone de protocolo XML en este repositorio.
- Los datos tienen finalidad demostrativa y no deben emplearse para determinar actuaciones reales de conservación y seguridad vial.
