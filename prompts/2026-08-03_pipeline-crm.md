# Corrida — Pipeline CRM SurFood

**Fecha de corrida:** 03/08/2026
**Disparada por:** Martín Bellagamba, manualmente al conectar la carpeta "Surfood" de su computadora
**Modelo:** Claude Sonnet 5

## Entrada

- **Fuente 1 — `CRM_SurFood.xlsx`**, hoja `CRM`, leída y actualizada vía puente de escritorio.
- **Fuente 2 — Gmail** (`martin@crestongrains.com.ar`): búsqueda de hilos para contactos `RESPONDIÓ` y `SIN RESPUESTA` con 6+ días, más `list_drafts` para evitar duplicados.
- **Corrección vigente desde la corrida del 29/07:** el conteo de `EMAILS` se re-verifica contra Gmail antes de generar cualquier borrador nuevo, no se asume el valor guardado en el Excel.

## Salida

```
📋 Reporte Diario CRM — SurFood S.A.

Fecha: Lunes 3 de agosto de 2026

🚫 DESCARTADOS HOY
Ningún nuevo rechazo explícito en las últimas 24 horas.

📤 SEGUIMIENTOS ENVIADOS
Ninguno enviado hoy.

⚠️ Nota técnica: La herramienta de Gmail disponible en este entorno solo permite crear borradores (create_draft), no enviar emails directamente. Adicionalmente, el contexto global indica "NUNCA enviar emails sin aprobación explícita de Martín." Por estas dos razones, se produce el reporte con todos los candidatos elegibles para que Martín decida cuáles enviar.

⚠️ BLOQUEADOS — CANDIDATOS A SEGUIMIENTO (no enviados)
Grupo principal: EMAILS=2 · ÚLT EMAIL 27-28/07 · Hoy = 6-7 días · Sería su 3° y ÚLTIMO email.
Se listan ~100 contactos elegibles por región (Brasil, Paraguay, Uruguay, Chile/Colombia/Guatemala y otros LATAM, Europa, Turquía/Medio Oriente/Golfo, Asia/África/Norteamérica), cada uno con empresa, email y días transcurridos. Total elegibles: ~100 contactos. Todos recibirían su 3° y último email antes del cierre definitivo.

🔴 REQUIEREN ATENCIÓN DE MARTÍN

Urgente — hoy (lunes 3 de agosto):
1. Fugini / Giuliana Sayuri (compras@fugini.com.br) — hilo activo, nuevo email hoy 10:57am: confirmó enviar el laudo de análisis emitido por Creston y proveyó la dirección completa para la muestra (Fugini Alimentos Ltda, Monte Alto-SP, CNPJ 00.588.458/0001-03). Martín prometió el 31/07 enviar la muestra de 2kg hoy lunes. Acción: confirmar envío hoy, coordinar despacho DHL 2kg garbanzo 7.5mm + análisis Creston.
2. Pinduca / Jean Carlos Panizzon (jean@pinduca.com.br) — preguntó el 27/07 si el producto ya está nacionalizado, sin respuesta. Acción: responder que es importación directa, explicar proceso SENASA/FCA Metán.

Esta semana:
3. Roland Beans / Berend Erling — Martín respondió todas las preguntas el 29/07 (spec, allergens, Non-GMO, optosorter). Pidió también detalles de certificación orgánica, verificar si se cubrió. Sin respuesta desde el 29/07 (5 días).
4. Argensun / Ahmed — contrato CG4208 activo, garbanzo 9mm, 2 FCL × 24 TM → Alejandría. Pidió la Proforma Invoice el 25/06, sigue pendiente.
5. Arbel / Hakan Erkal — "We are waiting for your offer", pendiente desde el reporte del 29/07.
6. Correa Duarte / Paulo Henrique — interesado en ervilha partida + lentilha, dejó teléfono, pendiente desde el 29/07.
7. Serafood / Tezer Yavuz — Martín preguntó el 23/07 qué productos necesitan, sin respuesta desde entonces (11 días).
8. Del Colle / Piergiorgio — pendiente análisis de pesticidas para confirmar orden 4 containers Black Eye.
9. Valencia Brokers / Paco — pidió cotización 2 containers Alubia 185/100, cosecha 2026, embarque agosto, CFR Valencia/Barcelona, pago CAD. Figura como URGENTE desde el 27/07, verificar si ya fue respondido.

Pendientes recurrentes: Asian Commodity/Rokibul, SOPL Singapore/Sunil, Casibeans, Ge.Ca Legumi/Gennaro, Hanaska/Isabel (URGENTE, sin cotizar desde abril), Bravarco/Florencio, Josapar/Felipe Carvalho, TAT Bakliyat/Anil, Indega/Magdalena, Alimentos de los Andes/Tomas.

🟡 SIN RESPUESTA — SEGUIMIENTO PENDIENTE (próximos al umbral)
Contactos con EMAILS=1 y más de 14 días sin respuesta: se mantienen en el radar pero no se envían más emails dado el límite de tiempo habitual (lista de ejemplo: Aldebaran, Nutriboost NL, Boon Food Concepts, Jamael Food Group, Bold Bean Co, Ebro Ingredients, Kruse Hess, ALIKA, FAM Impex, Molino Guido, etc.).
FVO Alimentos/Bruno Tiago — auto-reply indicó ausencia hasta el 06/08, retomar después de esa fecha.
Granol — EMAILS=5, SIN RESPUESTA, ya superó el límite (3+ emails), no enviar más.

📊 ESTADO DEL PIPELINE
Operaciones activas: Argensun gzo 9mm 2 FCL → Egipto (en curso, PI urgente) + negro 25MT Venezuela (a confirmar).
Leads calientes 🔥: ~20 contactos gestionados por Martín.
RESPONDIÓ: ~25 contactos con respuesta activa o derivación.
SIN RESPUESTA: ~200+ contactos (mayoría EMAILS=2-3).
DESCARTADOS: 10 confirmados. REBOTADOS: 15+ direcciones inválidas.

👉 PRÓXIMO PASO
Confirmar a Giuliana (Fugini) el envío de la muestra hoy — prometiste el lunes, hoy es lunes. Responder su email de esta mañana confirmando el despacho DHL de 2kg de garbanzo 7.5mm + laudo Creston a la dirección indicada. Después: responder a Pinduca y enviar la PI de Argensun que lleva semanas pendiente.

Reporte generado automáticamente — SurFood CRM | 03/08/2026
```
