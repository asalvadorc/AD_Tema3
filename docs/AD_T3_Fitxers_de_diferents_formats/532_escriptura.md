Accés a Dades

# 5.3.2 Escriptura

També farem 2 exemples d'escriptura, el d'Empleats i el de Bicicas, els
mateixos que en la pregunta 5.2.2

Cas 1: Empleats

Per a **Empleats.json** aquest és l'estructura que volem aconseguir:

{ "empresa":  
{ "empleat":  
[ {  
"num": "1",  
"nom": "Andreu",  
"departament": "10",  
"edat": "32",  
"sou": "1000.0"  
},  
{  
"num": "2",  
"nom": "Bernat",  
"departament": "20",  
"edat": "28",  
"sou": "1200.0"  
},  
{  
"num": "3",  
"nom": "Clàudia",  
"departament": "10",  
"edat": "26",  
"sou": "1100.0"  
},  
{  
"num": "4",  
"nom": "Damià",  
"departament": "10",  
"edat": "40",  
"sou": "1500.0"  
}  
]  
}  
}

L'estructura de les classes serà la mateixa que en el cas de la lectura, però
ara les dades estaran definides en 4 arrays. A partir dels arrays
inicialitzarem els objectes Kotlin, i quan ho tinguem tot ple passarem a JSON.

NO HEM DE DEFINIR LES CLASSES, ja que les tenim definides des punt 5.3.1. Les
posem ací únicament per a il·lustrar:

    
    
    class Empleat (var num: Int, var nom: String, var departament: Int, var edat: Int, var sou: Double)
    
    
    class Empleats(val empleats: List<Empleat> = listOf<Empleat>())
    
    
    class Empresa(val empresa: Empleats)

Aleshores, inicialitzem els objectes a partir de les dades que tenim en els 4
arrays, i després **guardem** en JSON. Mireu que la definició de les classes
de **Moshi** és la mateixa, però ara utilitzem el mètode **toJson()** per a
passar d'objectes Kotlin a JSON. Obtenim un String que és la cadena JSON, i
senzillament l'escrivim en un fitxer.

Copieu el següent codi en un fitxer Kotlin anomenat
**Exemple_3_5_23_MOSHIEscriureEmpleats.kt** :

    
    
    package exemples
    
    import com.squareup.moshi.*
    import java.io.File
    
    fun main (args: Array<String>){
        val noms = arrayOf("Andreu", "Bernat", "Clàudia", "Damià")
        val departaments = arrayOf(10, 20, 10, 10)
        val edats = arrayOf(32, 28, 26, 40)
        val sous = arrayOf(1000.0, 1200.0, 1100.0, 1500.0)
    
        val empleats = arrayListOf<Empleat>()
        for (i in 0..3)
            empleats.add(Empleat((i+1),noms[i],departaments[i],edats[i],sous[i]))
    
        val empresa = Empresa(Empleats(empleats))
    
        val moshi = Moshi.Builder().build()
        val adapter = moshi.adapter(Empresa::class.java)
        val json = adapter.toJson(empresa)
    
        File("Empleats2.json").writeText(json)
    
    }

Cas 2: Bicicas

Farem el mateix exercici que en l'apartat 5.2.2, però ara utilitzant la
llibreria **Moshi**.

Voldrem aconseguir aquesta estructura i guardar-la en el fitxer

{ "bicicas":  
[ {"num":"01","nom":"UJI - FCHS","llocs":28,"ocupats":11,"lliures":17},  
{"num":"02","nom":"ESTACIÓN DE FERROCARRIL Y
AUTOBUSES","llocs":28,"ocupats":8,"lliures":20},  
{"num":"03","nom":"PLAZA DE PESCADERÍA","llocs":28,"ocupats":13,"lliures":15},  
...  
]  
}

Tindrem per una banda l'estructura d'origen, per a la qual utilitzem les
classes ja definides en l'apartat 5.3.1, és a dir, per a llegir el fitxer
Bicicas.json original fem el mateix que l'altra vegada, per tant serà copiar
les primeres sentències del programa
**Exemple_3_5_22_MOSHIAnalitzarBicicas.kt** .

A partir dels objectes inicialitzats amb la lectura de Bicicas.json,
construirem els objectes que ens serviran per a l'escriptura. L'estructura
d'aquestes classes que ens serviran per a l'escriptura seran:

    
    
    class EstDesti (val num: Int, val nom: String, val llocs: Int, val ocupats: Int, val lliures: Int)
    
    class Bicicas2 (val bicicas: List<EstDesti>)

Per comoditat, com que només anem a utilitzar-les en aquesta ocasió, les
definirem en el mateix fitxer del programa.

Observeu com senzillament llegim passant a les classes d'entrada, convertim a
les classes d'eixida, i utilitzem aquestes últimes per escriure el fitxer.

Copieu el següent codi en el fitxer Kotlin anomenat
**Exemple_3_5_24_MOSHIEscriureBicicas.kt** :

    
    
    package exemples
    
    import com.squareup.moshi.*
    import java.io.File
    
    class EstDesti (val num: Int, val nom: String, val llocs: Int, val ocupats: Int, val lliures: Int)
    
    class Bicicas2 (val bicicas: List<EstDesti>)
    
    fun main(args: Array<String>) {
        val json = File("Bicicas.json").readText()
    
        val moshi = Moshi.Builder().build()
        val llistaTipus = Types.newParameterizedType(List::class.java, Estacions::class.java)
        val adapter: JsonAdapter<List<Estacions>> = moshi.adapter(llistaTipus)
        val estacions = adapter.fromJson(json).get(0).ocupacion
    
        val llistaEstDesti = arrayListOf<EstDesti>()
        val bicicas2 = Bicicas2(llistaEstDesti)
        for (e in estacions)
            llistaEstDesti.add(EstDesti(e.id.toInt(),e.punto,e.puestos,e.ocupados,e.puestos-e.ocupados))
    
        val adapter2 = moshi.adapter(Bicicas2::class.java)
        val json2 = adapter2.toJson(bicicas2)
    
        File("Bicicas2.json").writeText(json2)
    }

Segurament aquesta estructura és la més coherent. Amb la finalitat únicament
d'escriure un JSON que té com a arrel un array i no un objecte, anem a ferun
altre exemple en el qual guardarem en un fitxer aquesta estructura:

[  
{"num":"01","nom":"UJI - FCHS","llocs":28,"ocupats":11,"lliures":17},  
{"num":"02","nom":"ESTACIÓN DE FERROCARRIL Y
AUTOBUSES","llocs":28,"ocupats":8,"lliures":20},  
{"num":"03","nom":"PLAZA DE PESCADERÍA","llocs":28,"ocupats":13,"lliures":15},  
...  
]

En aquesta ocasió tenim només un array d'objectes.

L'única estructura d'eixida que ens cal és la classe **EstDesti** , ja
definida en l'exercici anterior, i per tant ara no caldrà definir-la. Només
hem d'anar amb compte amb l'estructura d'eixida, que ara no serà una classe
sinó una combinació de **List** i **EstDesti**.

Copieu el següent codi en el fitxer Kotlin
**Exemple_3_5_25_MOSHIEscriureBicicas3.kt** :

    
    
    package exemples
    
    import com.squareup.moshi.*
    import java.io.File
    
    fun main(args: Array<String>) {
        val json = File("Bicicas.json").readText()
    
        val moshi = Moshi.Builder().build()
        val llistaTipus = Types.newParameterizedType(List::class.java, Estacions::class.java)
        val adapter: JsonAdapter<List<Estacions>> = moshi.adapter(llistaTipus)
        val estacions = adapter.fromJson(json).get(0).ocupacion
    
        val llistaEstDesti = arrayListOf<EstDesti>()
        for (e in estacions)
            llistaEstDesti.add(EstDesti(e.id.toInt(),e.punto,e.puestos,e.ocupados,e.puestos-e.ocupados))
    
        val llistaTipusDesti = Types.newParameterizedType(List::class.java, EstDesti::class.java)
        val adapter2: JsonAdapter<List<EstDesti>> = moshi.adapter(llistaTipusDesti)
        val json2 = adapter2.toJson(llistaEstDesti)
    
        File("Bicicas3.json").writeText(json2)
    }


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

