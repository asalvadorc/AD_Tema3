Accés a Dades

  
# 4.2.2 Escriptura

Anem ara a crear un nou document XML i a guardar-lo en un fitxer. Utilitzarem
com a exemple **Empleats**. Al final de tot convertirem el fitxer
**Empleats.obj** , generat en la pregunta 3, en el fitxer **Empleats.xml**.

La primera consideració a fer és que partirem d'un document buit. Anirem
construint els elements i posant els atributs, i quan tinguem un element creat
del tot, l'afegirem a l'estructura, és a dir farem que siga el fill d'un que
ja està en l'estructura. Podríem fer-ho també al revés, és a dir, primer
penjar-lo de l'estructura i després anar omplint-lo.

Els principals mètodes per anar construint l'estructura són:

**Mètodes de DOCUMENT**

Valor tornat | Mètode | Descripció  
---|---|---  
Element | **createElement(** String nom**)** | crea un nou element amb el nom indicat (s'haurà de penjar en l'estructura)  
Text | **createTextNode(** String dades**)** | crea un nou element de text (amb contingut)  
Node | **appendChild(** Node nou**)** | afegeix el node nou, que serà l'arrel  
  
**Mètodes de NODE**

Valor tornat | Mètode | Descripció  
---|---|---  
Node | **appendChild(** Node nou**)** | afegeix el node nou com a l'últim fill fins el moment  
void | **removeChild(** Node vell**)** | lleva el node vell com a fill  
  
**Mètodes de ELEMENT**

Valor tornat | Mètode | Descripció  
---|---|---  
void | **setAttribute(** String nom,String valor**)** | afegeix un nou atribut a l'element, amb el nom i valor indicats  
void | **removeAttribute(** String nom**)** | lleva l'atribut de l'element  
void | **setTextContent(** String dades**)** | afegeix un fill que serà de text, amb el contingut passat com a paràmetre  
  
Anem a fer directament ja l'exemple dels empleats. Totes les dades seran
elements, excepte el número d'empleat, que farem que siga un atribut d'empleat
per a practicar. A l'element arrel li direm **empleats.** El resultat ha de
ser el fitxer **Empleats.xml**. Copieu el següent codi en el fitxer Kotlin
**Exemple_3_4_4_XMLCrearEmpleats.kt** :

    
    
    import java.io.ObjectInputStream
    import java.io.FileInputStream
    import javax.xml.parsers.DocumentBuilderFactory
    import java.io.EOFException
    import javax.xml.transform.TransformerFactory
    import javax.xml.transform.dom.DOMSource
    import javax.xml.transform.stream.StreamResult
    import javax.xml.transform.OutputKeys
    
    fun main(args: Array<String>) {
    	val f = ObjectInputStream(FileInputStream ("Empleats.obj"))
    
    	val doc = DocumentBuilderFactory.newInstance ().newDocumentBuilder().newDocument()
    	val arrel = doc.createElement ("empleats")
    	doc.appendChild(arrel)
    
    	try {
    		while (true) {
    			val e = f.readObject () as Empleat
    			val emp = doc.createElement ("empleat")
    			emp.setAttribute("numero", Integer.toString(e.num))
    
    			val nom = doc.createElement ("nom")
    			nom.appendChild(doc.createTextNode(e.nom)) // forma llarga: afegim un fill que és un node de text
    			emp.appendChild(nom)
    
    			val dep = doc.createElement("departament")
    			dep.setTextContent(e.departament.toString()) // forma curta: amb setTextContent() li posem contingut
    			emp.appendChild(dep)
    
    			val edat = doc.createElement("edat")
    			edat.setTextContent(e.edat.toString())
    			emp.appendChild(edat)
    
    			val sou = doc.createElement("sou");
    			sou.setTextContent(e.sou.toString())
    			emp.appendChild(sou)
    
    			arrel.appendChild(emp)
    		}
    
    	} catch (eof: EOFException) {
    		f.close();
    	}
    	val trans = TransformerFactory.newInstance().newTransformer()
    
    	trans.transform(DOMSource(doc), StreamResult("Empleats.xml"))
    }

En el fitxer XML generat, observareu que no hi ha retorns de carro, tot està
en una mateixa línia. Si voleu veure'l bé, el podeu obrir per exemple amb un
navegador web, que interpreta bé el format XML.

Però també podem crear el fitxer donant-li un aspecte agradable incorporant
aquestes dues línies immediatament abans de crear el fitxer, és a dir,
immediatament abans de la línia 49:

    
    
        trans.setOutputProperty(OutputKeys.INDENT, "yes")
        trans.setOutputProperty("{http://xml.apache.org/xslt}indent-amount", "2")
    

I el fitxer quedarà així:

    
    
    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <empleats>
      <empleat numero="1">
        <nom>Andreu</nom>
        <departament>10</departament>
        <edat>32</edat>
        <sou>1000.0</sou>
      </empleat>
      <empleat numero="2">
        <nom>Bernat</nom>
        <departament>20</departament>
        <edat>28</edat>
        <sou>1200.0</sou>
      </empleat>
      <empleat numero="3">
        <nom>Clàudia</nom>
        <departament>10</departament>
        <edat>26</edat>
        <sou>1100.0</sou>
      </empleat>
      <empleat numero="4">
        <nom>Damià</nom>
        <departament>10</departament>
        <edat>40</edat>
        <sou>1500.0</sou>
      </empleat>
    </empleats>


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

