# Evidencias — prompt v1 (rapida)
Modelo: `inclusionai/ling-3.0-flash-fin:free` · Evaluador: `gemini-3.6-flash` · Embeddings: `local` · top-k: 4 · umbral: 0.49 · Fecha: 20260925_2358

| ID | Herramientas | Correctness | Faithfulness | Latencia (s) |
|---|---|---|---|---|
| P01 | buscar_informacion_municipal | nan | nan | 2.91 |
| P05 | consultar_estado_solicitud | nan | nan | 2.2 |
| P06 | calcular_plazo_habil | nan | nan | 3.38 |

## P01 — ¿Qué documentos necesito para renovar el permiso de circulación de mi auto particular?

**Herramienta:** `buscar_informacion_municipal` · `{"consulta": "renovar permiso de circulación auto particular documentos requisitos"}`

<details><summary>Contexto recuperado</summary>

```
[1] Fuente: curacavi_licencias_conducir.pdf, pág. 3 | similitud 0.727
las 08:30 hasta las 13:30 hrs.
Se debe reservar la hora por lo menos con 20 
días de anticipación.
Enlace
Renovación  y/o Control de licencia de conducir clase A-1, A-2 (Ley N° 18.290). Este trámite es para • Licencias 
antiguas A1 para todo tipos de transportes de personas y A2 para todo tipos de transportes de Carga, 
concedidas por la antigua Ordenanza General de Transito, ley 18290.
• Licencia de conducir anterior.
• Copia de la cédula de identidad vigente (fotocopia por ambos lados).
• Certificado de antecedentes para fines de obtención de Licencia de Conducir, este debe tener una 
duración de 30 días y se cancela el mismo dia que se realizan los exámenes en la Dirección de

---

[2] Fuente: curacavi_licencias_conducir.pdf, pág. 4 | similitud 0.688
• Copia de la cédula de identidad vigente (fotocopia por ambos lados).
• Licencia de conducir anterior.
• En caso de no registrarse licencia de origen en hoja de vida del conductor, deberá solicitarse copia 
completa de carpeta de antecedentes, trámite que puede demorar según Municipio. 
• Documento que acredite su domicilio actual. 
• En caso de estar extraviada su licencia, presentar su bloqueo definitivo en el Registro Civil.
• Certificado de antecedentes para fines de obtención de Licencia de Conducir, este debe tener una 
duración de 30 días y se cancela el mismo dia que se realizan los exámenes en la Dirección de 
Tránsito. Pasado este plazo deberá cancelarlo nuevamente. 
• Examen médico: sensométrico, psicométrico y entrevista médica. NO 0.53 UTM
Municipalidad de Curacaví. Avenida

---

[3] Fuente: curacavi_licencias_conducir.pdf, pág. 5 | similitud 0.669
Cambio de domicilio licencia de conducir. Dirigidos a Titulares de licencia de conducir que hayan cambiado de 
domicilio.
• Copia de la cédula de identidad vigente (fotocopia por ambos lados).
• Acreditar residencia en la comuna: Certificado de Junta de Vecinos o declaración jurada ante 
Notario.
• Se debe solicitar la carpeta completa de conductor a la Municipalidad de origen, lo que puede 
implicar tiempo de espera adicional para el contribuyente. Debe venir  de manera presencial a solicitar el cambio de domicilio 
de su licencia de conducir. NO 0.18 UTM
Municipalidad de Curacaví. Avenida 
Ambrosio O'Higgins 1305.
Todos los lunes, martes, miércoles, jueves, 
viernes, y los segundos sábados de cada mes, 
desde las 08:30 hasta las 13:30 hrs.
Se debe reservar la hora por lo menos con 20

---

[4] Fuente: curacavi_permiso_circulacion.pdf, pág. 1 | similitud 0.668
RENOVACIÓN PERMISO DE CIRCULACIÓN VEHÍCULOS PARTICULARES. 
Descripción Es la autorización que se otorga, año tras año, a los vehículos 
motorizados para circular en la vía pública. 
 
A quien está dirigido A todos  los vehículos motorizados. 
 
Requisitos Tener todos los documentos exigidos al día. 
 
Documentación - Permiso de circulación año anterior. 
- Revisión Técnica al día u homologación. 
- Certificado de gases al día u homologación. 
- Seguro obligatorio de accidentes personales. 
- Si el vehículo es del año, debe presentar Factura original. 
 
En caso de no contar con originales, deben ser fotocopias 
legalizadas ante notario. 
 
Costo El trámite no tiene costo determinado; se cancela en función 
del valor correspondiente al pago del Permiso de Circulación
```
</details>

**Respuesta:**

Para renovar el **permiso de circulación** de tu auto particular en la Municipalidad de Curacaví, necesitas los siguientes documentos:

### 📋 Documentación requerida:

1. **Permiso de circulación del año anterior** (el vigente o del último año renovado).
2. **Revisión Técnica al día** o homologación vigente.
3. **Certificado de gases al día** o homologación vigente.
4. **Seguro obligatorio de accidentes personales** vigente.
5. **Si el vehículo es del año**, debes presentar la **factura original**.

> ⚠️ **Importante:** En caso de no contar con los documentos originales, estos deben ser **fotocopias legalizadas ante notario**.

### 💰 Costo:
El trámite no tiene un costo fijo; el monto se cancela según el valor correspondiente al pago del Permiso de Circulación del vehículo.

---

**Requisito general:** Todos los documentos exigidos deben estar **al día**.

¿Tienes alguna otra duda sobre este trámite o necesitas saber sobre horarios o cómo agendar una hora?


## P05 — ¿En qué estado está mi solicitud con folio SOL-2026-0012?

**Herramienta:** `consultar_estado_solicitud` · `{"folio": "SOL-2026-0012"}`

<details><summary>Contexto recuperado</summary>

```
Folio SOL-2026-0012 | Trámite: Renovación permiso de circulación | Ingreso: 2026-07-18 | Estado: Rechazada | Unidad: Dirección de Tránsito y Transporte Público | Observación: No cumple requisitos; se notificó al solicitante. | Fuente: solicitudes.csv
```
</details>

**Respuesta:**

Tu solicitud **SOL-2026-0012** tiene el siguiente estado:

- **Trámite:** Renovación de permiso de circulación
- **Fecha de ingreso:** 18 de julio de 2026
- **Estado:** ❌ **Rechazada**
- **Unidad responsable:** Dirección de Tránsito y Transporte Público
- **Observación:** No cumple con los requisitos; se notificó al solicitante.

Lamentablemente, tu solicitud fue rechazada por no cumplir con los requisitos necesarios. Según consta, se te notificó al respecto. Si deseas repostular o conocer los requisitos que faltaron, puedo ayudarte a buscar esa información. ¿Hay algo más en lo que te pueda asistir?


## P06 — Ingresé un reclamo el 15 de septiembre de 2026. Si la municipalidad tiene 10 días hábiles para responder, ¿cuándo vence el plazo?

**Herramienta:** `calcular_plazo_habil` · `{"fecha_inicio": "2026-09-15", "dias_habiles": 10}`

<details><summary>Contexto recuperado</summary>

```
Vencimiento: 2026-09-30 (10 días hábiles desde 2026-09-15). Feriados omitidos: 2026-09-18 (Independence Day).
```
</details>

**Respuesta:**

El plazo de **10 días hábiles** para responder su reclamo vence el **miércoles 30 de septiembre de 2026**.

**Detalle del cálculo:**
- **Fecha de inicio:** 15 de septiembre de 2026 (martes)
- **Feriado nacional omitido:** 18 de septiembre (Independencia de Chile)
- **Días hábiles contados:** 16, 17, 21, 22, 23, 24, 25, 28, 29 y 30 de septiembre

> **Nota:** Si necesita consultar el estado de su solicitud, puede hacerlo proporcionando su número de folio (formato SOL-AAAA-NNNN).
