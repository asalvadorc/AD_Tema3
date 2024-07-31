Accés a Dades

# 2.- Accés directe a fitxers

Els exemples de l'apartat anterior ens poden fer reflexionar sobre un altre
tipus d'accés als fitxers. De moment tots els accessos que hem fet als
fitxers, tant binaris com de caràcter, ha estat **seqüencials**. Això vol dir
que sempre comencem pel principi del fitxer fins que arribem a la informació
que volem, o en la major part dels casos fins el final de fitxer.

Però, i si volem únicament una determinada informació? En l'exemple de
l'apartat anterior, què hauríem de fer si volguérem només la informació de
l'empleada 3 (Clàudia)? Doncs hauríem de passar primer per tots els anteriors.
Com només hi ha 2 davant, no sembla molta feina, però és fàcil de veure la
dificultat (o millor dit el cost) si el fitxer constara de centenars o milers
d'empleats. Suposem un fitxer de 10.000 empleats. Si volem accedir a l'empleat
9.500 hauríem de passar pels 9.499 empleats anteriors, ja que l'accés
seqüencial obliga a començar pel principi i anar passant fins que trobem la
informació. I encara pitjor: i si després de consultar l'empleat 9.500 ara
volem consultar el 9.000? Doncs hauríem de començar des del principi, perquè
ja ens l'havíem passat.

Afortunadament i ha una altra manera d'accedir, un altre tipus d'accés.
S'anomena **accés directe** perquè permetrà anar directament a una posició
determinada del fitxer. Moltes vegades també es diu **accés relatiu** o
**accés aleatori** , però el funcionament sempre és el mateix. I mireu que
estem parlant d'accés. Per tant el que canviarà no és la classe **File** sinó
qui permet accedir al contingut, és a dir les classes de **flux d'informació**
(els Streams).

Les classes **InputStream-OutputStream** i **Reader-Writer** només permeten
l'accés seqüencial. Per tant per a l'accés directe disposarem d'una altra
classe que ens permetrà fer totes les operacions, tant de lectura com
d'escriptura. Té l'avantatge que disposa de molts mètodes per a poder accedir
a la informació. No ens fara falta, per tant, les classes "decoradores" que
afegeixen funcionalitats. Amb aquesta classe tindrem suficient.

RandomAccessFile

La classe **RandomAccessFile** ens permetrà accedir de forma directa a un
fitxer. No ens farà falta, en principi, cap altra classe més. Ens
proporcionarà tota la funcionalitat necessària.

En els constructors aniran 2 paràmetres. El primer farà referència al fitxer.
El segon al mode d'accés: només lectura (**r**) o lectura-escriptura (**rw**).

    
    
    RandomAccessFile(fitxer: File, mode: String)
    
    
    RandomAccessFile(fitxer: String, mode: String)

En el primer cas li especifiquem un **File** en el primer paràmetre. En el
segon un **String** que correspondrà amb el nom del fitxer

En ambdós casos, el segon paràmetre indicarà el mode:

  * **"r"** indica només lectura
  * **"rw"** indica lectura escriptura

A pesar de ser una classe completament diferent de la jerarquia de
**InputStream-OutpuStream** (o **Readre-Writer**) implementa mètodes que es
diuen exactament igual que els d'aquelles classes, cosa que fa molt més còmoda
la utilització. Els mètodes més importants són:

Mètode | Explicació | Mètode  
---|---|---  
int **read()** | llig (escriu) un byte (encara així torna o se li passa un enter) | void **write(int)**  
int **read(byte[])** | llig (escriu) un sèrie de bytes, tants com la grandària de l'array (si pot) | int **write(byte[])**  
byte **readByte()** | llig (escriu) un byte interpretat com número de 8 bits amb signe | void **writeByte(int)**  
char **readChar()** | llig (escriu) un caràcter | void **write(char)**  
int **readInt()** | llig (escriu) un enter (4 bytes) | void **write(int)**  
short **readShort()** | llig (escriu) un enter xicotet (2 bytes) | void **write(short)**  
long **readLong()** | llig (escriu) un enter llarg (8 bytes) | void **write(long)**  
float **readFloat()** | llig (escriu) un número real en simple precisió (4 bytes) | void **write(float)**  
double **readDouble()** | llig (escriu) un número real en doble precisió (8 bytes) | void **write(double)**  
String **readUTF()** | llig (escriu) un cadena de caràcters (interpretat com UTF-8) | void **writeUTF()**  
|  |   
void **seek(long)** | situa el punter en la posició, mesurat des del principi del fitxer |   
long **length()** | torna la grandària del fitxer |   
void **close()** | Tanca el flux de l'accés directe |   
  
En cada lectura, després de llegir el punter que apunta al fitxer estarà
situat després de la dada llegida, siga quina siga la grandària.

Anem a veure un exemple utilitzant el fitxer **Empleats.dat** creat en
l'apartat anterior. Obrim l'accés directe únicament en mode lectura, i ens
situem directament a la posició 56, que és on comença la informació de
l'empleada 3 (Clàudia). Posteriorment utilitzem el mètode de lectura apropiat
per a cada tipus de dada. Copieu el codi següent en un fitxer de kotlin
anomenat **Exemple_3_2_1_EmpleatsDirecte.kt**

    
    
    package exemples
    
    import java.io.RandomAccessFile
    
    fun main(args: Array<String>) {
        val f = RandomAccessFile("Empleats.dat", "r")
        f.seek(56)
        println("Núm.: " + f.readInt())
        println("Nom: " + f.readUTF())
        println("Depart: " + f.readInt())
        println("Edat: " + f.readInt())
        println("Sou: " + f.readDouble())
        f.close()
    }

Ràpidament observem una cosa: com sabíem que ens havíem de situar en la
posició 56? I si els noms del dos primers hagueren sigut més llargs o més
curts?

Per a poder solucionar els problemes anteriors, podríem fer que els noms
siguen de llargària fixa. Les altres dades no donen problemes. Intentarem ara
donar sempre una grandària de 10 caràcters a cada nom (si sospitàrem que no en
tenim prou, hauríem de fer-los més grans). Anem a crear el fitxer
**Empleats2.dat** , i serà exactament igual al de l'anterior apartat, excepte
que en el moment de posar els noms (en un array de strings) posem exactament
10 caràcters, omplint amb blancs si és necessari. Evidentment, aquesta no és
l'única manera, però per a les poques dades que tenim, sí la més ràpida.

    
    
    val noms = arrayOf("Andreu    ","Bernat    ","Clàudia   ","Damià     ")

La sentència anterior és l'única diferència respecte al programa de creació
d'Empleats.dat de la pregunta anterior, a banda del nom del fitxer, que ara
serà **Empleat2.dat** :. Copieu el següent codi en el fitxer Kotlin:
**Exemple_3_2_2_Crear_Empleats2.kt**

    
    
    package exemples
    
    import java.io.DataOutputStream
    import java.io.FileOutputStream
    
    fun main(args: Array<String>) {
        val f = DataOutputStream(FileOutputStream("Empleats2.dat"))
    
        val noms = arrayOf("Andreu    ","Bernat    ","Clàudia   ","Damià     ")
        val departaments = arrayOf( 10, 20, 10, 10 )
        val edats = arrayOf( 32, 28, 26, 40 )
        val sous = arrayOf( 1000.0, 1200.0, 1100.0, 1500.0 )
    
        for (i in 0..3){
            f.writeInt(i + 1)
            f.writeUTF(noms[i])
            f.writeInt(departaments[i])
            f.writeInt(edats[i])
            f.writeDouble(sous[i])
        }
        f.close()
        println("Fitxer creat")
    }

Ara que sabem la grandària exacta del nom, podem saber que la informació de
cada empleat és:

Número d'empleat (enter) | 4 bytes  
---|---  
Nom (10 caràcters + 2 bytes)  | 10 + 2 bytes  
Departament (enter) | 4 bytes  
Edat (enter) | 4 bytes  
Sou (doble precisió) | 8 bytes  
**Total:  
** | **32 bytes  
**  
  
Sabent que cada registre (la informació de cada empleat) ocupa 32 bytes,
sembla fàcil anar a un determinat empleat. Per a poder provar-lo bé,
introduirem el número d'empleat per teclat, fins introduir 0. Observeu que si
s'introdueix 1, hem d'anar a pel primer registre, que està a principi de
fitxer. Si introduïm 2, anem a pel segon, que només en té un davant, per tant
32 bytes. Copieu el següent codi en un fitxer Kotlin anomenat
**Exemple_3_2_3_Empleats2Directe.kt** :

    
    
    package exemples
    
    import java.io.RandomAccessFile
    import java.util.Scanner
    
    fun main(args: Array<String>) {
        val f = RandomAccessFile("Empleats2.dat", "rw")
        val sc = Scanner(System.`in`)
        println("Quin registre? (-1 per a eixir): ")
        var num = sc.nextInt ()
        while (num != -1) {
            f.seek(32 * (num - 1).toLong())
            println("Núm.: " + f.readInt())
            println("Nom: " + f.readUTF())
            println("Depart: " + f.readInt())
            println("Edat: " + f.readInt())
            println("Sou: " + f.readDouble())
            println()
            println("Quin registre? (-1 per a eixir): ")
            num = sc.nextInt()
    
        }
        f.close()
    }

El problema ara també sembla obvi. Hem assumit que cada caràcter ocupa un
byte. Com es codificarà en UTF-8, mentre siguen caràcters normals així serà.
Però què passarà quan hi haja un caràcter accentuat? Que ocuparà 2 caràcters.
Així, com Clàudia té un d'aquestos caràcters, la cadena no ocuparà 10+2 = 12
bytes, sinó 13. Aleshores, si intentem anar al quart empleat, ens donarà
problemes.

La manera de solucionar-lo serà escriure de manera que tots els caràcters
ocupen sempre el mateix. Hi ha un mètode que ens ho permet: **writeChars**.
Guardarà cada caràcter amb dos bytes, i no es guardarà la llargària de la
cadena. Podem intentar utilitzar-lo per construir el fitxer **Empleats3.dat**.
Només haurem de substituir la següent sentència:

    
    
    f.writeChars(noms[i])

a banda del nom del fitxer, clar. Copieu el següent codi en un fitxer Kotlin
anomenat **Exemple_3_2_4_CrearEmpleat3.kt**

    
    
    package exemples
    
    import java.io.DataOutputStream
    import java.io.FileOutputStream
    
    fun main(args: Array<String>) {
        val f = DataOutputStream(FileOutputStream("Empleats3.dat"))
    
        val noms = arrayOf("Andreu    ","Bernat    ","Clàudia   ","Damià     ")
        val departaments = arrayOf( 10, 20, 10, 10 )
        val edats = arrayOf( 32, 28, 26, 40 )
        val sous = arrayOf( 1000.0, 1200.0, 1100.0, 1500.0 )
    
        for (i in 0..3){
            f.writeInt(i + 1)
            f.writeChars(noms[i])
            f.writeInt(departaments[i])
            f.writeInt(edats[i])
            f.writeDouble(sous[i])
        }
        f.close()
        println("Fitxer creat")
    }

Lamentablement la lectura no és tan fàcil. Haurem de llegir exactament 10
caràcters (podríem utilitzar algun altre mètode, però el que es mostra permet
identificar clarament que es llegiran 10 caràcters). Els anirem acumulant en
un String poder mostrar-lo. Recordeu també que ara el nom ocupa 20 bytes, que
sumats als altres 20 de les altres dades fan un total de 40 bytes per
registre. Copieu el següent codi en el fitxer Kotlin
**Exemple_3_2_5_Empleats3Directe.kt** :

    
    
    package exemples
    
    import java.io.RandomAccessFile
    import java.util.Scanner
    
    fun main(args: Array<String>) {
        val f = RandomAccessFile("Empleats3.dat", "rw")
        val sc = Scanner(System.`in`)
        println("Quin registre? (-1 per a eixir): ")
        var num = sc.nextInt()
        while (num != -1) {
            f.seek(40 * (num - 1).toLong())
            println("Núm.: " + f.readInt())
            var nom = ""
            for (i in 1..10)
                nom += f.readChar()
            println("Nom: " + nom)
            println("Depart: " + f.readInt())
            println("Edat: " + f.readInt())
            println("Sou: " + f.readDouble())
            println()
            println("Quin registre? (-1 per a eixir): ")
            num = sc.nextInt()
    
        }
        f.close()
    }


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

