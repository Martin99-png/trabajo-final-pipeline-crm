# Decisiones — cómo llegué acá

## Iteración 1 — 27/08/2026 · el disparo automático

**Qué probé.** La idea original era que el pipeline corriera solo, programado a las 7am todos los
días, sin que yo tuviera que acordarme de nada.

**Qué falló.** Las tareas programadas en la nube no pueden mantener una conexión persistente a una
carpeta de mi computadora — es una limitación actual del producto, no algo que se pueda resolver
desde el prompt. Un disparo automático a las 7am simplemente no iba a poder leer `CRM_SurFood.xlsx`
ni escribir en él.

**Qué cambié.** El pipeline pasa a ejecución **manual**: yo lo disparo cuando conecto la carpeta
"Surfood", idealmente como rutina apenas prendo la computadora. No es lo que quería al principio,
pero es lo que realmente funciona con la herramienta que tengo.

## Iteración 2 — 29/07/2026 · el error de duplicación

**Qué probé.** Dejar que el agente enviara el 3° y último email a los contactos elegibles basándose
en el conteo de `EMAILS` guardado en el Excel, sin volver a chequearlo contra Gmail antes de cada
corrida.

**Qué falló.** El 29/07 detecté esto en el reporte, textual:

```
⚠️ ALERTA DE SISTEMA — Error de duplicación
Al revisar Gmail se detectó que 5 contactos recibieron un 4° email el 28/07, un día después de
haber recibido ya su email de cierre el 27/07. Esto ocurrió porque el CRM no fue actualizado con
los conteos reales tras el run del 27/07.
```

Cinco contactos reales (ICASA, Carozzi, CIACAM, Goya, KABSA) recibieron un email de más porque el
Excel tenía un conteo desactualizado entre una corrida y la siguiente.

**Qué cambié.** Agregué dos restricciones al contrato: (1) antes de crear cualquier borrador nuevo,
el agente revisa `list_drafts` para confirmar que no exista ya uno pendiente para ese contacto, y
(2) el conteo de `EMAILS` en el Excel se actualiza inmediatamente después de cada corrida, nunca se
asume que sigue vigente de una corrida a la otra.

## Iteración 3 — 03/08/2026 · por qué nunca envía solo

**Qué probé.** Al principio pensé en que el agente mandara los emails de seguimiento directo, para
no tener que revisarlos yo uno por uno — con ~100 candidatos elegibles en una sola corrida
(ver `corridas/2026-08-03_pipeline-crm.md`), revisar cada borrador a mano parecía mucho trabajo.

**Qué falló, y por qué en realidad está bien que haya fallado.** La herramienta de Gmail disponible
en este entorno solo permite crear borradores (`create_draft`), no enviar directamente. Y aunque
pudiera, mi propia regla número uno para todos mis agentes es "nunca enviar emails sin aprobación
explícita mía". Las dos razones aparecen documentadas textualmente en el reporte del 3/08.

**Qué cambié — y qué decidí no cambiar.** No intenté buscar la forma de que enviara directo. Dejé
el límite tal como está: el agente redacta, nunca envía. Es la restricción que hace que el resto
del sistema (edición directa del CRM, tope de 25 borradores por corrida) sea seguro de tener
activado sin que yo tenga que aprobar cada fila.

## Iteración 4 — 27/08/2026 · la excepción de edición directa del CRM

**Qué probé.** Al principio, siguiendo la regla general de todos mis agentes ("nunca modificar el
CRM sin aprobación previa"), el pipeline me mostraba cada cambio de `ESTADO`/`NOTAS` antes de
guardarlo.

**Qué falló.** Con ~450 contactos y corridas diarias, aprobar cambio por cambio no era sostenible —
una corrida normal actualiza docenas de filas (ver el reporte del 24/08, con más de 15 estados
tocados solo ese día). Terminaba aprobando todo sin mirar en detalle, que es peor que no revisar
nada: una aprobación automática no es una aprobación.

**Qué cambié.** El 27/08/2026 autoricé una excepción explícita **solo para esta skill**: puede
modificar `CRM_SurFood.xlsx` directamente, sin aprobación fila por fila, a cambio de documentar
cada cambio relevante en el reporte del día. Cambié aprobación previa por trazabilidad posterior —
puedo revisar lo que hizo, aunque no lo autoricé línea por línea.

## Lo que quedó roto

El contador `EMAILS` puede ir momentáneamente atrasado: un borrador creado hoy no suma al contador
hasta que yo efectivamente lo envío, y el sistema recién lo confirma comparando contra la carpeta de
enviados en una corrida futura. Entre que lo envío y la próxima corrida, el Excel puede mostrar un
número más bajo del real. No lo resolví — preferí este desfase transitorio antes que inflar el
contador con envíos que todavía no pasaron de verdad.

## Registro de cambios

| Fecha | Qué cambió | Motivo |
|-------|-----------|--------|
| — | Idea original: disparo automático 7am | Punto de partida |
| 27/08/2026 | Pipeline pasa a ejecución manual | Iteración 1 — limitación de conexión persistente |
| 29/07/2026 | Chequeo de `list_drafts` antes de crear borrador + actualización inmediata de `EMAILS` | Iteración 2 — error de duplicación real (5 contactos) |
| 03/08/2026 | Confirmación de que el agente nunca envía, solo crea borradores | Iteración 3 — límite de herramienta + regla global |
| 27/08/2026 | Excepción de edición directa del CRM, documentada en cada reporte | Iteración 4 — aprobación fila por fila era inviable a escala |
