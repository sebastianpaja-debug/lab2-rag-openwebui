# Informe de Laboratorio 2: Pipeline RAG con Open WebUI y Ollama

- **Estudiante / Equipo:** Sebastian
- **Stack Utilizado:** qwen2.5:1.5b-instruct + nomic-embed-text + Open WebUI + Docker Compose
- **Fecha:** 20/11/2026

---

## 1. Comparativa: Antes del RAG (Línea Base)
Sin contexto, el modelo no entregó ni el monto ni el código. Se negó a responder ("Lo siento, pero no puedo proporcionar información...") argumentando que son áreas protegidas por derechos de privacidad o regulaciones laborales, y luego rellenó la respuesta con generalidades sin relación con la pregunta: inversión de gobiernos latinoamericanos en telecomunicaciones, telemedicina durante la pandemia y crecimiento de empresas de teletrabajo. Esto evidencia que el SLM no conoce la política privada de Innovatech y que, en lugar de reconocerlo con claridad, produce contenido genérico e irrelevante.

## 2. Comparativa: Después del RAG (Con Knowledge Base)
Con la colección Politicas-Innovatech-2027 adjunta (embeddings con nomic-embed-text vía Ollama), el modelo respondió con precisión: el auxilio mensual de conectividad neto es de $245.000 COP a partir de enero de 2027, independiente del salario básico, y el código interno para solicitarlo es #INV-CON-2027. Open WebUI citó como fuente politica_innovatech_2027.txt. La respuesta pasó de un rechazo con texto genérico a un dato exacto, verificable y con fuente. Observación: en un primer intento sin adjuntar la colección, el modelo respondió igual que en la línea base, lo que confirma que el RAG solo se activa al adjuntar la colección con #.

## 3. Evaluación de Control de Alucinaciones (Fuera de Dominio)
Ante la pregunta sobre el horario de la cafetería los sábados, el modelo indicó que no encontró ninguna referencia específica en el contexto proporcionado y no inventó un horario, por lo que respetó en lo esencial los límites del contexto recuperado. Sin embargo, la respuesta no fue totalmente limpia: añadió generalidades ("normalmente los horarios para una cafetería pueden variar...") y recomendó consultar directamente con la empresa o su sitio web. Además, Open WebUI mostró politica_innovatech_2027.txt como fuente aunque el documento no contiene información sobre la cafetería, ya que el sistema recupera siempre los fragmentos más cercanos aunque no sean relevantes. En conclusión, el RAG evitó la alucinación de un dato falso, pero un SLM de 1.5B no rechaza la pregunta de forma tajante; esto podría reforzarse con un prompt de sistema que ordene responder "no tengo esa información" cuando el contexto no contenga la respuesta.

---

## 4. Evidencias Adjuntas (Capturas de Pantalla)
- [x] Captura 1: Respuesta del modelo sin RAG (desconocimiento y texto genérico) — `capturas/captura1_sin_rag.png`.
- [x] Captura 2: Respuesta fundamentada usando `#Politicas-Innovatech-2027`, con la colección adjunta y la cita a `politica_innovatech_2027.txt` — `capturas/captura2_con_rag.png`.
- [x] Captura 3: Prueba anti-alucinación (cafetería) con la colección adjunta — `capturas/captura3_cafeteria.png`.
