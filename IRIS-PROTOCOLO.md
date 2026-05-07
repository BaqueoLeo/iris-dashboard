# IRIS · Protocolo de Operacion

_Este documento define como yo (Iris) uso el Control Panel para gestionar la vida de Leo._

## Archivos de Datos

| Archivo | Proposito | Frecuencia de uso |
|---------|-----------|-------------------|
| `iris-tasks.json` | Tareas detectadas, asignadas, o propuestas para Leo | Cada heartbeat |
| `iris-research.json` | Investigaciones activas, hallazgos, propuestas de solucion | Segun necesidad |
| `iris-memory.json` | Notas corto plazo (contexto que necesito entre sesiones) | Cada heartbeat |
| `iris-systems.json` | Estado de sistemas conectados (emails, calendario, trading) | Cada heartbeat |
| `index.html` | UI visual del dashboard (Leo puede ver, yo opero) | Referencia visual |

## Workflow de Heartbeat (cada 45 min)

1. **Leer iris-tasks.json** — Verificar vencimientos, actualizar estados
2. **Leer iris-memory.json** — Recordar contexto relevante para este momento
3. **Leer iris-systems.json** — Verificar que sistemas esten online
4. **Evaluar necesidad de mensaje** — Si hay algo urgente, enviar mensaje autonomo
5. **Actualizar timestamps** — Marcar ultima sincronizacion

## Reglas de Mensajeria Autonoma

**SI enviar mensaje:**
- Tarea vencida o vence en < 4h sin completar
- Evento de calendario en < 2h sin preparacion
- Sistema offline (email no responde, trading API caida)
- Insight genuino que descubri durante investigacion
- Gold-Swing detecta setup fuerte (score >= +2 o <= -2)

**NO enviar mensaje:**
- 23:00 - 08:00 (a menos que sea emergencia)
- Conversacion activa en los ultimos 10 min
- Solo para decir "todo bien"
- Mas de 1 mensaje por ventana de 45 min

## Formato de tareas

```json
{
  "title": "string",
  "created": "ISO datetime",
  "deadline": "YYYY-MM-DD o null",
  "priority": "high | med | low",
  "source": "auto | manual | heartbeat",
  "status": "pend | prog | block | done",
  "done": false,
  "assigned": "Leo o Iris",
  "context": "por que existe esta tarea"
}
```

## Formato de investigacion

```json
{
  "title": "tema",
  "body": "hallazgos y contexto",
  "proposal": "solucion propuesta o recomendacion",
  "status": "investigando | solucion | esperando | cerrado",
  "tag": "investigacion",
  "created": "ISO",
  "updated": "ISO"
}
```

## Formato de memoria

```json
{
  "tag": "urgente | recordar | contexto | idea",
  "body": "nota corta",
  "created": "ISO"
}
```

---

_Creado: 2026-05-07_
_Version: 1.0_
