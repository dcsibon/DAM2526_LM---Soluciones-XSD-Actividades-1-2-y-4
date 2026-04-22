
# XSD – Resumen

XSD responde siempre a estas preguntas:

1. ¿Qué elementos hay?
2. ¿En qué orden aparecen?
3. ¿Cuántas veces pueden aparecer?
4. ¿Qué tipo de datos tienen?
5. ¿Tienen atributos?
6. ¿Hay restricciones sobre los valores?

---

## 1. Estructura básica → `xs:element`, `xs:simpleType` y `xs:complexType`

### 1.1. Elemento simple *(solo texto)*  → `xs:element` + atributo `type`

```xml
<xs:element name="nombre" type="xs:string"/>
```

👉 Se usa cuando el elemento **solo contiene texto sin restricciones adicionales**.

### 1.2. Elemento simple con restricciones → `xs:simpleType`

```xml
<xs:element name="nivel">
    <xs:simpleType>
        <xs:restriction base="xs:string">
            <xs:enumeration value="Alto"/>
            <xs:enumeration value="Medio"/>
            <xs:enumeration value="Bajo"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

👉 Se usa cuando el elemento contiene **solo texto pero con restricciones**.

### 1.3. Elemento complejo *(con hijos)* → `xs:complexType` + `xs:sequence` | `xs:choice` | `xs:all`

```xml
<xs:element name="persona">
    <xs:complexType>
        <xs:sequence>
            ...
        </xs:sequence>
    </xs:complexType>
</xs:element>
```

👉 Se usa cuando hay **elementos hijos**.

### 1.4. Elemento simple, sin contenido, pero con atributos → `xs:complexType` + `xs:attribute`

```xml
<xs:element name="competencia">
    <xs:complexType>
        <xs:attribute name="nombre" type="xs:string"/>
        <xs:attribute name="nivel" type="xs:string"/>
    </xs:complexType>
</xs:element>
```

👉 No tiene contenido, solo atributos.

### 1.5. Elemento complejo *(con hijos)* y también con atributos → `xs:complexType` + `xs:sequence` | `xs:choice` | `xs:all` + `xs:attribute`

```xml
<xs:element name="competencia">
    <xs:complexType>
        <xs:sequence>
            ...
        </xs:sequence>
        <xs:attribute name="nombre" type="xs:string"/>
        <xs:attribute name="nivel" type="xs:string"/>
    </xs:complexType>
</xs:element>
```

👉 No tiene contenido de texto, solo elementos hijos y atributos.

### 1.6. Elemento con texto + atributos → `xs:simpleContent` + `xs:extension` + `xs:attribute`

```xml
<xs:element name="precio">
    <xs:complexType>
        <xs:simpleContent>
            <xs:extension base="xs:decimal">
                <xs:attribute name="moneda" type="xs:string"/>
            </xs:extension>
        </xs:simpleContent>
    </xs:complexType>
</xs:element>
```

👉 Se usa cuando hay:

* contenido de texto
* atributos

👉 Clave:

* `base` → tipo del contenido
* `extension` → añadimos atributos

### 1.7. Elemento con texto *(con restricciones)* + atributos → `xs:simpleContent` + `xs:restriction`

```xml
<xs:complexType name="precioConMoneda">
    <xs:simpleContent>
        <xs:restriction base="xs:decimal">
            <xs:minInclusive value="0"/>
            <xs:attribute name="moneda" type="xs:string" use="required"/>
        </xs:restriction>
    </xs:simpleContent>
</xs:complexType>
```

👉 Se usa cuando hay:

* contenido de texto
* atributos
* y además queremos **restringir el valor del contenido**

👉 Clave:

* `restriction` → limita el tipo base
* también permite mantener o definir atributos

### 1.8. Regla general

| Caso                          | Solución                      |
| ----------------------------- | ----------------------------- |
| Texto simple                  | `type`                        |
| Texto con restricciones       | `simpleType`                  |
| Hijos                         | `complexType + sequence`      |
| Solo atributos                | `complexType`                 |
| Texto + atributos             | `simpleContent`               |
| Texto restringido + atributos | `simpleContent + restriction` |

> Todo elemento en XSD se declara con `xs:element`.
>
> Después, su contenido puede definirse de tres formas:
>
> * con un `type="..."` directamente,
> * con un `xs:simpleType`,
> * o con un `xs:complexType`.
>
> Además, `xs:simpleContent` no es una alternativa a `xs:complexType`, sino una forma especial de usarlo cuando hay texto + atributos.

---

## 2. Organización de elementos → `xs:sequence`, `xs:choice`, `xs:all`

### 2.1. `xs:sequence`

Define **orden obligatorio**:

```xml
<xs:sequence>
    <xs:element name="nombre" type="xs:string"/>
    <xs:element name="edad" type="xs:integer"/>
</xs:sequence>
```

✔ primero `nombre`, luego `edad`

### 2.2. `xs:choice`

Permite **una opción entre varias**:

```xml
<xs:choice>
    <xs:element name="email" type="xs:string"/>
    <xs:element name="telefono" type="xs:string"/>
</xs:choice>
```

✔ uno u otro, no ambos

### 2.3. `xs:all`

Elementos en **cualquier orden**, pero:

* máximo una vez cada uno

```xml
<xs:all>
    <xs:element name="nombre" type="xs:string"/>
    <xs:element name="edad" type="xs:integer"/>
</xs:all>
```

---

## 3. Repeticiones → `minOccurs` y `maxOccurs`

Controlan cuántas veces puede aparecer un elemento. Si no se indican, por defecto su valor es 1.

```xml
<xs:element name="item" minOccurs="0" maxOccurs="unbounded"/>
```

### 3.1. Valores clave

| Atributo                | Significado |
| ----------------------- | ----------- |
| `minOccurs="0"`         | opcional    |
| `minOccurs="1"`         | obligatorio |
| `maxOccurs="1"`         | una vez     |
| `maxOccurs="unbounded"` | ilimitado   |

### 3.2. Ejemplos

```xml
<!-- opcional -->
<xs:element name="descripcion" minOccurs="0"/>

<!-- 1 o más -->
<xs:element name="producto" maxOccurs="unbounded"/>

<!-- 0 o más -->
<xs:element name="tag" minOccurs="0" maxOccurs="unbounded"/>

<!-- obligatorio uno (por defecto si no se indica nada, minOccurs="1" y maxOccurs="1") -->
<xs:element name="descripcion"/>
```

---

## 4. Tipos de datos → `type`

Permiten validar el contenido del XML.

```xml
<xs:element name="precio" type="xs:decimal"/>
<xs:element name="fecha" type="xs:date"/>
<xs:element name="anio" type="xs:gYear"/>
<xs:element name="activo" type="xs:boolean"/>
```

### 4.1. Tipos más usados

| Tipo         | Uso        |
| ------------ | ---------- |
| `xs:string`  | texto      |
| `xs:integer` | entero     |
| `xs:decimal` | decimal    |
| `xs:boolean` | true/false |
| `xs:date`    | fecha      |
| `xs:gYear`   | año        |

[Tabla completa con los tipos de datos que se pueden definir en XSD](xsd_tipos_datos.md)

### 4.2. Definición de tipos personalizados → `xs:simpleType`

Permite definir o restringir tipos de datos.

👉 Se usa cuando un elemento contiene **solo texto**, pero queremos limitar sus valores.

#### Ejemplo (enumeración)

```xml
<xs:element name="nivel">
    <xs:simpleType>
        <xs:restriction base="xs:string">
            <xs:enumeration value="Alto"/>
            <xs:enumeration value="Medio"/>
            <xs:enumeration value="Bajo"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

✔ Solo texto
✔ Sin atributos
✔ Con restricciones

👉 Es la forma correcta de restringir valores cuando **no hay atributos**.

---

## 5. Atributos → `xs:attribute`

Se declaran dentro de `complexType`.

```xml
<xs:attribute name="id" type="xs:string" use="required"/>
```

### 5.1. Opciones de `use`

| Valor        | Significado  |
| ------------ | ------------ |
| `required`   | obligatorio  |
| `optional`   | opcional     |
| `prohibited` | no permitido |

Si no se indica, su valor por defecto es `optional`.

### 5.2. Ejemplo

```xml
<xs:element name="producto">
    <xs:complexType>
        <xs:sequence>
            <xs:element name="nombre" type="xs:string"/>
        </xs:sequence>
        <xs:attribute name="codigo" type="xs:string" use="required"/>
    </xs:complexType>
</xs:element>
```

---

## 6. Restricciones → `xs:restriction`

Permiten limitar los valores que puede tomar un elemento.

### 6.1. Enumeraciones

```xml
<xs:element name="nivel">
    <xs:simpleType>
        <xs:restriction base="xs:string">
            <xs:enumeration value="Alto"/>
            <xs:enumeration value="Medio"/>
            <xs:enumeration value="Bajo"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

✔ solo esos valores son válidos

### 6.2. Rangos numéricos

```xml
<xs:element name="edad">
    <xs:simpleType>
        <xs:restriction base="xs:integer">
            <xs:minInclusive value="0"/>
            <xs:maxInclusive value="120"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

👉 También existen variantes:

* `xs:minExclusive` → mayor que (>)
* `xs:maxExclusive` → menor que (<)

### 6.3. Longitud de texto

```xml
<xs:restriction base="xs:string">
    <xs:minLength value="3"/>
    <xs:maxLength value="10"/>
</xs:restriction>
```

👉 O longitud exacta:

```xml
<xs:restriction base="xs:string">
    <xs:length value="9"/>
</xs:restriction>
```

### 6.4. Patrón de texto → `xs:pattern`

Permite definir el formato mediante expresiones regulares.

```xml
<xs:element name="codigo-postal">
    <xs:simpleType>
        <xs:restriction base="xs:string">
            <xs:pattern value="[0-9]{5}"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>
```

👉 Obliga a que el valor tenga exactamente **5 dígitos**.

### 6.5. Restricciones más usadas

| Restricción         | Qué controla             | Ejemplo                  |
| ------------------- | ------------------------ | ------------------------ |
| `xs:enumeration`    | valores permitidos       | Alto, Medio, Bajo        |
| `xs:minInclusive`   | valor mínimo incluido    | ≥ 0                      |
| `xs:maxInclusive`   | valor máximo incluido    | ≤ 120                    |
| `xs:minExclusive`   | valor mínimo no incluido | > 0                      |
| `xs:maxExclusive`   | valor máximo no incluido | < 100                    |
| `xs:minLength`      | longitud mínima          | mínimo 3 caracteres      |
| `xs:maxLength`      | longitud máxima          | máximo 10 caracteres     |
| `xs:length`         | longitud exacta          | exactamente 9 caracteres |
| `xs:pattern`        | formato mediante regex   | 5 dígitos                |
| `xs:totalDigits`    | número total de cifras   | 5 cifras                 |
| `xs:fractionDigits` | cifras decimales         | 2 decimales              |
| `xs:whiteSpace`     | tratamiento de espacios  | trim, normalize          |

> Las restricciones permiten validar no solo el tipo de dato, sino también **sus valores concretos** *(rangos, longitud, formato, etc.)*.

---

## 7. Ejemplo completo

### 7.1. XML de ejemplo

```xml
<?xml version="1.0" encoding="UTF-8"?>
<pedido codigo="PED-001" estado="pendiente" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="pedido.xsd">

    <cliente>
        <nombre>Marta</nombre>
        <edad>35</edad>
        <email>marta@correo.com</email>
    </cliente>

    <productos>
        <producto tipo="hardware">
            <nombre>SSD 1TB</nombre>
            <cantidad>2</cantidad>
            <precio moneda="EUR">89.95</precio>
        </producto>

        <producto tipo="periferico">
            <nombre>Teclado mecánico</nombre>
            <cantidad>1</cantidad>
            <precio moneda="EUR">59.90</precio>
        </producto>
    </productos>

    <observaciones>Entrega por la mañana</observaciones>
</pedido>
```

### 7.2. XSD para validar el ejemplo

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <!-- Elemento raíz -->
    <xs:element name="pedido">
        <xs:complexType>
            <xs:sequence>

                <!-- Elemento complejo con hijos -->
                <xs:element name="cliente">
                    <xs:complexType>
                        <xs:sequence>
                            <!-- Elemento simple con type -->
                            <xs:element name="nombre" type="xs:string"/>

                            <!-- Restricción con simpleType -->
                            <xs:element name="edad">
                                <xs:simpleType>
                                    <xs:restriction base="xs:integer">
                                        <xs:minInclusive value="0"/>
                                        <xs:maxInclusive value="120"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>

                            <!-- Otro elemento simple -->
                            <xs:element name="email" type="xs:string"/>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>

                <!-- Contenedor de varios productos -->
                <xs:element name="productos">
                    <xs:complexType>
                        <xs:sequence>

                            <!-- 1 o más productos -->
                            <xs:element name="producto" maxOccurs="unbounded">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element name="nombre" type="xs:string"/>
                                        <xs:element name="cantidad" type="xs:positiveInteger"/>

                                        <!-- Texto + atributo: simpleContent -->
                                        <xs:element name="precio">
                                            <xs:complexType>
                                                <xs:simpleContent>
                                                    <xs:extension base="xs:decimal">
                                                        <xs:attribute name="moneda" type="xs:string" use="required"/>
                                                    </xs:extension>
                                                </xs:simpleContent>
                                            </xs:complexType>
                                        </xs:element>
                                    </xs:sequence>

                                    <!-- Atributo con restricción -->
                                    <xs:attribute name="tipo" use="required">
                                        <xs:simpleType>
                                            <xs:restriction base="xs:string">
                                                <xs:enumeration value="hardware"/>
                                                <xs:enumeration value="periferico"/>
                                                <xs:enumeration value="software"/>
                                            </xs:restriction>
                                        </xs:simpleType>
                                    </xs:attribute>
                                </xs:complexType>
                            </xs:element>

                        </xs:sequence>
                    </xs:complexType>
                </xs:element>

                <!-- Elemento opcional -->
                <xs:element name="observaciones" type="xs:string" minOccurs="0"/>

            </xs:sequence>

            <!-- Atributo obligatorio -->
            <xs:attribute name="codigo" type="xs:string" use="required"/>

            <!-- Atributo con valores limitados -->
            <xs:attribute name="estado" use="required">
                <xs:simpleType>
                    <xs:restriction base="xs:string">
                        <xs:enumeration value="pendiente"/>
                        <xs:enumeration value="enviado"/>
                        <xs:enumeration value="entregado"/>
                    </xs:restriction>
                </xs:simpleType>
            </xs:attribute>

        </xs:complexType>
    </xs:element>

</xs:schema>
```

### 7.3. Qué conceptos aparecen en este ejemplo

* **`xs:element`**
  Para declarar elementos como `pedido`, `cliente`, `nombre`, `precio`, etc.

* **`xs:complexType`**
  En elementos que tienen hijos o atributos, como `pedido`, `cliente` o `producto`.

* **`xs:sequence`**
  Para obligar a que los hijos aparezcan en orden.

* **`type`**
  En elementos simples como `nombre`, `email` o `cantidad`.

* **`minOccurs` / `maxOccurs`**

  * `observaciones` es opcional con `minOccurs="0"`
  * `producto` puede repetirse con `maxOccurs="unbounded"`

* **`xs:attribute`**
  En `codigo`, `estado`, `tipo` y `moneda`

* **`xs:simpleType`**
  Para restringir:

  * `edad` con rango
  * `estado` con enumeración
  * `tipo` con enumeración

* **`xs:simpleContent`**
  En `precio`, porque tiene:

  * contenido de texto: `89.95`
  * atributo: `moneda="EUR"`

---

## 8. Reutilización entre esquemas → `xs:import` y `ref`

Cuando trabajamos con XML complejos, es habitual dividir la información en varios **esquemas XSD** independientes (por ejemplo: personas, direcciones, productos, etc.).

Para poder utilizar elementos definidos en otros esquemas, se utilizan `xs:import` y `ref`.

### 8.1. `xs:import`

Permite **usar definiciones de otro XSD que pertenece a un namespace distinto**.

```xml
<xs:import namespace="http://empresa.com/personas" schemaLocation="personas.xsd"/>
```

👉 Clave:

* `namespace` → namespace que queremos importar
* `schemaLocation` → fichero XSD donde está definido

👉 Solo se usa cuando los esquemas tienen **namespaces diferentes**.

### 8.2. `ref`

Permite **utilizar un elemento que ya ha sido definido en otro esquema**.

```xml
<xs:element ref="per:persona"/>
```

👉 Clave:

* `ref` → reutiliza un elemento existente
* se usa con prefijo (`per`) porque pertenece a otro namespace

### 8.3. Diferencia entre `name` y `ref`

|                                 | `name` | `ref` |
| ------------------------------- | ------ | ----- |
| Define un elemento nuevo        | ✔      | ❌     |
| Reutiliza un elemento existente | ❌      | ✔     |
| Permite usar otros namespaces   | ❌      | ✔     |

### 8.4. Ejemplo sencillo

Supongamos que existe un esquema `personas.xsd` que define el elemento `<persona>`.

En otro esquema podemos hacer:

```xml
<xs:import namespace="http://empresa.com/personas" schemaLocation="personas.xsd"/>

<xs:element name="registro">
    <xs:complexType>
        <xs:sequence>
            <xs:element ref="per:persona"/>
        </xs:sequence>
    </xs:complexType>
</xs:element>
```

👉 De esta forma no es necesario volver a definir el elemento `persona`.

### 8.5. Idea clave

> `xs:import` permite usar otros esquemas
> `ref` permite reutilizar sus elementos

---

## 9. Tipos de datos personalizados → `xs:simpleType`

Permiten crear **tipos de datos propios con restricciones**, que se pueden reutilizar en distintos elementos o atributos.

### 9.1. Definición de un tipo personalizado

```xml
<xs:simpleType name="nivelType">
    <xs:restriction base="xs:string">
        <xs:enumeration value="bajo"/>
        <xs:enumeration value="medio"/>
        <xs:enumeration value="alto"/>
    </xs:restriction>
</xs:simpleType>
```

👉 Se ha creado un tipo llamado `nivelType`.

### 9.2. Uso en un elemento

```xml
<xs:element name="nivel" type="nivelType"/>
```

### 9.3. Uso en un atributo

```xml
<xs:attribute name="nivel" type="nivelType"/>
```

### 9.4. Ejemplo con patrón

```xml
<xs:simpleType name="codigoType">
    <xs:restriction base="xs:string">
        <xs:pattern value="[A-Z]{3}-[0-9]{4}"/>
    </xs:restriction>
</xs:simpleType>
```

👉 Ejemplo válido: `ABC-1234`

### 9.5. Ejemplo con números

```xml
<xs:simpleType name="notaType">
    <xs:restriction base="xs:decimal">
        <xs:minInclusive value="0"/>
        <xs:maxInclusive value="10"/>
    </xs:restriction>
</xs:simpleType>
```

### 9.6. Ventajas

* ✔ Permiten reutilizar reglas
* ✔ Hacen el esquema más claro
* ✔ Evitan repetir código
* ✔ Facilitan el mantenimiento

### 9.7. Importante

* Solo se usan con **datos simples** (texto, números, fechas…)
* No pueden contener:

  * elementos hijos
  * atributos

👉 Para eso se utiliza `xs:complexType`

### 9.8. Idea clave

> `xs:simpleType` permite definir tipos de datos personalizados con restricciones y reutilizarlos en todo el esquema.

---
