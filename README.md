<div align="center">

# CLConnect

**Tu terminal de Claude Code, desde el celular.**

Dejás a Claude trabajando en la PC, guardás el teléfono, y seguís desde ahí.

[**Descargar**](../../releases/latest)

</div>

---

## Qué resuelve

Claude Code corre en tu computadora. Si te levantás de la silla, te quedaste afuera:
no sabés si terminó, si se trabó esperando una respuesta, ni qué hizo mientras tanto.

CLConnect es la otra mitad de esa terminal. Ves lo mismo que la pantalla de tu PC,
le escribís, y te avisa cuando termina.

No reemplaza nada: tu terminal sigue siendo tu terminal.

## Cómo se usa

**En la computadora**, abrís Claude Code como siempre:

```
claude
```

Nada cambia. Misma terminal, mismos colores, mismo todo. Por debajo, la sesión
queda disponible para el teléfono.

**En el celular**, abrís la app y ahí está:

```
EQUIPOS
┌────────────────────────────────────────┐
│ 🖥  DESKTOP-BOSOT      este equipo    › │
│    ● en linea · 3 sesiones             │
└────────────────────────────────────────┘
```

Entrás a un equipo, elegís una sesión, y leés el hilo tal como se ve en la
terminal — con los diffs coloreados, las herramientas que usó, las listas de
tareas:

```
sesion    6%   vuelve en 47 min
semana   50%   vuelve en 2 d
────────────────────────────────────────

╭──────────────────────────────────────╮
│ > arreglá el bug de auth             │
╰──────────────────────────────────────╯

⏺ Read(.../auth/middleware.ts)
  ⎿  142 líneas leídas

⏺ El check de expiry usa < en vez de <=.

⏺ Update(.../auth/middleware.ts)
  ⎿  1 adición, 1 eliminación
     47 - if (exp < now) return null;
     47 + if (exp <= now) return null;

✻ Rumiando... (12s · ↑ 1.2k tokens)
```

Arriba está cuánto de tu cuota llevás usado, para saber si conviene mandar el
prompt largo ahora o esperar al reset.

Abajo hay una caja de texto. Lo que escribas aparece **tipeado en la terminal de
tu PC** y se ejecuta ahí. Si te arrepentiste, hay un botón para interrumpir.

También podés **abrir una sesión nueva desde el teléfono** — se abre una terminal
en tu PC, en el proyecto que elijas, y entrás directo. Y una conversación de ayer
se retoma con un botón, en el mismo directorio en el que estabas.

Cuando el turno termina, te llega un aviso:

```
CLConnect
El agente terminó - 4 min
Arreglo del middleware de auth
```

Tocarlo te lleva directo a esa sesión.

## Qué pasa por detrás

Tres piezas, y ninguna toca lo que ya tenías:

**1. Un intermediario invisible.** Cuando tipeás `claude`, en realidad arranca un
programita que abre el Claude Code de siempre por debajo y le pasa tu teclado tal
cual. Está ahí para una sola cosa: poder escribirle desde afuera.

**2. Una app de fondo en la PC.** Claude Code deja un registro de todo lo que hace
en un archivo. La app lo lee mientras se escribe y lo publica. Vive en la bandeja
del sistema y no molesta.

**3. Un intermediario en la nube.** Guarda lo que la PC publica y se lo pasa al
teléfono al instante. También al revés, con los prompts que mandás.

```
      Tu terminal                              Tu celular
           │                                        │
     [ claude ] ──► registro ──► [ app PC ] ◄──► nube ◄──► [ app ]
           ▲                                                  │
           └──────────────── tu prompt ◄──────────────────────┘
```

**Tus credenciales de Claude nunca salen de la computadora.** La nube ve prompts y
respuestas, no tu sesión de Anthropic.

## Instalación

| | |
|---|---|
| **Windows** | `CLConnect-Setup-X.Y.Z.exe` — instala la app, el intermediario y el comando `claude` |
| **Android** | `clconnect-X.Y.Z.apk` — instalalo y entrá con la misma cuenta de Google |

Los dos se actualizan solos de ahí en más.

> **Windows te va a avisar "editor desconocido"** y Android te va a pedir permitir
> instalar de fuentes desconocidas. Es porque no están firmados con un certificado
> comercial — un gasto anual que no tiene sentido para esto.

Después de instalar en Windows, abrí una terminal **nueva** para que el comando
`claude` tome el cambio.

El login con Google está en modo de prueba: por ahora solo entran las cuentas
autorizadas explícitamente. Si querés probarlo, abrí un issue.

### Si no aparece ninguna sesión

La app de la PC revisa su instalación al arrancar y avisa arriba si algo no
cuadra: dónde buscó los transcripts, dónde está el `claude` real, y quién gana
cuando tipeás `claude`.

Si separás cuentas con `CLAUDE_CONFIG_DIR` —los transcripts te quedan en
`~/.claude-max`, `~/.claude-team` y demás— se leen todos, no hace falta elegir.

## Estado

Funciona de punta a punta.

| Anda | Falta |
|---|---|
| Ver las sesiones de cada PC | Cambiar de cuenta de Claude sin salir de la app |
| Leer el hilo en vivo, con diffs y listas | Aviso si escribís desde los dos lados a la vez |
| Escribirle e interrumpirlo desde el teléfono | Ícono propio |
| Abrir y retomar sesiones desde el celular | |
| Ver cuánto de la cuota llevás usado | |
| Aviso al terminar, con resumen | |
| Actualización automática en ambas apps | |

## Principios

1. **El intermediario nunca puede romper tu terminal.** Si la app de la PC no
   está, se vuelve transparente y reintenta en segundo plano.
2. **Las credenciales de Claude nunca salen de la computadora.**
3. **Sin rotación automática de cuentas.** El cambio es manual y explícito.
4. **Solo sesión de suscripción, nunca API key.** Cobro por uso accidental,
   imposible.
5. **Un fallo silencioso es peor que uno ruidoso.** Cada pieza que puede quedarse
   muda muestra su estado en pantalla.
