
## Tipos de datos en XSD (XML Schema)

### 1. Tipos de texto

| Tipo XSD              | Significado                              | Ejemplo válido                         |
| --------------------- | ---------------------------------------- | -------------------------------------- |
| `xs:string`           | Cadena de texto libre                    | Hola mundo                             |
| `xs:normalizedString` | Texto sin saltos de línea                | Hola mundo                             |
| `xs:token`            | Texto sin espacios extra (los normaliza) | Hola mundo                             |
| `xs:language`         | Código de idioma                         | es, en, fr                             |
| `xs:anyURI`           | URL o URI válida                         | [http://google.com](http://google.com) |

> **IMPORTANTE:**  
> La diferencia que existe entre los primeros tres tipos de datos no está en “qué almacenan” (los tres son texto), sino en **cómo se tratan los espacios en blanco**.  
> Los tres derivan de `xs:string`, pero aplican **distintos niveles de normalización del texto**:

* `xs:string`: no modifica absolutamente nada.
* `xs:normalizedString`: elimina saltos de línea y tabulaciones, pero no elimina espacios duplicados ni los de los extremos.
* `xs:token`: aplica lo mismo que `normalizedString` y además limpia espacios “extra” completamente.

---

### 2. Tipos numéricos

| Tipo XSD                | Significado                    | Ejemplo |
| ----------------------- | ------------------------------ | ------- |
| `xs:integer`            | Número entero                  | 10      |
| `xs:positiveInteger`    | Entero positivo (> 0)          | 5       |
| `xs:negativeInteger`    | Entero negativo (< 0)          | -3      |
| `xs:nonNegativeInteger` | Entero ≥ 0                     | 0, 5    |
| `xs:nonPositiveInteger` | Entero ≤ 0                     | 0, -2   |
| `xs:decimal`            | Número decimal                 | 10.5    |
| `xs:float`              | Número real (precisión simple) | 3.14    |
| `xs:double`             | Número real (mayor precisión)  | 3.14159 |

---

### 3. Tipos booleanos

| Tipo XSD     | Significado  | Ejemplo válido    |
| ------------ | ------------ | ----------------- |
| `xs:boolean` | Valor lógico | true, false, 1, 0 |

---

### 4. Tipos de fecha y tiempo

| Tipo XSD        | Significado    | Ejemplo             |
| --------------- | -------------- | ------------------- |
| `xs:date`       | Fecha completa | 2026-04-19          |
| `xs:time`       | Hora           | 14:30:00            |
| `xs:dateTime`   | Fecha y hora   | 2026-04-19T14:30:00 |
| `xs:gYear`      | Año            | 2026                |
| `xs:gYearMonth` | Año y mes      | 2026-04             |
| `xs:gMonth`     | Mes            | --04                |
| `xs:gDay`       | Día            | ---19               |

---

### 5. Tipos binarios

| Tipo XSD          | Significado                          | Ejemplo                      |
| ----------------- | ------------------------------------ | ---------------------------- |
| `xs:base64Binary` | Datos binarios codificados en Base64 | QWxhZGRpbjpvcGVuIHNlc2FtZQ== |
| `xs:hexBinary`    | Datos binarios en hexadecimal        | 4A6F686E                     |

---

### 6. Tipos especiales

| Tipo XSD    | Significado                         | Ejemplo     |
| ----------- | ----------------------------------- | ----------- |
| `xs:ID`     | Identificador único en el documento | id123       |
| `xs:IDREF`  | Referencia a un ID                  | id123       |
| `xs:IDREFS` | Lista de referencias                | id1 id2     |
| `xs:QName`  | Nombre cualificado (con namespace)  | ns:elemento |

---

## Idea clave

En XSD, a diferencia de DTD:

* No todo es texto (`#PCDATA`)
* Podemos **validar el tipo de dato**
* Esto permite evitar errores como:

  * poner texto donde debería haber números
  * fechas mal formateadas
  * emails/URLs incorrectas (si se restringe)

---

## Ejemplo aplicado

```xml
<xs:element name="fecha" type="xs:date"/>
<xs:element name="precio" type="xs:decimal"/>
<xs:element name="anio" type="xs:gYear"/>
<xs:element name="activo" type="xs:boolean"/>
```

👉 Esto hace que el XML sea mucho más fiable que con DTD.

---
