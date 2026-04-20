---
layout: post
title: "Muros duros alrededor de la IA"
date: 2026-04-20
---

Hay una escena que se repite en cualquier equipo que ha dejado entrar a la IA en su repositorio. El modelo escribe a una velocidad que marea, el diff se lee bien, la PR se *mergea* casi por inercia. Meses después, alguien abre un directorio cualquiera y descubre que la arquitectura está carcomida por dentro: la fachada sigue en pie, pero las vigas de carga se han convertido en serrín.

La conclusión a la que he llegado —y que cada vez es más difícil defender— es que intentar revisar esa inmensa obra ladrillo a ladrillo es una batalla perdida de antemano. Lo que sí podemos hacer es **levantar muros**. Muros implacables que no opinan, que no se dejan seducir por excusas bien redactadas y que solo entienden de dos colores: verde y rojo. El humano abandona el andamio y pasa a ser el arquitecto.

---

## CRAP y mutation testing: el detector de mentiras

Cualquiera que lleve un tiempo midiendo código sabe que la cobertura es una métrica traicionera. Un test puede ejecutar una línea sin comprobar nada útil de ella, como un turista que entra en un museo solo para cruzarlo y salir por la otra puerta. La IA, además, **optimiza precisamente para ese número**: si le pides tests, te va a llenar los archivos de escenarios que tocan todas las líneas sin verificar casi nada. Un [artículo reciente en Medium](https://singhpr.medium.com/your-ai-generated-tests-are-lying-to-you-and-what-to-do-about-it-57fb0e5f2783) lo resume sin anestesia: *tus tests generados por IA te están mintiendo*.

Contra esa mentira existen dos herramientas viejas y buenas.

La primera es la métrica **CRAP** (*Change Risk Anti-Patterns*), que combina complejidad ciclomática con cobertura:

```text
CRAP(m) = comp(m)² × (1 − cov(m))³ + comp(m)
```

Un método retorcido y mal probado dispara su CRAP hasta el tejado. La única forma limpia de bajarlo es partir el método en trozos más pequeños o escribir tests de verdad. Cualquier otra cosa —subir el umbral, esconder el reporte— es pintar la alarma de incendios para que no se vea.

La segunda es el ***mutation testing***, que es una especie de interrogatorio policial para tu suite. [Infection](https://infection.github.io/) toma tu código, lo retoca: invierte un `if`, cambia un `>` por un `>=`, borra una llamada. Después ejecuta los tests. Si siguen pasando, el cambio —el *mutante*— ha sobrevivido, lo que quiere decir que tus tests no se habrían enterado de ese bug:

```bash
vendor/bin/infection --min-msi=80 --threads=max
```

El *Mutation Score Indicator* es, a día de hoy, la única forma honesta de saber si una batería de tests escrita por una IA está verificando algo o solo coreografiando cobertura. No hay dónde esconderse: o cazas a los mutantes o te cazan.

Usadas juntas, CRAP e Infection empujan el código hacia el mismo lugar: funciones pequeñas, dependencias explícitas, comportamientos que de verdad quedan firmemente anclados.

---

## Gherkin: el contrato firmado antes de la obra

Los tests de aceptación no examinan piezas sueltas; contemplan el sistema completo desde la mirada del usuario. Gherkin es su lenguaje claro, con esa cadencia casi litúrgica de `Given / When / Then`:

```gherkin
Feature: Create access assignment for students

  Scenario: A teacher creates an assignment with a valid deadline
    Given I am authenticated as a teacher
    When I create an assignment with a deadline of tomorrow
    Then the assignment is saved
    And the students can see it
```

En PHP/Symfony los ejecuta Behat. Pero la sintaxis es lo de menos. Lo importante es **quién tiene las llaves del archivo**. La investigación reciente muestra que los LLMs [escriben Gherkin con sorprendente soltura, pero también con omisiones y alucinaciones](https://arxiv.org/abs/2508.20744) que un humano debe revisar sí o sí. En dominios sensibles es directamente peligroso dejar que el modelo redacte el contrato que después va a cumplir. Es como pedirle al contratista que escriba también el pliego de condiciones.

La regla que me impongo es sencilla:

- El humano es dueño de los `.feature`. Los escribe, los discute, los versiona.
- La IA solo puede tocar código de producción hasta que los escenarios se pongan verdes.

El día que la IA edita un `.feature` «para que pase», ha reescrito el contrato para encajar la obra. Es la forma más sutil —y más letal— de derribar un muro: hacerlo desde dentro.

---

## *Scrap analyzer*: el jardín de atrás

El código de tests se parece al jardín trasero de una casa: siempre hay tiempo para arreglarlo *mañana*. Y así se llena de malas hierbas. *Scrap* significa chatarra, y un *scrap analyzer* es, básicamente, un jardinero que no deja que eso pase.

Lo que debería disparar alarmas:

- Duplicación entre tests que se han ido copiando en cadena.
- Archivos inflados con docenas de asserts que nadie relee.
- `@skipped` e `@incomplete` fosilizados desde hace meses.
- Data providers que han mutado hasta perder la forma.
- Asserts muertos del tipo `assertTrue(true)`.

En PHP se cubre con `phpcpd` sobre `tests/`, con PHPStan también aplicado al directorio de pruebas y con PHPUnit en modo `--fail-on-risky`. Nada sofisticado; lo sofisticado es no dejar que se pudra.

El código de tests **es** el muro. Si se desmorona, el muro se cae aunque desde fuera siga pareciendo intacto.

---

## *Dependency checker*: la valla que la IA no ve

Si los tests son el muro, la arquitectura es el plano de la ciudad. Un *dependency checker* traza líneas que dicen: *por aquí no se pasa*. El `Domain` no habla con `Infrastructure`, la UI no llama a Doctrine directamente, un *bounded context* no se cuela en otro. Si un archivo cruza una línea, la build se cae. Esto tiene nombre propio: son [*architecture fitness functions*](https://www.infoq.com/articles/fitness-functions-architecture/), esa idea que lleva años circulando de que la arquitectura debe poder comprobarse en cada commit, no contemplarse en un diagrama de la sala de reuniones.

En PHP las dos herramientas de referencia son [Deptrac](https://deptrac.github.io/deptrac/) y [PHPat](https://github.com/carlosas/phpat):

```bash
vendor/bin/deptrac analyse
```

Sin una valla así, la IA toma atajos que le resultan «prácticos» y que no aparecen en ningún diff evidente. Lo notas seis meses después, cuando revertir cuesta más que rehacer. Una regla de Deptrac vale más que un README con treinta convenciones que nadie relee.

---

## *Hard tools* frente a *soft tools*

La distinción que quiero defender es ésta: una herramienta **dura** emite verde o rojo; una **blanda** emite opinión.

| *Hard tools* | *Soft tools* |
|---|---|
| CRAP, MSI, coverage | «Parece correcto» |
| Deptrac, PHPat | Revisión estilística |
| Linters, type checkers | Intuición del revisor |
| Tests de aceptación | «Creo que es suficiente» |

Con IA **solo las duras escalan**. Las blandas dependen de atención humana, y esa atención es justo el recurso que la IA agota primero. A partir de cierto volumen, revisar ladrillo a ladrillo es imposible; mantener los muros en pie, no.

Las *soft tools* son un vigilante nocturno con linterna; las *hard tools*, un cerco electrificado. El vigilante es útil mientras la finca sea pequeña. Cuando la finca crece de golpe, no puedes poner un vigilante por hectárea; tienes que poner vallas.

---

## No dejar escapar a la IA

Ésta es la parte más fácil de enunciar y más difícil de respetar. Escapar de un muro es:

- Bajar un umbral (`--min-msi`, CRAP, nivel de PHPStan) «solo por esta vez».
- Añadir un `@SuppressWarnings` o `// phpstan-ignore` para silenciar la alarma.
- Reescribir un `.feature` para que pase.
- Excluir un archivo de Deptrac para desbloquear un merge.

Cada excepción parece barata en el momento. El problema es que **debilita el sistema entero**: un muro con una puerta abierta deja de ser un muro y pasa a ser una invitación.

El patrón que he aprendido a detectar es casi literario. Cuando la IA propone «relajar temporalmente» una regla para terminar la tarea, la tarea no está terminada: está mal planteada, o la regla estaba mal calibrada. Nunca hay una tercera opción, por muy bien redactada que venga la sugerencia.

---

## El precio de la productividad

La productividad con IA no es gratis ni automática. Se sostiene solo si los muros aguantan todos los días, sin concesiones. Es un mantenimiento; no un interés compuesto.

Cuando alguien presume de haber multiplicado su velocidad con IA, la pregunta útil no es *cuántas líneas genera por hora*, sino *qué muros tiene levantados y con qué umbrales*. Sin esa respuesta, alardear de velocidad es presumir de lo rápido que se apilan los escombros. Ruido con tipografía bonita.

---

## El cambio de rol

| Antes | Con IA |
|---|---|
| El programador escribe cada línea | La IA escribe cada línea |
| El humano revisa código por diff | El humano diseña y mantiene los muros |
| Los tests son un apoyo | Los tests son el contrato |
| Las métricas son orientativas | Las métricas son barreras no negociables |

El humano pasa de **encargado de obra** a **arquitecto**, y también a **autor de los `.feature`**. La IA es el obrero infatigable cercado por métricas duras. Es un papel menos vistoso —nadie celebra la PR que sube el MSI del 78% al 82%—, pero es el único que escala.

Si los muros ceden, el paradigma entero se cae. Y con él la promesa de productividad que justificaba, en primer lugar, entregarle el teclado a la máquina.
