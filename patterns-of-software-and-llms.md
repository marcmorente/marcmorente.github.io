# Patterns of Software en la era de los LLMs

> *«La tecnología, la ciencia y la organización empresarial son secundarias a las personas y a los intereses humanos.»*
> — Richard P. Gabriel, *Patterns of Software* (1996)

En 1996, Richard P. Gabriel publicó una colección de ensayos que examinaban la comunidad del software desde una perspectiva inusual: la del crítico literario. Inspirado en Christopher Alexander y en Samuel Johnson, Gabriel introdujo conceptos como *habitabilidad*, *crecimiento progresivo* y *compresión* para diagnosticar los males del desarrollo de software de su época.

Treinta años después, con los LLMs generando código a una velocidad y escala impensables entonces, cabe preguntarse: ¿siguen siendo válidas sus ideas? La respuesta es incómoda —y reveladora—.

---

## Lo que los LLMs amplifican (para mal)

### El problema de la habitabilidad se agrava

Gabriel definía *habitabilidad* como la cualidad del código que permite a los programadores entenderlo y modificarlo cómodamente, como si estuviesen en casa. El código generado por LLMs plantea una pregunta difícil: **¿quién lo habita?**

Nadie lo escribió con familiaridad. Nadie tomó las decisiones de diseño con responsabilidad propia. El resultado tiende a ser código *funcionalmente correcto pero espiritualmente vacío*: estructuras que nadie comprende del todo, con abstracciones aplicadas mecánicamente. Es exactamente el síntoma que Gabriel describe en su ensayo «Abstraction Descant».

La habitabilidad no es un lujo estético. Es la condición que hace posible el mantenimiento a largo plazo. Sin ella, el código se vuelve frágil ante cualquier cambio que no sea trivial.

### El *large lump development* en esteroides

Gabriel, siguiendo a Alexander, distinguía entre dos filosofías opuestas de construcción:

- **Crecimiento progresivo** (*piecemeal growth*): reparar y ampliar orgánicamente, como una granja de Nueva Inglaterra que añade habitaciones según las necesidades de la familia.
- **Desarrollo de gran bloque** (*large lump development*): generar un artefacto completo de una sola vez, perfectamente diseñado en papel... e inhabitable en la práctica.

Los LLMs incentivan lo segundo. La tendencia al *vibe coding* —«dame una aplicación completa a partir de este requisito»— es la materialización exacta de lo que Gabriel consideraba el mayor error en el desarrollo de software. Se genera mucho código de golpe que nadie comprende suficientemente bien como para repararlo con confianza después.

### La compresión sin contexto humano

El concepto de *compresión* de Gabriel —código cuyo significado depende del contexto que lo rodea— aplica de forma extraña a los LLMs: han comprimido miles de millones de líneas de código en pesos numéricos. Cuando generan código, *descomprimen* patrones estadísticos, no intenciones humanas.

El resultado puede parecer comprimido superficialmente, pero carece de la coherencia profunda que da sentido a la compresión real. Es poesía generada por un estadístico: métricamente correcta, emocionalmente hueca.

---

## Lo que los LLMs tensionan o complican

### Los patrones aplicados mecánicamente

Gabriel dedicó un ensayo entero al **fracaso de los lenguajes de patrones**: se usan como recetas, no como recordatorios de algo más profundo. Los LLMs son la aplicación mecánica de patrones llevada al extremo absoluto. Han aprendido todos los patrones existentes y los aplican sin la comprensión del problema concreto que los haría apropiados.

Esto no significa que el output sea siempre malo. Pero sí reproduce los vicios que Gabriel identificaba: jerarquías de clases gratuitas, abstracciones donde no hacen falta, estructuras que parecen correctas pero no tienen vida.

### La «calidad sin nombre» y la creatividad espontánea

En su ensayo «The Bead Game, Rugs, and Beauty», Gabriel recoge la idea de Alexander de que los mejores artefactos —alfombras turcas, edificios, sistemas de software— emergen de un **proceso espontáneo** en el que el creador se deja guiar por la estructura en construcción, no por un plan predeterminado.

Los LLMs operan exactamente al revés: predicen el token más probable dado el contexto. Es la antítesis de la espontaneidad creativa que produce esa calidad sin nombre. Pueden producir código sorprendentemente competente, pero difícilmente pueden producir ese código que «respira» —que Gabriel llama el equivalente al *being* en una alfombra de Anatolia—.

---

## Lo que los LLMs confirman (para bien)

### La tesis central sigue en pie

La tesis de Gabriel era sencilla y contundente: *la tecnología es secundaria a las personas*. Los LLMs son la herramienta más poderosa que ha existido para generar código, y aun así los equipos que los usan sin criterio humano fuerte fracasan exactamente de las formas que él predijo:

- Código que nadie entiende → mantenimiento imposible.
- Sin sentido de propiedad → sin responsabilidad ni motivación.
- Abstracciones forzadas → fragilidad ante cambios.

La historia de Lucid Inc. en «Into the Ground» —perfecta ejecución técnica, fracaso humano— se replica hoy en equipos que generan sistemas enteros con LLMs sin que nadie los *habite* de verdad.

### El programador como crítico

Paradójicamente, el rol que Gabriel abraza para sí mismo —el de **crítico**— se vuelve más valioso que nunca. Si los LLMs pueden escribir código medianamente bueno de forma automática, lo que diferencia a un equipo excelente de uno mediocre ya no es la velocidad de escritura, sino la **capacidad de juzgar, rechazar, reformar y habitar** lo que se genera.

El programador excelente en la era de los LLMs se parece más al crítico de Gabriel que al artesano clásico.

---

## Una nueva tensión que Gabriel no pudo anticipar

Hay un dilema que el libro no contempla porque es genuinamente nuevo: **la externalización del conocimiento tácito**.

Gabriel insistía en que dominar un lenguaje o un sistema lleva años de práctica real, de leer código excelente, de habitar proyectos complejos. Ese conocimiento tácito acumulado es lo que hace posible la habitabilidad y el crecimiento progresivo.

Los LLMs permiten a programadores con poco conocimiento tácito producir resultados superficialmente competentes. El riesgo no es solo que el código sea peor —aunque frecuentemente lo es—, sino que se interrumpe el proceso de acumulación de ese conocimiento. **Se desmitifica la profesión en el momento en que más se necesita criterio para usar bien las herramientas.**

Es la trampa de Basic English que Gabriel menciona en «Language Size»: al simplificar demasiado la herramienta de expresión, las frases resultantes son más largas, más torpes y menos precisas que las del idioma completo. Solo que ahora la simplificación no es del lenguaje, sino del proceso de pensar el problema.

---

## Síntesis

| Idea de Gabriel | En la era LLM |
|---|---|
| Habitabilidad | Se degrada: nadie «habita» el código generado |
| Crecimiento progresivo | Se amenaza: incentivo al *large lump* |
| Compresión | Se distorsiona: compresión estadística sin intención |
| Patrones como recordatorios | Se viola: aplicación mecánica masiva |
| Calidad sin nombre | Difícilmente alcanzable por generación estadística |
| Personas sobre tecnología | Más vigente que nunca |
| El programador como crítico | El rol más valioso de la era LLM |

---

## Conclusión

El libro de Gabriel no responde a la pregunta de qué hacer con los LLMs —no podía hacerlo—. Pero sí ofrece el **vocabulario correcto** para hacerse las preguntas importantes:

¿Es este código habitable? ¿Puede crecer de forma progresiva? ¿Alguien lo habita de verdad?

Si la respuesta es no, la sofisticación de la herramienta que lo generó no cambia el diagnóstico. Y si algo nos enseña Gabriel es que los problemas que ignoramos hoy los pagamos —con intereses— en el ciclo de vida del software.

Quizás la lección más urgente de *Patterns of Software* para 2026 sea esta: en una época en que cualquiera puede *generar* código, la habilidad escasa y valiosa es saber *habitarlo*.

---

*Escrito a partir de una lectura de «Patterns of Software» de Richard P. Gabriel (Oxford University Press, 1996).*
