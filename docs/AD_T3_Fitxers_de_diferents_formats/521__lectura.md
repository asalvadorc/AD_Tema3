Accés a Dades

# 5.2.1 - Lectura

JSONTokener

El primer que haurem de fer serà analitzar el document per a obtenir l'element
arrel (que com hem vist normalment serà un objecte, però de vegades pot ser un
array).

El constructor de **JSONTokener** admet un **String** , amb la qual cosa
analitzarem una cadena que posem directament, o també un **InputStream** o un
**Reader** , amb la qual cosa podrem analitzar directament el contingut d'un
fitxer

El mètode **nextValue()** de **JSONTokener** tornarà l'arrel del document
Json, i només haurem de fer un cast a l'ojecte que és l'arrel: un
**JSONObject** o un **JSONArray**

En el següent exemple agafem el json directament d'un **string** , on teníem
els números parells.

{ "p1" : 2 , "p2" : 4 , "p3" : 6 , "p4" : 8 , "p5" : 10 }

Observeu com per a la definició del String ens ha tocat escapar les dobles
cometes. Copieu el següent codi en el fitxer
Kotlin**Exemple_3_5_11_JSONAnalitzarCadena.kt**

    
    
    package exemples
    
    import org.json.JSONTokener
    import org.json.JSONObject
    
    fun main(args: Array<String>) {
    
        val cadena = "{ \"p1\" : 2 , \"p2\" : 4 , \"p3\" : 6 , \"p4\" : 8 , \"p5\" : 10 }"
    
        val arrel = JSONTokener(cadena).nextValue() as JSONObject
    
        println(arrel.get("p1"))
    }

Mentre que en aquest exemple accedim al fitxer **parelles.json** (que
assumirem que té el mateix contingut). Podríem passar el contingut del fitxer
a una cadena, però és molt més còmode passar el **Reader** com paràmetre en el
constructor, i ell s'encarrega de llegir-lo tot. Copieu el següent codi en el
fitxer **Exemple_3_5_12_JSONAnalitzarFitxer.kt** :

    
    
    package exemples
    
    import org.json.JSONTokener
    import org.json.JSONObject
    import java.io.FileReader
    
    fun main(args: Array<String>) {
    
        val r_json = FileReader("parelles.json")
    
        val arrel = JSONTokener(r_json).nextValue() as JSONObject
    
        println(arrel.get("p3"));
    }

JSONObject

Contindrà un **objecte json**. Recordeu que l'**objecte** es delimita amb les
claus: **{ }**

El mètode més important serà **get(_clau_)** al qual li passem la clau del
membre que volem obtenir. Si el membre és una parella clau-valor, obtindrem
directament el valor. Si és un altre objecte, doncs obtindrem l'objecte o
també podria ser un array. El cas que siga una parella clau-valor, ja l'hem
vist en l'exemple anterior.

En el següent exemple, en el fitxer **Empleat.json** tindrem un empleat

{ "empleat" :  
{ "num": 1 ,  
"nom": "Andreu" ,  
"departament": 10 ,  
"edat": 32 ,  
"sou": 1000.0  
}  
}

Observeu com ara de l'element arrel hem d'agafar **empleat** , que és un
objecte amb les parelles clau-valor **num** , **nom** , ....

Copieu el segúent codi en el fitxer Kotlin
**Exemple_3_5_13_JSONAnalitzarEmpleat.kt** :

    
    
    package exemples
    
    import org.json.JSONTokener
    import org.json.JSONObject
    import java.io.FileReader
    
    fun main(args: Array<String>) {
    
        val r_json = FileReader("Empleat.json")
    
        val arrel = JSONTokener(r_json).nextValue() as JSONObject
    
        val empleat = arrel.get("empleat") as JSONObject
    
        println("" + empleat.get("nom") + " (" + empleat.get("sou") + ")")
    }

També podem utilitzar els mètodes **getJSONObject()** , **getJSONArray** ,
**getInt()** , **getString()** , ..., que directament ens ho dóna en el tipus
especificat, i no cal fer cast.

JSONArray

Serà l'array, que recordem que ve limitat per els claudàtors: **[ ]**

Per a obtenir els elements de l'array utilitzarem el mètode **get(_index_)**,
on l'índex és el**número d'ordre** de l'element que volem obtenir.

També tindrem els mètodes **getJSONObject(_index_)**,
**getJSONArray(_index_)**, **getInt(_index_)**, ..., que tornen directament el
tipus especificat

Posem el primer exemple sobre un json que només té un array els elements del
qual són valors:

[ 5 , 7 , 8 , 7 ]

Observeu com ara l'arrel és un Array, i ens muntem un bucle per obtenir tots
els elements. Copieu el següent codi en el fitxer
**Exemple_3_5_14_JSONAnalitzarArray.kt**

    
    
    package exemples
    
    import org.json.JSONTokener
    import org.json.JSONArray
    
    fun main(args: Array<String>) {
    
        val cadena = "[ 5 , 7 , 8 , 7 ]"
    
        val arrel = JSONTokener(cadena).nextValue() as JSONArray
    
        for (i in arrel)
            println(i)
    }

Com veieu podem fer servir els bucles foreach, però lamentablement no podem
obtenir directament un JsonObject, i per tant l'hem de reconvertir. Ho
mostrarem en el següent exemple en el qual agafem tots els empleats de
l'empresa, on **Empresa.json** és així:

{ "empresa" :  
{ "empleats":  
[  
{ "num":1, "nom": "Andreu", "departament": 10, "edat": 32, "sou": 1000.0} ,  
{ "num":2, "nom": "Bernat", "departament": 20, "edat": 28, "sou": 1200.0} ,  
{ "num":3, "nom": "Clàudia", "departament": 10, "edat": 26, "sou": 1100.0} ,  
{ "num":4, "nom": "Damià", "departament": 10, "edat": 40, "sou": 1500.0}  
]  
}  
}

Tindrem un objecte arrel, amb només un objecte, **empresa** , que té un únic
membre empleat que és un array amb 4 elements, cadascun dels empleats. Copieu
el següent exemple en el fitxer kotlin
**Exemple_3_5_15_JSONAnalitzarEmpresa.kt**

    
    
    package exemples
    
    import org.json.JSONTokener
    import org.json.JSONObject
    import java.io.FileReader
    
    fun main(args: Array<String>) {
    
        val r_json = FileReader("Empresa.json")
    
        val arrel = JSONTokener(r_json).nextValue() as JSONObject
        val empresa = arrel.getJSONObject("empresa")
    
        for (e in empresa.getJSONArray("empleats")){
            val emp = e as JSONObject
            println("" + emp.get("nom") + " (" + emp.get("sou") + ")")
        }
    }

Mireu com ens hem estalviat un pas, i no hem definit la variable **empleats**
, sinó que ho especifiquem en el bucle. També podríem haver estalviat la
variable **empresa** si en el bucle haguérem posat :

    
    
    for (e in arrel.getJSONObject("empresa").getJSONArray("empleats")){

Exemple més complet

Mirem algun exemple ja més elaborat, on ens tocarà analitzar amb detall
l'estructura json. Fem-lo sobre l'exemple de **BICICAS**. Podeu tornar a fer
la consulta de l'estat actual en aquest moment a la pàgina
<http://gestiona.bicicas.es/apps/apps.php> , seleccionar-ho tot i guardar-lo
en el fitxer **Bicicas.json**. És molt possible que tinguem problemes amb els
caràcers especials, com les vocals accentuades, a causa de que el navegador
utilitzat no les reconega, i en copiar-les al fitxer no tinguem ja la
codificació correcta. No li donarem importància en aquest moment. Recordem ací
l'estructura:

[  
{"ocupacion":  
[ {"id":"01","punto":"UJI -
FCHS","puestos":28,"ocupados":11,"latitud":"39.99533","longitud":"-0.06999",  
"porcentajeAltaOcupacion":"80","porcentajeBajaOcupacion":"20"},  
{"id":"02","punto":"ESTACIÓN DE FERROCARRIL Y
AUTOBUSES","puestos":28,"ocupados":8,"latitud":"39.98765",  
"longitud":"-0.05281","porcentajeAltaOcupacion":"80","porcentajeBajaOcupacion":"20"},  
{"id":"03","punto":"PLAZA DE
PESCADERÍA","puestos":28,"ocupados":13,"latitud":"39.98580","longitud":"-0.03798",  
"porcentajeAltaOcupacion":"80","porcentajeBajaOcupacion":"20"},  
...  
]  
}  
]

Com podem observar, comença per un **array** , no per un objecte, com sol ser
habitual. L'únic que ens interessa és el **primer element** de l'array, ja que
en posteriors anirien en tot cas missatges. El primer element és un
**objecte** que té un únic membre **ocupacion** (o si en té més no ens
interessen), que és un **array** amb totes les estacions. Cada estació és un
objecte amb la informació que ens interessa.

Copieu el següent codi en el fitxer Kotlin
**Exemple_3_5_16_JSONAnalitzarBicicas.kt** :

    
    
    package exemples
    
    import org.json.JSONTokener
    import org.json.JSONObject
    import org.json.JSONArray
    import java.io.FileReader
    
    fun main(args: Array<String>) {
    
        val r_json = FileReader("Bicicas.json")
    
        val arrel = JSONTokener(r_json).nextValue() as JSONArray
    
        val estacions = arrel.getJSONObject(0).getJSONArray("ocupacion")
    
        for (e in estacions){
            val est = e as JSONObject
            println("" + e.get("id") + ".- " + e.get("punto") + " (" + e.get("ocupados") + "/" + e.get("puestos") + ")")
        }
    }

Hem agafat l'array, d'ell hem agafat el primer objecte i d'ell l'array. I hem
utilitzat un bucle **for** , per a tots els valors de l'array


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

