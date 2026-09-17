# SAP RICEFW Question Catalog

> Documento operativo. No es una guía para humanos ni un listado genérico de preguntas: es el motor de razonamiento que usa el agente para decidir qué preguntar, cuándo profundizar, cuándo detenerse y qué riesgo está intentando descartar con cada pregunta. Se usa siempre en conjunto con `SAP_Functional_Discovery_Framework.md` (en adelante, "el Framework").

---

## 1. Purpose

- **Objetivo del catálogo:** proveer al agente un banco de preguntas trazado a un propósito, un riesgo y una señal de parada explícitos, en vez de un guion fijo. El agente selecciona de aquí, nunca recita esta lista completa.
- **Relación con el Framework:** este catálogo alimenta la sección 6 (Dynamic Question Framework) del Framework. Las fases y modos del Framework (Discovery Lifecycle §2, Discovery Strategy Selection §3, Principle of Proportionality §4) determinan **qué parte** de este catálogo es aplicable en cada momento; este catálogo nunca decide la fase, solo aporta el contenido de la pregunta.
- **Uso durante Discovery (modo Exploración temprana / Refinamiento funcional, Framework §3):** se usa la sección 2 (Universal Discovery Questions) y la sección 10 (Gap Discovery Questions). El agente no entra todavía en el detalle de una categoría RICEFW concreta salvo que ya haya evidencia suficiente para clasificarla (Framework §5).
- **Uso durante Refinement (modo Refinamiento funcional, Framework §3-B):** se usan las secciones 3 a 9 (catálogos por categoría RICEFW y Cross-Cutting Questions) y la sección 11 (Refinement Questions), siempre acotadas por el Principle of Proportionality (Framework §4).
- **Uso durante Validation (modo Cierre funcional, Framework §3-C):** se usa exclusivamente la sección 12 (Validation Questions), nunca las preguntas de descubrimiento de las secciones 2-10. La sección 13 (Stop Conditions) gobierna transversalmente las tres fases.

---

## 2. Universal Discovery Questions

Preguntas aplicables a cualquier requisito SAP, antes de que exista evidencia suficiente para clasificarlo en una categoría RICEFW. Corresponden al modo Exploración temprana del Framework (§3-A).

### Objetivo de negocio

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Qué problema de negocio resuelve esto, más allá de la solución que se está pidiendo? | Separar el problema de la solución propuesta por el usuario (Framework, Principio 2) | Al inicio de cualquier requisito nuevo, siempre antes de cualquier otra pregunta | Una motivación de negocio verificable (pérdida de tiempo, error, incumplimiento, coste) | La respuesta es vaga ("sería más cómodo") | La respuesta identifica un problema concreto y medible |
| ¿Qué pasa hoy si esto no se resuelve? | Distinguir bloqueante de conveniencia | Inmediatamente después de conocer el objetivo | Consecuencia operativa, normativa o económica concreta | La consecuencia descrita es ambigua o minimizada por el propio usuario | Existe una consecuencia clara y cuantificable (tiempo, dinero, riesgo, cumplimiento) |

### Proceso actual (As-Is)

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Cómo se hace esto hoy (manualmente, en otro sistema, o no se hace)? | Establecer el as-is real, no el deseado | Modo Exploración temprana, antes de contrastar con el estándar | Descripción del proceso actual con actor y herramienta | El usuario describe el proceso en términos de "lo que quiere", no de "lo que pasa hoy" | El agente puede narrar el proceso de inicio a fin sin lagunas |
| ¿Cuál es el disparador que inicia este proceso hoy? | Delimitar el alcance temporal del proceso | Tan pronto se conoce el objetivo | El evento o condición que arranca el proceso | El disparador mencionado es en realidad un síntoma, no el inicio real | El disparador es un evento de negocio verificable y único |

### Proceso futuro (To-Be)

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿En qué debería diferir el proceso futuro del actual, más allá de "estar en SAP"? | Evitar que el usuario confunda "migrar a SAP" con "cambiar el proceso" | Tras completar el as-is | Diferencias funcionales reales pretendidas | El usuario no distingue cambios de proceso de cambios de herramienta | El as-is y el to-be quedan claramente diferenciados |
| ¿Qué del proceso actual debe conservarse sin cambios? | Evitar rediseñar procesos que ya funcionan bien | Tras identificar diferencias to-be | Elementos del proceso a preservar explícitamente | El usuario asume que "todo cambia" sin justificación | Existe una lista explícita de lo que se mantiene |

### Actores

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Quién ejecuta, quién aprueba y quién consume el resultado de este proceso? | Cubrir la dimensión "Actores" del Sufficiency Assessment (Framework §11) | Tan pronto se identifica el proceso | Tres roles diferenciados, no necesariamente tres personas | Alguno de los tres roles queda sin identificar | Los tres roles están identificados y no hay ambigüedad de "quién decide" |
| ¿Existen distintos perfiles de usuario con necesidades distintas dentro de este mismo proceso? | Anticipar variantes por rol antes del Refinement | Cuando el proceso involucra a más de un departamento o nivel jerárquico | Lista de perfiles con sus diferencias relevantes | El usuario menciona "depende de quién lo haga" sin especificar | Los perfiles relevantes están enumerados y diferenciados |

### Dolor actual

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Qué es específicamente lo que falla, tarda o genera error en el proceso actual? | Evitar aceptar quejas genéricas como requisito | Tras el as-is, antes del to-be | Un síntoma concreto y localizable en el proceso | La queja es genérica ("todo es lento") | El síntoma se ubica en un paso concreto del proceso |
| ¿Con qué frecuencia ocurre este dolor y qué coste tiene cada vez que ocurre? | Dimensionar el problema real (alimenta Principle of Proportionality, Framework §4) | Inmediatamente después de identificar el síntoma | Frecuencia y coste aproximados, no exactos | La frecuencia se describe solo cualitativamente ("a veces") | Existe un número o rango razonable |

### Excepciones

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Existen casos en los que este proceso no aplica o se ejecuta de forma distinta? | Anticipar casos borde antes de cerrar cualquier regla | Al cerrar cualquier regla de negocio, modo Refinamiento funcional | Lista de excepciones conocidas o confirmación explícita de que no existen | El usuario responde "puede que alguna" sin concretar | El usuario confirma explícitamente la lista completa o la ausencia de excepciones |

### Riesgos

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Qué podría salir mal si este requisito se implementa tal y como se ha descrito? | Detectar riesgos antes del Validation Gate | Al cerrar el Refinement de un requisito de complejidad media o alta (Framework §4) | Riesgos operativos, de datos o de adopción por el usuario | El usuario minimiza cualquier riesgo sin argumento | Existe al menos un riesgo identificado y mitigado, o una confirmación razonada de que no hay riesgo relevante |

### Cumplimiento normativo

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Existe alguna obligación legal, fiscal o regulatoria que condicione este proceso? | Distinguir requisito obligatorio de preferencia (alimenta Gap Severity, Framework §8) | Siempre que el proceso toque datos fiscales, legales o de cumplimiento | Confirmación de obligación normativa y su fuente | El usuario dice "creo que es obligatorio" sin certeza | Existe confirmación explícita de obligatoriedad (o de su ausencia) |

### Autorizaciones

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Quién debe poder ver, ejecutar o modificar esto, y quién explícitamente no debe poder hacerlo? | Cubrir la dimensión de autorizaciones del Sufficiency Assessment | Modo Refinamiento funcional, tras identificar actores | Reglas de acceso positivas y negativas | El usuario solo menciona quién sí puede, no quién no debe poder | Existen reglas de acceso para ambos sentidos |

### Integraciones

| Pregunta | Propósito | Cuándo usarla | Qué información busca obtener | Señal para profundizar | Señal para detener |
|---|---|---|---|---|---|
| ¿Este proceso depende de información que vive en otro sistema, o alimenta a otro sistema? | Detectar candidatos a categoría Interface (sección 4) | Tan pronto se completa el as-is | Confirmación de dependencia con otro sistema, con o sin nombre concreto | El usuario menciona "a veces se cruza con otro sistema" sin concretar cuál | Se confirma explícitamente la existencia o ausencia de dependencia externa |

---

## 3. Report (R)

### Usuarios
- **Pregunta clave:** ¿quién consume este informe y en qué momento de su trabajo lo necesita?
- **Propósito:** vincular el informe a una decisión de negocio real, no a una curiosidad.
- **Riesgo que intenta descubrir:** informes "bonitos" sin consumidor real ni decisión asociada.
- **Pregunta de seguimiento recomendada:** ¿qué decisión toma o qué acción realiza justo después de ver este informe?

### Filtros
- **Pregunta clave:** ¿bajo qué criterios necesita poder acotar la información (fecha, organización, estado, responsable)?
- **Propósito:** dimensionar la pantalla de selección y evitar informes "todo en uno" sin foco.
- **Riesgo que intenta descubrir:** ausencia de filtros obligatorios que hagan el informe inutilizable en volumen real.
- **Pregunta de seguimiento recomendada:** de estos filtros, ¿cuáles son obligatorios y cuáles opcionales?

### Datos
- **Pregunta clave:** ¿de qué proceso de negocio provienen los datos y con qué nivel de detalle (documento, posición, agregado)?
- **Propósito:** anticipar si el dato existe ya en un objeto estándar o requiere cálculo adicional.
- **Riesgo que intenta descubrir:** dato que en realidad no existe hoy en el sistema tal como se está pidiendo.
- **Pregunta de seguimiento recomendada:** ¿este dato ya se ve hoy en alguna transacción o informe, aunque sea de forma parcial?

### Volumen
- **Pregunta clave:** ¿cuántos registros aproximadamente maneja este informe en un periodo típico?
- **Propósito:** alimentar la complejidad esperada (Framework §4); un volumen alto puede convertir un "informe simple" en complejo.
- **Riesgo que intenta descubrir:** subestimar la complejidad de un informe que en realidad procesa grandes volúmenes.
- **Pregunta de seguimiento recomendada:** ¿este volumen es estable o crece de forma relevante con el tiempo?

### Rendimiento
- **Pregunta clave:** ¿en qué momento del día o del proceso se necesita este informe, y con qué tiempo de espera aceptable?
- **Propósito:** detectar expectativas de tiempo real quefuncionalmente puedan no ser necesarias.
- **Riesgo que intenta descubrir:** exigencia de "tiempo real" que en realidad es una preferencia, no una necesidad de negocio.
- **Pregunta de seguimiento recomendada:** si el informe tardara unos minutos en generarse, ¿afectaría a la decisión que se toma con él?

### Exportación
- **Pregunta clave:** ¿el informe se consume en pantalla, o necesita exportarse a otro formato o sistema?
- **Propósito:** distinguir necesidad de visualización de necesidad de distribución.
- **Riesgo que intenta descubrir:** requisito de exportación que en realidad esconde una necesidad de interfaz (categoría Interface).
- **Pregunta de seguimiento recomendada:** una vez exportado, ¿a dónde va ese archivo y quién lo recibe?

### Seguridad
- **Pregunta clave:** ¿contiene este informe datos que no todos los perfiles deberían ver (salarios, márgenes, datos personales)?
- **Propósito:** anticipar restricciones de autorización específicas del informe.
- **Riesgo que intenta descubrir:** exposición de datos sensibles a perfiles no autorizados.
- **Pregunta de seguimiento recomendada:** ¿debería existir una versión resumida del informe para perfiles con menos acceso?

### Navegación
- **Pregunta clave:** ¿desde este informe se necesita poder acceder al documento o transacción de origen (drill-down)?
- **Propósito:** anticipar necesidad de navegación funcional, no solo de visualización estática.
- **Riesgo que intenta descubrir:** informe entregado como "foto fija" cuando el usuario en realidad necesita investigar el detalle.
- **Pregunta de seguimiento recomendada:** cuando encuentra un dato que le llama la atención en el informe, ¿qué hace a continuación hoy?

### Casos excepcionales
- **Pregunta clave:** ¿qué debe mostrar el informe cuando no hay datos, hay datos incompletos, o hay un error de cálculo?
- **Propósito:** evitar comportamiento indefinido ante datos ausentes o erróneos.
- **Riesgo que intenta descubrir:** interpretaciones erróneas de un informe vacío o con datos parciales.
- **Pregunta de seguimiento recomendada:** ¿ha tenido antes un caso de datos incompletos con el informe o proceso actual? ¿qué se hizo?

### Validación del resultado
- **Pregunta clave:** ¿cómo sabrá que el informe está calculando correctamente una vez construido?
- **Propósito:** obtener el criterio de aceptación exigido por los Quality Gates (Framework §9).
- **Riesgo que intenta descubrir:** ausencia de un caso de referencia contra el que contrastar el resultado.
- **Pregunta de seguimiento recomendada:** ¿puede darme un ejemplo con números reales (aunque estén anonimizados) del resultado esperado?

**Indicadores de que el requisito Report ya está suficientemente definido:** consumidor y decisión asociada identificados; filtros obligatorios listados; fuente de datos y granularidad conocidas; formato de salida confirmado; al menos un caso de validación con datos de ejemplo disponible; comportamiento ante datos vacíos definido.

---

## 4. Interface (I)

### Sistemas involucrados
- **Objetivo:** identificar origen y destino exactos del intercambio.
- **Riesgo detectado:** asumir que "el sistema externo" es uno solo cuando en realidad son varios con comportamientos distintos.
- **Cuándo profundizar:** si el usuario nombra el sistema de forma genérica ("el CRM", "el banco") sin especificar cuál si hay varios.
- **Cuándo detener:** origen y destino están nombrados sin ambigüedad y confirmados por el usuario.

### Dirección de datos
- **Objetivo:** confirmar si el flujo es de entrada, salida o bidireccional.
- **Riesgo detectado:** interfaces que en realidad son bidireccionales pero se describen solo en un sentido, dejando fuera la mitad del diseño.
- **Cuándo profundizar:** el usuario describe solo el envío sin mencionar si hay confirmación o respuesta desde el otro sistema.
- **Cuándo detener:** la dirección y, si aplica, el mecanismo de confirmación de vuelta están confirmados.

### Frecuencia
- **Objetivo:** establecer si es tiempo real, near real-time o batch, y cada cuánto.
- **Riesgo detectado:** exigencia de tiempo real que en realidad no es necesaria para el proceso de negocio (ver también Report/Rendimiento).
- **Cuándo profundizar:** el usuario pide "en tiempo real" sin justificar el porqué operativo.
- **Cuándo detener:** existe una frecuencia concreta justificada por una necesidad de negocio, no por preferencia.

### Trigger
- **Objetivo:** identificar el evento de negocio que dispara el envío o la recepción.
- **Riesgo detectado:** confundir un disparador técnico (una hora fija) con un disparador de negocio real (un evento como "pedido confirmado").
- **Cuándo profundizar:** el disparador propuesto es una hora del reloj sin relación con un evento de negocio.
- **Cuándo detener:** el disparador está descrito como evento de negocio verificable.

### Datos intercambiados
- **Objetivo:** listar los campos o entidades de negocio relevantes que viajan en la interfaz, a nivel funcional.
- **Riesgo detectado:** aceptar "todos los datos del pedido" sin acotar qué es realmente necesario en el otro extremo.
- **Cuándo profundizar:** el usuario no puede nombrar los campos concretos que el sistema destino necesita.
- **Cuándo detener:** existe una lista funcional de qué información viaja y con qué propósito en el destino.

### Tratamiento de errores
- **Objetivo:** definir el comportamiento de negocio ante un fallo de envío o recepción.
- **Riesgo detectado:** ausencia total de definición de qué pasa si la interfaz falla (mensajes perdidos, duplicados, bloqueos).
- **Cuándo profundizar:** el usuario no ha pensado en el escenario de fallo (respuesta habitual: "no debería fallar").
- **Cuándo detener:** existe una regla de negocio explícita de reintento, alerta o bloqueo del proceso ante fallo.

### Monitorización
- **Objetivo:** identificar quién debe enterarse de que la interfaz está funcionando o ha fallado.
- **Riesgo detectado:** nadie es responsable de detectar un fallo silencioso.
- **Cuándo profundizar:** no hay un rol claro de "quién se entera si esto deja de funcionar".
- **Cuándo detener:** existe un responsable de negocio identificado para el seguimiento de la interfaz.

### Reconciliación
- **Objetivo:** definir cómo se confirma que ambos sistemas quedan consistentes tras el intercambio.
- **Riesgo detectado:** discrepancias de datos entre sistemas que nadie detecta hasta que causan un problema mayor.
- **Cuándo profundizar:** el usuario asume que "siempre va a cuadrar" sin mecanismo de verificación.
- **Cuándo detener:** existe una forma definida (aunque sea manual) de verificar consistencia periódica.

### Seguridad
- **Objetivo:** identificar sensibilidad de los datos que viajan y necesidad de protección.
- **Riesgo detectado:** datos personales o financieros viajando sin que se haya considerado su sensibilidad.
- **Cuándo profundizar:** los datos intercambiados incluyen información personal, financiera o confidencial.
- **Cuándo detener:** se ha confirmado el nivel de sensibilidad y no quedan dudas sobre el tratamiento requerido a nivel funcional.

### Responsabilidades
- **Objetivo:** definir quién es dueño de negocio de cada extremo de la interfaz.
- **Riesgo detectado:** interfaz sin propietario claro, lo que impide resolver incidencias o cambios futuros.
- **Cuándo profundizar:** nadie asume la propiedad del dato en alguno de los dos extremos.
- **Cuándo detener:** existe un propietario de negocio identificado en origen y en destino.

**Checklist de completitud para una interfaz:** sistemas y dirección confirmados · frecuencia y disparador de negocio definidos · datos a nivel funcional listados · comportamiento ante fallo definido · responsable de monitorización identificado · mecanismo de reconciliación acordado · propietarios de negocio en ambos extremos identificados.

---

## 5. Conversion (C)

- **Fuente de datos:** ¿de qué sistema, archivo o proceso manual provienen los datos a migrar? — *Señal de riesgo:* la fuente es "varias hojas de Excel distintas mantenidas por personas distintas" sin una fuente única de verdad.
- **Calidad:** ¿existen datos duplicados, incompletos o inconsistentes conocidos en el origen? — *Señal de riesgo:* el usuario asume que los datos "están bien" sin haberlos revisado nunca.
- **Transformaciones:** ¿los datos deben transformarse, combinarse o traducirse a otra codificación al migrar? — *Señal de riesgo:* reglas de transformación que solo existen en la cabeza de una persona, no documentadas.
- **Volumen:** ¿cuántos registros aproximados hay que migrar? — *Señal de riesgo:* volumen desconocido que puede convertir una conversión "simple" en compleja (Framework §4).
- **Cargas iniciales:** ¿esta es una carga única de arranque? — *Señal de riesgo:* asumir carga única cuando en realidad habrá varias oleadas (por país, por unidad de negocio).
- **Cargas recurrentes:** ¿se necesitará repetir esta carga periódicamente tras el arranque? — *Señal de riesgo:* confundir una conversión (única) con lo que en realidad es una interfaz recurrente (categoría Interface).
- **Validación:** ¿quién revisará los datos migrados y con qué criterio de corrección? — *Señal de riesgo:* nadie asignado a validar la migración antes de dar por buena la carga.
- **Responsables:** ¿quién es el dueño de negocio de los datos de origen y quién debe aprobar la carga final? — *Señal de riesgo:* ausencia de un aprobador de negocio, dejando la decisión solo en manos técnicas.
- **Cutover:** ¿cuál es la fecha o evento a partir del cual el dato legacy deja de ser válido? — *Señal de riesgo:* no existe una ventana de corte clara, lo que genera datos duplicados o contradictorios durante la transición.
- **Reconciliación:** ¿cómo se confirmará que el número de registros y los totales migrados cuadran con el origen? — *Señal de riesgo:* no hay ningún mecanismo de cuadre definido antes de dar la migración por completada.

---

## 6. Enhancement (E)

Esta es la categoría de mayor riesgo de sobreingeniería y de aceptación indebida de desarrollos; requiere el nivel de rigor más alto.

### Evento disparador
¿Qué acción concreta del usuario o del sistema debería activar este comportamiento adicional?

### Regla de negocio
¿Cuál es exactamente la lógica que debe aplicarse (condición → resultado)? Debe poder expresarse como una regla verificable, no como una descripción general.

### Validaciones
¿Qué combinaciones de datos deberían bloquearse o alertar al usuario, y en qué momento del proceso?

### Excepciones
¿Existen situaciones en las que esta regla no debería aplicarse?

### Mensajes
¿Qué debe comunicarse al usuario cuando se dispara esta lógica (bloqueo duro, aviso, o solo un registro silencioso)?

### Actores
¿Quién provoca la situación que dispara esta lógica, y quién debe actuar cuando ocurre?

### Impacto
¿Qué ocurre operativamente hoy, sin esta mejora? ¿Es una molestia, un riesgo de error, o un incumplimiento?

### Seguridad
¿Esta lógica debe aplicar igual a todos los perfiles o existen roles exentos?

### Auditoría
¿Es necesario dejar constancia de cuándo se disparó esta lógica y quién actuó sobre ella?

### Datos afectados
¿Qué información de negocio (no técnica) participa en esta lógica?

### Situación actual
¿Cómo se resuelve esto hoy sin el sistema (proceso manual, control externo, no se controla)?

### Justificación del gap
¿Por qué el proceso estándar SAP, tal y como se ha mostrado, no es suficiente para este caso?

**Propósito / Riesgo por bloque:** cada pregunta anterior persigue evitar el mismo riesgo de fondo — aceptar como "enhancement" algo que en realidad es una preferencia, una configuración no explorada, o una excepción tan infrecuente que no justifica desarrollo. Ninguna pregunta de este bloque se da por respondida con un "sí, hace falta" sin una razón de negocio verificable detrás.

### Cómo diferenciar Configuración / Gap real / Preferencia / Desarrollo necesario

| Señal en la respuesta del usuario | Clasificación | Acción del agente |
|---|---|---|
| "Así lo prefiero" / "es más cómodo" / "así lo hacíamos antes" sin impacto operativo demostrable | Preferencia de usuario | Descartar como gap; documentar como observación (Framework §8, severidad Observación) |
| La necesidad se resuelve activando una opción o variante ya existente en el sistema | Configuración | No es gap; se resuelve por customizing (Framework §8, severidad Configuración) |
| Existe una razón normativa, de control interno o de imposibilidad operativa sin el cambio, y no hay opción estándar que lo cubra | Gap real | Continuar con el árbol de decisión completo (Framework §8) |
| El gap real requiere lógica no soportada por configuración ni por un punto de extensión estándar razonable | Desarrollo necesario | Clasificar como severidad Desarrollo (Framework §8) y proceder a Refinement |

### Preguntas de Fit-to-Standard (aplicación a Enhancement)

1. "Este es el comportamiento estándar SAP para este proceso: [descripción]. ¿Esto cubre tu caso, o hay algo que deba funcionar de otra manera?" — formulada siempre antes de cualquier otra pregunta de esta sección (Framework §7).
2. "¿Se ha explorado ya con el equipo de configuración si existe una variante o parámetro estándar para este comportamiento?" — obligatoria antes de aceptar el Enhancement como gap real.
3. "Si esto no se resuelve, ¿el proceso de negocio se detiene, se hace mal, o simplemente se hace de forma menos cómoda?" — determina si el gap tiene severidad Desarrollo o si es en realidad una Observación.

---

## 7. Form (F)

- **Destinatarios:** ¿quién recibe este documento y en qué idioma se comunica habitualmente con esa audiencia?
- **Diseño:** ¿existen elementos obligatorios de contenido (no de estética) que deben aparecer siempre?
- **Idiomas:** ¿en cuántos idiomas debe poder generarse el mismo documento?
- **Canales de salida:** ¿el documento se imprime, se envía por email, se sube a un portal, o se transmite por EDI?
- **Requisitos legales:** ¿existe una obligación legal o fiscal que dicte parte del contenido o formato?
- **Branding:** ¿qué elementos son de imagen corporativa (logo, colores) frente a elementos funcionales del documento?
- **Archivos adjuntos:** ¿este documento debe acompañarse de anexos (certificados, condiciones, otros documentos)?
- **Distribución:** ¿el envío es automático al generarse el documento, o requiere una acción manual de aprobación previa?
- **Retención documental:** ¿existe una obligación de conservar copia de este documento y durante cuánto tiempo?
- **Casos especiales:** ¿existen variantes del documento para clientes, países o situaciones específicas (p. ej. anulación, duplicado, corrección)?

**Criterios de completitud para un Form:** destinatario y canal de salida confirmados; idiomas necesarios listados; distinción explícita entre contenido obligatorio (legal) y estético; comportamiento de distribución (automático/manual) definido; variantes especiales identificadas o descartadas explícitamente.

---

## 8. Workflow (W)

- **Aprobaciones:** ¿cuántos niveles de aprobación existen y qué determina cada nivel (importe, categoría, organización)?
- **Escalados:** ¿qué ocurre si un aprobador no actúa dentro de un plazo razonable?
- **Delegaciones:** ¿puede un aprobador delegar su capacidad de aprobar de forma temporal o permanente?
- **Sustituciones:** ¿quién actúa en ausencia planificada (vacaciones, baja) del aprobador titular?
- **Rechazos:** ¿qué ocurre con la solicitud cuando se rechaza (vuelve al origen, se cancela, requiere corrección)?
- **Reenvíos:** ¿puede un aprobador reenviar la solicitud a otra persona para consulta antes de decidir?
- **Notificaciones:** ¿quién debe ser notificado en cada paso del flujo, y por qué medio?
- **SLA:** ¿existe un tiempo máximo esperado para completar cada paso o el flujo completo?
- **Trazabilidad:** ¿es necesario poder reconstruir quién aprobó, cuándo y con qué comentario?
- **Auditoría:** ¿este flujo está sujeto a revisión de auditoría interna o externa?
- **Excepciones:** ¿existen casos que deban saltarse el flujo estándar de aprobación (urgencias, importes mínimos)?
- **Cierre:** ¿qué marca el fin del flujo desde el punto de vista de negocio (todas las aprobaciones, una acción posterior, ambas)?

**Indicadores de complejidad de un Workflow:** un único nivel de aprobación sin condiciones → complejidad Simple; enrutamiento condicional por importe/organización con un nivel de escalado → complejidad Media; múltiples niveles, reglas de enrutamiento variables, delegación, sustitución y trazabilidad auditable → complejidad Compleja (Framework §4).

---

## 9. Cross-Cutting Questions

Preguntas que pueden aparecer al refinar cualquier categoría RICEFW; se activan solo si la conversación las toca, nunca de forma preventiva (ver Conversation Stop Rules, Framework §12).

### Datos maestros
¿Este requisito depende de que un dato maestro (cliente, material, proveedor, empleado) esté completo y correcto? ¿Quién es su propietario?

### Seguridad
¿Existen perfiles que no deberían tener acceso a esta información o funcionalidad?

### Segregación de funciones
¿Podría la misma persona crear y aprobar la misma transacción? ¿Es esto aceptable para el negocio?

### Compliance
¿Existe una política interna o normativa externa que condicione este requisito más allá de lo fiscal/legal ya cubierto en la sección 2?

### Auditoría
¿Es necesario poder demostrar, ante una auditoría, cómo y cuándo ocurrió esto?

### Rendimiento
¿Existe una expectativa de tiempo de respuesta que condicione el diseño funcional (más allá de lo cubierto en Report)?

### Internacionalización
¿Este requisito debe funcionar igual en distintos idiomas o solo en el idioma principal de la operación?

### Multiempresa
¿Esta necesidad aplica igual en todas las sociedades/empresas del grupo, o varía entre ellas?

### Localización legal
¿Existen diferencias legales por país que afecten a este requisito?

### Accesibilidad
¿Existen usuarios con necesidades de accesibilidad específicas que deban considerarse en el diseño funcional?

---

## 10. Gap Discovery Questions

Bloque especializado para ayudar al agente a identificar gaps reales, aplicado dentro del Fit-to-Standard Method (Framework §7).

1. "Este es el proceso estándar SAP para tu situación: [descripción]. ¿Cubre tu caso o hay algo distinto?" — pregunta de apertura obligatoria, nunca sustituible por una pregunta abierta de diseño.
2. "¿Qué parte exacta de lo que acabo de describir no encaja con tu forma de trabajar?" — usada solo si la respuesta anterior es negativa; acota la desviación a un punto concreto.
3. "¿Se ha comprobado si existe una variante de proceso o una opción de configuración para este caso?" — obligatoria antes de aceptar cualquier desviación como gap.
4. "¿Por qué es necesario este cambio: hay una obligación externa, un riesgo de error, o es una preferencia de cómo se ha trabajado hasta ahora?" — determina si hay justificación de negocio real (ver Gap Analysis Framework, Framework §8).
5. "Si este desarrollo no se hiciera, ¿qué se perdería realmente?" — pregunta de cierre para cuestionar desarrollos innecesarios antes de aceptarlos.

---

## 11. Refinement Questions

Se usan solo cuando el requisito ya existe y está clasificado; no sirven para descubrir procesos nuevos.

| Finalidad | Pregunta tipo | Cuándo se usa |
|---|---|---|
| Completar información | "De los datos/reglas que ya tenemos, ¿falta alguno que no hayamos mencionado?" | Cuando un campo Imprescindible del Sufficiency Assessment sigue vacío |
| Cerrar inconsistencias | "Antes dijiste [A], ahora mencionas [B]; ¿cuál de las dos es la regla correcta, o aplican en momentos distintos?" | En cuanto el agente detecta una contradicción, nunca se posterga |
| Construir casos de prueba | "Dame un ejemplo concreto de cuándo esta regla se cumple y uno de cuándo no se cumple." | Al cerrar cualquier regla de negocio, para alimentar los Quality Gates (Framework §9) |
| Definir criterios de aceptación | "¿Cómo sabrá quien pruebe esto que funciona correctamente?" | Antes de dar un requisito por cerrado, siempre |

---

## 12. Validation Questions

Se usan exclusivamente en modo Cierre funcional (Framework §3-C), alineadas con Sufficiency Assessment (Framework §11), Quality Gates (Framework §9) y Validation Gate (Framework §13). Nunca se usan para descubrir información nueva, solo para confirmar lo ya entendido.

- "Este es mi entendimiento del proceso y de las reglas: [resumen]. ¿Es correcto?"
- "Estas son las suposiciones que he asumido sin confirmación explícita: [lista]. ¿Alguna es incorrecta?"
- "Estos son los riesgos que he identificado: [lista]. ¿Falta alguno que conozcas?"
- "Esta información queda pendiente y no bloquea la especificación: [lista de información técnica pendiente, Framework §14]. ¿De acuerdo?"
- La pregunta de cierre literal del Validation Gate (Framework §13): *"Dispongo de información suficiente para generar la especificación funcional. ¿Desea: A) Generarla ahora? B) Continuar refinando el análisis?"*

---

## 13. Stop Conditions

Reglas que impiden formular una pregunta más, incluso si técnicamente "podría" hacerse una pregunta adicional del catálogo. Se aplican transversalmente en cualquier sección de este documento.

- **La pregunta no cambiaría el diseño funcional.** Si la respuesta previsible no alteraría ninguna regla, actor o criterio de aceptación ya documentado, no se formula.
- **La respuesta ya fue obtenida.** Si la información ya se dedujo de algo dicho antes (en esta categoría o en otra), no se vuelve a preguntar; se confirma como asunción si hace falta.
- **Solo falta información Opcional.** Según el Sufficiency Assessment (Framework §11), los campos Opcionales nunca justifican una pregunta adicional que retrase el avance a Validation.
- **Ya existen criterios de aceptación suficientes.** Si el requisito ya tiene al menos un criterio de aceptación verificable, no se insiste en obtener más de uno salvo que el requisito sea de complejidad alta (Framework §4).
- **Ya existen casos de prueba representativos.** Un caso positivo y uno negativo por regla de negocio son suficientes para requisitos de complejidad Simple o Media; solo los de complejidad Compleja pueden justificar casos adicionales.
- **El Confidence Score global ya es Alto.** Si las dimensiones imprescindibles (objetivo, alcance, actores, reglas, validaciones) están en nivel Alto (Framework §10), cualquier pregunta adicional debe justificarse explícitamente por su impacto, no formularse por rutina.
- **La pregunta es de naturaleza técnica, no funcional.** Volumetría exacta, release SAP, nombre de objetos técnicos o diseño de arquitectura nunca son motivo para seguir preguntando (Framework §14); se registran como información técnica pendiente y se continúa.

---

## 14. Discovery Playbooks

Guion prioritario por área, para reducir el tiempo hasta la información crítica.

### SAP SD
- **Preguntas prioritarias:** determinación de precios y descuentos especiales; condiciones de bloqueo de pedidos; tratamiento de devoluciones parciales.
- **Riesgos típicos:** aceptar reglas de descuento "caso a caso" sin estructurarlas; ignorar el tratamiento de devoluciones hasta que ya es tarde.
- **Errores habituales de descubrimiento:** preguntar por el flujo completo de venta cuando el requisito real es puntual (p. ej. solo el bloqueo de crédito).
- **Señal de suficiencia:** reglas de precio, bloqueo y devoluciones cubiertas con al menos un ejemplo numérico cada una.

### SAP MM
- **Preguntas prioritarias:** reglas de aprobación de compra por importe/organización; tratamiento de discrepancias en recepción; proveedores con condiciones especiales.
- **Riesgos típicos:** asumir un único flujo de aprobación cuando existen variantes por categoría de compra.
- **Errores habituales de descubrimiento:** no preguntar por el caso de compra urgente/excepcional hasta el final de la conversación.
- **Señal de suficiencia:** reglas de aprobación, discrepancias y excepciones de proveedor confirmadas con actores identificados.

### SAP FI
- **Preguntas prioritarias:** validaciones previas a contabilización; requisitos fiscales o legales locales; reglas de asignación contable automática frente a manual.
- **Riesgos típicos:** dar por hecho un requisito fiscal sin confirmarlo con quien tiene la obligación normativa.
- **Errores habituales de descubrimiento:** profundizar en detalle técnico contable antes de confirmar si el requisito es realmente obligatorio o preferencia de reporting interno.
- **Señal de suficiencia:** validaciones, obligación normativa y regla de asignación contable confirmadas.

### SAP BP (Business Partner)
- **Preguntas prioritarias:** propietario del dato maestro; reglas de detección de duplicados; ciclo de vida (creación, bloqueo, baja).
- **Riesgos típicos:** asumir una única fuente de verdad del dato maestro cuando hay varias áreas manteniéndolo de forma descoordinada.
- **Errores habituales de descubrimiento:** no preguntar qué pasa cuando un socio de negocio deja de estar activo.
- **Señal de suficiencia:** propietario, reglas de duplicados y ciclo de vida confirmados.

### SAP Workflow
- **Preguntas prioritarias:** niveles de aprobación y reglas de enrutamiento; comportamiento ante rechazo; sustitución del aprobador.
- **Riesgos típicos:** aceptar un flujo de aprobación sin preguntar por el caso de ausencia del aprobador.
- **Errores habituales de descubrimiento:** centrarse solo en la aprobación positiva sin cubrir rechazo y escalado.
- **Señal de suficiencia:** enrutamiento, rechazo, escalado y sustitución cubiertos con actores identificados.

### Interfaces
- **Preguntas prioritarias:** sistemas y dirección; disparador de negocio; tratamiento de errores.
- **Riesgos típicos:** exigir tiempo real sin justificación de negocio; no definir comportamiento ante fallo.
- **Errores habituales de descubrimiento:** enfocarse en el formato técnico del mensaje antes de confirmar la necesidad de negocio.
- **Señal de suficiencia:** checklist de completitud de la sección 4 cumplido.

### Informes
- **Preguntas prioritarias:** consumidor y decisión asociada; filtros obligatorios; fuente y granularidad del dato.
- **Riesgos típicos:** construir informes sin decisión de negocio identificada detrás.
- **Errores habituales de descubrimiento:** centrarse en el formato visual antes de confirmar el dato y su origen.
- **Señal de suficiencia:** indicadores de la sección 3 cumplidos.

### Formularios
- **Preguntas prioritarias:** obligación legal frente a preferencia de branding; idiomas; canal de salida.
- **Riesgos típicos:** invertir esfuerzo en diseño estético antes de confirmar contenido legal obligatorio.
- **Errores habituales de descubrimiento:** no preguntar por variantes especiales (anulación, duplicado, corrección) hasta que aparecen en producción.
- **Señal de suficiencia:** criterios de completitud de la sección 7 cumplidos.

---

## 15. Completeness Matrix

| Tipo RICEFW | Información imprescindible | Información recomendable | Información opcional | Señal de suficiencia | Señal de sobreanálisis |
|---|---|---|---|---|---|
| Report | Consumidor, decisión asociada, fuente de dato, filtros obligatorios, criterio de validación | Formato de salida, comportamiento ante datos vacíos, necesidad de drill-down | Preferencias visuales, orden de columnas | Todos los imprescindibles cerrados y un ejemplo numérico validado | Se sigue preguntando por estética o variantes de formato sin impacto funcional |
| Interface | Sistemas y dirección, disparador de negocio, datos funcionales intercambiados, comportamiento ante fallo | Frecuencia exacta, mecanismo de reconciliación, responsable de monitorización | Protocolo técnico, formato exacto del mensaje | Checklist de la sección 4 completo | Se pregunta por protocolo, formato técnico o arquitectura antes de cerrar lo funcional |
| Conversion | Fuente de datos, volumen aproximado, ventana de cutover, responsable de validación | Reglas de transformación, mecanismo de reconciliación | Herramienta técnica de carga | Fuente, volumen, cutover y validación confirmados | Se indaga en el detalle técnico de la herramienta de carga en vez del dato de negocio |
| Enhancement | Evento disparador, regla de negocio verificable, justificación del gap, impacto si no se resuelve | Excepciones, mensajes al usuario, auditoría | Frecuencia exacta del escenario | Regla de negocio expresable como condición→resultado y gap confirmado por el árbol de decisión (Framework §8) | Se sigue preguntando tras confirmar que es una preferencia sin impacto de negocio (debería descartarse, no seguir refinando) |
| Form | Destinatario, canal de salida, contenido legal obligatorio | Idiomas, variantes especiales, retención documental | Branding y estética | Contenido obligatorio y canal confirmados | Se profundiza en diseño visual antes de confirmar el contenido legal obligatorio |
| Workflow | Niveles de aprobación, reglas de enrutamiento, comportamiento ante rechazo | Escalado, delegación, sustitución, SLA | Detalle exacto del texto de las notificaciones | Enrutamiento, rechazo y actores confirmados | Se indaga en el redactado exacto de notificaciones antes de cerrar el enrutamiento principal |

**Uso de la matriz:** el agente contrasta el estado real de la conversación contra esta tabla, por tipo RICEFW detectado, para decidir si puede pasar a Validation (Framework §11 y §13) o si debe seguir en Refinement. Una fila con todos los "Imprescindible" cerrados es condición suficiente para intentar el paso a Validation, independientemente de cuánto quede en "Recomendable" u "Opcional".
