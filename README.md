# 🎮 eSports Manager — Tarea 1 BDD

## Integrantes
| Nombre | \
|-------------------------| \
| Andrés Ferrer | \
| Florencia Orellana | \
| Vicente Dubo | 

---

## Instrucciones para ejecutar la aplicación

### 1. Crear la base de datos
```bash
psql -U postgres -c "CREATE DATABASE tarea1;"
```

### 2. Cargar el esquema
```bash
psql -U postgres -d tarea1 -f schema.sql
```

### 3. Cargar los datos
```bash
psql -U postgres -d tarea1 -f data.sql
```

### 4. Levantar el servidor PHP
```bash
cd app && php -S localhost:8000
```

### 5. Abrir en el navegador
http://localhost:8000

---

## Variables de entorno

La aplicación lee la configuración de conexión desde variables de entorno.
Si no se definen, usa los siguientes valores por defecto:

| Variable     | Default     |
|--------------|-------------|
| DB_HOST      | localhost   |
| DB_PORT      | 5432        |
| DB_USER      | postgres    |
| DB_PASSWORD  | postgres    |
| DB_NAME      | tarea1      |

Para definirlas manualmente (opcional):
```bash
# Windows
set DB_PASSWORD=mi_contraseña

# Linux / Mac
export DB_PASSWORD=mi_contraseña
```

---

## Estructura de archivos
tarea1/ \
├── schema.sql          # Creación de tablas \
├── data.sql            # Datos sintéticos \
├── informe.pdf         # Parte A \
├── llm-log.pdf         # Registro de uso de LLMs \
├── README.md           # Este archivo \
└── app/ \
<pre>├── conexion.php            # Conexión a PostgreSQL </pre>\
<pre>├── index.php               # Página de inicio </pre>\
<pre>├── torneos.php             # Lista de torneos </pre>\
<pre>├── torneo_detalle.php      # Detalle de un torneo </pre>\
<pre>├── stats_torneos.php       # Selección de torneo para stats </pre>\
<pre>├── detalle_stats.php       # Opciones de stats de un torneo </pre>\
<pre>├── ranking_jugadores.php   # Ranking por ratio KOs/restarts </pre>\
<pre>├── seleccionar_equipo.php  # Selección de equipo finalista </pre>\
<pre>├── evolucion_por_fase.php  # Evolución de stats por fase </pre>\
<pre>├── busqueda.php            # Búsqueda de jugadores y equipos </pre>\
<pre>├── sponsors.php            # Sponsors por videojuego </pre>\
<pre>├── inscripcion.php         # Formulario de inscripción </pre>\
<pre>├── buscar_jugadores.php    # Lista filtrable de jugadores </pre>\
<pre>└── buscar_equipos.php      # Lista filtrable de equipos 


---

## Navegación de la aplicación

### Inicio
`index.php` — Página principal con acceso a todas las secciones.

---

### Torneos
`torneos.php` — Lista todos los torneos con nombre, videojuego, fechas y prize pool.
- Torneos sin cupo completo: bloqueados (notificación roja).
- Torneos completos sin partidas: bloqueados (notificación naranja).
- Torneos completos con partidas: accesibles.

`torneo_detalle.php?id=X` — Detalle completo del torneo:
- Tabla de posiciones separada en **Grupo A** y **Grupo B**, con badge **SEMIS** para los 2 primeros de cada grupo.
- Partidas jugadas con resultado, ganador y fase (badges de color).
- Equipos inscritos y sponsors con monto aportado.

---

### Stats de Torneos
`stats_torneos.php` — Lista torneos como cards. Mismo sistema de bloqueo que torneos.php.

`detalle_stats.php?id=X` — Elige entre dos vistas:

**Ranking de Jugadores** → `ranking_jugadores.php?id=X`
- Jugadores ordenados por ratio KOs / restarts.
- Solo jugadores con al menos 2 partidas en el torneo.
- Muestra gamertag, equipo, KOs, restarts, assists y ratio.
- Medallas 🥇🥈🥉 para el top 3.

**Evolución por Fase** → `seleccionar_equipo.php?id=X`
- Muestra los 2 equipos finalistas con 🥇 y 🥈.

`evolucion_por_fase.php?id=X&equipo=Y`
- Comparativa de promedios por jugador entre **Fase de Grupos** y **Eliminatorias** (semifinal + final).
- Detalle individual con ranking por KOs dentro de cada fase usando window functions (`RANK() OVER`).

---

### Buscador

`busqueda.php` - Muestra la opción de buscar jugadores o buscar equipos, se puede clickear alguna de estas dos para ir a otra página.

`buscar_jugadores.php` - Muestra en principio una lista de todos los jugadores y justo arriba dos searchbars que permiten filtrar dicha lista ya sea por gamertag o por país (solo un atributo a la vez)

`buscar_equipos.php` - Mismo concepto que en buscar_jugadores.php, pero la lista es de equipos y hay solo una searchbar que permite filtar por el nombre del equipo.

---

### Sponsors

`sponsors.php` — Lista de juegos, acceder a cualquiera de ellos te lleva a:

`sponsors_detalle.php` - Lista de los sponsors que patrocinan a todos los torneos de un determinado juego


---

### Inscripción
`inscripcion.php` — Formulario para inscribir un equipo en un torneo.
- Muestra cupo disponible en tiempo real al seleccionar el torneo.
- Rechaza la inscripción si el torneo está lleno → mensaje de error en rojo.
- Rechaza si el equipo ya está inscrito en ese torneo.
- Si pasa ambas validaciones, guarda en la base de datos.