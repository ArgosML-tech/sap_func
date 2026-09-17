# SAP Functional Specification Template

> Documento operativo. No contiene preguntas ni técnicas de descubrimiento — esas viven en `SAP_Functional_Discovery_Framework.md` y `SAP_RICEFW_Question_Catalog.md`. Este documento define la estructura, el contenido esperado y las reglas de cumplimentación para que el agente transforme requisitos ya descubiertos, refinados y validados en una especificación funcional SAP completa y homogénea. Aplica a cualquier producto SAP (S/4HANA, ECC, SuccessFactors, Ariba, IBP u otro) y a cualquier categoría RICEFW.

---

## 1. Document Control

**Contenido esperado:** tabla de metadatos del documento.

| Campo | Valor |
|---|---|
| Título |  |
| ID |  |
| Proyecto |  |
| Cliente |  |
| Autor |  |
| Fecha |  |
| Estado | Borrador / En revisión / Aprobado / Obsoleto |
| Versión |  |

**Reglas de cumplimentación:** el ID es único y estable; no se reutiliza aunque el documento se archive. El Estado nunca es "Aprobado" mientras la sección 21 (Approval) no esté cumplimentada. Cada cambio sustantivo de contenido incrementa la Versión; una corrección de forma no lo hace.

---

## 2. Executive Summary

**Contenido esperado:** explicación en lenguaje ejecutivo de:
- objetivo del requisito
- problema de negocio
- beneficio esperado

**Reglas de cumplimentación:** extensión máxima una página. No contiene reglas de negocio, datos, ni diseño de RICEFW — esos van en las secciones correspondientes. Se redacta como síntesis de las secciones 3 a 5, en último lugar, nunca como primer borrador. No incluye información marcada como pendiente.

---

## 3. Business Context

**Contenido esperado:**
- Situación actual (As-Is)
- Situación futura (To-Be)
- Motivación
- Drivers de negocio
- Contexto funcional

**Reglas de cumplimentación:** As-Is y To-Be se redactan siempre como pares contrastados. La motivación debe estar directamente respaldada por lo capturado durante el Discovery (Framework §2, fase 1); sin esa trazabilidad, la sección se considera incompleta. No se incluyen aquí reglas de negocio detalladas (sección 10) ni datos (sección 13): esta sección es narrativa.

---

## 4. Requirement Overview

**Contenido esperado:**

| Campo | Valor |
|---|---|
| Requisito |  |
| Prioridad | Crítico / Importante / Deseable |
| Categoría RICEFW | Report / Interface / Conversion / Enhancement / Form / Workflow |
| Gap Severity | Observación / Configuración / Extensión estándar / Desarrollo / Cambio organizativo |
| Origen del requisito | Taller / entrevista / incidencia / auditoría / otro, con fecha y referencia |

**Reglas de cumplimentación:** todos los campos son obligatorios y proceden directamente del Discovery Framework y del RICEFW Question Catalog; el agente traslada el resultado ya validado, no infiere valores nuevos aquí. Si Gap Severity es "Observación", el requisito no debería estar generando una especificación funcional completa: el agente lo marca como candidato a descarte y lo señala explícitamente.

---

## 5. Scope

### In Scope
**Contenido esperado:** procesos, transacciones, objetos de negocio, países, sociedades o roles explícitamente cubiertos.

### Out of Scope
**Contenido esperado:** elementos relacionados que podrían asumirse incluidos y explícitamente no lo están, cada uno con su razón de exclusión.

**Reglas de cumplimentación:** ambas listas son obligatorias; un documento sin Out of Scope se considera incompleto. Todo lo listado en Out of Scope debe poder rastrearse a una decisión tomada durante Refinement o Validation, nunca a una omisión del agente.

---

## 6. Stakeholders

**Contenido esperado:**

| Rol | Nombre / Puesto | Responsabilidad en este requisito |
|---|---|---|
| Actor |  |  |
| Responsable |  |  |
| Aprobador |  |  |
| Consumidor |  |  |

**Reglas de cumplimentación:** los cuatro roles se identifican de forma independiente aunque coincidan en la misma persona (se anota explícitamente si coinciden). Ningún rol queda en blanco: si no aplica, se indica "No aplica" con su razón, nunca se omite la fila.

---

## 7. Fit-to-Standard Assessment

Sección obligatoria en todo documento, independientemente de la categoría RICEFW.

### Proceso estándar SAP
**Contenido esperado:** descripción funcional del proceso estándar aplicable, tal como se presentó durante el Fit-to-Standard (Framework §7).

### Comportamiento requerido
**Contenido esperado:** descripción del comportamiento que el negocio necesita, contrastado punto por punto contra el proceso estándar.

### Gap identificado
**Contenido esperado:** enunciado preciso de la diferencia entre el estándar y el comportamiento requerido.

### Justificación de negocio
**Contenido esperado:** razón (normativa, operativa, de control interno) que sostiene el gap, tal como se confirmó durante el Gap Analysis Framework.

### Resultado
**Contenido esperado:** una única clasificación entre:
- Fit
- Configuración
- Extensión estándar
- Desarrollo
- Cambio organizativo

**Reglas de cumplimentación:** las subsecciones se redactan siempre en este orden (estándar → requerido → gap → justificación → resultado), nunca describiendo primero el gap o el resultado. Si el Resultado es "Fit", el documento no debería continuar generándose como especificación de cambio: el agente lo marca y detiene la generación, remitiendo al Framework §7. Todo enunciado debe ser trazable al Fit-to-Standard Method y al Gap Analysis Framework, nunca una suposición del agente. Todo gap debe tener justificación, clasificación (esta sección) y severidad (sección 4) — sin las tres, la sección está incompleta.

---

## 8. Functional Requirements

Sección principal del documento. Se repite una ficha completa por cada requisito funcional dentro del alcance (sección 5).

### ID
Identificador único del requisito, referenciable desde el resto de secciones (9-12, 20).

### Descripción
Enunciado del requisito en una o dos frases, en lenguaje de negocio verificable.

### Actor
Rol que ejecuta o dispara la acción descrita (referenciado a la sección 6).

### Trigger
Evento de negocio que inicia la ejecución de este requisito.

### Precondiciones
Estado del sistema o del proceso que debe cumplirse antes de que el requisito pueda ejecutarse.

### Regla de negocio
Lógica condición → resultado que gobierna el requisito (referenciada a su ID en la sección 10, no duplicada en texto libre).

### Validaciones
Combinaciones de datos o situaciones que deben bloquearse, alertar, o quedar registradas.

### Excepciones
Casos conocidos en los que la regla de negocio no aplica o se comporta de forma distinta.

### Resultado esperado
Efecto observable en el proceso de negocio tras la ejecución correcta del requisito.

### Prioridad
Crítico / Importante / Deseable.

### Criterio de aceptación
Al menos una condición verificable (formato Given/When/Then o equivalente, ver sección 11) que determina cuándo el requisito se considera correctamente implementado.

**Reglas de cumplimentación:** todo requisito debe tener, sin excepción, Actor, Trigger y Criterio de aceptación (Regla de redacción 5); un requisito sin alguno de estos tres campos no se considera listo para especificación. Cada campo se redacta en lenguaje funcional, verificable, sin lenguaje ambiguo ("normalmente", "en general"). Si una Regla de negocio no tiene ID en la sección 10, la ficha del requisito está incompleta.

---

## 9. RICEFW-Specific Section

Subsecciones dinámicas: **solo aparecen las aplicables** a la Categoría RICEFW indicada en la sección 4 para el/los requisito(s) de este documento. Las no aplicables se omiten del documento generado, no se muestran como "No aplica".

### Report — *(incluir solo si Categoría RICEFW = Report)*
- Filtros (obligatorios y opcionales)
- Datos (entidades y granularidad, en lenguaje de negocio)
- Cálculos (fórmulas o agregaciones esperadas, descritas funcionalmente)
- Formato de salida

### Interface — *(incluir solo si Categoría RICEFW = Interface)*
- Origen
- Destino
- Frecuencia
- Datos intercambiados
- Comportamiento ante errores

### Conversion — *(incluir solo si Categoría RICEFW = Conversion)*
- Fuentes
- Reglas de transformación
- Volúmenes
- Validación

### Enhancement — *(incluir solo si Categoría RICEFW = Enhancement)*
- Lógica (referenciada a la regla de negocio de la sección 10, no duplicada)
- Eventos
- Validaciones
- Excepciones

### Form — *(incluir solo si Categoría RICEFW = Form)*
- Destinatarios
- Idiomas
- Distribución
- Contenido obligatorio

### Workflow — *(incluir solo si Categoría RICEFW = Workflow)*
- Niveles
- Reglas
- Escalado
- Sustitución
- Rechazo

**Reglas de cumplimentación:** cada punto de la subsección aplicable se completa a nivel funcional exclusivamente; ninguno incluye nombres de objetos técnicos. Un requisito que combine dos categorías RICEFW (p. ej. un Enhancement que además dispara un Workflow) incluye ambas subsecciones, cada una completa por separado.

---

## 10. Business Rules

**Contenido esperado:** tabla única de reglas de negocio del documento.

| ID | Regla | Justificación | Prioridad |
|---|---|---|---|
|  |  |  |  |

**Reglas de cumplimentación:** cada regla es verificable (Regla de redacción 4): expresable como condición → resultado, sin lenguaje ambiguo. El ID es el mismo que se referencia desde la sección 8 (Regla de negocio) y desde la sección 9 (Enhancement/Lógica) cuando aplique. La Justificación enlaza con la razón de negocio capturada en el discovery, nunca se redacta como una suposición nueva del agente.

---

## 11. Acceptance Criteria

**Contenido esperado:** un bloque por cada requisito de la sección 8, en formato:

```
Given <precondición>
When <acción o evento>
Then <resultado esperado y verificable>
```

O el formato equivalente "Cuando ocurra [condición], el sistema debe [comportamiento]" cuando el formato Given/When/Then no encaje con naturalidad en el requisito.

**Reglas de cumplimentación:** cada requisito de la sección 8 tiene como mínimo un criterio de aceptación aquí; un requisito de complejidad Media o Compleja puede requerir más de uno para cubrir sus principales reglas y excepciones. Todo criterio se redacta en lenguaje de negocio, sin pasos técnicos de ejecución.

---

## 12. Test Scenarios

**Contenido esperado:** por cada requisito de la sección 8:

### Escenario positivo
Secuencia de acción de negocio en la que la regla se cumple según lo esperado.

### Escenario negativo
Secuencia de acción de negocio en la que la regla debe bloquear, alertar o comportarse de forma distinta.

### Resultado esperado
Efecto observable correspondiente a cada escenario.

**Reglas de cumplimentación:** todo requisito de la sección 8 tiene como mínimo un escenario positivo y uno negativo. Ningún escenario incluye pasos técnicos de ejecución (transacciones, rutas de menú, capturas de pantalla): describe la acción de negocio y el resultado, no la interacción con la pantalla.

---

## 13. Data Requirements

**Contenido esperado:**
- Datos utilizados (entidades y campos de negocio relevantes)
- Origen
- Propietario funcional
- Calidad requerida (completitud, unicidad, actualización esperada)

**Reglas de cumplimentación:** no se incluyen nombres de tablas ni estructuras técnicas; el nivel es siempre de negocio. Si el requisito no introduce datos adicionales a los ya estándar del proceso, se indica explícitamente "No aplica — usa los datos estándar del proceso" en lugar de omitir la sección.

---

## 14. Security and Authorizations

**Contenido esperado:**
- Roles
- Accesos (qué puede hacer cada rol)
- Restricciones (qué no debe poder hacer cada rol)

**Reglas de cumplimentación:** exclusivamente a nivel funcional; no técnico (sin perfiles, roles compuestos técnicos ni objetos de autorización). Se redacta siempre en pares acceso permitido / restricción, nunca solo en positivo. Si no hay necesidades de seguridad distintas a las ya estándar del proceso, se indica explícitamente.

---

## 15. Integrations

**Contenido esperado:**
- Sistemas relacionados
- Dependencias
- Impacto funcional

**Reglas de cumplimentación:** no se incluye diseño técnico (protocolos, formatos de mensaje, arquitectura); esta sección documenta la relación funcional con otros sistemas, no cómo se implementa. Si el requisito no tiene integraciones relacionadas, se indica explícitamente en lugar de omitir la sección.

---

## 16. Reporting and Monitoring

**Contenido esperado:**
- Métricas
- KPIs
- Alertas
- Monitorización funcional (quién debe enterarse de qué, y cuándo, sobre el funcionamiento de este requisito)

**Reglas de cumplimentación:** cada métrica o KPI listado debe tener un propietario funcional identificado; una métrica sin responsable de seguimiento se marca como pendiente en la sección 19, no se documenta aquí como cerrada. Si el requisito no requiere monitorización específica, se indica explícitamente.

---

## 17. Assumptions

**Contenido esperado:**
- Supuestos aceptados
- Hipótesis

**Reglas de cumplimentación:** toda entrada debe coincidir con lo presentado literalmente en el Validation Gate (Framework §13, bloque "Suposiciones"); el agente no añade supuestos nuevos en la fase de generación del documento.

---

## 18. Risks

**Contenido esperado:**
- Riesgos funcionales
- Dependencias
- Impactos

**Reglas de cumplimentación:** procede directamente del bloque "Riesgos" del Validation Gate (Framework §13); no se generan riesgos nuevos en esta fase. Un riesgo sin mitigación conocida se documenta igualmente como pendiente, nunca se descarta por falta de solución.

---

## 19. Open Items

**Contenido esperado:** decisiones pendientes, exclusiones e información pendiente, separadas en dos bloques:

### Pendiente funcional
Información de negocio aún no confirmada que sí condiciona la implementación (decisiones pendientes, exclusiones sin cerrar).

### Pendiente técnica
Volumetría exacta, release o versión SAP, objetos técnicos concretos, diseño de arquitectura o de integración, consideraciones de rendimiento técnico.

**Reglas de cumplimentación:** lo listado en Pendiente técnica nunca bloquea el estado "Aprobado" del documento (sección 1); su función es señalar, no condicionar. Lo listado en Pendiente funcional sí puede impedir el paso a "Aprobado" si afecta a un requisito de la sección 8. Si un bloque no tiene contenido, se indica explícitamente ("Sin pendientes funcionales" / "Sin pendientes técnicas") en lugar de omitirlo.

---

## 20. Traceability Matrix

**Contenido esperado:** tabla que relaciona cada requisito con sus elementos asociados.

| Requisito (ID, sección 8) | Gap (sección 7) | Regla de negocio (ID, sección 10) | Criterio de aceptación (sección 11) | Escenario de prueba (sección 12) |
|---|---|---|---|---|
|  |  |  |  |  |

**Reglas de cumplimentación:** cada fila corresponde a un requisito de la sección 8; un requisito sin al menos una entrada en cada columna indica una sección incompleta en el documento (regla de negocio sin criterio de aceptación, criterio sin escenario de prueba, etc.) y debe resolverse antes de marcar el documento como "Lista para construcción" (ver sección AI Generation Instructions).

---

## 21. Approval

**Contenido esperado:**

| Rol | Nombre | Confirmación | Fecha |
|---|---|---|---|
| Preparado por |  |  |  |
| Revisado por |  |  |  |
| Aprobado por |  |  |  |

**Reglas de cumplimentación:** el Estado del documento (sección 1) solo pasa a "Aprobado" cuando las tres filas están cumplimentadas. Esta sección se corresponde con la respuesta "A) Generarla ahora" del Validation Gate (Framework §13); el agente puede prellenar "Preparado por" con su propia identificación de sesión y la fecha, pero nunca cumplimenta "Revisado por" ni "Aprobado por" por sí mismo.

---

## Reglas de redacción

1. Escribir siempre en lenguaje funcional, comprensible sin conocimiento técnico de SAP.
2. No incluir diseño técnico en ninguna sección.
3. No incluir bajo ninguna circunstancia: tablas SAP, BAdIs, user exits, APIs, ni arquitectura — con la única excepción de la sección 19 (Pendiente técnica), donde se registran como pendientes, nunca como contenido resuelto.
4. Toda regla de negocio debe ser verificable: expresable como condición → resultado, sin lenguaje ambiguo.
5. Todo requisito (sección 8) debe tener, sin excepción: Actor, Trigger y Criterio de aceptación.
6. Todo gap (sección 7) debe tener, sin excepción: justificación de negocio, clasificación de Resultado y Gap Severity (sección 4).
7. Toda especificación debe poder ser entendida, sin aclaraciones adicionales, por: un usuario de negocio, un consultor funcional, y un equipo de QA.

---

## AI Generation Instructions

Esta sección orienta al agente sobre cómo transformar los resultados de `SAP_Functional_Discovery_Framework.md` y `SAP_RICEFW_Question_Catalog.md` en este documento.

### Transformación de entradas

- El resumen del Validation Gate (Framework §13) alimenta directamente las secciones 2 (Executive Summary), 3 (Business Context), 17 (Assumptions) y 18 (Risks) — bloque a bloque, sin reinterpretación.
- El resultado del Fit-to-Standard Method y del Gap Analysis Framework (Framework §§7-8) alimenta la sección 7 completa y el campo Gap Severity de la sección 4.
- Cada requisito que superó los Quality Gates (Framework §9) y el Sufficiency Assessment (Framework §11) genera una ficha en la sección 8; el catálogo de preguntas usado para refinarlo (Question Catalog, secciones 3-9 según categoría RICEFW) determina qué subsección de la sección 9 se activa.
- Las reglas de negocio recogidas durante Refinement alimentan la sección 10; los ejemplos y casos obtenidos durante el mismo Refinement alimentan las secciones 11 y 12.
- La información marcada como "información técnica pendiente" en el Framework (§14) y en el Question Catalog (Stop Conditions) alimenta exclusivamente la subsección "Pendiente técnica" de la sección 19, nunca otra sección.

### Campos obligatorios

Sin estos campos, el documento no se genera o se genera marcado como Incompleto: ID y Estado (sección 1); las tres frases de Executive Summary (sección 2); As-Is y To-Be (sección 3); los cinco campos de Requirement Overview (sección 4); In Scope (sección 5); los cuatro roles de Stakeholders (sección 6, aunque sea con "No aplica"); las cinco subsecciones de Fit-to-Standard Assessment (sección 7); Actor, Trigger y Criterio de aceptación de cada requisito (sección 8); al menos un criterio de aceptación por requisito (sección 11); al menos un escenario positivo y uno negativo por requisito (sección 12).

### Campos que pueden quedar vacíos (con marcador explícito)

Out of Scope puede ser una lista corta si el alcance es muy acotado, pero nunca se omite. Data Requirements, Security and Authorizations, Integrations y Reporting and Monitoring (secciones 13-16) pueden completarse con "No aplica" justificado cuando el requisito no lo necesite, nunca dejarse en blanco sin esa frase. Open Items (sección 19) puede tener ambos bloques vacíos, siempre con la frase explícita de ausencia de pendientes.

### Campos que deben generar advertencia

El agente genera una advertencia visible al usuario (no un bloqueo silencioso) cuando detecta: un requisito en la sección 8 sin Regla de negocio asociada en la sección 10; una fila de la Traceability Matrix (sección 20) con alguna columna vacía; un Gap Severity "Observación" en la sección 4 conviviendo con una especificación completa (contradicción: una observación no debería generar una FS); un Resultado "Fit" en la sección 7 (la especificación no debería existir); Pendiente funcional (sección 19) no vacío mientras el Estado (sección 1) se intenta marcar como "Aprobado".

### Criterios de estado de la especificación

| Estado | Criterio |
|---|---|
| **Incompleta** | Falta al menos uno de los campos obligatorios listados arriba, o la Traceability Matrix (sección 20) tiene columnas vacías para algún requisito |
| **Aceptable** | Todos los campos obligatorios están cumplimentados y la Traceability Matrix está completa, pero persisten uno o más elementos en "Pendiente funcional" (sección 19) sin resolver |
| **Lista para construcción** | Todos los campos obligatorios están cumplimentados, la Traceability Matrix está completa, no hay elementos en "Pendiente funcional" sin resolver, y la sección 21 (Approval) tiene al menos "Preparado por" cumplimentado — quedando solo la confirmación humana de Revisado por/Aprobado por para el cierre formal |

El agente nunca marca una especificación como "Lista para construcción" únicamente por sí mismo declarándolo en el texto: el estado se deriva mecánicamente de los tres criterios anteriores, verificables sección por sección.
