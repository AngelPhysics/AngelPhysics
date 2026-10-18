# Atajo de iPhone: aviso por WhatsApp al llegar al Airbnb (Baltimore)

Objetivo: cada vez que vuelvas del **STScI** al **Airbnb**, el iPhone manda
automáticamente por WhatsApp a **Emma** y a **Mamá** el mensaje
"Ya estoy en la casa 🏠".

| Lugar | Dirección |
|-------|-----------|
| STScI (origen) | 3700 San Martin Drive, Baltimore, MD 21218, United States |
| Airbnb (destino) | 3966 Elm Avenue, Baltimore, MD 21211, United States |

> Apple no permite instalar automatizaciones desde un archivo, así que hay que
> crearlas a mano en la app **Atajos** (Shortcuts). Son unos 10 minutos.

---

## Cómo funciona (la idea)

El disparador "Llegar a" de iOS no sabe **de dónde vienes**. Para que el mensaje
solo salga cuando vuelves del STScI (y no, por ejemplo, cuando vuelves del
supermercado), usamos un pequeño "interruptor" guardado en un archivo de texto:

1. **Automatización A** — al **salir** del STScI → escribe `yes` en el archivo
   `baltimore_flag.txt`.
2. **Automatización B** — al **llegar** al Airbnb → lee el archivo. Si dice
   `yes`, manda los dos WhatsApp y vuelve a poner el archivo a `no`.

Así se envía **una sola vez por cada vuelta del STScI**.

---

## Requisitos previos

- iOS 17 o posterior (para que las automatizaciones de ubicación se ejecuten
  sin pedir confirmación).
- **Ajustes → Privacidad y seguridad → Localización → Atajos → "Siempre"**
  y **"Ubicación exacta" activada**.
- Emma y Mamá guardadas en **Contactos** con el número que usan en WhatsApp.
- Haber abierto WhatsApp al menos una vez con cada una (que exista el chat).
- Opcional pero recomendable: crear primero el archivo. Si no existe, la
  automatización B simplemente no hará nada hasta la primera salida del STScI.

---

## Automatización A — "Salgo del STScI"

1. Abre **Atajos** → pestaña **Automatización** → **+** (Nueva automatización).
2. Elige **Salir** (Leave).
3. **Ubicación** → busca `3700 San Martin Drive, Baltimore, MD 21218` → ajusta
   el radio (unos 150–200 m va bien) → **OK**.
4. Horario: **A cualquier hora**.
5. Marca **Ejecutar inmediatamente** (Run Immediately) y desactiva
   "Notificar al ejecutar" si no quieres el aviso.
6. **Siguiente** → **Nuevo atajo en blanco** y añade estas acciones:

   | # | Acción | Configuración |
   |---|--------|---------------|
   | 1 | **Texto** | `yes` |
   | 2 | **Guardar archivo** | Entrada: *Texto* · "Preguntar dónde guardar": **desactivado** · Subruta: `baltimore_flag.txt` · "Sobrescribir si existe": **activado** |

7. **OK**.

---

## Automatización B — "Llego al Airbnb"

1. **Automatización** → **+** → **Llegar** (Arrive).
2. **Ubicación** → `3966 Elm Avenue, Baltimore, MD 21211` → radio ~100–150 m
   → **OK**.
3. Horario: **A cualquier hora**.
4. **Ejecutar inmediatamente** activado.
5. **Siguiente** → **Nuevo atajo en blanco** y añade:

   | # | Acción | Configuración |
   |---|--------|---------------|
   | 1 | **Obtener archivo** (de la carpeta) | Carpeta: *Atajos* · Ruta: `baltimore_flag.txt` · "Error si no se encuentra": **desactivado** |
   | 2 | **Obtener texto de la entrada** | Entrada: *Archivo* |
   | 3 | **Si** | *Texto* **es** `yes` |
   | 4 | ↳ **Enviar mensaje** (de la app **WhatsApp**) | Mensaje: `Ya estoy en la casa 🏠` · Destinatario: **Emma** |
   | 5 | ↳ **Enviar mensaje** (de la app **WhatsApp**) | Mensaje: `Ya estoy en la casa 🏠` · Destinatario: **Mamá** |
   | 6 | ↳ **Texto** | `no` |
   | 7 | ↳ **Guardar archivo** | Entrada: *Texto* · "Preguntar dónde guardar": **desactivado** · Subruta: `baltimore_flag.txt` · "Sobrescribir si existe": **activado** |
   | 8 | **Fin del Si** | (se añade solo) |

   Para encontrar la acción de WhatsApp: en el buscador de acciones escribe
   **WhatsApp** → **Enviar mensaje**. En cada una, despliega la flecha y
   desactiva **"Mostrar al ejecutar"** para que no pida confirmación.

   Se usan **dos acciones separadas** (una por persona) a propósito: si pones
   las dos en la misma acción, WhatsApp puede intentar crear un grupo o fallar.

6. **OK**.

---

## Cómo probarlo antes del viaje

1. Abre la automatización B y toca el botón ▶︎ del atajo **después** de haber
   ejecutado manualmente la A (▶︎ también). Debería mandar los dos mensajes.
2. Ejecuta la B otra vez: **no** debería mandar nada (el archivo ya dice `no`).
3. Para no molestar a Emma y a Mamá en la prueba, cámbialas temporalmente por
   tu propio número o por "Notas a mí mismo".

---

## Limitaciones a tener en cuenta

- **iPhone bloqueado:** WhatsApp a veces necesita que el iPhone esté
  desbloqueado para enviar desde una automatización. Si al llegar no se envía,
  te aparecerá una notificación; tócala y se completará. Esto depende de
  WhatsApp/iOS y no se puede forzar desde Atajos.
- **Precisión del GPS:** la geocerca puede dispararse con 1–2 minutos de
  retraso o unos metros antes de la puerta. El STScI y el Airbnb están a
  ~1,5 km, así que las dos zonas no se solapan.
- **Si un día pasas por el STScI sin volver a casa** (p. ej. vas a cenar a otro
  sitio), el mensaje saldrá igualmente la próxima vez que llegues al Airbnb, que
  es justo lo que interesa: "ya estoy en casa".
- **Al acabar el viaje:** en **Automatización**, desliza cada una a la
  izquierda → **Eliminar**, o desactívalas.
