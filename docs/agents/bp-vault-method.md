<!-- bp-vault-skills:start -->

# Contrato de método de BP Vault

## Propósito

Este vault conserva material universitario del ITBA: clases, teoría, prácticas, guías, resúmenes, parciales, finales, TPs y defensas.

Los BP Vault Skills deben usar los objetos y términos existentes. No deben introducir una taxonomía personal, un inbox, estados de aprendizaje ni nuevas propiedades sin una solicitud separada.

## Autoridad de las fuentes

Antes de responder una pregunta académica, teórica o técnica, buscar primero en el vault.

La autoridad depende del contexto:

1. Para notación, alcance, métodos exigidos y respuestas esperadas en una evaluación, prevalecen la cátedra y las notas del vault.
2. Para exactitud jurídica, técnica, histórica o normativa fuera del examen, prevalece la fuente primaria oficial y vigente.
3. Las fuentes secundarias sirven como apoyo cuando no existe una fuente primaria adecuada.

Cuando la cátedra y una fuente oficial discrepan:

- mostrar ambas versiones;
- identificar la procedencia de cada una;
- indicar cuál usar para la evaluación;
- indicar cuál describe el dato oficial o vigente;
- no elegir una versión en silencio.

Distinguir:

- hecho: afirmación respaldada por una fuente o resultado observable;
- interpretación: explicación, inferencia o síntesis;
- experiencia: resultado de un intento, práctica, examen, TP o aplicación.

No se impone una sintaxis especial para estas distinciones. Deben quedar claras en el texto.

## Captura

No existe un objeto local dedicado a capturas ni una carpeta de inbox.

Si el usuario pide desarrollar una nota incompleta ya existente, esa nota puede tratarse como captura durante esa operación. Esto no crea una categoría permanente ni autoriza moverla.

Una captura no se considera desarrollada por estar almacenada en la raíz.

## Intención

No existe una propiedad local de intención.

`develop-note` infiere entre pensamiento, decisión, referencia y aprendizaje usando:

- el pedido del usuario;
- el contenido de la nota;
- su procedencia;
- su relación con la materia;
- el resultado que la nota debe permitir.

Si dos intenciones producirían cambios distintos, se consulta al usuario antes de redactar.

La mayoría del contenido académico existente corresponde a referencia o aprendizaje. Esto no debe imponerse a cada nota nueva.

## Conocimiento durable y estado `developed`

Una nota está desarrollada cuando:

- cumple el frontmatter mínimo;
- enlaza la `.base` de su materia;
- su propósito y alcance se entienden sin contexto externo;
- organiza el contenido para recuperarlo y usarlo;
- identifica la fuente o procedencia cuando importa;
- muestra conflictos y dudas relevantes;
- termina con un bloque válido de notas relacionadas;
- señala sus huecos deliberados.

Una nota desarrollada no necesita ser exhaustiva ni tener todas sus preguntas resueltas.

`developed` no se guarda en una propiedad. Es una evaluación en lenguaje natural.

Una nota desarrollada no implica que el usuario haya aprendido su contenido.

## Práctica

Los objetos locales de práctica incluyen:

- notas cuyo nombre o título indica práctica;
- guías y ejercicios resueltos;
- preguntas de defensa;
- repasos que exigen responder sin mirar;
- intentos documentados dentro de un TP.

Una práctica produce evidencia solo cuando registra un intento u otro resultado observable. Leer soluciones o marcar material como revisado no demuestra comprensión.

## Aplicación

Una aplicación lleva conocimiento fuera de la revisión pasiva. En este vault puede ser:

- resolver un ejercicio nuevo;
- explicar o derivar un concepto sin ayuda;
- implementar código;
- ejecutar un notebook o experimento;
- aplicar teoría en un TP;
- defender una decisión frente a preguntas;
- resolver un caso distinto del ejemplo estudiado.

Toda aplicación propuesta debe indicar:

- contexto;
- acción o artefacto;
- criterio observable de éxito;
- evidencia que se conservará;
- condición concreta de revisión.

Usar una nota de práctica, TP, evaluación o contenido ya existente cuando corresponda. Crear una nota nueva requiere borrador y aprobación.

## Evidencia de aprendizaje

La evidencia útil incluye:

- errores concretos;
- brechas pendientes;
- conexiones nuevas;
- recuperación lograda sin apoyo;
- transferencia lograda a un caso distinto;
- resultados observables de una práctica, TP o evaluación;
- una condición para revisar de nuevo.

No conservar transcripts completos ni respuestas rutinarias correctas.

Destino:

- La evidencia de una práctica, TP, defensa o repaso se guarda en esa misma nota.
- La evidencia de un test sobre una nota teórica se guarda bajo `## Evidencia de aprendizaje` en esa nota.
- No se crea una nota ni una propiedad nueva solo para registrar evidencia.

Cada registro contiene:

- fecha;
- evidencia observada;
- brecha pendiente, si existe;
- próxima condición de revisión.

Agregar o cambiar evidencia requiere mostrar el borrador y obtener aprobación.

## Estado `learned`

Una nota pulida, el tiempo de estudio, el reconocimiento de una respuesta o una explicación recién mostrada no prueban aprendizaje.

El contenido puede considerarse aprendido solamente cuando el usuario:

1. recupera o produce el conocimiento sin apoyo;
2. lo transfiere correctamente a un caso diferente.

La evidencia debe provenir de una interacción observada, como una sesión de `test-understanding`, y quedar registrada según la sección anterior.

`learned` no se guarda en una propiedad. Si falta recuperación o transferencia, se describe la evidencia disponible y el estado queda indeterminado.

## Modos de revisión

El vault admite revisión de notas, prácticas, aplicaciones, TPs y evaluaciones. No tiene convenciones de revisión diaria, semanal o mensual.

### Revisión de una nota o tema

Se activa cuando el usuario pide evaluar desarrollo, comprensión o progreso sobre una nota.

Fuentes:

- la nota objetivo;
- su sección de evidencia;
- prácticas, TPs y evaluaciones relacionadas;
- notas enlazadas de la misma materia.

Preguntas:

- ¿La nota está desarrollada según este contrato?
- ¿Existe recuperación observada?
- ¿Existe transferencia a un caso nuevo?
- ¿Existe aplicación cuando el tema la requiere?
- ¿Qué evidencia falta?

Destino:

- `## Evidencia de aprendizaje` en la nota objetivo, con aprobación;
- si no se aprueba la escritura, el resultado queda solo en la conversación.

La próxima revisión se expresa como una condición concreta, no como una automatización.

### Revisión de práctica, aplicación o TP

Se activa al evaluar un intento, ejercicio, proyecto, notebook, implementación o defensa.

Fuentes:

- consigna;
- intento o artefacto;
- resultados y checklists;
- decisiones documentadas;
- feedback y preguntas de defensa;
- teoría enlazada.

Preguntas:

- ¿Qué resultado observable se obtuvo?
- ¿Qué parte fue resuelta sin apoyo?
- ¿Qué se transfirió desde la teoría?
- ¿Cuál es la brecha que limita el siguiente intento?
- ¿Cuál es el siguiente control proporcional?

Destino:

- la misma nota de práctica, aplicación o TP;
- nunca una nota nueva sin aprobación.

La próxima revisión se registra como condición, por ejemplo después de corregir la brecha o resolver un caso nuevo.

### Revisión de evaluación o preparación

Se activa para parcialitos, parciales, finales, bancos de preguntas y controles de preparación.

Fuentes:

- intentos sin ayuda;
- errores registrados;
- ejercicios o preguntas de años anteriores;
- material y notación de cátedra;
- fuentes oficiales cuando existe una discrepancia;
- evidencia de transferencia.

Preguntas:

- ¿Qué puede recuperar el usuario sin apoyo?
- ¿Puede aplicar el concepto ante un enunciado distinto?
- ¿Qué errores se repiten?
- ¿Qué respuesta espera la cátedra?
- ¿Existe alguna diferencia con la respuesta oficial?
- ¿Qué evidencia falta para afirmar preparación?

Destino:

- la nota existente de evaluación o repaso;
- si el objetivo es un tema concreto sin nota de evaluación, la nota teórica correspondiente.

La próxima revisión se expresa mediante una condición concreta, como resolver otra variante sin ayuda o volver a probar una brecha determinada.

## Ausencias explícitas

No hay mapeo local para:

- inbox o captura permanente;
- estados mediante propiedades;
- revisión diaria, semanal o mensual;
- una nota universal de evidencia;
- una taxonomía de pensamiento, decisión, referencia y aprendizaje.

Estas ausencias deben respetarse. No se completan inventando carpetas, propiedades o categorías.

<!-- bp-vault-skills:end -->
