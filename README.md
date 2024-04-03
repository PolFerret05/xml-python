# Apunts sobre minidom i XSLT
## minidom a Python
El llenguatge minidom és un mòdul a Python que permet la manipulació de documents XML d'una manera senzilla i eficient. Algunes de les seves característiques i funcionalitats clau són les següents:

Biblioteca estàndard: minidom és part de la biblioteca estàndard de Python, la qual cosa significa que no és necessari instal·lar biblioteques addicionals per utilitzar-lo.
Facilitat d'ús: Proporciona una interfície intuïtiva i fàcil d'utilitzar per crear, modificar i analitzar documents XML.
DOM (Model d'Objecte de Document): Utilitza el model d'objecte de document per representar l'estructura del document XML com un arbre de nodes, la qual cosa facilita la navegació i la manipulació del document.
### Exemple de creació d'un document XML amb minidom
```python
import xml.dom.minidom
doc = xml.dom.minidom.Document()
root = doc.createElement("root")
doc.appendChild(root)
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
