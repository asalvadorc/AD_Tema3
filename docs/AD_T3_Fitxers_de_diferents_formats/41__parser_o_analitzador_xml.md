Accés a Dades

# 4.1 - Parser o analitzador XML

Un **_Parser_ XML** és una classe que ens permet analitzar i classificar el
contingut d’un arxiu XML extraient la informació continguda en cada una de les
etiquetes, i relacionar-la d’acord amb la seua posició dins la jerarquia.

Hi ha dos tipus d'analitzadors depenent de la manera de funcionar.

Analitzadors seqüencials

Els **analitzadors seqüencials** permeten extreure el contingut a mida que es
van descobrint les etiquetes d’obertura i tancament. També s’anomenen
**analitzadors sintàctics**. Són analitzadors molt ràpids, però presenten el
problema que cada vegada que es necessita accedir a una part del contingut,
s'ha de rellegir tot el document de dalt a baix.

En Java, l’analitzador sintàctic més popular s’anomena **SAX** , que vol dir
**Simple API for XML**. És una analitzador molt utilitzat en diverses
biblioteques de tractament de dades XML, però no sol utilitzar-se en
aplicacions finals, pel problema abans comentat d'haver de llegir-se tot el
document XML a cada consulta. Per aquesta raó no els veurem en aquest curs.

Analitzadors jeràrquics

Generalment, les aplicacions finals que han de treballar amb dades XML solen
utilitzar analitzadors jeràrquics.

Els **analitzadors jeràrquics** guarden totes les dades del document XML en
memòria dins una estructura jeràrquica, a mida que van analitzant el seu
contingut. I per això són ideals per a aplicacions que requereixen una
consulta contínua de les dades.

El**format de l’estructura** on es guarda la informació en memòria ha estat
especificat per l’organisme internacional W3C (World Wide Web Consortium) i es
coneix com a **DOM** (_**Document Object Model**_). És una estructura que HTML
i javascript han popularitzat molt i es tracta d’una especificació que Java
materialitza en forma d’interfícies. La principal s’anomena **_Document_** i
representa tot un document XML. En tractar-se d’una interfície, pot ser
implementada per diverses classes.

L’estàndard W3C defineix la classe **DocumentBuilder** (constructor de
documents) per a poder crear estructures DOM a partir d’un XML. Aquesta classe
**DocumentBuilder** és una classe abstracta, i per tal que es puga adaptar a
les diferents plataformes, pot necessitar fonts de dades o requeriments
diversos. Recordeu que les classes abstractes no es poden instanciar de forma
directa. Per aquest motiu, el consorci W3 especifica també la classe
**DocumentBuilderFactory** , és a dir, el  _fabricador_ de  _DocumentBuilder_.

Les llibreries des d'on importarem les classes comentades són:

  * **DocumentBuilderFactory** i **DocumentBuilder** les importarem de la llibreria **javax.xml.parsers.***
  * **Document** l'importarem de **org.w3c.dom.***

Hem de cuidar sobretot aquesta última importació, perquè per defecte Java ens
ofereix moltes llibreries des d'on importar **Document**. I si no la importem
de la llibreria correcta, evidentment després tindrem errors.

Les instruccions necessàries per llegir un fitxer XML i crear un objecte
**Document** serien les següents:

    
    
    import java.io.FileInputStream
    
    import javax.xml.parsers.DocumentBuilder
    import javax.xml.parsers.DocumentBuilderFactory
    
    import org.w3c.dom.Document
    
    ...
    
        val dbFactory = DocumentBuilderFactory.newInstance()
    
        val dBuilder = dbFactory.newDocumentBuilder()
    
        val doc = dBuilder.parse(File(“fitxer.xml”))

Tornem a insistir en la necessitat d'importar **Document** de la llibreria
**org.w3c.dom.***

Anem a basar-nos en un exemple per poder veure a poc a poc la manera
d'utilitzar el parser. És un exemple que possiblement es va veure en primer,
en el mòdul de Llenguatge de Marques. Suposarem que està en el fitxer
**cotxes.xml** , i que està en el directori del projecte on farem les proves.

<?xml version="1.0" encoding="UTF-8"?>

<oferta>

<vehiculo>

<marca>ford</marca>

<modelo color="gris">focus</modelo>

<motor combustible="gasolina">duratorc 1.4</motor>

<matricula>1234AAA</matricula>

<kilometros>12500</kilometros>

<precio_inicial>12000</precio_inicial>

<precio_oferta>10000</precio_oferta>

<extra valor="250">pintura metalizada</extra>

<extra valor="300">llantas</extra>

<foto>11325.jpg</foto>

<foto>11326.jpg</foto>

</vehiculo>

<vehiculo>

<marca>ford</marca>

<modelo color="gris">focus</modelo>

<motor combustible="diesel">duratorc 2.0</motor>

<matricula>1235AAA</matricula>

<kilometros>125000</kilometros>

<precio_inicial>10000</precio_inicial>

<precio_oferta>9000</precio_oferta>

<extra valor="250">pintura metalizada</extra>

<extra valor="200">spoiler trasero</extra>

<extra valor="500">climatizador</extra>

<foto>11327.jpg</foto>

<foto>11328.jpg</foto>

</vehiculo>

</oferta>

El primer que farem serà intentar connectar amb aquest fitxer, però d'una
forma un poc més reduïda que abans, sense definir objectes del
**DocumentBuilderFactory** ni **DocumentBuilder**. Tampoc ens caldrà definir-
nos el **File** (**FileInputStream**) ja que el mètode **parse** també agafa
un **String** com a paràmetre:

    
    
    import javax.xml.parsers.DocumentBuilderFactory
    
    import org.w3c.dom.Document
    
    ...
    
        val doc = DocumentBuilderFactory.newInstance().newDocumentBuilder().parse("cotxes.xml")

Però, i si el procés que necessitem és l'invers? És a dir, i si el que volem
és guardar una estructura DOM en un fitxer XML?

En aquest cas el que haurem de fer serà construir un **document buit** , anar
posant els elements i atributs (amb els seus valors) d'alguna manera, i
posteriorment guardar-lo en un fitxer. Deixem per a un poc més avant com anar
construint els nodes del document i centrem-nos en el fet de crear el document
buit i guardar-lo en un fitxer. Podem construir un document nou a partir del
**DocumentBuilder** , utilitzant el mètode **newDocument()** :

    
    
    import javax.xml.parsers.DocumentBuilderFactory
    
    import org.w3c.dom.Document
    
    ...
    
        val doc1 = DocumentBuilderFactory.newInstance().newDocumentBuilder().newDocument()

Per a escriure la informació continguda al DOM a un fitxer, es pot fer
utilitzant una altra utilitat de Java anomenada **_Transformer_**. Es tracta
d’una utilitat que permet realitzar fàcilment conversions entre diferents
representacions d’informació jeràrquica. És capaç, per exemple, de passar la
informació continguda en un objecte **Document a un fitxer de text en format
XML**. També seria capaç de fer l’operació inversa, però no val la pena perquè
el mateix _DocumentBuilder_ ja s’encarrega d’això.

**_Transformer_** és també una classe abstracta i requereix d’una _factory_
per poder ser instanciada. La classe **Transformer** pot treballar amb
multitud de contenidors d’informació perquè en realitat treballa amb un parell
de tipus adaptadors (classes que fan compatibles jerarquies diferents) que
s’anomenen **Source** i **Result**. Les classes que implementen aquestes
interfícies s’encarregaran de fer compatible un tipus de contenidor específic
al requeriment de la classe **Transformer**. Així, disposem de les classes
**DOMSource** , **SAXSource** o **StreamSource** com a adaptadors del
contenidor de la font d’informació (DOM, SAX o Stream respectivament).
**DOMResult** , **SAXResult** o **StreamResult** són els adaptadors
equivalents del contenidor destí. A nosaltres ara, com que volem passar un
document DOM a un fitxer, ens convindrà un **DOMSource** i un **StreamResult**

El codi bàsic per realitzar una transformació de DOM a fitxer de text XML
seria el següent:

    
    
    val trans = TransformerFactory.newInstance().newTransformer()
    
    val source = DOMSource(doc)
    val result = StreamResult(file)
    
    trans.transform(source, result)

De tota manera, veurem més avant un exemple on ens guardarem una estructura
DOM en un fitxer XML.

Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

