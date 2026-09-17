# SAP Requirement Decomposition Rules

> Documento operativo. No es una guía para humanos, ni material de formación, ni teoría de análisis funcional. Define reglas de decisión para que un agente de IA transforme una necesidad de negocio expresada en lenguaje natural en un conjunto de requisitos funcionales independientes, trazables y clasificables, **antes** de que cualquiera de ellos entre en el discovery detallado.

---

## 1. Purpose

- **Misión del documento:** proveer al agente el algoritmo y las reglas de decisión para pasar de "lo que el usuario dijo" a "una lista de requisitos individuales, delimitados y clasificables", evitando tanto los requisitos gigantes ambiguos como la fragmentación excesiva.
- **Relación con `SAP_Functional_Discovery_Framework.md`:** este documento actúa **antes** de la fase 1 (Discovery) del Framework. El Framework asume que ya existe un requisito delimitado sobre el que aplicar sus fases (Discovery Lifecycle §2), sus modos (Discovery Strategy Selection §3) y su Principle of Proportionality (§4); este documento es el que produce esa unidad delimitada.
- **Relación con `SAP_RICEFW_Question_Catalog.md`:** el catálogo de preguntas se activa **por requisito individual**, no por necesidad de negocio completa. Este documento decide cuántos requisitos existen y qué categoría(s) RICEFW tiene cada uno, antes de que el catálogo de preguntas empiece a usarse sobre cada uno por separado.
- **Relación con `SAP_Functional_Specification_Template.md`:** cada requisito resultante de la descomposición se convierte, tras superar el Framework, en una ficha de la sección 8 (Functional Requirements) de la plantilla; un Requirement Group (sección 7 de este documento) puede dar lugar a una única especificación con varias fichas de requisito, o a varias especificaciones relacionadas, según el resultado de la sección 14 (Traceability Rules).
- **Momento exacto de aplicación:** inmediatamente al recibir cualquier declaración de necesidad de negocio, y siempre antes de formular la primera pregunta del Question Catalog. Si en cualquier punto posterior del discovery aparece una necesidad nueva no descompuesta todavía (p. ej. el usuario menciona de pasada una necesidad distinta durante el Refinement de otra), el agente interrumpe brevemente el flujo y aplica este documento sobre esa nueva necesidad antes de continuar.

---

## 2. Core Principle

- **Una necesidad de negocio NO es un requisito.** Una necesidad de negocio es una declaración de intención o de dolor ("necesitamos mejorar el proceso de compras"); no es accionable ni verificable por sí misma.
- **Un requisito NO es un objeto RICEFW.** Un requisito es una unidad de valor de negocio; puede materializarse en uno o varios objetos RICEFW (Framework §5), o en ninguno si resulta ser un Fit puro tras el Fit-to-Standard Assessment (Template §7).
- **Un requisito debe representar una unidad funcional coherente y verificable:** un único objetivo, un único actor principal, un único resultado esperado, y al menos un criterio de aceptación propio que pueda validarse de forma independiente del resto de requisitos del mismo Requirement Group.

Este documento existe porque violar cualquiera de estos tres principios produce, más adelante, alguno de los síntomas descritos en la sección 18 (Anti-Patterns): requisitos gigantes, mezcla de categorías, o pérdida de trazabilidad.

---

## 3. Decomposition Trigger Rules

Señales que obligan al agente a dividir una necesidad de negocio en dos o más requisitos. Basta con que se cumpla **una** señal para disparar la división de esa parte de la necesidad.

### Multiple Business Goals
Cuando la necesidad contiene más de un objetivo de negocio independiente.
**Ejemplo:** "Aprobar pedidos y generar informes" → objetivo 1: controlar la aprobación (Workflow); objetivo 2: dar visibilidad del estado (Report). → **Dividir.**

### Multiple Primary Actors
Cuando el actor principal cambia entre partes de la necesidad, no como colaborador secundario sino como responsable del resultado.
**Ejemplo:** "Compras debe aprobar el pedido y Finanzas debe validar el presupuesto" → dos actores principales con responsabilidades distintas → **Dividir en requisitos diferentes.**

### Multiple Acceptance Criteria
Cuando existen criterios de aceptación que pueden cumplirse o incumplirse de forma completamente independiente entre sí.
**Regla:** si el criterio de aceptación A puede ser verdadero mientras el criterio B es falso, sin que eso invalide el requisito, son dos requisitos. → **Dividir.**

### Multiple Business Outcomes
Cuando el resultado esperado (el efecto observable en el proceso de negocio) cambia de naturaleza entre partes de la necesidad, no solo de detalle.
**Regla:** si se puede completar un resultado sin haber completado el otro, y ambos aportan valor de negocio por separado, son resultados distintos. → **Dividir.**

### Multiple Business Rules
Cuando existen reglas de negocio independientes, cada una evaluable sin depender de la otra para tener sentido.
**Regla:** dos reglas que comparten el mismo disparador pero producen efectos de negocio distintos y evaluables por separado se consideran reglas independientes. → **Dividir.**

### Multiple RICEFW Categories
Cuando la necesidad, tal como se ha expresado, apunta claramente a más de una categoría RICEFW (Framework §5) que no son consecuencia directa la una de la otra.
**Ejemplos:** Workflow + Report · Workflow + Form · Enhancement + Interface → **Requisitos distintos**, cada uno con su propia ficha en la Functional Specification Template (§8-9), aunque compartan el mismo Requirement Group (sección 7).

**Regla de aplicación conjunta:** cuando varias señales de esta sección coinciden en la misma necesidad (caso frecuente), el agente divide por la señal de mayor jerarquía según el orden de la sección 16 (AI Decision Rules), no señal por señal de forma aislada, para no sobre-fragmentar.

---

## 4. Aggregation Rules

Señales que indican que el agente **no debe dividir**, aunque exista variación dentro de la necesidad. Estas reglas tienen precedencia sobre una aplicación mecánica de la sección 3 cuando la variación detectada es de detalle, no de naturaleza.

- **Excepciones de una misma regla de negocio.** Si la variación es "la regla se cumple, salvo en el caso X", sigue siendo la misma regla con una excepción documentada (Framework §9, criterio "Con criterio de aceptación"), no una regla nueva.
- **Variantes menores de un mismo resultado.** Si dos formas de ejecutar la acción llevan al mismo resultado de negocio y comparten actor y disparador, son variantes del mismo requisito, no requisitos distintos.
- **Filtros distintos del mismo informe.** Añadir un criterio de selección adicional a un Report ya identificado no crea un nuevo requisito; se documenta como un filtro más dentro del mismo requisito (Question Catalog §3).
- **Validaciones relacionadas dentro de la misma regla.** Varias validaciones que protegen la misma regla de negocio (p. ej. varios campos obligatorios de una misma transacción) se agrupan bajo el mismo requisito.
- **Diferentes condiciones de una misma lógica condicional.** Si las condiciones alimentan la misma tabla de decisión (mismo disparador, mismo actor, mismo resultado de negocio general), son ramas de la misma regla, no reglas independientes.

**Regla de decisión rápida:** ante la duda entre dividir y agregar, el agente se pregunta: "¿Puedo dar por completado uno sin el otro y seguir aportando valor de negocio reconocible?" Si la respuesta es no, se agrega. Si la respuesta es sí, se divide (ver también sección 16).

---

## 5. Requirement Hierarchy Model

Jerarquía oficial que estructura cualquier conjunto de requisitos derivado de una necesidad de negocio:

```
Business Need
  ↓
Requirement Group
  ↓
Requirement
  ↓
Business Rule
  ↓
Acceptance Criteria
  ↓
Test Scenario
```

| Nivel | Propósito | Granularidad | Cuándo se crea | Cuándo se reutiliza |
|---|---|---|---|---|
| **Business Need** | Captura la declaración original del usuario, sin procesar | Una frase o párrafo tal como se recibió | Al recibir cualquier declaración de necesidad | Nunca se reutiliza entre necesidades distintas; una nueva declaración crea una nueva Business Need, aunque esté relacionada con una anterior |
| **Requirement Group** | Agrupa los requisitos que resultan de una misma Business Need y comparten un proceso de negocio común | Un proceso de negocio identificable (p. ej. "Purchase Approval Process") | Cuando la descomposición (sección 3) produce más de un requisito de la misma Business Need | Se reutiliza cuando una nueva Business Need resulta pertenecer al mismo proceso ya cubierto por un grupo existente (sección 7) |
| **Requirement** | Unidad funcional coherente y verificable (Core Principle, sección 2) | Un objetivo, un actor principal, un resultado, verificable de forma independiente | Cada vez que la sección 3 dispara una división, o cuando la necesidad ya es atómica desde el origen | No se reutiliza; cada requisito es único, aunque puede depender de otro (sección 8) |
| **Business Rule** | Lógica condición → resultado dentro de un requisito | Una condición evaluable | Durante el Refinement del requisito en el Framework | Se reutiliza entre requisitos del mismo grupo solo si la regla es idéntica y verificada como tal; en caso de duda, se duplica documentalmente antes que asumir una reutilización incorrecta |
| **Acceptance Criteria** | Condición verificable de cierre de un requisito o de una Business Rule | Una condición Given/When/Then (Template §11) | Al cerrar cada Business Rule, durante Refinement/Validation | No se reutiliza entre requisitos distintos |
| **Test Scenario** | Caso positivo o negativo que demuestra un Acceptance Criteria | Una secuencia de acción de negocio con resultado esperado | Al construir la especificación (Template §12) | No se reutiliza entre requisitos distintos |

**Regla de integridad:** ningún nivel se salta hacia abajo sin haber completado el nivel superior; no puede existir una Business Rule sin un Requirement al que pertenezca, ni un Acceptance Criteria sin al menos una Business Rule o un resultado esperado del Requirement al que se vincule.

---

## 6. Requirement Boundary Detection

Heurísticas para detectar dónde termina un requisito y empieza otro, usando seis elementos de contraste:

- **Objetivo:** el propósito de negocio que persigue.
- **Actor:** quién es responsable del resultado.
- **Trigger:** qué evento de negocio lo inicia.
- **Resultado:** qué efecto observable produce.
- **Regla:** la lógica condición → resultado que aplica.
- **Decisión de negocio:** qué elección o juicio humano interviene.

**Regla de límite:** si al comparar dos partes de una misma necesidad, **cualquiera** de estos seis elementos cambia de forma significativa (no como matiz, sino como sustitución completa del valor: otro objetivo, otro actor, otro disparador, otro resultado, otra regla, otra decisión), el agente considera la creación de un nuevo requisito. Un cambio menor o parcial en un solo elemento, con los otros cinco idénticos, no es suficiente por sí solo (ver Aggregation Rules, sección 4); la sección 16 detalla el peso relativo de cada elemento cuando hay ambigüedad.

---

## 7. Requirement Group Model

Un Requirement Group agrupa los requisitos derivados de la misma Business Need (o de necesidades distintas que resultan pertenecer al mismo proceso de negocio), preservando el contexto compartido sin forzar a que compartan una única especificación.

**Ejemplo:**

```
Requirement Group: Purchase Approval Process
  REQ-001  Workflow Approval
  REQ-002  Approval Notification
  REQ-003  Approval Reporting
  REQ-004  Approval Audit Tracking
```

- **Cuándo crear un grupo:** en cuanto la sección 3 produce más de un requisito a partir de la misma Business Need, o en cuanto se detecta que una nueva Business Need pertenece al mismo proceso de negocio de un grupo ya existente.
- **Cuándo reutilizar un grupo existente:** cuando una nueva Business Need comparte el mismo proceso de negocio (mismo macro-objetivo y mismo conjunto de actores principales) que un grupo ya abierto y no cerrado; el nuevo requisito se añade a ese grupo en lugar de crear uno nuevo.
- **Cuándo cerrar un grupo:** cuando todos sus requisitos han alcanzado el estado "Lista para construcción" en la Functional Specification Template (AI Generation Instructions), o cuando el usuario confirma explícitamente que no habrá más requisitos derivados de ese proceso de negocio en el alcance actual.

---

## 8. Dependency Classification

Toda relación entre dos requisitos del mismo Requirement Group (o de grupos distintos que se referencian) se clasifica en uno de estos seis tipos:

| Tipo | Definición | Señales | Impacto en la generación de especificaciones |
|---|---|---|---|
| **Independent** | Ningún requisito necesita al otro para tener sentido de negocio ni para implementarse | Pueden validarse y construirse en cualquier orden | Se generan como especificaciones separadas sin restricción de secuencia |
| **Dependent** | Un requisito necesita que el otro exista o esté implementado para tener sentido | El actor menciona "esto solo tiene sentido si ya existe X" | La especificación del requisito dependiente declara la dependencia en su sección de Assumptions and Dependencies (Template §17) |
| **Sequential** | Los requisitos deben ejecutarse en un orden de negocio determinado, aunque ambos sean independientes en su construcción | El resultado de uno es la precondición funcional del siguiente | Se documenta el orden en la Traceability Matrix (Template §20) y en el Business Process Description acumulado del grupo |
| **Alternative** | Solo uno de los requisitos aplicará, según una condición de negocio, nunca ambos a la vez | El usuario describe "o se hace así, o se hace de esta otra forma" | Se generan ambas especificaciones marcadas explícitamente como alternativas mutuamente excluyentes, nunca como opciones ambiguas dentro de una misma especificación |
| **Optional** | El requisito aporta valor por sí mismo pero no es indispensable para que el proceso de negocio funcione | Se describe como "sería bueno tener también" | Se prioriza como Deseable (Template §4) y puede quedar fuera del alcance inicial sin bloquear el resto del grupo |
| **Derived** | El requisito surge como consecuencia directa de otro (p. ej. un Report de auditoría que nace porque existe un Workflow) | El requisito no existiría si el otro no se hubiera identificado primero | Se referencia explícitamente al requisito origen en su Business Context (Template §3) |

**Regla de registro:** toda dependencia identificada se documenta en el momento en que se detecta, nunca se pospone a la fase de Validation; una dependencia no clasificada bloquea la Decomposition Quality Gate (sección 17).

---

## 9. RICEFW Mapping Rules

### Requirement → Single Category
Caso ideal: el requisito, tras aplicar el Framework (§5) y el Question Catalog, se clasifica en una única categoría RICEFW. No requiere tratamiento especial; avanza directamente con esa clasificación.

### Requirement → Multiple Categories
Cuando un mismo requisito no puede resolverse sin más de una categoría RICEFW a la vez (p. ej. un Enhancement que dispara necesariamente un Workflow de aprobación de la excepción), el agente no fuerza una única categoría: documenta el requisito con categorías combinadas en la Functional Specification Template (§9, incluyendo ambas subsecciones aplicables) y lo señala explícitamente en la Requirement Overview (Template §4) como "Categoría combinada", en lugar de elegir una y ocultar la otra.

### Business Need → Multiple Requirements
Cuando la descomposición (sección 3) produce varios requisitos a partir de la misma Business Need, la trazabilidad se preserva mediante el Requirement Group (sección 7): todos los requisitos resultantes referencian la misma Business Need de origen y el mismo Requirement Group, aunque tengan categorías RICEFW distintas y avancen por el Framework de forma independiente.

### Category Conflict Resolution
Cuando un requisito parece pertenecer a más de una categoría de forma ambigua (no combinada, sino dudosa — por ejemplo, no está claro si es un Report o una Interface), el agente resuelve el conflicto preguntando por el consumidor final del resultado (Question Catalog §2, categoría Objetivo de negocio): si el consumidor es una persona que visualiza o exporta, es Report; si el consumidor es otro sistema de forma automática y recurrente, es Interface. Si tras esta pregunta persiste la ambigüedad, el requisito se marca como "Decisión pendiente" (Framework §7) y no avanza a Refinement hasta resolverse.

---

## 10. Requirement Identification Scheme

**Nomenclatura base:** `REQ-NNN`, numeración secuencial de tres dígitos como mínimo, única dentro del Requirement Group, nunca reutilizada aunque un requisito se descarte.

**Sufijo de categoría RICEFW**, cuando el requisito ya tiene categoría confirmada: `REQ-NNN-<letra>`, donde la letra corresponde a R (Report), I (Interface), C (Conversion), E (Enhancement), F (Form) o W (Workflow).

**Ejemplos:**
- `REQ-001` — requisito recién identificado, categoría aún no confirmada.
- `REQ-001-W` — el mismo requisito, ya confirmado como Workflow.
- `REQ-001-W` y `REQ-002-R` — dos requisitos distintos del mismo Requirement Group, con categorías distintas.
- Un requisito con categoría combinada (sección 9) usa ambos sufijos separados por `+`: `REQ-003-E+W`.

**Reglas de consistencia:**
- El ID se asigna en el momento en que la sección 3 confirma que existe un requisito individual, nunca antes (una Business Need no descompuesta no recibe ID de requisito).
- El sufijo de categoría se añade únicamente cuando la clasificación RICEFW es firme (no en "Decisión pendiente"); mientras tanto, el requisito circula solo con `REQ-NNN`.
- El ID de requisito es el mismo que se usa después en la Functional Specification Template (§8, campo ID) y en la Traceability Matrix (Template §20); no se genera un ID nuevo al pasar de este documento al Framework o a la plantilla.

---

## 11. Requirement Decomposition Workflow

Algoritmo obligatorio, en orden, ante cualquier Business Need nueva:

1. **Capturar la necesidad.** Registrar la declaración del usuario tal cual, sin interpretar ni corregir todavía.
2. **Identificar objetivos.** Enumerar cada objetivo de negocio distinto contenido en la declaración (Decomposition Trigger Rules, sección 3).
3. **Identificar actores.** Enumerar cada actor principal mencionado o implícito.
4. **Identificar resultados.** Enumerar cada resultado de negocio esperado, distinguiendo los que son independientes entre sí.
5. **Identificar reglas.** Enumerar, al nivel de detalle disponible en este momento (sin refinar todavía), las reglas de negocio mencionadas o insinuadas.
6. **Detectar categorías RICEFW.** Aplicar una primera clasificación provisional por cada combinación objetivo-actor-resultado, usando las señales del Framework (§5).
7. **Separar requisitos.** Aplicar las Decomposition Trigger Rules (sección 3) y las Aggregation Rules (sección 4) para decidir la partición final.
8. **Identificar dependencias.** Clasificar cada relación entre los requisitos resultantes según la sección 8.
9. **Crear grupos.** Agrupar los requisitos resultantes en uno o más Requirement Groups (sección 7), reutilizando grupos existentes cuando aplique.
10. **Validar completitud.** Verificar que se cumple la Decomposition Quality Gate (sección 17) antes de continuar.
11. **Pasar al Discovery Framework.** Entregar cada requisito, ya delimitado, con su ID (sección 10) y su Requirement Group, al `SAP_Functional_Discovery_Framework.md` para iniciar su discovery individual (Framework §2, fase 1).

**Regla de no bloqueo:** si en el paso 6 o 7 persiste una ambigüedad de categoría o de límite que no se resuelve con la información disponible en la Business Need original, el agente no detiene todo el algoritmo: marca ese requisito específico como "Decisión pendiente" (Framework §7) y continúa descomponiendo el resto, resolviendo la ambigüedad puntual ya dentro del Discovery Framework de ese requisito concreto.

---

## 12. Complexity Assessment

Clasificación de complejidad de cada requisito ya delimitado, previa a su entrada en el Framework (donde alimenta directamente el Principle of Proportionality, Framework §4).

| Complejidad | Señales (a partir de este documento) |
|---|---|
| **Simple** | Un solo actor principal; una sola regla de negocio identificada; cero o una excepción; una sola categoría RICEFW; sin dependencias, o como máximo una dependencia Independent |
| **Medium** | Dos actores relacionados (uno principal, uno secundario); dos a tres reglas de negocio; una o dos excepciones; una categoría RICEFW, o una categoría combinada simple; una o dos dependencias, de tipo Dependent o Sequential |
| **Complex** | Más de dos actores con responsabilidades distintas; más de tres reglas de negocio; múltiples excepciones; categorías combinadas (sección 9) o ambigüedad de categoría no resuelta; tres o más dependencias, o presencia de dependencias Alternative |

**Impacto en el Framework:**
- **Discovery:** un requisito Complex entra directamente en modo Refinamiento funcional si ya tiene evidencia suficiente al salir de este documento, evitando repetir la Exploración temprana ya cubierta por el algoritmo de la sección 11 (Framework §3).
- **Refinement:** determina el número de preguntas del Question Catalog a formular (Framework §6.2, regla de proporcionalidad) y el número de reglas de negocio y excepciones que se espera cerrar antes de pasar a Validation.
- **Validation:** un requisito Complex exige que el Confidence Score (Framework §10) esté en Alto en más dimensiones simultáneamente antes de disparar el avance automático; un requisito Simple puede avanzar con menos verificación cruzada.

---

## 13. Requirement Portfolio View

Estructura que el agente mantiene para gestionar múltiples requisitos de forma simultánea, especialmente cuando una Business Need se descompone en varios:

| Business Need | Requirement Group | Requirement | Category | Complexity | Dependency | Status |
|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |

- **Business Need / Requirement Group:** identifican el origen y el proceso de negocio (secciones 5 y 7).
- **Requirement:** el ID (sección 10).
- **Category:** la categoría RICEFW confirmada o "Decisión pendiente".
- **Complexity:** el resultado de la sección 12.
- **Dependency:** el tipo de dependencia (sección 8) y el ID del requisito relacionado, si aplica.
- **Status:** la fase del Discovery Lifecycle en la que se encuentra ese requisito dentro del Framework (§2) — Discovery / Fit-to-Standard Assessment / Gap Analysis / Refinement / Validation / Functional Specification.

**Regla de uso:** esta vista se actualiza cada vez que un requisito cambia de fase en el Framework o de estado en la Functional Specification Template; el agente la consulta antes de decidir en qué requisito centrar la siguiente pregunta, para no mezclar el discovery de dos requisitos distintos en la misma línea de conversación.

---

## 14. Traceability Rules

Relación obligatoria que debe poder reconstruirse para cada requisito, de principio a fin:

```
Business Need
  ↓
Requirement Group
  ↓
Requirement
  ↓
Business Rule
  ↓
Acceptance Criteria
  ↓
Test Scenario
  ↓
Functional Specification
```

**Criterios de trazabilidad completa:**
- Todo Requirement referencia exactamente una Business Need y exactamente un Requirement Group (nunca huérfano, nunca múltiple en este sentido).
- Toda Business Rule referencia el Requirement al que pertenece (Framework §9, Template §10).
- Todo Acceptance Criteria referencia al menos una Business Rule o, en su defecto, el Resultado esperado del Requirement (Template §11).
- Todo Test Scenario referencia al menos un Acceptance Criteria (Template §12).
- Toda Functional Specification referencia el/los Requirement(s) que documenta, y estos a su vez conservan la cadena completa hacia arriba hasta la Business Need original.

**Regla de bloqueo:** una cadena rota en cualquier eslabón (p. ej. una Business Rule sin Requirement, o un Test Scenario sin Acceptance Criteria) impide que ese requisito alcance el estado "Lista para construcción" en la Functional Specification Template, independientemente de cuán completa esté el resto de su información (Template, AI Generation Instructions).

---

## 15. Decomposition Examples

### Ejemplo 1 — "Necesitamos automatizar las aprobaciones de compras."

- **Análisis:** un único verbo de acción ("aprobar") pero con necesidades implícitas de visibilidad y trazabilidad que suelen aparecer en la misma conversación.
- **División:** al indagar (Question Catalog §2, categoría Riesgos y Autorizaciones), surgen tres necesidades independientes: controlar el enrutamiento de aprobación, notificar a los interesados, y poder auditar quién aprobó qué.
- **Requisitos resultantes:**
 - `REQ-001-W` — Enrutamiento y niveles de aprobación de pedidos de compra.
 - `REQ-002-F` — Notificación al aprobador y al solicitante en cada paso.
 - `REQ-003-R` — Informe de trazabilidad de aprobaciones para auditoría.
- **Categorías RICEFW:** Workflow, Form, Report — Requirement Group: *Purchase Approval Process*.

### Ejemplo 2 — "Necesitamos controlar las devoluciones."

- **Necesidad original:** declaración de dolor genérico, sin actor ni resultado explícito.
- **Descomposición:** el discovery temprano (Framework §3-A) revela dos procesos distintos que el usuario está mezclando: la autorización de la devolución (quién puede aceptarla) y el registro del motivo de la devolución para análisis posterior.
- **Grupos:** ambos requisitos pertenecen al mismo Requirement Group (*Returns Management*) por compartir el mismo proceso de negocio, pero son requisitos independientes entre sí (Dependency Classification: Independent).
- **Reglas:** `REQ-001` (autorización) contiene la regla "una devolución por encima de un importe requiere aprobación de un supervisor"; `REQ-002` (registro de motivo) contiene la regla "todo motivo debe seleccionarse de una lista cerrada, con un campo libre solo si se elige 'Otro'".

### Ejemplo 3 — "Necesitamos informar automáticamente a los responsables."

- **Cuándo es Workflow:** si "informar" implica que el responsable debe tomar una acción (aprobar, revisar, decidir) y el sistema debe controlar ese enrutamiento y su estado.
- **Cuándo es Form:** si "informar" implica generar un documento con contenido específico (p. ej. un resumen formal) que se envía o distribuye, sin que el destinatario deba actuar dentro del sistema.
- **Cuándo es Report:** si "informar" implica que el responsable debe poder consultar información actualizada bajo demanda, sin que exista un envío proactivo ni una acción de aprobación asociada.
- **Regla de resolución:** el agente aplica la Category Conflict Resolution (sección 9) preguntando si el destinatario debe actuar (→ Workflow), solo recibir un documento (→ Form), o solo consultar (→ Report); estas tres opciones no son necesariamente excluyentes y pueden generar tres requisitos Derived (sección 8) del mismo Requirement Group.

### Ejemplo 4 — Caso complejo multi-RICEFW

- **Necesidad:** "Necesitamos que, cuando se cree un pedido de venta por encima de un importe, se bloquee automáticamente, se solicite aprobación al director comercial, se le avise por correo con el detalle del pedido, y quede un registro para la auditoría anual; además, esta información debe llegar también al sistema de tesorería para actualizar el flujo de caja previsto."
- **Descomposición completa:**
 - `REQ-001-E` — Enhancement: validación y bloqueo automático del pedido por importe.
 - `REQ-002-W` — Workflow: enrutamiento de aprobación al director comercial.
 - `REQ-003-F` — Form: correo de notificación con el detalle del pedido.
 - `REQ-004-R` — Report: informe de auditoría anual de bloqueos y aprobaciones.
 - `REQ-005-I` — Interface: envío de la información del pedido bloqueado/aprobado al sistema de tesorería.
- **Dependencias:** `REQ-002` es Dependent de `REQ-001` (solo hay aprobación si hubo bloqueo); `REQ-003` es Derived de `REQ-002` (la notificación nace porque existe el workflow); `REQ-004` es Derived de `REQ-001` y `REQ-002` combinados; `REQ-005` es Independent en su construcción pero Sequential en el proceso de negocio (solo tiene sentido enviar a tesorería un pedido ya aprobado).
- **Trazabilidad:** las cinco requisitos comparten la misma Business Need y el mismo Requirement Group (*Sales Order Credit Control*), con cadenas de trazabilidad independientes desde su propia Business Rule hasta su propia Functional Specification, tal como exige la sección 14.

---

## 16. AI Decision Rules

Sección crítica: reglas explícitas de decisión, priorizadas por orden de aplicación (de mayor a menor peso). El agente evalúa en este orden y se detiene en la primera regla que dispare una acción clara.

1. **Si cambia la categoría RICEFW → dividir.** Máxima prioridad: una diferencia de categoría siempre implica un requisito distinto, incluso si el resto de elementos (actor, trigger) coinciden.
2. **Si cambian los objetivos de negocio → dividir.** Dos objetivos de negocio distintos no se fuerzan en un mismo requisito aunque compartan actor y disparador.
3. **Si cambia el actor principal → dividir.** Un cambio de responsable del resultado (no de colaborador secundario) es señal de un requisito distinto.
4. **Si cambia el resultado esperado → dividir.** Un efecto de negocio distinto, aunque comparta disparador y actor, se documenta como requisito separado.
5. **Si cambia el trigger → evaluar división.** No dispara división automática por sí solo; se evalúa junto con el resultado esperado (regla 4) — un mismo resultado con dos disparadores distintos puede seguir siendo un único requisito con dos rutas de entrada.
6. **Si solo cambian excepciones → no dividir.** Ver Aggregation Rules, sección 4.
7. **Si solo cambia un filtro → no dividir.** Aplica exclusivamente a Report; ver sección 4.
8. **Si solo cambia una validación relacionada → no dividir.** La validación se agrega a la Business Rule existente.

**Regla de desempate:** cuando dos reglas de esta lista entran en conflicto para el mismo caso (p. ej. el trigger cambia pero el resultado no), prevalece siempre la regla de número más bajo (mayor prioridad) de esta lista.

---

## 17. Decomposition Quality Gates

El agente no puede entregar ningún requisito al `SAP_Functional_Discovery_Framework.md` hasta que se cumplan, para el conjunto completo derivado de la Business Need en curso:

- **Todos los requisitos están identificados:** no queda ningún fragmento de la declaración original sin asignar a un requisito, un grupo, o explícitamente descartado como ruido conversacional.
- **Todos tienen categoría tentativa:** cada requisito tiene al menos una categoría RICEFW provisional o está marcado explícitamente como "Decisión pendiente" (nunca sin ningún valor).
- **Las dependencias están clasificadas:** toda relación detectada entre requisitos tiene un tipo asignado según la sección 8, ninguna queda "por definir".
- **No existen duplicidades:** ningún par de requisitos del mismo grupo comparte el mismo objetivo, actor y resultado simultáneamente (si ocurre, se fusionan antes de continuar).
- **Existe trazabilidad mínima:** cada requisito referencia su Business Need y su Requirement Group (sección 14), incluso si las Business Rules y Acceptance Criteria todavía no existen (esos se completan durante el Framework, no aquí).

Si alguno de estos criterios no se cumple, el agente permanece en el Requirement Decomposition Workflow (sección 11) y no avanza ningún requisito del conjunto al Discovery Framework, para evitar que unos requisitos avancen mientras otros quedan huérfanos o mal delimitados.

---

## 18. Anti-Patterns

Comportamientos que este documento existe para prevenir:

- **Requisito gigante.** Aceptar una declaración de negocio completa como un único requisito sin aplicar las Decomposition Trigger Rules (sección 3), produciendo una especificación inmanejable y con criterios de aceptación imposibles de verificar de forma independiente.
- **Requisito ambiguo.** Dejar un requisito avanzar al Framework sin haber resuelto el Requirement Boundary Detection (sección 6), arrastrando la ambigüedad a fases posteriores donde es más costoso corregirla.
- **Dividir en exceso.** Aplicar las Trigger Rules de forma mecánica sobre variaciones que en realidad son Aggregation Rules (sección 4), generando decenas de micro-requisitos sin valor de negocio independiente y multiplicando innecesariamente el esfuerzo de discovery (contradice el Principle of Proportionality del Framework, §4).
- **Mezclar varias categorías sin declararlo.** Documentar un requisito con comportamiento de dos categorías RICEFW distintas bajo una sola categoría, ocultando la necesidad real de una de ellas (ver Requirement → Multiple Categories, sección 9).
- **Crear requisitos sin valor de negocio.** Generar un requisito que no se sostiene en ningún objetivo de negocio verificable, habitualmente por descomponer literalmente cada verbo de la declaración original en lugar de razonar sobre el valor real.
- **Crear objetos RICEFW antes del Gap Analysis.** Asignar una categoría RICEFW definitiva y avanzar en su diseño antes de que el Fit-to-Standard Assessment y el Gap Analysis Framework (Framework §§7-8) hayan confirmado que existe un gap real; este documento solo asigna categorías **provisionales** (sección 6 del Requirement Decomposition Workflow), nunca definitivas.
- **Perder trazabilidad.** Permitir que un requisito, una Business Rule, un Acceptance Criteria o un Test Scenario existan sin su referencia hacia arriba en la jerarquía (sección 5) o sin cumplir las Traceability Rules (sección 14), haciendo imposible reconstruir después por qué existe ese elemento del documento final.
