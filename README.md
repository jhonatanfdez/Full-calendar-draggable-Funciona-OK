# Proyecto Full Calendar con Eventos Draggables funcionadndo correctamente
![](https://pandao.github.io/editor.md/images/logos/editormd-logo-180x180.png)

Este proyecto demuestra cómo implementar un calendario interactivo utilizando Full Calendar con la opción de arrastrar y soltar (draggable) eventos sin crear duplicados.

## Introducción

Crear un calendario interactivo con eventos arrastrables puede ser un desafío, especialmente cuando se trata de evitar la creación de eventos duplicados. Este `readme.md` proporciona una guía paso a paso para configurar un calendario con eventos `draggable` utilizando Full Calendar.

## Requisitos

- FullCalendar JavaScript Library
- jQuery (opcional, dependiendo de tu versión de FullCalendar)

#### Código HTML base.
Ver archivo **ejemplo1.html**

```html
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.min.css"
      rel="stylesheet"
    />
    <link
      href="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.print.min.css"
      rel="stylesheet"
      media="print"
    />
    <script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
    <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"></script>
    <script src="https://fullcalendar.io/releases/fullcalendar/3.10.0/lib/moment.min.js"></script>
    <script src="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.min.js"></script>
    <style>
      #external-events {
        padding: 10px;
        width: 20%;
        background: #f7f7f7;
      }
      #calendar {
        flex-grow: 1;
      }

      .fc-event {
        margin: 10px 0;
        cursor: pointer;
      }
      .vacaciones-event {
        background-color: red;
        color: white;
        border: none;
      }
      .permisos-event {
        background-color: #ffc107 !important;
        color: black;
        border: none;
      }
    </style>
  </head>
  <body>
    <div style="display:flex;">
      <div id="external-events">
        <h4>Tipos de Eventos</h4>
        <div class="fc-event vacaciones-event" data-title="Vacaciones">
          Vacaciones
        </div>
        <div class="fc-event permisos-event" data-title="Permisos">
          Permisos
        </div>
      </div>
      <div id="calendar"></div>
    </div>
    <script>
      //Creamos varios eventos de ejemplos

      let eventos = [
        {
          title: "Meeting",
          start: "2024-01-10T10:30:00",
          end: "2024-01-10T12:30:00",
        },
        {
          title: "Meeting",
          start: "2024-01-11T10:30:00",
          end: "2024-01-11T12:30:00",
        },
        {
          title: "Meeting",
          start: "2024-01-12T10:30:00",
          end: "2024-01-12T12:30:00",
        },
      ];

      $(document).ready(function () {
        // Inicializar eventos arrastrables
        $("#external-events .fc-event").each(function () {
          $(this).data("event", {
            title: $.trim($(this).data("title")),
            stick: true,
            color: $(this).css("background-color"), // Asignar el color del evento
          });
          $(this).draggable({
            zIndex: 999,
            revert: true,
            revertDuration: 0,
          });
        });

        // Inicializar el calendario
        $("#calendar").fullCalendar({
          header: {
            left: "prev,next today",
            center: "title",
            right: "month,agendaWeek,agendaDay",
          },
          editable: true,
          droppable: true,
          drop: function () {
            if ($("#drop-remove").is(":checked")) {
              $(this).remove();
            }
          },
          events: eventos,
        });
      });
    </script>
  </body>
</html>
```

#### Explicación de lo que hace el código

- Se genera el HTML de la página base.
- Se cargan los CSS y javascripts
- Se genera un div con id **calendar**.
- Se genera una variable llamada **Eventos** con varios eventos de ejemplos
- Se añaden dos tipos de eventos **Vacaciones** y **Permisos** con el fin de arrastrarse al calendario.
- Al arastrar un tipo de evento sobre el calendario, se genera un único evento pero que abarca todo el día.

**Nota:** _Hasta aquí el arrastrar un tipo de eventos y mostrarlo en el calendario funciona correctamente, no obstante, la dificultad viene cuando necesitamos añadirle una opción para que cada tipo de evento tenga una hora de inicio y otra hora final._

He realizado diversas pruebas y aun con **chatgpt 4** he presentado problemas.

Adjunto un ejemplo con el evento duplicado cuando intento añadir una hora de inicio y otra final:

Ver archivo **FullCalendarDuplicado.html**

#### Código HTML base.

ver archivo **FullCalendarDuplicado.html**

```html
<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.min.css"
      rel="stylesheet"
    />
    <link
      href="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.print.min.css"
      rel="stylesheet"
      media="print"
    />
    <script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
    <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"></script>
    <script src="https://fullcalendar.io/releases/fullcalendar/3.10.0/lib/moment.min.js"></script>
    <script src="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.min.js"></script>
    <style>
      #external-events {
        padding: 10px;
        width: 20%;
        background: #f7f7f7;
      }
      #calendar {
        flex-grow: 1;
      }
      .fc-event {
        margin: 10px 0;
        cursor: pointer;
      }
      .vacaciones-event {
        background-color: red;
        color: white;
        border: none;
      }
      .permisos-event {
        background-color: #ffc107 !important;
        color: black;
        border: none;
      }
    </style>
  </head>
  <body>
    <div style="display:flex;">
      <div id="external-events">
        <h4>Tipos de Eventos</h4>
        <div class="fc-event vacaciones-event" data-title="Vacaciones">
          Vacaciones
        </div>
        <div class="fc-event permisos-event" data-title="Permisos">
          Permisos
        </div>
      </div>
      <div id="calendar"></div>
    </div>

    <script>
      $(document).ready(function () {
        // Inicializar eventos arrastrables
        $("#external-events .fc-event").each(function () {
          $(this).data("event", {
            title: $.trim($(this).data("title")),
            stick: true,
            color: $(this).css("background-color"),
            processed: false,
          });
          $(this).draggable({
            zIndex: 999,
            revert: true,
            revertDuration: 0,
          });
        });

        // Inicializar el calendario
        $("#calendar").fullCalendar({
          header: {
            left: "prev,next today",
            center: "title",
            right: "month,agendaWeek,agendaDay",
          },
          editable: true,
          droppable: true,
          drop: function (date, jsEvent, ui, resourceId) {
            var originalEventObject = $(this).data("event");

            if (!originalEventObject.processed) {
              var copiedEventObject = $.extend({}, originalEventObject);
              copiedEventObject.start = moment(
                date.format("YYYY-MM-DD") + "T08:00:00"
              );
              copiedEventObject.end = moment(
                date.format("YYYY-MM-DD") + "T17:00:00"
              );
              copiedEventObject.allDay = false;
              copiedEventObject.processed = true;
              $("#calendar").fullCalendar(
                "renderEvent",
                copiedEventObject,
                true
              );
            }
          },
          events: [
            // Eventos preexistentes
          ],
        });
      });
    </script>
  </body>
</html>
```

El código que está aquí tratando de añadir la mágia a nuestro calendario es el siguiente código javascript

####Javascript

```javascript
drop: function(date, jsEvent, ui, resourceId) {
          var originalEventObject = $(this).data('event');

          if (!originalEventObject.processed) {
            var copiedEventObject = $.extend({}, originalEventObject);
            copiedEventObject.start = moment(date.format('YYYY-MM-DD') + 'T08:00:00');
            copiedEventObject.end = moment(date.format('YYYY-MM-DD') + 'T17:00:00');
            copiedEventObject.allDay = false;
            copiedEventObject.processed = true;
            $('#calendar').fullCalendar('renderEvent', copiedEventObject, true);
          }
        },
```

#### Explicación de lo que hace el código

Esta función `drop` es un manejador de eventos en FullCalendar que se activa cuando un evento es arrastrado y soltado en el calendario. Aquí está lo que hace, paso a paso:

1. **Obtiene el Evento Original**: La variable `originalEventObject` recupera el objeto de evento arrastrado.

2. **Verificación**: Comprueba si el evento ya ha sido procesado para evitar duplicados.

3. **Copia y Modifica el Evento**: Crea una copia del evento original (`copiedEventObject`) y establece las propiedades de `start`, `end`, y `allDay`. La hora de inicio se fija a las 8:00 AM y la de fin a las 5:00 PM del día seleccionado.

4. **Renderiza el Evento en el Calendario**: Usa `$('#calendar').fullCalendar('renderEvent', copiedEventObject, true)` para añadir el evento modificado al calendario.

5. **Marca como Procesado**: Establece `processed` a `true` para indicar que el evento ha sido añadido al calendario y no debe duplicarse.

No obstante, ni con **chatgpt 4** pude solucionar que se pueda generar un evento duplicado al arrastrar un tipo de eventos.

Ahora viene la gran pregunta:

###¿Cómo solucionamos los eventos duplicados?

con el siguiente código:

#### Código HTML base.

ver archivo **FullCalendarioFuncionaOk.html**

```html
<script>
  let eventos = [
    {
      title: "Meeting",
      start: "2024-01-10T10:30:00",
      end: "2024-01-10T12:30:00",
    },
    {
      title: "Meeting",
      start: "2024-01-11T10:30:00",
      end: "2024-01-11T12:30:00",
    },
    {
      title: "Meeting",
      start: "2024-01-12T10:30:00",
      end: "2024-01-12T12:30:00",
    },
  ];
</script>

<!DOCTYPE html>
<html>
  <head>
    <link
      href="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.min.css"
      rel="stylesheet"
    />
    <link
      href="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.print.min.css"
      rel="stylesheet"
      media="print"
    />
    <script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
    <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"></script>
    <script src="https://fullcalendar.io/releases/fullcalendar/3.10.0/lib/moment.min.js"></script>
    <script src="https://fullcalendar.io/releases/fullcalendar/3.10.0/fullcalendar.min.js"></script>
    <style>
      #external-events {
        padding: 10px;
        width: 20%;
        background: #f7f7f7;
      }

      #calendar {
        flex-grow: 1;
      }

      .fc-event {
        margin: 10px 0;
        cursor: pointer;
      }

      .vacaciones-event {
        background-color: red;
        color: white;
        border: none;
      }

      .permisos-event {
        background-color: #ffc107 !important;
        color: black;
        border: none;
      }
    </style>
  </head>

  <body>
    <div style="display:flex;">
      <div id="external-events">
        <h4>Tipos de Eventos</h4>
        <div
          class="fc-event vacaciones-event"
          data-title="Vacaciones"
          data-start="08:00"
          data-end="17:00"
        >
          Vacaciones
        </div>
        <div
          class="fc-event permisos-event"
          data-title="Permisos"
          data-start="08:00"
          data-end="17:00"
        >
          Permisos
        </div>
      </div>
      <div id="calendar"></div>
    </div>

    <script>
      $(document).ready(function () {
        $("#external-events .fc-event").each(function () {
          $(this).data("event", {
            title: $.trim($(this).data("title")),
            stick: true,
            color: $(this).css("background-color"),
            startTime: $(this).data("start"),
            endTime: $(this).data("end"),
          });
          $(this).draggable({
            zIndex: 999,
            revert: true,
            revertDuration: 0,
          });
        });

        $("#calendar").fullCalendar({
          header: {
            left: "prev,next today",
            center: "title",
            right: "month,agendaWeek,agendaDay",
          },
          editable: true,
          droppable: true,
          drop: function () {
            if ($("#drop-remove").is(":checked")) {
              $(this).remove();
            }
          },
          events: eventos,
          // Configuraciones adicionales del calendario...
        });
      });
    </script>
  </body>
</html>
```

¿donde está la mágia en este código?

```html
<div id="external-events">
  <h4>Tipos de Eventos</h4>
  <div
    class="fc-event vacaciones-event"
    data-title="Vacaciones"
    data-start="08:00"
    data-end="17:00"
  >
    Vacaciones
  </div>
  <div
    class="fc-event permisos-event"
    data-title="Permisos"
    data-start="08:00"
    data-end="17:00"
  >
    Permisos
  </div>
</div>
```

#### Explicación de lo que hace el código

- Se puede observar que hay 3 propiedades de tipo data:
  - `data-title`: Aquí se mostrará que tipo de evento es, ya sea **Permisos** o **Vacaciones**
  - `data-start`: Pusimos una hora de inicio
  - `data-end`: Pusimos una hora final

La otra mágia está en el siguiente trozo de código:

####Javascript

```javascript

//aquí podemos ver que añadimos las opciones startTime y endTime
$(this).data("event", {
  title: $.trim($(this).data("title")),
  stick: true,
  color: $(this).css("background-color"),
  startTime: $(this).data("start"),
  endTime: $(this).data("end"),
});


// modificamos la opción drop y lo dejamos como se ve a continuación.
drop: function () {
    if ($('#drop-remove').is(':checked')) {
        $(this).remove();
    }
},
```


---

###End
