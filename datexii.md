# Jornadas de SIG Libre 2018 - Uso de fuentes abiertas de datos de tráfico en DATEX II y seguridad vial

José Gómez Castaño
[@jgcasta](https://twitter.com/jgcasta)
jgcasta@inspide.com

Juan José Cabrera García
jjcabrera@inspide.com

* Introducción
* [INSPIDE](http://www.inspide.com/)
* Modelos UML y XML
* Qué es DATEX
* Modelo de Datos
* Herramientas necesarias
* Parseo del XML o JSON
* Integración de la informacion en un mapa

## Introducción

* Etherpad compoartido https://bit.ly/2HuLKZS

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

* Mostrar la ubicación de las cámaras en Leaflet
![Camaras](mapa.png)

## Aplicaciones prácticas en Seguridad Vial
* Integración Comobity

[![Integración Comobity](http://img.youtube.com/vi/AOkOKtZNoHo/0.jpg)](https://www.youtube.com/watch?v=AOkOKtZNoHo "Comobity")

* Integración Auxilio en Carretera

[![Integración Auxilio en Carretera](http://img.youtube.com/vi/O_t6WM5TA8s/0.jpg)](https://www.youtube.com/watch?v=O_t6WM5TA8s "Integración Auxilio en Carretera")

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
