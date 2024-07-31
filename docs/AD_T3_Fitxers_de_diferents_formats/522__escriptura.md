Accés a Dades

# 5.2.2 - Escriptura

Ens falta veure com escriure un document JSON. És molt fàcil anar creant
l'estructura JSON. Només hem de recordar la manera d'afegir al **JSONObject**
i al **JSONArray**.

  * **put("nom", element)** per afegir un nou membre a un **JSONObject**
  * **put(element)** per afegir un nou element a un **JSONArray**

_**Nota**_

La major part de drivers utilitzen **add(element)** per a afegir un element a
un JSONArray, en compte de **put******

Una vegada tinguem l'estructura, podrem passar-la a un **string** amb el
mètode **toString** , i la podrem guardar directament en un fitxer, per
exemple.

Farem més d'un exemple, per poder practicar.

En el primer exemple generarem un JSON a partir de dades definides en el
mateix programa, per mig de vectors. Intentarem generar aquest fitxer JSON:

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

Analitzem l'estructura. Tenim un objecte arrel, que consta d'un únic membre,
empresa, que és un objecte. Aquest objecte té un únic membre que és un array.
Cada element de l'array és un objecte, i els seus membres ja són clau-valor.

Copieu el següent codi en el fitxer kotlin
**Exemple_3_5_17_JSONEscriureEmpleats.kt** :

    
    
    import org.json.JSONObject
    import org.json.JSONArray
    import java.io.FileWriter
    
    fun main(args: Array<String>) {
    	val noms = arrayOf("Andreu", "Bernat", "Clàudia", "Damià")
    	val departaments = arrayOf( 10, 20, 10, 10 )
    	val edats = arrayOf( 32, 28, 26, 40 )
    	val sous = arrayOf( 1000.0, 1200.0, 1100.0, 1500.0)
    
    	val arrel = JSONObject()
    	val empresa = JSONObject()
    	arrel.put("empresa", empresa)
    	val empleats = JSONArray()
    	empresa.put("empleat", empleats)
    
    	for (i in 0..3){
    		val emp = JSONObject()
    		emp.put("num", i + 1)
    		emp.put("nom", noms[i])
    		emp.put("departament", departaments[i])
    		emp.put("edat", edats[i])
    		emp.put("sou", sous[i])
    		empleats.put(emp)
    	}
    
    	val f = FileWriter("Empleats.json")
    	f.write(arrel.toString(4))
    	f.close()
    }

Hem posat en el moment de passar el JSONObject a String el paràmetre 4, per a
que idente en diferents linies a 4 espais. Com veureu el resultat és quasi
perfecte

    
    
    {"empresa": {"empleat": [
        {
            "num": 1,
            "sou": 1000,
            "nom": "Andreu",
            "edat": 32,
            "departament": 10
        },
        {
            "num": 2,
            "sou": 1200,
            "nom": "Bernat",
            "edat": 28,
            "departament": 20
        },
        {
            "num": 3,
            "sou": 1100,
            "nom": "Clàudia",
            "edat": 26,
            "departament": 10
        },
        {
            "num": 4,
            "sou": 1500,
            "nom": "Damià",
            "edat": 40,
            "departament": 10
        }
    ]}}

Tenim el problema aparent que els membres de cada empleat estan
**desordenats**. No és que haja eixit malament, perquè hem de recordar que un
objecte JSON és un conjunt no ordenat de membres.

Exemple BICICAS

Un altre exemple, el de **Bicicas** , i el que farem per a practicar tant la
lectura com l'escriptura, serà donar-li una altra estructura. Per tant
llegirem l'original, i anirem construint la següent estructura en què està la
informació més resumida:

{ "bicicas":  
[ {"num":"01","nom":"UJI - FCHS","llocs":28,"ocupats":11,"lliures":17},  
{"num":"02","nom":"ESTACIÓN DE FERROCARRIL Y
AUTOBUSES","llocs":28,"ocupats":8,"lliures":20},  
{"num":"03","nom":"PLAZA DE PESCADERÍA","llocs":28,"ocupats":13,"lliures":15},  
...  
]  
}

Tindrem per una banda les estacions d'origen, i per una altra les de destí .

Copieu el següent codi en el fitxer Kotlin
**Exemple_3_5_18_JSONEscriureBicicas.kt** :

    
    
    import org.json.JSONTokener
    import org.json.JSONObject
    import org.json.JSONArray
    import java.io.FileReader
    import java.io.FileWriter
    
    fun main(args: Array<String>) {
    
    	val r_json = FileReader("Bicicas.json")
    	val arrel = JSONTokener(r_json).nextValue() as JSONArray
    	val estacions = arrel.getJSONObject(0).getJSONArray("ocupacion")
    
    	val destEstacions = JSONArray()	
    
    	for (e in estacions){
    		e as JSONObject
    		val destE = JSONObject();
    		destE.put("num", e.get("id"));
    		destE.put("nom", e.get("punto"));
    		destE.put("llocs", e.get("puestos"));
    		destE.put("ocupats", e.get("ocupados"));
    		val lliures = e.get("puestos") as Int - e.get("ocupados") as Int
    		destE.put("lliures", lliures)
    		destEstacions.put(destE)
    	}
    
    	val bicicas = JSONObject()
    	bicicas.put("bicicas", destEstacions)
    
    	val w_json = FileWriter("Bicicas2.json");
    	w_json.write(bicicas.toString(4));
    	w_json.close();
    }

El resultat seria aquest (observeu que havíem posat una tabulació de 4):

    
    
    {"bicicas": [
        {
            "num": "01",
            "ocupats": 23,
            "llocs": 28,
            "nom": "UJI - FCHS",
            "lliures": 5
        },
        {
            "num": "02",
            "ocupats": 0,
            "llocs": 28,
            "nom": "ESTACIÃ³N DE FERROCARRIL Y AUTOBUSES",
            "lliures": 28
        },
    ...
    ]}


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

