# Apunts sobre minidom i XSLT
## minidom a Python
El llenguatge minidom és un mòdul a Python que permet la manipulació de documents XML d'una manera senzilla i eficient. Algunes de les seves característiques i funcionalitats clau són les següents:

Biblioteca estàndard: minidom és part de la biblioteca estàndard de Python, la qual cosa significa que no és necessari instal·lar biblioteques addicionals per utilitzar-lo.
Facilitat d'ús: Proporciona una interfície intuïtiva i fàcil d'utilitzar per crear, modificar i analitzar documents XML.
DOM (Model d'Objecte de Document): Utilitza el model d'objecte de document per representar l'estructura del document XML com un arbre de nodes, la qual cosa facilita la navegació i la manipulació del document.
### Exemple de com mostrar la etiqueta arrel d'un document xml amb minidom
```python
from xml.dom import minidom
doc = minidom.parse("fitxer.xml")

node_arrel = doc.documentElement

text_node_arrel = node_arrel.tagName

print (text_node_arrel)
```
### Exemple de com mostrar tot el document xml amb minidom
```python
from xml.dom import minidom
doc = minidom.parse("fitxer.xml")

tot = doc.toxml()
print (tot)
```
### Exemple de crear un document html amb la informació agafada de un xml amb minidom
```python
from xml.dom import minidom
doc = minidom.parse("fitxer.xml")

f_=open("dom.html","w")

llistapersona = doc.getElementsByTagName("person")
f_.write(f"<html><head><title>Diaris DOM</title></head>\n")
f_.write(f"  <body>\n")
for person in llistapersona:
    atributID = person.getAttribute("id")
    nom = person.getElementsByTagName("name")[0]
    nom_text = nom.firstChild.data
    gender = nom.getAttribute("gender")
    edat = person.getElementsByTagName("age")[0].firstChild.data
    naixament = person.getElementsByTagName("naixement")[0].firstChild.data

    f_.write(f"       <h2>{atributID} - {nom_text}</h2>\n")
    f_.write(f"       <ul>\n")
    f_.write(f"           <li>age - {edat}</li>\n")
    f_.write(f"           <li>sex - {gender}</li>\n")  
    f_.write(f"           <li>naixament - {naixament}</li>\n")
    f_.write(f"       </ul>\n")  
f_.write(f"   </body>\n")
f_.write(f"</html>\n")
```
### Exemple de crear un horari amb html amb la base de un xml creat amb el xml
```python
from xml.dom import minidom

doc = minidom.parse("UF2-3.xml")
f_ = open("taula.html", "w")

alumne = doc.getElementsByTagName("alumne")[0]
nom_estudiant = alumne.getElementsByTagName("nom")[0].firstChild.data
curs = alumne.getElementsByTagName("curs")[0].firstChild.data
foto_estudiant = alumne.getElementsByTagName("foto")[0].firstChild.data

franges = doc.getElementsByTagName("franja")
classes = doc.getElementsByTagName("classes")[0]
dies = classes.getElementsByTagName("dia")

# Diccionario para almacenar colores asociados a asignaturas
colors_dict = {}
colors = doc.getElementsByTagName("colors")[0].getElementsByTagName("assignatura")
for color_node in colors:
    assignatura = color_node.firstChild.data
    color = color_node.getAttribute("color")
    colors_dict[assignatura] = color

f_.write("<!DOCTYPE html>\n")
f_.write("<html>\n")
f_.write("  <head>\n")
f_.write("     <meta charset='UTF-8'>\n")
f_.write("     <meta name='viewport' content='width=device-width, initial-scale=1.0'>\n")
f_.write(f"      <title>Horari de {nom_estudiant}</title>\n")
f_.write("  </head>\n")
f_.write("  <body>\n")
f_.write("      <header>\n")
f_.write(f"              <h1>Horari de {nom_estudiant}</h1>\n")
f_.write(f"              <p>Curs: {curs}</p>\n")
f_.write(f"              <img src='{foto_estudiant}' alt='Foto de {nom_estudiant}'>\n")
f_.write("      </header>\n")
f_.write("      <table border='1'>\n")
f_.write("          <thead>\n")
f_.write("              <tr>\n")
f_.write("                  <th>Franges Horàries</th>\n")

for dia in dies:
    nom_dia = dia.getElementsByTagName("nom")[0].firstChild.data
    f_.write(f"                  <th>{nom_dia}</th>\n")
f_.write("              </tr>\n")
f_.write("          </thead>\n")
f_.write("          <tbody>\n")

for i, franja in enumerate(franges):
    f_.write("              <tr>\n")
    f_.write(f"                  <td>{franja.firstChild.data}</td>\n")
    for dia in dies:
        assignatures = dia.getElementsByTagName("assignatura")
        assig = assignatures[i].firstChild.data if i < len(assignatures) else ""
        color = colors_dict.get(assig, "#FFFFFF")  # Si no hay color asignado, por defecto blanco
        f_.write(f"                  <td style='background-color: {color};'>{assig}</td>\n")
    f_.write("              </tr>\n")

f_.write("          </tbody>\n")
f_.write("      </table>\n")
f_.write("  </body>\n")
f_.write("</html>\n")

f_.close()from xml.dom import minidom

doc = minidom.parse("UF2-3.xml")
f_ = open("taula.html", "w")

alumne = doc.getElementsByTagName("alumne")[0]
nom_estudiant = alumne.getElementsByTagName("nom")[0].firstChild.data
curs = alumne.getElementsByTagName("curs")[0].firstChild.data
foto_estudiant = alumne.getElementsByTagName("foto")[0].firstChild.data

franges = doc.getElementsByTagName("franja")
classes = doc.getElementsByTagName("classes")[0]
dies = classes.getElementsByTagName("dia")

# Diccionario para almacenar colores asociados a asignaturas
colors_dict = {}
colors = doc.getElementsByTagName("colors")[0].getElementsByTagName("assignatura")
for color_node in colors:
    assignatura = color_node.firstChild.data
    color = color_node.getAttribute("color")
    colors_dict[assignatura] = color

f_.write("<!DOCTYPE html>\n")
f_.write("<html>\n")
f_.write("  <head>\n")
f_.write("     <meta charset='UTF-8'>\n")
f_.write("     <meta name='viewport' content='width=device-width, initial-scale=1.0'>\n")
f_.write(f"      <title>Horari de {nom_estudiant}</title>\n")
f_.write("  </head>\n")
f_.write("  <body>\n")
f_.write("      <header>\n")
f_.write(f"              <h1>Horari de {nom_estudiant}</h1>\n")
f_.write(f"              <p>Curs: {curs}</p>\n")
f_.write(f"              <img src='{foto_estudiant}' alt='Foto de {nom_estudiant}'>\n")
f_.write("      </header>\n")
f_.write("      <table border='1'>\n")
f_.write("          <thead>\n")
f_.write("              <tr>\n")
f_.write("                  <th>Franges Horàries</th>\n")

for dia in dies:
    nom_dia = dia.getElementsByTagName("nom")[0].firstChild.data
    f_.write(f"                  <th>{nom_dia}</th>\n")
f_.write("              </tr>\n")
f_.write("          </thead>\n")
f_.write("          <tbody>\n")

for i, franja in enumerate(franges):
    f_.write("              <tr>\n")
    f_.write(f"                  <td>{franja.firstChild.data}</td>\n")
    for dia in dies:
        assignatures = dia.getElementsByTagName("assignatura")
        assig = assignatures[i].firstChild.data if i < len(assignatures) else ""
        color = colors_dict.get(assig, "#FFFFFF")  # Si no hay color asignado, por defecto blanco
        f_.write(f"                  <td style='background-color: {color};'>{assig}</td>\n")
    f_.write("              </tr>\n")

f_.write("          </tbody>\n")
f_.write("      </table>\n")
f_.write("  </body>\n")
f_.write("</html>\n")

f_.close()
```

## XSLT (Transformacions de Llenguatge d'Estil Extensible)
XSLT és un llenguatge de transformació utilitzat per transformar documents XML en altres formats, com ara HTML, XML o text pla. Algunes de les característiques i funcionalitats més importants de XSLT inclouen:

Basat en patrons: XSLT utilitza regles de transformació basades en patrons per definir com s'ha de processar cada element i atribut del document XML d'entrada.
Separació de contingut i presentació: Permet separar l'estructura del contingut XML de la seva presentació, la qual cosa facilita la generació de diferents vistes de les mateixes dades.
Potent i flexible: Proporciona una àmplia gamma de funcions i capacitats per manipular i transformar documents XML de manera eficient.
### Exemple d'una fulla d'estil XSLT
```xsl
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="/">
    <html>
      <body>
        <h2>Exemple de transformació XSLT</h2>
        <ul>
          <xsl:for-each select="books/book">
            <li><xsl:value-of select="title"/></li>
          </xsl:for-each>
        </ul>
      </body>
    </html>
  </xsl:template>
</xsl:stylesheet>
```
### Exemple d'horari amb xslt
```xsl
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method="html" indent="yes"/>
    <xsl:template match="/horari">
        <head>
            <style type="text/css">
                table {
                    width: 100%;
                }
                th, td {
                    border: 1px solid gray;
                    padding: 5px;
                    text-align: center;
                }
                th {
                    background-color: lightgray;
                }
                .M01 { background-color: #ff9999; }
                .M02 { background-color: #99ff99; }
                .M03 { background-color: #9999ff; }
                .M04 { background-color: #ffff99; }
                .M08 { background-color: #cc99ff; }
                .M09 { background-color: #ff99ff; }
                .M10 { background-color: #ffcc99; }
                .M11 { background-color: #99ffff; }
                ul, li {
                    text-align: center;
                }
            </style>
        </head>
        <html>
            <body>
                <img src="{@header}" width="auto"/>
                <table>
                    <tr>
                        <xsl:for-each select="setmana/dia">
                            <th>
                                <xsl:value-of select="@nom"/>
                            </th>
                        </xsl:for-each>
                    </tr>
                    <tr>
                        <xsl:for-each select="setmana/dia">
                            <td>
                                <xsl:for-each select="modul">
                                    <p class="{codi}">
                                        <xsl:value-of select="codi"/>
                                        <xsl:text> - </xsl:text>
                                        <xsl:value-of select="nom"/>
                                    </p>
                                </xsl:for-each>
                            </td>
                        </xsl:for-each>
                    </tr>
                </table>
                <ul>
                    <h1><xsl:value-of select="links/@nom"/></h1>
                    <xsl:for-each select="links/link">
                        <xsl:sort select="nom"/>
                        <li>
                            <a href="{url}">
                                <xsl:value-of select="nom"/>
                            </a>
                        </li>
                    </xsl:for-each>
                </ul>
            </body>
        </html>
    </xsl:template>
</xsl:stylesheet>
```
