```markdown
# Generador de Mapas Interactivos con Heatmap

Este proyecto genera mapas interactivos con **heatmaps** para analizar factores específicos en el departamento de Santander, Colombia. Se pueden visualizar las zonas más afectadas según los datos proporcionados.

## Características

- Genera mapas interactivos centrados en Santander.
- Muestra un **heatmap** con las 5 zonas más afectadas por un factor seleccionado.
- Incluye marcadores personalizados con íconos únicos según el tipo de zona.
- Ofrece múltiples capas de mapas, como normal, terreno, blanco y negro, y satelital.

## Estructura del Proyecto

```
.
├── datos_sinteticos_santander_con_coordenadas.csv  # Archivo CSV con los datos
├── generar_mapa.py                                # Código principal
└── README.md                                      # Este archivo
```

## Requisitos

Instala las siguientes dependencias con `pip`:

```bash
pip install pandas folium
```

## Archivo CSV

El archivo CSV debe incluir las siguientes columnas mínimas:

- `Ubicación`: Nombre del municipio.
- `Factor o Carencia`: Nombre del factor a analizar.
- `Unidades`: Valor numérico asociado al factor.
- `Actores`: Tipo de zona, como Puentes, Escuelas, etc.

## Uso

1. Asegúrate de tener un archivo CSV con los datos requeridos.
2. Modifica el diccionario `municipios_santander` para incluir las coordenadas de todos los municipios presentes en el CSV.
3. Ejecuta el script en la terminal:

   ```bash
   python generar_mapa.py
   ```

4. Introduce el factor que deseas analizar cuando se te solicite.
5. El mapa generado se guardará como un archivo HTML, por ejemplo: `mapa_santander_<factor>_heatmap.html`.
6. Abre el archivo HTML en un navegador para visualizar el mapa.

## Ejemplo de Ejecución

```plaintext
Ingresa el factor a analizar (ejemplo: 'Escuela', 'Puentes', 'Placa Huella'): Puentes
```

El archivo `mapa_santander_Puentes_heatmap.html` se generará con el mapa interactivo.

## Personalización

### Íconos

Puedes cambiar los íconos actualizando las URLs en la función `obtener_icono`:

```python
def obtener_icono(tipo_zona, tamaño='32'):
    iconos = {
        'Puentes': f"https://cdn.icon-icons.com/icons2/995/PNG/{tamaño}/Bridge_Constructor_icon-icons.com_75321.png",
        ...
    }
    return iconos.get(tipo_zona, f'https://cdn.icon-icons.com/icons2/3601/PNG/{tamaño}/default_icon.png')
```

### Coordenadas

Asegúrate de que todos los municipios del CSV estén incluidos en el diccionario `municipios_santander`:

```python
municipios_santander = {
    'Barrancabermeja': [7.05, -73.85],
    'Bucaramanga': [7.12, -73.12],
    ...
}
```

## Contribuciones

¡Contribuciones son bienvenidas! Si encuentras algún error o tienes ideas para mejorar este proyecto, no dudes en abrir un *issue* o enviar un *pull request*.

## Licencia

Este proyecto está licenciado bajo la Licencia MIT. Consulta el archivo `LICENSE` para más detalles.
```

Copia este bloque completo en el archivo `README.md` de tu repositorio para GitHub. ¡Es ideal para presentar tu proyecto de manera profesional y detallada! 🚀
