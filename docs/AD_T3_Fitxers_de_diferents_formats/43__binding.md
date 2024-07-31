Accés a Dades

# 4.3 - Binding

# VOLUNTARI

El **_Binding_** és una tècnica que consisteix en generar (i vincular)
**automàticament** classes de Java amb formats específics d’emmagatzematge,
que en el cas que ens interessa seran documents XML.

D'aquesta manera, cada etiqueta o atribut de XML es correspondrà amb una
propietat d'una determinada classe. Això s'anomena _**mapar**_ , perquè és fer
una espècie de mapa per a indicar que una etiqueta (o atribut) es correspon a
una propietat de la classe. Per a poder fer la aquesta correspondència o
_**mapatge**_ de forma correcta entre les classes de Java i el document XML,
farà falta una miqueta d'ajuda o aclaracions, perquè com ja hem vist la
mateixa informació es pot guardar de més d'una manera en XML (utilitzant o no
atributs, ...).

En Java existeixen diverses biblioteques per gestionar el _binding_ , com per
exemple _JAXB_ , _JiBX_ , _XMLBinding_ , etc. Des de la versió 6.0 s’ha
incorporat en el JDK estàndard **JAXB** (_**Java Architecture for XML
Binding**_), una potent biblioteca.

**JAXB** utilitza **_Anotacions_** per aconseguir la informació extra
necessària per _mapar_ el _binding_. Les _Anotacions_ són unes classes de Java
molt especials. Serveixen per associar informació i funcionalitat als objectes
sense interferir en l’estructura del model de dades. Abans d'aparéixer les
_Anotacions_ era necessari fer servir l’herència per poder afegir
funcionalitat a una classe sense haver de codificar-la, però ho complicava
molt.

Si, per contra, fem servir _Anotacions_ , els objectes disposaran d’informació
o de funcionalitat extra sense que el model de dades quede modificat, ja que
les _Anotacions_ no són visibles des dels objectes. Les _Anotacions_ poden
associar-se a un paquet, a una classe, a un atribut o fins i tot a un
paràmetre. Aquestes classes especials es declaren en el codi de l’aplicació
posant el símbol **@** davant del nom de l’ _Anotació_. Quan el compilador de
Java detecta una _Anotació_ crea una instància i la injecta dins l’element
afectat (paquet, classe, mètode, atribut, etc.). Això fa que aquestes no
apareguen com a atributs o mètodes propis de l’objecte, i per això diem que no
interacciona amb el model de dades, però les aplicacions que ho necessiten
poden obtenir la instància injectada i fer-la servir.

Nosaltres en aquest curs, únicament aspirarem a veure com realment a partir
d'un **esquema XML** que valida una sèrie de documents XML, podem generar
còmodament les classes en Java, que permetrien guardar objectes d'aquestes
classes. Una vegada construïdes les classes es podria utilitzar JAXB per a
transferir informació dels fitxers XML (vàlids per a l'esquema) cap als
objectes o a l'inrevès.

La generació de les classes és extremadament senzilla a partir d'una
determinada versió d'Eclipse. Com hem comentat la llibreria JAXB està present
des de la versió 6.0 del JDK SE (Standard Edition). Però per a la generació
còmoda ens fan falta eines (tools) proporcionades en la JDK EE 7.0 (Enterprise
Edition). Si tenim aquesta plataforma podrem utilitzar la versió 2.2 de JAXB.

**_Nota_**

Lamentablement, a partir de la versió de Java JDK EE 8.0 comença a estar en
desús el JAXB (_deprecated_), i a la versió 11 ja no està implementat. Per
tant, per a poder provar el que va a continuació **és preferible que utilitzeu
una versió antiga de Java** (Java 7 o Java 8). Si no la teniu instal·lada, no
valdrà la pena que feu aquest exemple. **Per això aquesta pregunta és
voluntària**.

Evidentment, es pot aconseguir que funcione en Java 11, a pesar que no està
implementat JAXB, però és a costa d'assenyalar que agafe configuracions
anteriors, o important tots els jar necessaris, baixant-los prèviament. No
valdrà la pena, perquè no l'utilitzarem

Ens recolzarem en un exemple utilitzat l'any passat. El següent esquema (.xsd)
valida documents com el de l'oferta de vehicles, vist en apartats anteriors.
Podeu guardar-lo amb el nom **cotxes.xsd**

    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
     
    <xs:simpleType name="Combust">
      <xs:restriction base="xs:string">
          <xs:enumeration value="gasolina"/>
          <xs:enumeration value="diesel"/>
      </xs:restriction>
    </xs:simpleType>
     
    <xs:simpleType name="Matr">
      <xs:restriction base="xs:string">
          <xs:pattern value="[0-9]{4}[A-Z]{3}"/>  
      </xs:restriction>
    </xs:simpleType>
     
    <xs:element name="oferta">
      <xs:complexType>
        <xs:sequence>
            <xs:element name="vehiculo" maxOccurs="unbounded" >
                <xs:complexType>
                    <xs:sequence>
                        <xs:element name="marca" type="xs:string"/>
                        <xs:element name="modelo" >
                            <xs:complexType>
                                <xs:simpleContent>
                                    <xs:extension base="xs:string">
                                        <xs:attribute name="color" type="xs:string" />
                                    </xs:extension>
                                </xs:simpleContent>
                            </xs:complexType>  
                        </xs:element>  
                        <xs:element name="motor" >
                            <xs:complexType>
                                <xs:simpleContent>
                                    <xs:extension base="xs:string">
                                        <xs:attribute name="combustible" type="Combust" />
                                    </xs:extension>
                                </xs:simpleContent>
                            </xs:complexType>   
                        </xs:element>
                        <xs:element name="matricula" type="Matr"/>
                        <xs:element name="kilometros" type="xs:integer"/>
                        <xs:element name="precio_inicial"  type="xs:integer"/>
                        <xs:element name="precio_oferta" type="xs:integer"/>
                        <xs:element name="extra" maxOccurs="unbounded" >
                            <xs:complexType>
                                <xs:simpleContent>
                                    <xs:extension base="xs:string">
                                        <xs:attribute name="valor" type="xs:integer" />
                                    </xs:extension>
                                </xs:simpleContent>
                            </xs:complexType>  
                        </xs:element>   
                        <xs:element name="foto" maxOccurs="unbounded" type="xs:string"/>     
                    </xs:sequence>
                </xs:complexType>
            </xs:element>        
        </xs:sequence>    
      </xs:complexType>
    </xs:element>
     
    </xs:schema>

Per exemple, el següent document XML **cotxes.xml** (el vist en els apartats
anteriors) és vàlid segons l'anterior esquema.

    
    
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

Però per a generar les classes el que ens fa falta és el de l'esquema,
**cotxes.xsd**.

En el següent vídeo es veu com generar les classes a partir de l'esquema.**No
cal que ho feu si no voleu, perquè es mostra a nivell únicament il·lustratiu
de com es generen les classes, però no ho utilitzarem després**

I aquest és el programa per provar que es pot accedir que es comenta en el
vídeo:

    
    
    package classesJAXB
    
    import java.io.File
    import javax.xml.bind.JAXBContext
    import javax.xml.bind.JAXBException
    
    fun main() {
        try {
            val file = File("cotxes.xml")
            val jaxbContext = JAXBContext.newInstance(Oferta::class.java)
            val jaxbUnmarshaller = jaxbContext.createUnmarshaller()
            val oferta = jaxbUnmarshaller.unmarshal(file) as Oferta
            for (v in oferta.getVehiculo())
                println(v.getMatricula() + " (" + v.getMarca() + ")")
        } catch (e: JAXBException) {
            e.printStackTrace()
        }
    }


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

