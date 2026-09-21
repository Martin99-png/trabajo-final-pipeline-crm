# Pipeline de CRM — SurFood S.A. / Creston Grains

## Qué construí

Un agente que mantiene al día mi pipeline comercial de exportación de legumbres y granos. Cruza el
CRM (`CRM_SurFood.xlsx`, ~450 contactos internacionales) contra mi Gmail real
(`martin@crestongrains.com.ar`), actualiza estados y notas, redacta (nunca envía) los seguimientos
que corresponden según reglas fijas, y me deja un reporte accionable cada vez que lo corro — más un
tablero que se republica con el estado del día. Lo uso yo, en mi trabajo real como vendedor y
trader de exportación en SurFood S.A., parte del Grupo Familia Creston.

## Cómo se lo pedí

El contrato está en `prompts/`: `system_prompt.md` (rol de agente comercial, el contexto completo
del CRM y las herramientas, la tarea de las 5 pasadas, once restricciones puntuales, el formato de
7 secciones del reporte, y dos ejemplos) y `user_prompt.md` (el pedido fijo de cada corrida, más la
variante para cuando pasaron varios días desde la última vez). Las iteraciones completas están en
`DECISIONES.md`.

## Qué funciona

- Corre sobre datos reales: tres corridas guardadas en `corridas/` (29/07, 03/08 y 24/08 de 2026),
  cada una con entrada, salida y fecha, sobre el CRM real de ~450 contactos y mi Gmail real.
- Distingue correctamente contactos que respondieron de los que siguen en silencio, cruzando contra
  Gmail en vez de confiar ciegamente en el Excel — esto es lo que detectó, por ejemplo, que 5
  contactos habían recibido un email de más el 28/07 (ver `DECISIONES.md`, iteración 2).
- Respeta el idioma del contacto, nunca ofrece FOB Rosario (usa los incoterms correctos para cada
  mercado), y nunca envía nada sin que yo lo revise — solo crea borradores.
- Reconoce cuándo un contacto es de alto valor (operación en curso, como Del Colle o Argensun) y
  me deja esas decisiones a mí en vez de automatizarlas — ver "revisión manual" en el reporte del
  24/08.
- El tablero se republica cada corrida con los números del día.

## Qué falta o qué falló

- El error de duplicación del 29/07 (5 contactos recibieron un 4° email) fue real y quedó
  documentado tal cual pasó, no disimulado — ver `DECISIONES.md`, iteración 2. Se corrigió, pero
  pasó.
- El contador `EMAILS` puede quedar momentáneamente atrasado entre que envío un borrador a mano y
  la corrida siguiente, que es cuando se confirma contra la carpeta de enviados. No lo resolví.
- Sigue siendo manual: no hay forma de que corra solo a la mañana sin que yo prenda la computadora
  con la carpeta conectada (ver iteración 1 de `DECISIONES.md`) — es una limitación de la
  herramienta que tengo hoy, no algo que se arregle con más prompt.
- No mido sistemáticamente cuántos de los borradores que reviso termino enviando tal cual contra
  cuántos edito antes de mandar — sería el primer indicador que agregaría si esto siguiera.

## Qué aprendí

Que la pieza que más cambió el resultado no fue la tarea, fue las restricciones: la regla de
"nunca enviar, solo crear borradores" es la que me permitió después darle al agente permiso de
editar el CRM directamente sin aprobar fila por fila — si pudiera enviar solo, esa segunda
autorización hubiera sido mucho más arriesgada. También aprendí que un pipeline que depende de una
carpeta local no puede programarse en la nube todavía, y que a veces la solución correcta no es
insistir con la automatización completa sino aceptar el paso manual que sí funciona.

## Análisis económico

Medido sobre una corrida representativa (referencia: la del 24/08/2026):

| Componente | Tokens (estimado) |
|---|---|
| Contrato (system + user prompt) | ~1.350 |
| Hoja CRM (~450 filas) + hilos de Gmail cruzados | ~20.000 |
| **Entrada total** | **~21.350** |
| Salida (reporte del día) | ~1.800 |

*Metodología de la estimación:* los archivos del contrato y de las tres corridas guardadas se
midieron por caracteres reales (1 token ≈ 2,5–5 caracteres en español), lo que da un rango de
~900 a ~2.000 tokens de salida por corrida — se usó 1.800 como referencia. El tamaño de la hoja
CRM y de los hilos de Gmail no se puede medir exportando el Excel completo a texto en este informe,
así que se estimó a partir de su tamaño real conocido (~450 filas con varias columnas de texto,
más entre 20 y 40 hilos de Gmail leídos por corrida): es una estimación declarada como tal, no una
medición exacta.

Precio del modelo (Claude Sonnet 5, el que corre esta skill) consultado el 21/09/2026 en la página
oficial de precios de Anthropic: USD 2,00 por millón de tokens de entrada, USD 10,00 de salida.

```
costo_corrida = (21.350 / 1.000.000) x 2,00 + (1.800 / 1.000.000) x 10,00
              = 0,0427 + 0,0180
              = USD 0,0607 por corrida
```

**Proyección — supuesto de volumen explícito:** lo corro manualmente cada día hábil (lunes a
viernes), 5 corridas por semana.

```
costo_semanal = 5 x 0,0607 ≈ USD 0,30
costo_anual   = 52 x costo_semanal ≈ USD 15,80
```

**Elección de modelo, justificada en plata.** Uso Sonnet 5 (no un modelo más chico como Haiku 4.5,
más barato: USD 1,00/5,00 por MTok) porque la tarea no es solo extraer datos de una planilla: tiene
que distinguir un "RESPONDIÓ 🔥" genuino de un auto-reply, mantener el idioma y el tono correctos en
portugués/inglés/español según cada contacto, y reconocer cuándo un contacto de alto valor necesita
mi criterio en vez de un borrador automático. No hice una prueba formal comparando los dos modelos
lado a lado — sería lo primero que haría si tuviera que defender el ahorro de pasar a un modelo más
chico, y lo dejo anotado como pendiente en vez de inventar un resultado que no medí.

Con un costo anual de menos de USD 16 corriendo todos los días hábiles del año, el análisis de
costo no es lo que decide qué modelo usar acá — lo decide la calidad del criterio, que es
justamente lo que un modelo más chico pondría en riesgo por una diferencia de centavos.

## Gobierno y riesgo

**Qué toca.** Lee y escribe `CRM_SurFood.xlsx` en la carpeta "Surfood" de mi computadora, vía el
puente de escritorio (lectura y escritura directa del Excel, autorizada el 27/08/2026 — ver
`DECISIONES.md`). Lee hilos de Gmail de la cuenta `martin@crestongrains.com.ar` y crea borradores
sin enviar — nunca envía, nunca elimina nada, no toca ningún otro sistema.

**Qué puede salir mal.**

1. El CRM queda con un conteo desactualizado entre corridas y el agente duplica un seguimiento.
   Ya pasó una vez (29/07/2026, 5 contactos afectados) y quedó documentado sin disimularlo.
   Mitigación implementada: chequeo de `list_drafts` antes de crear cualquier borrador nuevo, y
   actualización inmediata del conteo de `EMAILS` después de cada corrida.
2. Sin la carpeta "Surfood" conectada, el pipeline simplemente no corre — no inventa datos ni
   corre sobre información vieja sin avisar.
3. Un borrador queda en el idioma o el tono equivocado para el contacto. Mitigación: restricción
   explícita del contrato de usar el idioma del hilo, y que **nunca se envía automáticamente** —
   yo reviso cada borrador antes de mandarlo.

**Qué reviso antes de confiar.** Reviso cada borrador antes de enviarlo manualmente — ninguno sale
sin que yo lo lea. Reviso también la sección "revisión manual" del reporte (los candidatos al 3er y
último email de cuentas de alto valor) antes de decidir el enfoque, y los cambios de `ESTADO` que
el agente hizo directamente en el CRM, que quedan documentados en cada reporte aunque no los haya
aprobado uno por uno.

**Nivel de delegación: L2 — ejecuta con revisión.** El agente lee, cruza, actualiza el CRM y
redacta solo; ningún email sale de mi bandeja sin que yo lo revise y lo envíe personalmente.

**Firma:** Martín Bellagamba, vendedor y trader de exportación, SurFood S.A. / Creston Grains.
