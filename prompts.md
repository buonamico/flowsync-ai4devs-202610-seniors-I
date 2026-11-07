# Prompts

Prompts lanzados para el ejercicio "Recorta un MVP hasta que puedas defenderlo" (Módulo 2), en el orden en que los lancé.

> **Nota de honestidad:** no soy desarrollador. Hice el ejercicio en modo guiado con un tutor de IA (Claude): los prompts 1 a 3 los redactó el tutor y yo los lancé tal cual en mi agente; los prompts 4 y 5 son míos. Las decisiones del recorte (qué queda dentro, qué queda fuera y por qué) y las tres líneas de la Parte B son mías.

---

## Prompt 1

**Modelo:** Sonnet 4.5
**Herramienta:** Claude Code 2.0.27 (terminal)

```
Antes de especificar nada nuevo, necesito entender qué existe ya en este proyecto. Lee el código del backend y del frontend y respóndeme en un máximo de 10 líneas:
1. Qué capabilities están construidas y funcionan de punta a punta: qué puede hacer hoy un usuario con la app.
2. Cómo es el modelo de datos actual: qué entidades existen y cómo se relacionan, explicado en lenguaje de producto, sin código ni nombres de tablas.
3. Qué cosas parecen preparadas pero no terminadas.
No modifiques ningún archivo y no propongas nada nuevo todavía.
```

**Qué salió:** buen resumen (solo hay cuentas: registro, login, perfil y logout; no hay tareas). Pero se inventó que `backend/database/schema.ts` "aparece modificado porque alguien empezó a tocar el modelo": en realidad lo regeneró mi `make setup`. Sonó seguro aunque estaba adivinando.

---

## Prompt 2

**Modelo:** Sonnet 4.5
**Herramienta:** Claude Code 2.0.27 (terminal)

```
Ahora quiero definir el alcance de un MVP a partir de este párrafo del cliente:

«Quiero que FlowSync sea una herramienta para que los equipos remotos sepan en qué está trabajando cada uno sin tener que hacer reuniones de sincronización. Algo tipo tareas compartidas pero más en tiempo real y menos rollo que Jira.»

Antes de proponer nada, hazme las 5 preguntas que más reducirían la incertidumbre sobre el problema, los usuarios y el alcance. Reglas:
- Una sola ronda: exactamente 5 preguntas, numeradas, y luego te detienes a esperar mis respuestas.
- Nada de modelo de datos, endpoints, tecnología ni arquitectura: solo producto.
- No propongas el alcance todavía.
```

**Qué salió:** funcionó a la primera. Cinco preguntas de producto (qué duele, para quién, qué es "tiempo real", qué reunión desaparece, de dónde sale el estado), todas cubiertas por la ficha de hechos.

---

## Prompt 3

**Modelo:** Sonnet 4.5
**Herramienta:** Claude Code 2.0.27 (terminal)

```
Respuestas. Esta es la ficha de hechos del producto, ya decidida. Úsala como respuesta a tus 5 preguntas:

- Qué duele hoy: la daily de sincronización y el "¿en qué estás?" constante por Slack/chat. Nadie ve el estado del equipo sin interrumpir a alguien.
- Quién cobra el valor: los pares, no un lead. No hay reporte hacia arriba y a un manager le daría igual. Duele a los dos devs que descubren tarde que iban a lo mismo, y al que interrumpe a otro para preguntar.
- Episodio concreto: dos personas del equipo tocaron el mismo módulo la misma semana porque una empezó sin que la otra lo supiera. Dos días perdidos.
- Qué reunión desaparece (respuesta honesta, no la vendas de más): la daily NO desaparece entera. Desaparece la ronda de "¿en qué estás?", que hoy se come la mitad de los 15 minutos. La parte de bloqueos sigue, y este MVP no la resuelve.
- Usuarios / equipo: equipos remotos pequeños, 3–10 personas. Roles planos: en el MVP todos ven y editan lo mismo, sin jerarquía de permisos.
- Primer usuario concreto: equipo de 6 personas de producto SaaS, en 3 husos horarios, que hoy usa un gestor de tareas pesado y una daily de 15 minutos por videollamada. Es un CASO DE ESTUDIO, no un cliente real.
- Fronteras: un espacio único compartido, sin entidad "equipo". Varios equipos separados, o gente en más de uno, queda FUERA del MVP: se anota como supuesto en el PRD, no se construye.
- "Tiempo real" = ver los cambios de estado de las tareas sin refrescar ni preguntar. NO es chat, NO es videollamada, NO es colaboración simultánea sobre el mismo documento.
- Es frescura, no presencia: el estado es de la TAREA, no de la persona. Nada de "quién está conectado ahora" ni indicadores de actividad; eso es vigilancia y lo rechazamos a propósito.
- Forma de la señal: resumen que espera, no aviso que interrumpe. El caso es "llego por la mañana o vuelvo de una reunión y veo qué se ha movido". Sin notificaciones push.
- Qué decisión cambia: no empezar algo que otra persona ya está tocando, y elegir lo siguiente sabiendo qué está libre. Si la única respuesta fuera "sentirse informado", el tiempo real no valdría lo que cuesta.
- De dónde sale el estado: lo teclea la persona que hace la tarea, en segundos. Derivarlo de señales externas (Git/PRs, CI, calendario) está FUERA del MVP: es otro producto, con integraciones y OAuth de terceros.
- Por qué se sostiene: no porque sea más agradable, sino porque son dos clics sobre una lista ya abierta, sin campos obligatorios, sin decidir sprint ni estimación. Y quien lo escribe cobra en el momento: esa misma lista es su cola de trabajo, la mira para decidir qué coge, y de paso deja de recibir interrupciones preguntándole cómo va. Si el beneficio fuera solo para los demás, no lo escribiría.
- Si la información se queda vieja: el producto pierde el sentido, y lo asumo. Es el riesgo #1 a validar, no un detalle. La mitigación es que actualizar cueste dos clics, no obligar a nadie.
- Es donde se hace el trabajo, no donde se cuenta: sustituye al gestor de tareas, no convive con él. FlowSync crea las tareas, no lee las de otro sitio. Convivir exigiría doble actualización, que es como muere esta categoría.
- Renuncia explícita a sprints, estimaciones, épicas, backlog priorizado e informes. Un equipo que necesite eso no es nuestro usuario.
- "Menos rollo que Jira" = crear una tarea y cambiarle el estado en segundos, sin flujos de configuración ni campos obligatorios. Lo mínimo para saber quién está en qué.
- Qué necesita una tarea en el MVP: título, responsable, estado y fecha de vencimiento. La fecha, para ver de un vistazo qué se ha pasado de plazo.
- Cómo se consume la lista: filtrando por estado, para centrarse en lo pendiente.
- Éxito para el usuario: dejar de hacer la ronda de "¿en qué estás?" de la daily porque el estado del equipo se ve de un vistazo.
- Criterio a una semana de uso real: que el equipo cancele esa ronda y nadie pida que vuelva. Si la siguen haciendo igual, no funcionó.
- Cuánto construir: una vertical fina y usable de punta a punta, no el andamiaje amplio de un producto. Prefiero una capability terminada a tres a medias.

Ya decidido fuera del MVP: notificaciones push, integración con Slack, roles/permisos avanzados, analítica/reporting, comentarios en tareas.

Con esto, propón el alcance del MVP en cinco bloques, en este orden: Problema · Usuarios · Propuesta de valor · Alcance · NO-alcance. Reglas:
- Sé agresivo recortando: una vertical fina, lo mínimo para validar la hipótesis.
- En Alcance, una lista numerada de capacidades, una por línea, para poder contarlas.
- En NO-alcance, cada exclusión con su porqué: qué hipótesis del producto no ayuda a validar. "No da tiempo" no vale como razón.
- Respeta lo que ya existe en el código (las cuentas); no lo vuelvas a especificar.
- Es un documento de producto: sin modelo de datos, tablas, endpoints, arquitectura, casos de uso ni requisitos numerados. Que quepa en una pantalla.
- Si algo no lo cubre la ficha, decide tú y márcalo como SUPUESTO. Al final, lista todos los supuestos que tomaste.
- Responde aquí en el chat, no escribas ningún archivo.
```

**Qué salió:** propuso 6 cosas dentro del alcance y 21 exclusiones, cada una con su porqué. Pero se le colaron dos cosas: un supuesto de tecnología ("polling o WebSockets"), que quité, y una incoherencia. La propuesta de valor promete "ver qué está libre y elegir qué coger", mientras un supuesto exige un responsable en cada tarea y el NO-alcance prohíbe cambiarlo, así que nada podía estar libre. Decidí resolverla agregando "tomar una tarea libre" al alcance, pero no llegué a aplicarlo en el tramo 3 antes del reloj.

---

## Prompt 4

**Modelo:** Sonnet 4.5
**Herramienta:** Claude Code 2.0.27 (terminal)

```
Quita de los supuestos detalles de implementacion como WEbsocket o polling
```

**Qué salió:** sacó el supuesto técnico. Este prompt es mío.

---

## Prompt 5

**Modelo:** Sonnet 4.5
**Herramienta:** Claude Code 2.0.27 (terminal)

```
La fecha de Vencimiento sale del alcance
```

**Qué salió:** rehízo los cinco bloques sin fecha de vencimiento (y por arrastre sin "tareas pasadas de plazo" ni vista de calendario). Este prompt también es mío, y la decisión de sacar la fecha va contra la ficha, que la pedía.

---

## Prompt preparado y no lanzado

El tutor me preparó un prompt para que Claude Code reescribiera el archivo de alcance entero con mis decisiones. No lo lancé: armé el archivo copiando las respuestas y editando a mano.
