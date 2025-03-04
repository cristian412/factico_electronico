# factico_electronico
facturación electrónica

```markdown
# Manual Técnico SIFEN (v150) - Resumen

## 1. Introducción
El **Sistema Integrado de Facturación Electrónica Nacional (SIFEN)** regula la emisión, validación y gestión de documentos tributarios electrónicos en Paraguay.  
**Base legal**: 
- Decreto N° 7.795/2017.
- Ley N° 4.017/2010 (validez de la firma digital).

---

## 2. Estructura del SIFEN
### Subsistemas:
- **Validación**: 
  - Para grandes/medianos contribuyentes.
  - Transmisión de DE a la SET en ≤72 horas.
- **E-kuatia**:
  - Solución gratuita para bajos volúmenes de emisión.

---

## 3. Documentos Tributarios Electrónicos (DTE)
| Tipo                     | Código | Descripción                  |
|--------------------------|--------|------------------------------|
| Factura Electrónica (FE) | 1      | Operaciones comerciales.     |
| Autofactura (AFE)        | 4      | Emitida por el comprador.    |
| Nota de Crédito (NCE)    | 5      | Ajustes a favor del cliente. |
| Nota de Remisión (NRE)   | 7      | Traslado de mercaderías.     |

---

## 4. Modelo Operativo
### Plazos Clave:
| Acción                     | Plazo               |
|----------------------------|---------------------|
| Transmisión DE a la SET    | ≤72 horas post-firma|
| Cancelación de FE          | ≤48 horas           |
| Eventos del receptor       | ≤45 días            |

---

## 5. Especificaciones Técnicas
### Formato XML:
- Codificación: `UTF-8`.
- Namespace: `http://ekuatia.set.gov.py/sifen/xsd`.
- **Campos obligatorios**:
  ```xml
  <DE Id="CDC">
    <dFecFirma>AAAA-MM-DDThh:mm:ss</dFecFirma>
    <gEmis>
      <dRucEm>80000000</dRucEm>
      <dNomEmi>RAZÓN SOCIAL</dNomEmi>
    </gEmis>
  </DE>
  ```

### Firma Digital:
- Estándar: W3C XML Signature.
- Algoritmos: `SHA-256` y `RSA-2048`.
- Certificados: Emitidos por PSC autorizados (ej: `AC Raíz PY`).

---

## 6. Servicios Web
| Servicio               | Tipo      | URL (Producción)                              |
|------------------------|-----------|-----------------------------------------------|
| `siRecepDE`            | Síncrono  | `https://sifen.set.gov.py/de/ws/sync/recibe`  |
| `siRecepLoteDE`        | Asíncrono | `https://sifen.set.gov.py/de/ws/async/lote`   |
| `siConsDE`             | Síncrono  | `https://sifen.set.gov.py/de/ws/consulta`     |

---

## 7. Código de Control (CDC)
- **Estructura**:  
  `[Tipo DE(2)][RUC(8)][Timbrado(8)][Número(7)][DV(1)]`.  
  Ejemplo: `01 80000001 12345678 0000001 8`.

- **Dígito Verificador**:  
  Calculado con módulo 11 ([detalles](https://www.set.gov.py)).

---

## 8. KuDE (Representación Gráfica)
- **Campos requeridos**:
  - CDC.
  - Datos del emisor/receptor.
  - Montos totales.
  - **Código QR**:
    ```plaintext
    https://sifen.set.gov.py/consulta?cdc=01444444017...&cv=123456789
    ```

---

## 9. Validaciones
### Códigos de Error Comunes:
| Código | Descripción                     |
|--------|---------------------------------|
| 0160   | XML mal formado.               |
| 0420   | CDC no existe en SIFEN.        |
| 0501   | RUC sin permisos para consulta.|

---

## 10. Anexos
- **Tablas de codificación**:  
  Descargar desde [ekuatia.set.gov.py/xsd](https://ekuatia.set.gov.py/xsd).
- **Glosario técnico**:  
  `CDC`, `DTE`, `KuDE`, `WS-Security`.

```

**Instrucciones para guardar**:
1. Copia todo el contenido entre los triple backticks (incluyendo las líneas ````markdown` y ````).
2. Pégalo en un editor de texto (ej: Notepad++, VS Code).
3. Guarda el archivo con extensión `.md` (ej: `resumen_sifen.md`).

¡Listo! Tendrás el resumen en formato Markdown para consultar offline. 📥
