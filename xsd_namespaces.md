
# Espacios de nombres (Namespaces) y conexión XML–XSD

## 1. ¿Qué es un espacio de nombres?

Un **namespace** sirve para **evitar conflictos de nombres** en XML.

👉 Permite distinguir elementos que se llaman igual pero pertenecen a contextos distintos.

### 1.1. Ejemplo sin namespace (problema)

```xml
<id>123</id>
<id>ABC</id>
```

👉 ¿Cuál es cuál?
Puede haber ambigüedad.

### 1.2. Ejemplo con namespace (solución)

```xml
<usuario:id>123</usuario:id>
<producto:id>ABC</producto:id>
```

Cada uno pertenece a un namespace diferente.

---

## 2. Cómo se declara un namespace

Se usa el atributo `xmlns`.

### 2.1. Namespace por defecto

```xml
<persona xmlns="http://miempresa.com/personas">
```

👉 Todos los elementos heredan ese namespace.

### 2.2. Namespace con prefijo

```xml
<per:persona xmlns:per="http://miempresa.com/personas">
```

👉 `per` es solo un alias (puede llamarse como quieras).

---

## 3. Namespace en XSD

Siempre aparece esto:

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
```

👉 Aquí:

* `xs` → prefijo
* URL → namespace oficial de XML Schema **(NO SE PUEDE CAMBIAR)**

---

## 4. Conectar XML con XSD

Hay varias formas. Esta es una de las partes más importantes.

### 4.1. Sin namespace (la más sencilla)

#### XML

```xml
<persona xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="persona.xsd">
    <nombre>Juan</nombre>
</persona>
```

#### Qué significa

* `xmlns:xsi` → activa el soporte para esquemas
* `xsi:noNamespaceSchemaLocation` → indica el XSD

👉 Se usa cuando el XML **no tiene namespace propio**

### 4.2. Con namespace (la forma profesional)

#### XML

```xml
<persona xmlns="http://miempresa.com/personas"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://miempresa.com/personas persona.xsd">
    <nombre>Juan</nombre>
</persona>
```

Podemos observar que ahora usamos lo siguiente:

```xml
xsi:schemaLocation="namespace ruta.xsd"
```

👉 Tiene dos partes:

1. namespace *(en el ejemplo anterior es `http://miempresa.com/personas`)*
2. ruta del XSD *(en el ejemplo anterior es `persona.xsd`)*

#### XSD

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://miempresa.com/personas"
           xmlns="http://miempresa.com/personas"
           elementFormDefault="qualified">

    <xs:element name="persona">
        ...
    </xs:element>

</xs:schema>
```

* `targetNamespace` → namespace del esquema
* `xmlns` en XSD → aplica ese namespace a los elementos
* `elementFormDefault="qualified"` → obliga a usar namespace en los elementos

### 4.3. Múltiples esquemas

Un XML puede usar varios XSD:

```xml
xsi:schemaLocation="
http://empresa.com/personas personas.xsd
http://empresa.com/productos productos.xsd"
```

Suele ser habitual utilizar prefijos para cada namespace:

```xml
<per:persona xmlns:per="http://miempresa.com/personas">
```

👉 Aquí:

* `per:` es un prefijo
* el namespace sigue siendo la URL

#### Ejemplo de XML con dos namespaces

```xml
<?xml version="1.0" encoding="UTF-8"?>
<pedido xmlns:per="http://empresa.com/personas"
        xmlns:prod="http://empresa.com/productos"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="
            http://empresa.com/personas personas.xsd
            http://empresa.com/productos productos.xsd">

    <!-- Datos del cliente (namespace personas) -->
    <per:cliente>
        <per:nombre>Marta</per:nombre>
        <per:email>marta@correo.com</per:email>
    </per:cliente>

    <!-- Lista de productos (namespace productos) -->
    <prod:productos>
        <prod:producto>
            <prod:nombre>SSD 1TB</prod:nombre>
            <prod:precio>89.95</prod:precio>
        </prod:producto>

        <prod:producto>
            <prod:nombre>Teclado mecánico</prod:nombre>
            <prod:precio>59.90</prod:precio>
        </prod:producto>
    </prod:productos>

</pedido>
```

👉 Dos dominios distintos dentro del mismo XML:

* `<pedido>` → elemento raíz “neutral”
* `<per:cliente>` → pertenece al namespace de **personas**
* `<prod:productos>` → pertenece al namespace de **productos**

#### Esquema XSD de personas (`personas.xsd`)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://empresa.com/personas"
           xmlns="http://empresa.com/personas"
           elementFormDefault="qualified">

    <xs:element name="cliente">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="nombre" type="xs:string"/>
                <xs:element name="email" type="xs:string"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

</xs:schema>
```

#### Esquema XSD de productos (`productos.xsd`)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           targetNamespace="http://empresa.com/productos"
           xmlns="http://empresa.com/productos"
           elementFormDefault="qualified">

    <xs:element name="productos">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="producto" maxOccurs="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="nombre" type="xs:string"/>
                            <xs:element name="precio" type="xs:decimal"/>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

</xs:schema>
```

👉 El XML usa:

```xml
xmlns:per="http://empresa.com/personas"
xmlns:prod="http://empresa.com/productos"
```

Y cada XSD define:

```xml
targetNamespace="http://empresa.com/personas"
targetNamespace="http://empresa.com/productos"
```

✔ Deben coincidir exactamente

---

## 5. Resumen  

* Namespace → evita conflictos de nombres  
* `xmlns` → lo define  
* `xsi:schemaLocation` → conecta XML con XSD  
* `targetNamespace` → define el namespace del esquema  
* `elementFormDefault="qualified"` → obliga a usar namespace  

**IMPORTANTE**  

👉 En general:  

> Un namespace es un identificador único (normalmente una URL),  
> pero **no tiene por qué ser accesible ni “existir” en Internet**.  

👉 Sin embargo:  

> Algunos namespaces están estandarizados (como el de XSD),  
> y en esos casos **sí deben escribirse exactamente como define la especificación** para que el validador los reconozca.  

👉 Pero **no porque sea una URL real**, sino porque es un **identificador reservado por la especificación**  

* Namespaces propios:  
  ✔ pueden ser cualquier URL  
  ✔ no tienen que existir  

* Namespace de XSD (definición del esquema):  
  ✔ debe ser exactamente: `http://www.w3.org/2001/XMLSchema`  

* Namespace de instancia (xsi, uso del esquema en XML):  
  ✔ debe ser exactamente: `http://www.w3.org/2001/XMLSchema-instance`  

> `xs` → se usa en el XSD para definir el esquema  
> `xsi` → se usa en el XML para conectarlo con el esquema  

---
