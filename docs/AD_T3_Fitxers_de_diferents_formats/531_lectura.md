Accés a Dades

# 5.3.1 Lectura

Com comentàvem en la pregunta anterior, la llibreria Moshi permet passar d'un
document JSON directament a objectes Java o Kotlin.

Ho mostrarem en dos exemples:

  * Primer per a poder llegir Empresa.json, en què l'element arrel és un objecte Json.
  * Després per a llegir Bicicas,json, en què l'element arrel és un array Json, cosa que ens complica un poc la lectura

Cas 1: l'arrel és un objecte

Tant en un com en l'altre primer haurem de crear les classes Java o Kotlin on
anirà la lectura del fitxer. Aquestes classes hauran de tenir la mateixa
estructura que en JSON, però adaptades a Java o Kotlin.

Anem per tant a analitzar **Empresa.json** , que ja el teníem creat i tenia
aquesta estructura:

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

També teníem creada la classe Empleat, des de la pregunta 3 d'aquest tema, i
tenia aquesta estructura. **Empleat.kt** :

    
    
    class Empleat (var num: Int, var nom: String, var departament: Int, var edat: Int, var sou: Double)

L'havíem fet **Serializable** , però això no importa en aquest exemple. El
podem deixar tal qual estava.

L'estuctura del fitxer JSON, ens marca que l'arrel és un **objecte** que té un
únic membre que es diu **empresa** , que és un **objecte** que té un únic
membre anomenat **empleats** , que és un **array** d'objectes (que
coincideixen ja amb la classe **Empleat**).

Per tant ens hem de construir una **classe** que que tinga una propietat
anomenada **empleats** , que siga un conjunt de **Empleat**. Ens vindrà bé un
**List**. Com que la utilitzarem en més d'un exemple, la guardem en el fitxer
de classe **Empleats.kt** ;

    
    
    class Empleats(val empleats: List<Empleat> = listOf<Empleat>())

I a partir d'aquesta hem de construir una altra classe que tinga una única
propietat anomenada **empresa** que siga de tipus **Empleats**. Com que la
utilitzarem en més d'un exemple, la guardem en el fitxer de classe
**Empresa.kt**

    
    
    class Empresa(val empresa: Empleats)

La lectura del fitxer la fem a partir d'un objecte **Moshi** al qual per mig
del mètode **adapter** definim quina classe volem llegir. Una vegada llegit ja
podem utilitzar els objectes Java o Kotlin.

Copieu el següent codi en el fitxer Kotlin
**Exemple_3_5_21_MOSHIAnalitzarEmpresa.kt** :

    
    
    package exemples
    
    import com.squareup.moshi.*
    import java.io.File
    
    fun main (args: Array<String>){
        val json = File("Empresa.json").readText()
    
        val moshi = Moshi.Builder().build()
        val adapter = moshi.adapter(Empresa::class.java)
        val empresa = adapter.fromJson(json)
    
        val llEmpleats= empresa!!.empresa.empleats
        println("Hi ha " + llEmpleats.size + " empleats:")
        for (e in llEmpleats)
            println(e.nom + " (" + e.sou  +")")
    }

Cas 2: l'arrel és un array

En cas que l'arrel del fitxer JSON siga un array, se'ns complica un poc,
perquè en el moment de definir quina classe volem llegir, no és en realitat
una classe, sinó un col·lecció (un List havíem vist que és el que
utilitzàvem).

Moshi utilitza una tècnica per a poder fer la lectura de l'array, que
consisteix a crear-se un tipus de dades que seria la concatenació d'un List i
de la classe de la qual volem l'array. Ho veurem molt millor en l'exemple.

L'exemple serà per a llegir el fitxer Bicicas.json, que recordem que començava
per un array:

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

Era un array d'un únic element , però en definitiva un array. Només ens
interessa el primer element, que serà un objecte amb un únic membre anomenat
**ocupacion** , que és un array on cada element és un objecte corresponent a
una estació, amb les seues característiques com a parelles clau-valor.

Definim per tant la classe **Estacio.kt** :

    
    
    class Estacio(
    	val id: Long, val punto: String, val puestos: Int,
    	val ocupados: Int, val latitud: Double, val longitud: Double,
    	val porcentajeAltaOcupacion: Int, val porcentajeBajaOcupacion: Int
    )

I la classe **Estacions.kt** ****:

    
    
    class Estacions(val ocupacion: List<Estacio> = listOf<Estacio>())

Copieu el següent codi en un fitxer Kotlin anomenat
**Exemple_3_5_22_MOSHIAnalitzarBicicas.kt** :

    
    
    package exemples
    
    import com.squareup.moshi.*
    import java.io.File
    
    fun main(args: Array<String>) {
        val json = File("Bicicas.json").readText()
    
        val moshi = Moshi.Builder().build()
        val llistaTipus = Types.newParameterizedType(List::class.java, Estacions::class.java)
        val adapter: JsonAdapter<List<Estacions>> = moshi.adapter(llistaTipus)
        val bicicas = adapter.fromJson(json)
    
        val estacions = bicicas!!.get(0).ocupacion
        println("Hi ha " + estacions.size + " estacions:")
        for (e in estacions)
            println("" + e.id + ": " + e.punto + " (" + e.ocupados + "/" + e.puestos + ")")
    }


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

