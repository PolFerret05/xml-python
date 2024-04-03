# Apunts sobre minidom i XSLT
## minidom a Python
[Guia per utilitzar dom](https://www.w3schools.com/xml/dom_intro.asp)

_El llenguatge minidom és un mòdul a Python que permet la manipulació de documents XML d'una manera senzilla i eficient. Algunes de les seves característiques i funcionalitats clau són les següents:_

- **Biblioteca estàndard:** minidom és part de la biblioteca estàndard de Python, la qual cosa significa que no és necessari instal·lar biblioteques addicionals per utilitzar-lo.
- **Facilitat d'ús:** Proporciona una interfície intuïtiva i fàcil d'utilitzar per crear, modificar i analitzar documents XML.
- **DOM (Model d'Objecte de Document):** Utilitza el model d'objecte de document per representar l'estructura del document XML com un arbre de nodes, la qual cosa facilita la navegació i la manipulació del document.
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
### Així es com queda:
![lala](https://github.com/PolFerret05/xml-python/assets/165801828/6bf4ad43-f25f-4a65-a9b1-96277993dd67)


## XSLT (Transformacions de Llenguatge d'Estil Extensible)
[Guia per utilitzar xslt](https://www.w3schools.com/xml/xsl_intro.asp)

_XSLT és un llenguatge de transformació utilitzat per transformar documents XML en altres formats, com ara HTML, XML o text pla. Algunes de les característiques i funcionalitats més importants de XSLT inclouen:_

**Basat en patrons:** XSLT utilitza regles de transformació basades en patrons per definir com s'ha de processar cada element i atribut del document XML d'entrada.
Separació de contingut i presentació: Permet separar l'estructura del contingut XML de la seva presentació, la qual cosa facilita la generació de diferents vistes de les mateixes dades.
**Potent i flexible:** Proporciona una àmplia gamma de funcions i capacitats per manipular i transformar documents XML de manera eficient.
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
### Exemple de taula amb xslt
```xsl
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:template match="/">
  <html>
  <head>
    <script src="https://kit.fontawesome.com/3e4c1a6931.js" crossorigin="anonymous"></script>
    <style>
      .c2005{
        background-color: #ff0202;
      }
      .c2007{
        background-color: #33FFFD;
      }
      .c2009{
        background-color: #02ff20;
      }
      .c2010{
        background-color: #ffd902;
      }
    </style>
  </head>
  <body>
    <h2>Exercici de la botiga</h2>
    <table border="1">
      <tr>
        <th>Titol</th>
        <th>Director</th>
        <th>Preu</th>
        <th>Any</th>
        <th>Idioma</th> 
      </tr>
      <xsl:for-each select="botiga/bluray">
        <tr class="c{any}">
          <td><a href="https://www.imdb.com/find?q={titol}"><xsl:value-of select="titol" /></a></td>
          <td><xsl:value-of select="director" /></td>
          <td><xsl:choose>
            <xsl:when test="preu &gt; 15">
              <i class="fa-solid fa-money-bills"></i>
            </xsl:when>
            <xsl:otherwise>
              <i class="fa-solid fa-money-bill"></i>
            </xsl:otherwise>
          </xsl:choose></td>
          <td><xsl:value-of select="any" /></td>
          <td><xsl:value-of select="titol/@idioma" /></td>
          <td><img width="30px" heigth="30px"><xsl:attribute name="src"><xsl:value-of select="titol/@idioma"/>.jpg</xsl:attribute></img></td>
        </tr>
      </xsl:for-each>
    </table>
  </body>
  </html>
</xsl:template>
</xsl:stylesheet>
```
### Així es com queda:
![botiga](https://github.com/PolFerret05/xml-python/assets/165801828/08dff8db-02a8-4251-a4b1-d7c07ce06ca4)

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
### Així es com queda:
![Captura de pantalla 2024-04-03 194828](https://github.com/PolFerret05/xml-python/assets/165801828/ed9ac62e-c0f1-4e32-ae1a-7b4d135307f9)


