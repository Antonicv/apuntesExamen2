# Temario Detallado de Programación en Profundidad

## Índice

- [Introducción: Panorama del Currículo Integral de Programación](#introducción-panorama-del-currículo-integral-de-programación)

- [TEMA 1 – JPA (Java Persistence API)](#tema-1--jpa-java-persistence-api)
    - [Conceptos Core de JPA](#conceptos-core-de-jpa)
    - [Relaciones: @OneToMany, @ManyToOne](#relaciones-onetomany-manytoone)
    - [Unidireccional vs Bidireccional](#unidireccional-vs-bidireccional)
    - [Repositorios (@Repository)](#repositorios-repository)
    - [Anotaciones de configuración: @Entity, @Id, @JoinColumn, etc.](#anotaciones-de-configuración-entity-id-joincolumn-etc)
    - [Comprensión del gráfico: diagrama de relaciones](#comprensión-del-gráfico-diagrama-de-relaciones)
    - [EAGER vs LAZY loading](#eager-vs-lazy-loading)
    - [Práctica: Modelo con relaciones y anotaciones](#práctica-modelo-con-relaciones-y-anotaciones)

- [TEMA 2 – React – Hooks y Context](#tema-2--react--hooks-y-context)
    - [Hooks](#hooks)
        - [useState](#usestate)
        - [useEffect](#useeffect)
        - [useContext](#usecontext)
    - [Context API](#context-api)
        - [Ventajas de usar Context](#ventajas-de-usar-context)

- [TEMA 3 – React – Navegación y Axios](#tema-3--react--navegación-y-axios)
    - [React Router](#react-router)
        - [Configuración básica](#configuración-básica)
        - [useNavigate](#usenavigate)
        - [Nested routes](#nested-routes)
    - [Axios](#axios)
        - [Peticiones GET y POST](#peticiones-get-y-post)
        - [Manejo de errores](#manejo-de-errores)
    - [Práctica: App React con rutas y llamadas a API falsas](#práctica-app-react-con-rutas-y-llamadas-a-api-falsas)

- [TEMA 4 – Full Stack + AWS + DynamoDB](#tema-4--full-stack--aws--dynamodb)
    - [Full Stack](#full-stack)
        - [Ventajas](#ventajas)
        - [Desventajas](#desventajas)
        - [Tecnologías implicadas](#tecnologías-implicadas)
    - [AWS y DynamoDB](#aws-y-dynamodb)
        - [¿Qué es DynamoDB?](#qué-es-dynamodb)
        - [Diferencias entre bases SQL y NoSQL](#diferencias-entre-bases-sql-y-nosql)
        - [Operaciones básicas en DynamoDB](#operaciones-básicas-en-dynamodb)

- [TEMA 5 – JUnit + Queries + Repaso general](#tema-5--junit--queries--repaso-general)
    - [JUnit](#junit)
        - [Cómo crear un test](#cómo-crear-un-test)
        - [Anotaciones importantes](#anotaciones-importantes)
    - [Queries en JPA](#queries-en-jpa)
        - [JPQL vs Native Queries](#jpql-vs-native-queries)
        - [Ejemplos de consultas JPQL](#ejemplos-de-consultas-jpql)
    - [Práctica: Escribir tests simples en Java y consultas JPQL](#práctica-escribir-tests-simples-en-java-y-consultas-jpql)
        - [Clase de ejemplo para pruebas JUnit](#clase-de-ejemplo-para-pruebas-junit)
        - [Modelo JPA para consultas JPQL](#modelo-jpa-para-consultas-jpql)



## **Introducción: Panorama del Currículo Integral de Programación**

Este currículo de **cinco días** ha sido diseñado para proporcionar una **base sólida** en tecnologías modernas de desarrollo web. A lo largo de esta semana intensiva, se explorarán **conceptos fundamentales** y **herramientas esenciales**, abarcando desde la **persistencia de datos** en el lado del servidor con **JPA** hasta la creación de **interfaces de usuario dinámicas** con **React**, pasando por la gestión de bases de datos **NoSQL** con **AWS DynamoDB** y las prácticas de prueba con **JUnit**.

El enfoque pedagógico se centra en ofrecer **explicaciones detalladas**, **ejemplos prácticos** y **analogías accesibles** para facilitar la comprensión de los temas, incluso para aquellos que se inician en el mundo de la programación. Cada día se construye sobre los conocimientos adquiridos previamente, culminando en una **visión integral** del desarrollo de aplicaciones web completas.

---

## **TEMA 1 – JPA (Java Persistence API)**

### **Conceptos Core de JPA**

**JPA** (*Java Persistence API*) es una **especificación de Java** que proporciona una forma estándar para que los desarrolladores de software interactúen con **bases de datos relacionales** desde aplicaciones Java. En su núcleo, JPA se basa en tres conceptos fundamentales: **Entidad**, **Unidad de Persistencia** y **EntityManager**.

- **Entidad**: Una clase de Java simple que representa una **tabla** en una base de datos relacional. Cada instancia de una entidad corresponde a una **fila** en esa tabla, y los atributos de la clase se mapean a las **columnas** de la tabla. Para indicar que una clase Java es una entidad JPA, se utiliza la anotación `@Entity`.

        ```java
        // Ejemplo de una entidad JPA que representa una tabla "Producto" en la base de datos.
        @Entity
        public class Producto {
                        @Id // Indica la clave primaria de la entidad.
                        private Long id;
                        private String nombre;
                        private double precio;
                        //... getters y setters
        }
        ```

        Aquí, la clase **Producto** está marcada como una entidad, lo que significa que JPA la gestionará y la sincronizará con una tabla llamada (por defecto) **Producto** en la base de datos.

- **Unidad de Persistencia**: Es una configuración que define cómo se gestionan las entidades. Se especifica típicamente en un archivo llamado **persistence.xml**. Este archivo contiene información crucial como la conexión a la base de datos, el proveedor de persistencia (por ejemplo, **Hibernate**, **EclipseLink**), y las entidades que deben ser gestionadas.

- **EntityManager**: Es una interfaz que se utiliza para interactuar con el contexto de persistencia. Permite realizar operaciones como **guardar** (persistir), **recuperar** (buscar), **actualizar** y **eliminar** instancias de entidades en la base de datos.

        ```java
        // Ejemplo de cómo crear un EntityManager para gestionar entidades en JPA.
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("miUnidadDePersistencia");
        EntityManager em = emf.createEntityManager();
        ```

> **Importante:** Comprender estos conceptos básicos es esencial, ya que forman la base sobre la cual se construyen todas las operaciones de JPA.

#### **Analogía para principiantes**

> Imagina que tienes una **caja de herramientas** (JPA) para trabajar con una colección de archivos (tu base de datos). Cada tipo específico de archivo es como una **Entidad** en JPA. Las instrucciones sobre cómo organizar y trabajar con estos archivos son como la **Unidad de Persistencia**. Y tus propias manos, que usas para abrir, escribir y guardar los archivos, son como el **EntityManager**.

---

### **Relaciones: `@OneToMany`, `@ManyToOne`**

Las **relaciones entre entidades** son un aspecto fundamental del modelado de datos en JPA. Las anotaciones `@OneToMany` y `@ManyToOne` se utilizan para definir las relaciones más comunes entre las tablas de una base de datos.

- **@OneToMany**: Una entidad puede tener **múltiples instancias** de otra entidad. Ejemplo: un **Autor** puede escribir muchos **Libros**.

        ```java
        // Ejemplo de relación uno a muchos: un autor tiene muchos libros.
        @Entity
        public class Autor {
                        @Id
                        private Long id;
                        private String nombre;
                        @OneToMany(mappedBy = "autor") // Relación uno a muchos con la entidad Libro.
                        private List<Libro> libros;
                        //... getters y setters
        }
        ```

- **@ManyToOne**: Múltiples instancias de una entidad pueden pertenecer a una única instancia de otra entidad.

        ```java
        // Ejemplo de relación muchos a uno: muchos libros pertenecen a un autor.
        @Entity
        public class Libro {
                        @Id
                        private Long id;
                        private String titulo;
                        @ManyToOne // Relación muchos a uno con la entidad Autor.
                        @JoinColumn(name = "autor_id") // Especifica la columna de clave foránea.
                        private Autor autor;
                        //... getters y setters
        }
        ```

> **Nota:** La anotación `@JoinColumn(name = "autor_id")` especifica la columna de clave foránea en la tabla **Libro**.

#### **Analogía para principiantes**

> Piensa en una **caja de herramientas** y las herramientas que contiene. Una caja de herramientas puede contener muchas herramientas (`@OneToMany`). Y cada herramienta pertenece a una sola caja de herramientas (`@ManyToOne`).

---

### **Unidireccional vs Bidireccional**

- **Unidireccional**: Solo una de las entidades conoce a la otra.

        ```java
        // Ejemplo de relación unidireccional: Pedido conoce a Cliente, pero Cliente no conoce a Pedido.
        @Entity
        public class Pedido {
                        @Id
                        private Long id;
                        @ManyToOne
                        @JoinColumn(name = "cliente_id")
                        private Cliente cliente;
                        //...
        }
        ```

- **Bidireccional**: Ambas entidades son conscientes la una de la otra.

        ```java
        // Ejemplo de relación bidireccional: Autor conoce a sus Libros y Libro conoce a su Autor.
        @Entity
        public class Autor {
                        @Id
                        private Long id;
                        private String nombre;
                        @OneToMany(mappedBy = "autor")
                        private List<Libro> libros;
                        //...
        }
        ```

#### **Analogía para principiantes**

> Imagina dos amigos, Ana y Luis. Si Ana tiene el número de teléfono de Luis, pero Luis no tiene el de Ana, es **unidireccional**. Si ambos tienen el número del otro, es **bidireccional**.

---

### **Repositorios (`@Repository`)**

Los **repositorios** en JPA proporcionan una capa de abstracción sobre la lógica de acceso a los datos.

```java
// Ejemplo de un repositorio en Spring Data JPA para la entidad Producto.
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductoRepository extends JpaRepository<Producto, Long> {
                // Métodos personalizados aquí si es necesario
}
```

> **Ventaja:** Al extender `JpaRepository`, Spring proporciona implementaciones para operaciones CRUD comunes.

#### **Analogía para principiantes**

> Piensa en un **bibliotecario**. No necesitas saber dónde está cada libro; simplemente le pides al bibliotecario (el **Repositorio**) un libro específico.

---

### **Anotaciones de configuración: `@Entity`, `@Id`, `@JoinColumn`, etc.**

- **@Entity**: Marca una clase como entidad JPA.
- **@Id**: Especifica el atributo que representa la clave primaria.
- **@GeneratedValue**: Configura cómo se genera el valor de la clave primaria.
- **@Column**: Personaliza el mapeo de un atributo a una columna.
- **@JoinColumn**: Especifica la columna de clave foránea.
- **@JoinTable**: Especifica la tabla intermedia en relaciones `@ManyToMany`.

#### **Analogía para principiantes**

> Estas anotaciones son como **etiquetas** que pones en tus bloques de LEGO. `@Entity` dice "este es un bloque especial", `@Id` dice "esta es la identificación única".

---

### **Comprensión del gráfico: diagrama de relaciones**

Un **Diagrama de Relaciones Entidad-Relación (ERD)** es una representación visual de la estructura de una base de datos.

#### **Analogía para principiantes**

> Un ERD es como un **mapa** de tu ciudad de LEGOs, mostrando todos los edificios importantes (entidades) y cómo están conectados por carreteras (relaciones).

---

### **EAGER vs LAZY loading**

- **EAGER loading**: Carga todas las entidades relacionadas automáticamente.
- **LAZY loading**: Solo carga las entidades relacionadas cuando se accede a ellas explícitamente.

#### **Analogía para principiantes**

> EAGER loading es como sacar todo el tren con todos sus vagones a la vez. LAZY loading es como sacar solo la locomotora primero y luego los vagones cuando los necesitas.

---

### **Práctica: Modelo con relaciones y anotaciones**

Ejemplo de un modelo de blog:

```java
// Entidad Usuario: representa un usuario que puede tener muchas publicaciones.
@Entity
public class Usuario {
                @Id
                @GeneratedValue(strategy = GenerationType.IDENTITY)
                private Long id;
                private String nombre;
                @OneToMany(mappedBy = "autor") // Un usuario puede tener muchas publicaciones.
                private List<Publicacion> publicaciones;
                //... getters y setters
}

// Entidad Publicacion: representa una publicación que pertenece a un usuario y puede tener muchos comentarios.
@Entity
public class Publicacion {
                @Id
                @GeneratedValue(strategy = GenerationType.IDENTITY)
                private Long id;
                private String titulo;
                private String contenido;
                @ManyToOne // Muchas publicaciones pueden pertenecer a un usuario.
                @JoinColumn(name = "autor_id")
                private Usuario autor;
                @OneToMany(mappedBy = "publicacion") // Una publicación puede tener muchos comentarios.
                private List<Comentario> comentarios;
                //... getters y setters
}

// Entidad Comentario: representa un comentario que pertenece a una publicación.
@Entity
public class Comentario {
                @Id
                @GeneratedValue(strategy = GenerationType.IDENTITY)
                private Long id;
                private String texto;
                @ManyToOne // Muchos comentarios pueden pertenecer a una publicación.
                @JoinColumn(name = "publicacion_id")
                private Publicacion publicacion;
                //... getters y setters
}
```

---

## **TEMA 2 – React – Hooks y Context**

### **Hooks**

Los **Hooks** permiten utilizar el **estado** y otras características de React en **componentes funcionales**.

#### **useState**

Permite declarar **variables de estado** y funciones para actualizarlas.

```javascript
// Ejemplo de uso de useState para crear un contador en React.
import React, { useState } from 'react';

function Contador() {
        const [contador, setContador] = useState(0); // contador es el valor, setContador la función para actualizarlo.

        const incrementar = () => {
                setContador(contador + 1); // Incrementa el contador en 1.
        };

        return (
                <div>
                        <p>Contador: {contador}</p>
                        <button onClick={incrementar}>Incrementar</button>
                </div>
        );
}
```

#### **useEffect**

Se utiliza para manejar **efectos secundarios** en componentes funcionales.

```javascript
// Ejemplo de uso de useEffect para simular la carga de datos desde una API.
import React, { useState, useEffect } from 'react';

function DatosDesdeApi() {
        const [datos, setDatos] = useState(null);
        const [cargando, setCargando] = useState(true);
        const [error, setError] = useState(null);

        useEffect(() => {
                // Simula una petición a una API con setTimeout.
                setTimeout(() => {
                        const datosSimulados = { mensaje: "Datos obtenidos exitosamente" };
                        setDatos(datosSimulados);
                        setCargando(false);
                }, 2000);

                return () => {
                        // Limpieza opcional al desmontar el componente.
                };
        }, []); // El array vacío indica que solo se ejecuta una vez al montar.

        if (cargando) return <p>Cargando datos...</p>;
        if (error) return <p>Error al cargar los datos.</p>;
        if (datos) return <p>{datos.mensaje}</p>;

        return null;
}
```

#### **useContext**

Permite acceder al valor de un **Context** de React dentro de un componente funcional.

```javascript
// Ejemplo de uso de Context y useContext para gestionar el tema (claro/oscuro) en una aplicación React.
import React, { useState, createContext, useContext } from 'react';

const TemaContext = createContext('claro'); // Crea un contexto con valor por defecto 'claro'.

function ProveedorDeTema({ children }) {
        const [tema, setTema] = useState('claro');

        const cambiarTema = () => {
                setTema(tema === 'claro' ? 'oscuro' : 'claro'); // Cambia entre claro y oscuro.
        };

        return (
                <TemaContext.Provider value={{ tema, cambiarTema }}>
                        {children}
                </TemaContext.Provider>
        );
}

function ComponenteConsumidor() {
        const { tema, cambiarTema } = useContext(TemaContext); // Accede al contexto.

        return (
                <div style={{ backgroundColor: tema === 'oscuro' ? 'black' : 'white', color: tema === 'oscuro' ? 'white' : 'black' }}>
                        <p>El tema actual es: {tema}</p>
                        <button onClick={cambiarTema}>Cambiar tema</button>
                </div>
        );
}
```

---

### **Context API**

Permite **compartir valores** entre todos los componentes en un árbol, sin tener que pasarlos explícitamente a través de cada nivel.

#### **Ventajas de usar Context**

- **Reducción de la complejidad del código**
- **Mejor reutilización de componentes**
- **Mayor facilidad de mantenimiento**

---

## **TEMA 3 – React – Navegación y Axios**

### **React Router**

Permite crear **aplicaciones de una sola página (SPA)** con múltiples vistas y navegar entre ellas sin recargar la página completa.

#### **Configuración básica**

```javascript
// Ejemplo básico de configuración de rutas con React Router.
import React from 'react';
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function Inicio() { return <h2>Inicio</h2>; }
function AcercaDe() { return <h2>Acerca de</h2>; }
function Contacto() { return <h2>Contacto</h2>; }

function App() {
        return (
                <BrowserRouter>
                        <nav>
                                <ul>
                                        <li><Link to="/">Inicio</Link></li>
                                        <li><Link to="/acerca">Acerca de</Link></li>
                                        <li><Link to="/contacto">Contacto</Link></li>
                                </ul>
                        </nav>
                        <Routes>
                                <Route path="/" element={<Inicio />} />
                                <Route path="/acerca" element={<AcercaDe />} />
                                <Route path="/contacto" element={<Contacto />} />
                        </Routes>
                </BrowserRouter>
        );
}
```

#### **useNavigate**

Permite navegar programáticamente a diferentes rutas.

```javascript
// Ejemplo de uso de useNavigate para cambiar de ruta desde un botón.
import React from 'react';
import { useNavigate } from 'react-router-dom';

function PaginaDeInicio() {
        const navigate = useNavigate();

        const irAPaginaAcercaDe = () => {
                navigate('/acerca'); // Navega a la ruta "/acerca".
        };

        return (
                <div>
                        <h2>Página de Inicio</h2>
                        <button onClick={irAPaginaAcercaDe}>Ir a la página "Acerca de"</button>
                </div>
        );
}
```

#### **Nested routes**

Permiten organizar interfaces de usuario complejas con layouts y sub-rutas.

```javascript
// Ejemplo de rutas anidadas (nested routes) en React Router.
import React from 'react';
import { BrowserRouter, Routes, Route, Link, Outlet } from 'react-router-dom';

function PanelDeControl() {
        return (
                <div>
                        <h2>Panel de Control</h2>
                        <nav>
                                <ul>
                                        <li><Link to="/admin/usuarios">Usuarios</Link></li>
                                        <li><Link to="/admin/productos">Productos</Link></li>
                                </ul>
                        </nav>
                        <Outlet /> {/* Renderiza las sub-rutas aquí */}
                </div>
        );
}
```

---

### **Axios**

Librería cliente HTTP basada en **Promesas** para realizar peticiones a APIs.

#### **Peticiones GET y POST**

```javascript
// Ejemplo de peticiones GET y POST usando Axios.
import axios from 'axios';

// GET: obtiene la lista de usuarios desde una API.
axios.get('https://api.ejemplo.com/usuarios')
 .then(respuesta => { console.log(respuesta.data); })
 .catch(error => { console.error('Error:', error); });

// POST: crea un nuevo usuario enviando datos a la API.
axios.post('https://api.ejemplo.com/usuarios', {
        nombre: 'Juan Pérez',
        email: 'juan.perez@ejemplo.com'
})
 .then(respuesta => { console.log('Usuario creado:', respuesta.data); })
 .catch(error => { console.error('Error:', error); });
```

#### **Manejo de errores**

```javascript
// Ejemplo de manejo de errores en una petición con Axios.
axios.get('https://api.ejemplo.com/recurso-inexistente')
 .then(respuesta => { console.log(respuesta.data); })
 .catch(error => {
                if (error.response) {
                        // El servidor respondió con un código de error.
                        console.error('Error del servidor:', error.response.status, error.response.data);
                } else if (error.request) {
                        // No se recibió respuesta del servidor.
                        console.error('No se recibió respuesta del servidor:', error.request);
                } else {
                        // Error al configurar la petición.
                        console.error('Error al configurar la petición:', error.message);
                }
        });
```

# Isomorfismo en Axios: Resumen

**Axios** es una herramienta (librería) que se usa en programación para enviar y recibir información a través de internet. Por ejemplo, si una aplicación necesita obtener datos de una página web o enviar información a un servidor, Axios puede hacer ese trabajo.

El término **isomorfismo** en Axios significa que esta herramienta funciona de la misma manera en dos lugares diferentes:
1. En el navegador (como cuando usas Google Chrome o Firefox).
2. En el servidor (la computadora que maneja la parte "invisible" de una aplicación).

Esto es útil porque puedes escribir un solo bloque de código y usarlo en ambos lugares sin necesidad de cambiar nada.

---

## Características clave del isomorfismo en Axios

1. **Código compartido**:  
   Puedes escribir una función (un conjunto de instrucciones) que use Axios para enviar o recibir información. Esa misma función funcionará tanto en el navegador como en el servidor.

2. **Abstracción del entorno**:  
   Axios detecta automáticamente dónde se está ejecutando tu código (en el navegador o en el servidor) y usa las herramientas adecuadas para cada caso.  
   - En el navegador, usa algo llamado `XMLHttpRequest`.  
   - En el servidor, usa herramientas llamadas `http` o `https`.

3. **Soporte para SSR (Server-Side Rendering)**:  
   En aplicaciones avanzadas como las creadas con **Next.js**, Axios permite obtener datos antes de que la página se muestre al usuario. Esto es útil para aplicaciones que necesitan cargar información rápidamente.

4. **Consistencia en la API**:  
   La forma en que usas Axios y los resultados que obtienes son los mismos, sin importar si el código se ejecuta en el navegador o en el servidor. Esto hace que sea más fácil de aprender y mantener.
   

---

## Ejemplo de uso isomórfico

Aquí tienes un ejemplo sencillo de cómo usar Axios para obtener información de una página web:

```javascript
import axios from 'axios'; // Importamos Axios para usarlo

async function obtenerDatos(url) {
  const respuesta = await axios.get(url); // Hacemos una solicitud para obtener datos
  return respuesta.data; // Devolvemos los datos obtenidos
}
```
---
¿Qué hace este código?
Importar Axios:
La primera línea trae Axios para que podamos usarlo en nuestro programa.

Crear una función:
Creamos una función llamada obtenerDatos que recibe una dirección web (URL) como entrada.

Hacer una solicitud:
Usamos Axios para pedir información de esa dirección web.

Devolver los datos:
Una vez que Axios obtiene la información, la función la devuelve para que podamos usarla en otro lugar.

¿Por qué es isomórfico?
Este código funcionará igual si lo usas en:

Un navegador (por ejemplo, en una página web).
Un servidor (por ejemplo, para procesar datos antes de enviarlos al navegador).
Resumen
El isomorfismo en Axios te permite escribir código que funciona tanto en el navegador como en el servidor. Esto es útil porque:

Ahorras tiempo al no tener que escribir dos versiones diferentes del mismo código.
Es más fácil mantener y entender tu programa.
Puedes crear aplicaciones que funcionan bien en cualquier entorno, como las que usan frameworks modernos como Next.js.
Incluso si no tienes experiencia en programación, piensa en Axios como un "cartero" que lleva y trae información entre tu aplicación y otras partes de internet. ¡Y lo mejor es que este cartero trabaja igual de bien en cualquier lugar! ```

## **TEMA 4 – Full Stack + AWS + DynamoDB**

### **Full Stack**

Un **desarrollador full stack** es capaz de trabajar en **frontend**, **backend**, **bases de datos**, **despliegue** y **mantenimiento**.

#### **Ventajas**

- **Ciclos de desarrollo más rápidos**
- **Mejor comprensión del sistema completo**
- **Colaboración mejorada**

#### **Desventajas**

- **Necesidad de un amplio conjunto de habilidades**
- **Dificultad para mantenerse actualizado**
- **Posibilidad de convertirse en un cuello de botella**

#### **Tecnologías implicadas**

- **Frontend**: HTML, CSS, JavaScript, React, Angular, Vue.js
- **Backend**: Java (Spring), Python (Django), Node.js (Express), etc.
- **Bases de datos**: SQL (MySQL, PostgreSQL), NoSQL (MongoDB, DynamoDB)
- **Control de versiones**: Git
- **Cloud**: AWS, Azure, Google Cloud

---

### **AWS y DynamoDB**

**Amazon Web Services (AWS)** es una plataforma de servicios en la nube.

#### **¿Qué es DynamoDB?**

- **DynamoDB** es una base de datos **NoSQL** totalmente gestionada por AWS.
- Modelo de datos: **clave-valor** y **documentos** (estructura flexible).

#### **Diferencias entre bases SQL y NoSQL**

| Característica      | Base de datos SQL         | Base de datos NoSQL         |
|---------------------|--------------------------|-----------------------------|
| Modelo de datos     | Relacional (tablas)      | Clave-Valor, Documento, etc.|
| Esquema             | Fijo y predefinido       | Flexible o sin esquema      |
| Escalabilidad       | Vertical                 | Horizontal                  |
| Propiedades         | ACID                     | BASE                        |
| Lenguaje de consulta| SQL                      | Varía                       |
| Casos de uso        | Transacciones complejas  | Alto rendimiento, escalabilidad|

---

#### **Operaciones básicas en DynamoDB**

- **PutItem (Inserción):**

        ```bash
        # Inserta un nuevo ítem en la tabla "MiTabla" con id, nombre y precio.
        aws dynamodb put-item --table-name MiTabla --item '{"id": {"N": "1"}, "nombre": {"S": "Producto A"}, "precio": {"N": "25.00"}}'
        ```

- **GetItem (Consulta por clave):**

        ```bash
        # Obtiene un ítem de la tabla "MiTabla" usando la clave primaria id=1.
        aws dynamodb get-item --table-name MiTabla --key '{"id": {"N": "1"}}'
        ```

- **Query (Consulta por clave de partición):**

        ```bash
        # Consulta ítems en la tabla "MiTabla" donde el nombre es "Producto A".
        aws dynamodb query --table-name MiTabla --key-condition-expression "nombre = :nombre" --expression-attribute-values '{":nombre": {"S": "Producto A"}}'
        ```

- **Scan (Consulta completa de la tabla):**

        ```bash
        # Escanea todos los ítems de la tabla "MiTabla".
        aws dynamodb scan --table-name MiTabla
        ```

---

## **TEMA 5 – JUnit + Queries + Repaso general**

### **JUnit**

**JUnit** es un framework de **pruebas unitarias** para Java.

#### **Cómo crear un test**

```java
// Clase Calculadora con un método para sumar dos números.
public class Calculadora {
                public int sumar(int a, int b) {
                                return a + b;
                }
}

// Test unitario para la clase Calculadora usando JUnit.
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculadoraTest {
                @Test
                public void testSumar() {
                                Calculadora calculadora = new Calculadora();
                                int resultado = calculadora.sumar(2, 3);
                                assertEquals(5, resultado); // Verifica que la suma es correcta.
                }
}
```

#### **Anotaciones importantes**

- **@Test**: Marca un método como caso de prueba.
- **@BeforeEach**: Se ejecuta antes de cada método de prueba.
- **@AfterEach**: Se ejecuta después de cada método de prueba.
- **@BeforeAll**: Se ejecuta una vez antes de todos los métodos.
- **@AfterAll**: Se ejecuta una vez después de todos los métodos.
- **@Ignore**: Ignora un método de prueba.

---

### **Queries en JPA**

#### **JPQL vs Native Queries**

- **JPQL**: Lenguaje de consulta orientado a objetos.
- **Native Queries**: Consultas SQL directas.

```java
// Consulta JPQL: obtiene productos cuyo precio es mayor a un valor mínimo.
Query query = em.createQuery("SELECT p FROM Producto p WHERE p.precio > :precioMinimo");
query.setParameter("precioMinimo", 10.0);
List<Producto> productos = query.getResultList();

// Consulta nativa: obtiene productos usando SQL puro.
Query query = em.createNativeQuery("SELECT * FROM Productos WHERE precio > ?", Producto.class);
query.setParameter(1, 10.0);
List<Producto> productos = query.getResultList();
```

#### **Ejemplos de consultas JPQL**

- **Filtro WHERE:**

        ```java
        // Consulta JPQL para buscar productos cuyo nombre contiene "Televisor".
        Query query = em.createQuery("SELECT p FROM Producto p WHERE p.nombre LIKE :nombrePatron");
        query.setParameter("nombrePatron", "%Televisor%");
        List<Producto> productos = query.getResultList();
        ```

- **JOIN:**

        ```java
        // Consulta JPQL para obtener libros de un autor específico.
        Query query = em.createQuery("SELECT l FROM Libro l JOIN l.autor a WHERE a.nombre = :nombreAutor");
        query.setParameter("nombreAutor", "Gabriel García Márquez");
        List<Libro> libros = query.getResultList();
        ```

- **Agregación:**

        ```java
        // Consulta JPQL para contar productos cuyo precio es mayor a un valor.
        Query query = em.createQuery("SELECT COUNT(p) FROM Producto p WHERE p.precio > :precioMinimo");
        query.setParameter("precioMinimo", 50.0);
        Long cantidad = (Long) query.getSingleResult();
        ```

---

### **Práctica: Escribir tests simples en Java y consultas JPQL**

#### **Clase de ejemplo para pruebas JUnit**

```java
// Clase CadenaUtil con métodos para invertir cadenas y verificar palíndromos.
public class CadenaUtil {
                public String invertirCadena(String cadena) {
                                return new StringBuilder(cadena).reverse().toString();
                }

                public boolean esPalindromo(String cadena) {
                                String invertida = invertirCadena(cadena).toLowerCase();
                                return cadena.toLowerCase().equals(invertida);
                }
}
```

#### **Modelo JPA para consultas JPQL**

```java
// Entidad Cliente: representa un cliente que puede tener muchos pedidos.
@Entity
public class Cliente {
                @Id
                @GeneratedValue(strategy = GenerationType.IDENTITY)
                private Long id;
                private String nombre;
                @OneToMany(mappedBy = "cliente") // Un cliente puede tener muchos pedidos.
                private List<Pedido> pedidos;
                //... getters y setters
}

// Entidad Pedido: representa un pedido que pertenece a un cliente.
@Entity
public class Pedido {
                @Id
                @GeneratedValue(strategy = GenerationType.IDENTITY)
                private Long id;
                private Date fecha;
                private double total;
                @ManyToOne // Muchos pedidos pueden pertenecer a un cliente.
                @JoinColumn(name = "cliente_id")
                private Cliente cliente;
                //... getters y setters
}
```
