```python
import pandas as pd
import folium
from folium.plugins import HeatMap

def obtener_icono(tipo_zona, tamaño='32'):
    # Diccionario con URLs de los íconos en icon-icons.com, donde el tamaño puede ser ajustado
    iconos = {
        'Puentes': f"https://cdn.icon-icons.com/icons2/995/PNG/{tamaño}/Bridge_Constructor_icon-icons.com_75321.png",
        'Antenas Telefónicas': f"https://cdn.icon-icons.com/icons2/1829/PNG/{tamaño}/antennasignalobservatoryradio-115839_115790.png",
        'Placa Huella': f"https://cdn.icon-icons.com/icons2/3601/PNG/{tamaño}/journey_road_street_asph_highway_icon_226304.png",
        'Escuelas': f"https://cdn.icon-icons.com/icons2/2313/PNG/{tamaño}/teacher_education_school_lecture_student_icon_141984.png",
    }

    # Si no se encuentra el tipo de zona, se usará un ícono por defecto
    icono_url = iconos.get(tipo_zona, f'https://cdn.icon-icons.com/icons2/3601/PNG/{tamaño}/default_icon.png')

    return icono_url 

def generar_mapa(factor_input, df_datos, municipios_santander):
    # Validar si el factor ingresado existe en los datos
    if factor_input not in df_datos['Factor o Carencia'].unique():
        print(f"El factor '{factor_input}' no existe en los datos. Intenta con otro factor.")
        return
    
    # Filtrar las 5 zonas más afectadas por el factor
    columna_factor = 'Unidades'
    zonas_afectadas = df_datos[df_datos['Factor o Carencia'] == factor_input].nlargest(5, columna_factor)
    
    # Crear el mapa centrado en Santander
    mapa_santander = folium.Map(location=[7.13, -73.13], zoom_start=10)
    
    # Crear una lista de coordenadas y valores para el heatmap
    heat_data = []
    
    # Añadir marcadores y puntos para el heatmap
    for _, fila in zonas_afectadas.iterrows():
        nombre = fila['Ubicación'] 
        
        if nombre in municipios_santander:
            latitud, longitud = municipios_santander[nombre]
        else:
            print(f"Advertencia: El municipio '{nombre}' no tiene coordenadas definidas. Saltando...")
            continue
        
        tipo_zona = fila.get('Actores', 'Otro')
        factor_value = fila[columna_factor]
        
        icon_url = obtener_icono(tipo_zona)
        popup_texto = f"""
        <strong>{nombre}</strong><br>
        Latitud: {latitud}<br>
        Longitud: {longitud}<br>
        {factor_input.capitalize()}: {factor_value}<br>
        Tipo de Zona: {tipo_zona}
        """
        
        folium.Marker(
            location=[latitud, longitud],
            popup=folium.Popup(popup_texto, max_width=300),
            icon=folium.CustomIcon(icon_url, icon_size=(30, 30))
        ).add_to(mapa_santander)

        heat_data.append([latitud, longitud, factor_value])
    
    HeatMap(heat_data).add_to(mapa_santander)
    
    # Añadir capas base
    folium.TileLayer("OpenStreetMap", name="Mapa Normal").add_to(mapa_santander)
    folium.TileLayer(
        tiles="http://{s}.tile.stamen.com/terrain/{z}/{x}/{y}.jpg",
        attr="Map tiles by Stamen Design, CC BY 3.0 — Map data © OpenStreetMap contributors",
        name="Mapa Terreno"
    ).add_to(mapa_santander)
    folium.TileLayer(
        tiles="http://{s}.tile.stamen.com/toner/{z}/{x}/{y}.png",
        attr="Map tiles by Stamen Design, CC BY 3.0 — Map data © OpenStreetMap contributors",
        name="Mapa Blanco y Negro"
    ).add_to(mapa_santander)
    folium.TileLayer(
        tiles="https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png",
        attr="Map data © OpenStreetMap contributors, SRTM | Map style: © OpenTopoMap (CC-BY-SA)",
        name="Mapa Satelital"
    ).add_to(mapa_santander)

    folium.LayerControl().add_to(mapa_santander)
    mapa_santander.save(f"mapa_santander_{factor_input}_heatmap.html")
    print(f"¡Mapa generado con heatmap! Abre 'mapa_santander_{factor_input}_heatmap.html' en tu navegador para verlo.")

def cargar_datos_csv(ruta_csv):
    try:
        df_datos = pd.read_csv(ruta_csv)
        return df_datos
    except FileNotFoundError:
        print("Archivo CSV no encontrado.")
        return None

if __name__ == "__main__":
    ruta_csv = "E:/Andrea v4/Trabajos/MED_Juan/Mapas/Santander/datos_sinteticos_santander_con_coordenadas.csv"
    df_datos = cargar_datos_csv(ruta
