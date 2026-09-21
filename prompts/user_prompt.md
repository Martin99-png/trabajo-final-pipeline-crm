# User prompt — una corrida del pipeline

Se usa tal cual cada vez que Martín conecta la carpeta "Surfood" y dispara la skill, manualmente,
normalmente al prender la computadora a la mañana.

```
Corré el pipeline de CRM de hoy.

FECHA: <<AAAA-MM-DD>>
CARPETA: Surfood (conectada)
CRM: CRM_SurFood.xlsx
GMAIL: martin@crestongrains.com.ar

Cruzá el CRM contra Gmail siguiendo el contrato del system prompt. Actualizá el CRM directamente
(excepción autorizada el 27/08/2026 — documentar cada cambio relevante en el reporte). Redactá
borradores para los candidatos elegibles, nunca los envíes. Generá el reporte del día completo,
con las siete secciones del formato fijo, y republicá el tablero si corresponde.
```

## Variante — primera corrida del día tras un fin de semana o feriado

Cuando pasaron más de 2 días desde la última corrida, se agrega esta línea al pedido:

```
Ojo: pasaron varios días desde la última corrida. Antes de generar borradores nuevos, revisá con
más cuidado si algún contacto SIN RESPUESTA en realidad ya respondió durante el fin de semana —
no asumas que el estado del Excel sigue vigente.
```
