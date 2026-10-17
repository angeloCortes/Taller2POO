# Taller 02 - La Fonda del Papu

Programa de consola en Java (POO) que permite registrar, consultar, editar y eliminar los choripanes y terremotos consumidos por un grupo de amigos, y calcular métricas a partir de esos registros. Los cambios quedan guardados en `Consumido.txt`.

## Integrantes

| Nombre | Carrera |
|---|---|---|---|
| Angelo Cortés | 22.025.292-2 | ICCI | @angeloCortes |
| Javiera Maluenda | 21.995.972-9 | ITI | | @idkjvttt |

## Requisitos

- Java JDK 8 o superior (`java -version` y `javac -version` para comprobarlo).
- Git.

## Clonación y ejecución

```bash
# 1. Clonar el repositorio
git clone <URL-DEL-REPOSITORIO>
cd <NOMBRE-DEL-REPOSITORIO>

# 2. Compilar (las clases están en src/taller2)
javac -d bin src/taller2/*.java

# 3. Ejecutar DESDE LA RAIZ del proyecto
java -cp bin taller2.App
```

Importante: el programa lee `Participantes.txt` y `Consumido.txt` con rutas relativas, por lo que debe ejecutarse desde la carpeta raíz del proyecto, donde están ambos archivos.

### Con Eclipse

1. `File > Import > Git > Projects from Git (with smart import)` y pegar la URL del repositorio.
2. Ejecutar `App.java` con `Run As > Java Application`.
3. Los `.txt` deben estar en la raíz del proyecto (al mismo nivel que `src`).

## Archivos de entrada

### Participantes.txt

Un participante por línea:

```
Koshao
Lemus
MrNacho
```

### Consumido.txt

Un registro por línea, con campos separados por `;` y fecha en formato `YYYY-MM-DD`:

```
Choripan;Consumidor;Largo;Ancho;Fecha
Terremoto;Consumidor;Litros;Fecha
```

Ejemplo:

```
Choripan;Koshao;10;3;2026-09-16
Terremoto;Lemus;0.5;2026-09-16
Choripan;MrNacho;9;3;2026-09-16
```

Los decimales se escriben con punto (`0.5`). Si `Consumido.txt` no existe, el programa avisa e inicia sin registros; al guardar el primer registro, el archivo se crea solo.

## Uso del programa

```
[1] Ver registros          -> Choripanes / Terremotos
[2] Actualizar registros   -> Agregar / Editar / Eliminar
[3] Calcular metricas      -> 6 métricas
[4] Salir
```

- **Ver registros:** tabla numerada con consumidor, medidas, fecha, KCal y totales.
- **Agregar:** se elige el tipo, el consumidor (de la lista de participantes), las medidas y la fecha.
- **Editar:** se elige el registro por su número. Dejar un campo vacío mantiene su valor actual. En consumidor se escribe el nombre de un participante existente.
- **Eliminar:** se elige el registro y se pide confirmación (`s/n`).
- En las listas de editar y eliminar, `0` cancela la operación.
- Cada cambio se escribe de inmediato en `Consumido.txt`.

## Reglas de cálculo

| Métrica | Fórmula |
|---|---|
| KCal de un choripán | `(Largo * Ancho) / 3` |
| KCal de un terremoto | `(Litros * 10) / 3` |
| Día ebrio | Un usuario suma 2 L o más de terremoto en una misma fecha (se suman todos sus terremotos de ese día) |

### Métricas disponibles

1. Choripanes comidos en total.
2. Terremotos tomados en total (con litros).
3. Choripanes comidos por usuario.
4. Terremotos tomados por usuario (cantidad y litros).
5. KCal consumidas por usuario (choripán, terremoto, total y total de la fonda).
6. Días ebrio por usuario, con el detalle de fecha y litros.

## Estructura del proyecto

```
.
├── src/taller2/
│   ├── App.java         # Main, menús, validaciones, carga y guardado de archivos
│   ├── Choripan.java    # Modelo: consumidor, largo, ancho, fecha
│   └── Terremoto.java   # Modelo: consumidor, litros, fecha
├── Participantes.txt
├── Consumido.txt
└── README.md
```

### Clases

- **`Choripan`**: guarda sus datos, calcula sus KCal (`calculoKCalorias()`) y genera su línea para el archivo (`toLineaArchivo()`).
- **`Terremoto`**: igual que `Choripan`, con litros en lugar de largo y ancho.
- **`App`**: contiene los vectores estáticos (`Choripan[200]`, `Terremoto[200]`, `String[200]`) con sus contadores, el menú principal y los submenús, y los métodos auxiliares de lectura y validación.

## Control de errores

- Opciones de menú: se rechazan letras, vacío y números fuera de rango, y se vuelve a pedir.
- Valores numéricos (largo, ancho, litros): deben ser números mayores a 0. Se acepta coma o punto como separador decimal.
- Fechas: se valida el formato `YYYY-MM-DD` y que la fecha exista (incluye años bisiestos).
- Consumidor: debe ser un participante de `Participantes.txt`.
- Confirmación de eliminar: solo acepta `s` o `n`.
- Archivos inexistentes: se muestra una advertencia y el programa continúa.
- Falla al guardar: se informa, y el cambio queda en memoria.

## Restricciones técnicas

- Programación orientada a objetos.
- Sin colecciones dinámicas (`ArrayList`, `HashMap`, etc.): solo vectores estáticos con un máximo de 200 registros por tipo.
- Librerías usadas: `Scanner`, `File`, `FileWriter`, `BufferedWriter` e `IOException`.

## Cómo probar el programa

1. Ejecutar con los archivos de ejemplo y revisar `Ver registros` y cada métrica.
2. Agregar un registro, cerrar el programa, volver a abrirlo y comprobar que sigue ahí (persistencia).
3. Editar dejando todos los campos vacíos: el registro no debe cambiar.
4. Eliminar el primero, uno del medio y el último; responder `n`, `x` y vacío a la confirmación.
5. Probar entradas inválidas: `hola`, vacío, `-2`, `0`, `2026-13-45`, `2026-02-30`, un consumidor inexistente.
6. Renombrar `Consumido.txt` y ejecutar: debe avisar y partir sin registros.
7. Probar `Volver` y `Salir` en todos los menús.
