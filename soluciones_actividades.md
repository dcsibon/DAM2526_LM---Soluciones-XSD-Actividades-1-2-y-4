# Soluciones con comentarios explicativos.

## Actividad 1: CVitae

### Fichero `cv.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    Documento XML del currículum vitae.
    Se enlaza con el esquema XSD mediante xsi:noNamespaceSchemaLocation,
    ya que no estamos usando espacios de nombres propios.
-->
<cv xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="cv.xsd">

  ...
  
</cv>
```

### Fichero `cv.xsd`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    Esquema XSD para validar el currículum vitae.
    Define la estructura, el orden de los elementos y los atributos.
-->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <!--
        Elemento raíz <cv>.
        Dentro definimos todos los elementos en el orden exacto
        en que deben aparecer en el XML.
    -->
    <xs:element name="cv">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="nombre" type="xs:string"/>
                <xs:element name="anio-nacimiento" type="xs:gYear"/>

                <!-- residencia = ciudad + pais -->
                <xs:element name="residencia">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="ciudad" type="xs:string"/>
                            <xs:element name="pais" type="xs:string"/>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>

                <!-- contacto = telefono + email + linkedin -->
                <xs:element name="contacto">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="telefono" type="xs:string"/>
                            <xs:element name="email" type="xs:string"/>
                            <xs:element name="linkedin" type="xs:anyURI"/>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>

                <xs:element name="descripcion" type="xs:string"/>

                <!-- Lista de competencias -->
                <xs:element name="competencias">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="competencia" maxOccurs="unbounded">
                                <xs:complexType>
                                    <!--
                                        Las competencias no tienen contenido interno,
                                        solo atributos.
                                    -->
                                    <xs:attribute name="nombre" type="xs:string" use="required"/>
                                    <xs:attribute name="nivel" type="xs:string" use="required"/>
                                </xs:complexType>
                            </xs:element>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>

                <!-- Lista de estudios -->
                <xs:element name="formacion">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="estudio" maxOccurs="unbounded">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element name="titulacion" type="xs:string"/>
                                        <xs:element name="centro" type="xs:string"/>
                                        <xs:element name="anio" type="xs:gYear"/>
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>

                <!-- Lista de experiencias -->
                <xs:element name="experiencia-profesional">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="experiencia" maxOccurs="unbounded">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element name="puesto" type="xs:string"/>
                                        <xs:element name="empresa" type="xs:string"/>
                                        <xs:element name="inicio" type="xs:gYear"/>
                                        <!--
                                            El fin se define como string para permitir
                                            que esté vacío si el trabajo sigue en curso.
                                        -->
                                        <xs:element name="fin" type="xs:string"/>
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>

            </xs:sequence>
        </xs:complexType>
    </xs:element>

</xs:schema>
```

---

## Actividad 2: Viajes turísticos

### Fichero `viajes_turisticos.xsd`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    Esquema XSD para validar el XML de viajes turísticos.
    Controla la estructura, el orden y los tipos de datos.
-->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <!-- Elemento raíz -->
    <xs:element name="viajes_turisticos">
        <xs:complexType>
            <xs:sequence>
                <!-- Puede haber uno o más viajes -->
                <xs:element name="viaje" maxOccurs="unbounded">
                    <xs:complexType>
                        <xs:sequence>

                            <!-- Clientes del viaje -->
                            <xs:element name="clientes">
                                <xs:complexType>
                                    <xs:sequence>
                                        <!-- Uno o más clientes -->
                                        <xs:element name="cliente" maxOccurs="unbounded">
                                            <xs:complexType>
                                                <xs:sequence>
                                                    <xs:element name="nombre" type="xs:string"/>
                                                    <xs:element name="apellidos" type="xs:string"/>
                                                </xs:sequence>
                                            </xs:complexType>
                                        </xs:element>
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>

                            <!-- Fechas del viaje -->
                            <xs:element name="fecha_inicio" type="xs:date"/>
                            <xs:element name="fecha_fin" type="xs:date"/>

                            <!-- Lugar de origen -->
                            <xs:element name="origen" type="xs:string"/>

                            <!-- Posibles destinos -->
                            <xs:element name="posibles_destinos">
                                <xs:complexType>
                                    <xs:sequence>
                                        <!-- Uno o más destinos -->
                                        <xs:element name="destino" maxOccurs="unbounded">
                                            <xs:complexType>
                                                <xs:sequence>
                                                    <xs:element name="ciudad" type="xs:string"/>
                                                    <xs:element name="hotel" type="xs:string"/>
                                                    <xs:element name="noches" type="xs:positiveInteger"/>
                                                </xs:sequence>
                                            </xs:complexType>
                                        </xs:element>
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>

                            <!-- Precio total del viaje -->
                            <xs:element name="precio" type="xs:decimal"/>

                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

</xs:schema>
```

---

## Actividad 4: Sistema de pedidos de componentes

### Fichero `pedidos.xsd`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    Esquema XSD para validar pedidos de componentes informáticos.
    Sustituye al antiguo DTD externo.
-->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <!-- Elemento raíz -->
    <xs:element name="pedidos">
        <xs:complexType>
            <xs:sequence>
                <!-- Debe haber uno o más pedidos -->
                <xs:element name="pedido" maxOccurs="unbounded">
                    <xs:complexType>
                        <xs:sequence>

                            <!-- Cliente -->
                            <xs:element name="cliente">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element name="nombre" type="xs:string"/>
                                        <xs:element name="email" type="xs:string"/>
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>

                            <!-- Fecha del pedido -->
                            <xs:element name="fecha" type="xs:string"/>

                            <!-- Lista de componentes -->
                            <xs:element name="componentes">
                                <xs:complexType>
                                    <xs:sequence>
                                        <!-- Uno o más componentes -->
                                        <xs:element name="componente" maxOccurs="unbounded">
                                            <xs:complexType>
                                                <xs:sequence>
                                                    <xs:element name="nombre" type="xs:string"/>
                                                    <xs:element name="marca" type="xs:string"/>
                                                    <xs:element name="precio" type="xs:string"/>
                                                    <xs:element name="cantidad" type="xs:string"/>
                                                </xs:sequence>

                                                <!--
                                                    Atributo obligatorio tipo
                                                    (por ejemplo: CPU, RAM, SSD, GPU...)
                                                -->
                                                <xs:attribute name="tipo" type="xs:string" use="required"/>
                                            </xs:complexType>
                                        </xs:element>
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>

                            <!-- Precio total del pedido -->
                            <xs:element name="precio-total" type="xs:string"/>

                        </xs:sequence>

                        <!-- Atributo obligatorio id del pedido -->
                        <xs:attribute name="id" type="xs:string" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

</xs:schema>
```
