# SAP_Demo_Build_Rules.md

Quinta fuente de la base de conocimiento del Arquitecto Funcional SAP.
Se añade junto a `SAP_Requirement_Decomposition_Rules.md`,
`SAP_Functional_Discovery_Framework_v2.md`, `SAP_RICEFW_Question_Catalog.md`
y `SAP_Functional_Specification_Template.md`.

**Hueco que cubre**: las cuatro fuentes existentes gobiernan *cómo se
descompone, se descubre y se redacta* una especificación. Ninguna gobierna
**qué puede afirmar la especificación sobre el sistema destino** ni **qué
objetos puede exigir** cuando el documento va a construirse de verdad.
`SAP_Requirement_Decomposition_Rules.md` solo trata la ambigüedad de
categoría RICEFW; no ofrece criterio general para hechos no confirmados.

---

## 1. Cuándo se aplica este documento

Se aplica cuando la especificación **va a implementarse a partir del propio
documento**, sin una fase de diseño técnico intermedia que corrija sus
supuestos. Típicamente: demos, pruebas de concepto y entregas de un solo
ciclo.

No se aplica a especificaciones destinadas a estimación, a licitación o a
un equipo que hará su propio diseño técnico: ahí las restricciones de la
sección 2 empobrecerían el documento sin necesidad.

Indicador de disparo: el usuario describe un perfil de entrega, un catálogo
de objetos permitido, o pide explícitamente una entrega autocontenida.

---

## 2. Catálogo cerrado de objetos

Cuando el usuario fije un catálogo de objetos, **el diseño técnico no puede
proponer ninguno fuera de él**, y cada renuncia debe presentarse en el
documento como decisión de alcance justificada, nunca como carencia.

Catálogo por defecto de una entrega autocontenida, salvo que el usuario
indique otro:

- Tablas transparentes Z.
- Clases globales, interfaces globales y programas ejecutables.

Lo que queda fuera, con la resolución que debe adoptar el documento:

| Objeto habitual | Resolución en la especificación |
|---|---|
| Dynpro, module pool, PF-STATUS | Listado sobre el contenedor de pantalla por defecto; acciones en ventanas de diálogo de pantalla de selección |
| Dominios y elementos de datos propios | Tipos predefinidos ABAP y elementos de datos estándar existentes |
| Ayudas de búsqueda | Ayuda de valores resuelta por código en tiempo de ejecución |
| Clase de mensajes | Catálogo de textos en una clase de constantes |
| Objeto de rango numérico | Numeración calculada internamente |
| Código de transacción | Ejecución por SE38/SA38 |
| Vista de mantenimiento | Programa cargador de datos, idempotente |
| Clases de prueba ABAP Unit | Guion de pruebas manual incluido en el documento |
| Objeto de bloqueo | Solo si la concurrencia está fuera de alcance |

**Consecuencias que el documento debe declarar** cuando adopte estas
resoluciones, en Assumptions: pérdida de traducibilidad por SE63 al
renunciar a la clase de mensajes y a los elementos de texto; ausencia de
etiquetas de campo centralizadas al renunciar a elementos de datos; y
numeración no segura ante concurrencia al renunciar al rango numérico.

---

## 3. Hechos del sistema: prohibido inventarlos

Toda afirmación del documento sobre el sistema destino pertenece a una de
estas tres categorías, y debe tratarse distinto según cuál sea:

| Categoría | Qué es | Dónde va |
|---|---|---|
| **Decisión de diseño** | Algo que la especificación elige y por tanto posee | Business Rules, Functional Requirements, diseño técnico |
| **Hecho verificado** | Dato del sistema aportado por el usuario o confirmado en las fuentes | Puede sustentar una regla; se cita como verificado |
| **Hecho no verificado** | Cualquier otra afirmación sobre datos, contenido de campos, semántica de tablas o comportamiento dependiente del entorno | **Assumptions (§17) u Open Items (§19). Nunca Business Rules** |

**La regla dura**: si una regla de negocio necesita un hecho que no está
verificado, la regla no se escribe como decidida. Se escribe la parte
decidible y el hecho pendiente se lleva a Open Items, indicando qué
consulta o comprobación lo resolvería.

El modo de fallo que esto evita no es la ignorancia, es la **suposición
plausible**: una regla que parece cerrada, que nadie cuestiona porque está
bien redactada, y que se descubre falsa al implementar.

### Casos reales observados

- Una especificación derivó el estado de picking de un campo de estado de
  la entrega. En el sistema destino ese campo venía vacío, de modo que la
  regla habría marcado "no iniciado" todos los casos. El hecho —si el campo
  está poblado— no era verificable desde el documento: pertenecía a Open
  Items.
- La misma especificación permitió atravesar categorías de documento no
  soportadas "cuando permitan alcanzar otro nodo soportado". En los datos
  reales, uno de esos nodos estaba compartido por cadenas de pedidos
  distintos, de modo que la reconstrucción arrastraba documentos ajenos.

---

## 4. Recorridos sobre relaciones

Cuando la especificación defina un recorrido sobre relaciones entre objetos
—flujo documental, jerarquías, explosiones de lista de materiales, grafos
de dependencias— debe declarar de forma cerrada:

- Qué relaciones se atraviesan y cuáles se tratan como hoja.
- Qué ocurre con una relación no contemplada: se ignora, o se muestra como
  informativa; **nunca "actúa de puente"**.
- Si un nodo puede estar compartido por varias cadenas, no puede usarse
  para pasar de una a otra. Un nodo compartido es hoja del objeto al que
  pertenece.

La deduplicación de nodos visitados **no** resuelve esto: evita repetir un
nodo, no evita que ese nodo conecte cadenas que deberían estar separadas.

---

## 5. Reglas de negocio

- **Condición comprobable**: toda regla se expresa como comparación
  concreta sobre campos identificados. Si no puede expresarse así, no se
  incluye. Una regla como "si los datos son contradictorios" sin definir la
  contradicción es una regla no implementable.
- **Disparador**: toda regla indica en qué punto del proceso se evalúa. Una
  regla sin punto de disparo dentro del alcance no se incluye — se convierte
  en una promesa que el código nunca cumple.
- **Cobertura**: toda regla que rechaza algo y toda acción que confirma algo
  tienen su mensaje en el catálogo.

---

## 6. Catálogo de mensajes

- Contiene **solo mensajes que el usuario llega a ver**. Los eventos
  internos del proceso —relación ya visitada, nodo descartado, iteración
  completada— no son mensajes; si interesan, son traza.
- Cada botón, acción y validación tiene su mensaje.
- Los códigos son correlativos y se referencian desde las reglas y desde
  los casos de prueba.

---

## 7. Una sola opción

No se escriben alternativas condicionadas al release, al producto o al
entorno ("en ECC léase X, en S/4HANA léase Y"). El sistema destino es uno.
Si se conoce, se elige. Si no se conoce, la elección es un Open Item con la
pregunta concreta que lo resuelve, no una bifurcación dentro de la regla.

---

## 8. Coherencia entre alcance y técnica

El alcance no puede prometer ninguna capacidad que la técnica elegida no
ofrezca. Antes de cerrar el documento, contrastar la lista de capacidades
prometidas en Scope contra la técnica descrita en el diseño técnico.

Ejemplo observado: un documento prometía exportación, ordenación y filtrado
estándar de ALV y, a la vez, expandir y contraer niveles. La primera lista
corresponde a un ALV de tabla y la segunda a un ALV de árbol, que no ofrece
la primera.

---

## 9. Límites de nomenclatura

Se respetan al proponer nombres, porque su incumplimiento impide activar:

| Objeto | Máximo |
|---|---|
| Campos de pantalla de selección y de ventanas de diálogo | **8** |
| Tabla transparente de diccionario | **16** |
| Clases, interfaces, programas, métodos, tipos, parámetros | **30** |

---

## 10. Selección y volumen

Cuando la salida se limite a un número máximo de filas, el documento debe
declarar **qué criterio de ordenación se aplica antes de truncar**. Ordenar
después de truncar cambia qué registros ve el usuario y convierte el límite
en un filtro arbitrario.

---

## 11. Datos de prueba

- Los casos de prueba usan **documentos y datos reales** del sistema, si el
  usuario los aporta. No se inventan identificadores.
- Si un caso de prueba no tiene ejemplo entre los datos aportados, se
  declara **no ejecutable** y se indica qué dato haría falta. No se rellena
  con un número inventado ni con un marcador que después nadie completa.
- Cuando el usuario no aporte datos, la matriz se entrega vacía y se
  registra en Open Items como bloqueante de la ejecución del guion, no del
  diseño.

---

## 12. Autocomprobación antes de entregar

Repasar, y corregir lo que falle:

1. ¿Algún objeto del diseño técnico queda fuera del catálogo permitido?
2. ¿Alguna regla de negocio depende de un hecho no verificado? Si sí, ¿está
   ese hecho en Assumptions u Open Items?
3. ¿Alguna regla carece de condición comprobable o de punto de disparo?
4. ¿Algún mensaje del catálogo corresponde a un evento interno?
5. ¿Alguna regla ofrece alternativas por release o entorno?
6. ¿Promete el alcance algo que la técnica elegida no da?
7. ¿Hay algún recorrido sobre relaciones que pueda atravesar un nodo
   compartido?
8. ¿Se ordena antes de truncar?
9. ¿Algún nombre supera su límite de caracteres?
10. ¿Hay identificadores de documento inventados en los casos de prueba?
