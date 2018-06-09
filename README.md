# Jornadas de SIG Libre 2018 - Uso de fuentes abiertas de datos de tráfico en DATEX II y seguridad vial

José Gómez Castaño
[@jgcasta](https://twitter.com/jgcasta)
jgcasta@inspide.com

Juan José Cabrera García
jjcabrera@inspide.com

## Sobre qué hablaremos
* [INSPIDE](http://www.inspide.com/)
* Modelos UML y XML
* Qué es DATEX
* Modelo de Datos
* Herramientas necesarias
* Parseo del XML o JSON
* Integración de la informacion en un mapa
* Casos de uso reales

## Introducción

* Condolencias a los que tengáis que utilizar este esquema
* DATEX se concibió como un estandar de intercambio de datos entre organismos de tráfico.
* Actualmente se encuentra la especificación DATEXII que es una extensión para permitir el intercambio con organizaciones privadas
* Es un estandard multiparte, mantenido por el  CEN Technical Committee 278, CEN/TC278, (Road Transport and Traffic Telematics) dividido en 3 partes:
    - Contexto y framework: metodología del modelado
    - Localización
    - Información de tráfico
* Usos principales
    - Administración de rutas y reorganización 
    - Administración de la infraestructura
    - Enlazado de la administración del tráfico con los sistemas de información
* Responde a un modelo UML establecido
* En contra: Necesidad de mejorar el modelo para hacerlo más eficiente
* A favor: Es un estándard y todos hablan el mismo lenguaje

## Qué es DATEX
* [Presentación Comisión Europea sobre DATEXII](http://akce.fd.cvut.cz/sites/default/files/datex2/presentations/D2_01b_02_Jorg_Freundenstein_Tour_through_DATEX_Model.pdf)
* [Guía de DATEXII proporcionada por DGT](http://infocar.dgt.es/datex2/informacion_adicional/Guia%20de%20Utilizacion%20de%20DATEX%20II.pdf)
* Nos damos de alta en el portal http://www.datex2.eu/

## Práctica
![Dioagrama](taller.png)

* Obtener los datos de las cámaras de tráfico desde el portal estadístico de DGT - http://www.dgt.es
* Parsear los datos a un GeoJSON

```txt
$ xml2json camaras.xml camaras.json

```
```python
#!/usr/bin/env python
# -*- coding: utf-8

'''
    Actualiza la tabla incidgt con las ultimas incidencias publicadas por le DGT en DATEX

   pip install json
   sudo apt-get install nodejs
   sudo apt-get install npm
   sudo apt-get install python-pip

   sudo aptitude install npm

'''

import json
import sys

reload(sys)
sys.setdefaultencoding('utf8')

f=open("datos.json","w")

salida = ""
salida = salida + "["


with open('ejemplos/camaras.json') as j:
    data = json.load(j)
    fecha_publicacion =  data['_0:d2LogicalModel']['_0:payloadPublication']['_0:publicationTime']

    lista_camaras = data['_0:d2LogicalModel']['_0:payloadPublication']['_0:genericPublicationExtension']['_0:cctvSiteTablePublication']['_0:cctvCameraList']['_0:cctvCameraMetadataRecord']

    for camara in lista_camaras:
        id = camara['id']
        version = camara['version']
        identificacion = camara['_0:cctvCameraIdentification']
        tipo = camara['_0:cctvCameraType']
        lat = camara['_0:cctvCameraLocation']['_0:locationForDisplay']['_0:latitude']
        lon = camara['_0:cctvCameraLocation']['_0:locationForDisplay']['_0:longitude']
        url = camara['_0:cctvStillImageService']['_0:stillImageUrl']['_0:urlLinkAddress']

        salida = salida + "{"
        salida = salida + "    \"type\": \"Feature\","
        salida = salida + "    \"geometry\": {"
        salida = salida + "        \"type\": \"Point\","
        salida = salida + "            \"coordinates\": [" + str(lon) + ", " + str(lat) + "]"
        salida = salida + "        },"
        salida = salida + "    \"properties\":"
        salida = salida + "        {"
        salida = salida + "            \"id\": \"" + str(id) + "\","
        salida = salida + "            \"identificacion\": \"" + str(identificacion) + "\","
        salida = salida + "            \"url\": \"" + str(url) + "\""
        salida = salida + "        }"
        salida = salida + "},"

salida = salida[0:len(salida)-1]

salida = salida + "]"
f.write(salida)
f.close()
```

* Mostrar la ubicación de las cámaras en Leaflet http://desa.inspidesingularity.es/siglibre2018/
![Camaras](mapa.png)

## Aplicaciones prácticas en Seguridad Vial
* Integración Comobity

[![Integración Comobity](http://img.youtube.com/vi/AOkOKtZNoHo/0.jpg)](https://www.youtube.com/watch?v=AOkOKtZNoHo "Comobity")

* Integración Auxilio en Carretera

[![Integración Auxilio en Carretera](http://img.youtube.com/vi/O_t6WM5TA8s/0.jpg)](https://www.youtube.com/watch?v=O_t6WM5TA8s "Integración Auxilio en Carretera")

* DGT 3.0

![DGT 3.0](dgt30.png)

## Referencias
* http://www.datex2.eu/
* Esquemas y utilidades - http://www.datex2.eu/current-version-reference
* Modelado - http://www.datex2.eu/sites/www.datex2.eu/files/d2-profile/pdf/DataModelTMPAndNavigationSystems_01-00-01_0.pdf
* Data Model Navigation online - http://www.datex2.eu/datex-model/HTML.Version_2.3/index.htm
* Ayuda para desarrolladores - http://www.datex2.eu/current-version-supporting
* Diccionario de datos - http://www.datex2.eu/content/datex-ii-v23-data-dictionary
* Guia para seguridad vial - http://www.datex2.eu/sites/www.datex2.eu/files/DATEX_II_Guide_for_safety_related_content_in_DATEX_v_2.3.pdf
* DGT3.0 -  http://www.dgt.es/es/el-trafico/dgt-3-0/index.shtml

## Herramientas
* xml2json - https://www.npmjs.com/package/xml2json-cli
