Accés a Dades

 
# 4.2.1 Lectura

Anem a fer proves per poder comprovar el funcionament. Ens basem en el
document **cotxes.xml** esmentat en la pregunta 4.1

**Nota**

Aneu amb compte, perquè en el document **cotxes.xml** davant de la primera
etiqueta no pot haver ni retorn de carro ni un espai en blanc ni res

Coieu el següent codi en un fitxer Kotlin anomenat
**Exemple_3_4_1_XMLMirar.kt**

    
    
    package exemples
    
    import javax.xml.parsers.DocumentBuilderFactory
    
    fun main(args: Array<String>){
    
        val doc = DocumentBuilderFactory.newInstance().newDocumentBuilder().parse("cotxes.xml")
        println(doc.getNodeName())     			// torna el nom del document. No és l'element arrel. Ens dirà #document
        val arrel = doc.getDocumentElement()	// apuntarà a l'element arrel
        println(arrel.getNodeName())			// torna el nom de l'element. Ens dirà oferta
        println(arrel.getNodeValue())			// torna el valor de l'element. Com és un element que conté altres elements, el valor és null
    }

Tal i com està documentat, aquesta serà l'eixida:

#document  
oferta  
null

Anem a comprovar ara que el primer fill de **oferta** no és **vehiculo** sinó
el retorn de carro. Els element **vehiculo** són el segon i el quart (índex 1
i 3). Copieu el següent codi enun fitxer Kotlin anomenat
**Exemple_3_4_2_XMLMirar2.kt******

    
    
    package exemples
    
    import javax.xml.parsers.DocumentBuilderFactory
    
    fun main(args: Array<String>) {
    
        val doc = DocumentBuilderFactory.newInstance().newDocumentBuilder().parse("cotxes.xml")
        val arrel = doc.getDocumentElement()  // apuntarà a l'element arrel
        val fills = arrel.getChildNodes()
        println(fills.item(0).getNodeName())   // el primer fill és el retorn de carro; ens dirà que és de text
        println(fills.item(1).getNodeName())   // el segon fill sí que és vehiculo
        println(fills.item(2).getNodeName())   // el tercer fill és el retorn de carro; ens dirà que és de text
        println(fills.item(3).getNodeName())   // el quart fill sí que és vehiculo
        println(fills.item(4).getNodeName())   // el cinquè fill és el retorn de carro; ens dirà que és de text
        println(fills.item(5).getNodeName())   // no existeix el sisè fill. Donarà error
    }

Observeu que en l'última sentència estem provocant un error:

#text  
vehiculo  
#text  
vehiculo  
#text  
Exception in thread "main" java.lang.NullPointerException  
at exemples.Exemple_4_2_MirarXML2.main(Exemple_4_2_MirarXML2.kt:15)

Per tant, hem d'anar molt en compte amb els _**retorns de carro**_.

  * Per a poder esquivar els _retorn de carro_ podríem mirar el tipus de cada node ( **getNodeType()** ), menysprear els els de tipus  _**TEXT_NODE**_ i considerar només els de tipus  _**ELEMENT_NODE**_.

  * Però normalment l'accés que farem serà un poc més directe i més fàcil. Agafarem la llista de tots els element que tinguen un determinat nom amb **getElementsByTagName(_nom_)** .**** Evidentment en la llista no estaran els retorns de carro**** i així no tindrem problemes amb ells.

En el següent exemple recorrerem tots els element **vehiculo**. De cadascun
agafarem el contingut dels elements **marca** i **matricula**. També agafem el
contingut de l'atribut **combustible** de l'element **motor**. Guardeu el
següent codi en un fitxer Kotlin anomenat **Exemple_3_4_3_XMLMirarCotxes.kt**
:

    
    
    package exemples
    
    import javax.xml.parsers.DocumentBuilderFactory
    import org.w3c.dom.Element
    
    fun main(args: Array<String>) {
    
        val doc = DocumentBuilderFactory.newInstance().newDocumentBuilder().parse("cotxes.xml")
        val arrel = doc.getDocumentElement()  // apuntarà a l'element arrel
        val llista = arrel.getElementsByTagName("vehiculo")
    
        for (i in 0 until llista.getLength()) {
            val el = llista.item(i) as Element
            println(el.getNodeName() + " " + (i + 1))
            println("Marca: " + el.getElementsByTagName("marca").item(0).getChildNodes().item(0).getNodeValue())
            println("Matrícula: " + el.getElementsByTagName("matricula").item(0).getFirstChild().getNodeValue())
            println("Motor: " + el.getElementsByTagName("motor").item(0).getTextContent())
            println("Combustible: " + el.getElementsByTagName("motor").item(0).getAttributes().item(0).getNodeValue())
            val m = el . getElementsByTagName ("motor").item(0) as Element
            println("Combustible: " + m.getAttribute("combustible"))
            println()
        }
        println(arrel.getTextContent())
    }

És molt important observar que quan tenim un element que ja té contingut, la
informació no és accessible, sinó que hem d'anar al primer fill, que aquest ja
és de tips  _**TEXT_NODE**_ , per agafar el seu valor.

En l'exemple:

  * Per a **marca** hem agafat de tota la llista de fills el primer, per traure el seu valor.
  * En **matricula** en compte d'agafar tota la llista de fills, només hem agafat el primer, i per tant és més ràpid.
  * I per a **motor** utilitzem el mètode **getTextContent** , que agafa el congingut de text de l'element i de tots els seus descendents. Com és un node de text ja sabem a priori que ens anirà bé, i per tant **és la forma més ràpida**.

L'atribut **combustible** de l'element **motor** l'hem tret de 2 maneres:

  * La primera agafant la llista d'atributs, i després el primer d'aquesta llista.
  * En la segona manera s'ha fet més elegant, anat a buscar la propietat en qüestió. Per això hem convertit el node en l'**element** **m** , per a poder utilitzar **getAttribute**.

Al final fem el **getTextContent()** sobre l'arrel per a comprovar que trau el
seu contingut i el de tots els seus fills, per això apareix la informació
duplicada

Aquest serà el resultat de l'exemple anterior:

vehiculo 1  
Marca: ford  
Matrícula: 1234AAA  
Motor: duratorc 1.4  
Combustible: gasolina  
Combustible: gasolina  
  
vehiculo 2  
Marca: ford  
Matrícula: 1235AAA  
Motor: duratorc 2.0  
Combustible: diesel  
Combustible: diesel  
  
  
  
ford  
focus  
duratorc 1.4  
1234AAA  
12500  
12000  
10000  
pintura metalizada  
llantas  
11325.jpg  
11326.jpg  
  
  
ford  
focus  
duratorc 2.0  
1235AAA  
125000  
10000  
9000  
pintura metalizada  
spoiler trasero  
climatizador  
11327.jpg  
11328.jpg  
  
  

Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

