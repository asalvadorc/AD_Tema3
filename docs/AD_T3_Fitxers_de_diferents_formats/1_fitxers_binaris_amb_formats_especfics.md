Accés a Dades

# 1.- Fitxers binaris amb formats específics

DataInputStream i DataOutputStream

Ja hem vist com utilitzar els fitxers de caràcters i també de bytes. Però en
aquest últim cas sempre ha estat per a llegir o escriure byte a byte, fins al
final de fitxer.

Ens plantegem ara com utilitzar els fitxer per a guardar dades estructurades
de tipus bàsics **diferents**. De moment no seran complicades però enseguida
veurem que ens fa falta alguna cosa per a poder treballar còmodament.

Suposem un exemple d'una empresa que vol guardar dades dels seus empleats.
Concretament vol guardar el número d'empleat, el nom, el departament al qual
pertany, l'edat i el sou.

**Número** | **Nom** | **Depart  
** | **Edat  
** | **Sou  
**  
---|---|---|---|---  
1 | Andreu | 10 | 32 | 1000.00  
2 | Bernat | 20 | 28 | 1200.00  
3 | Claudia | 10 | 26 | 1100.00  
4 | Damià | 10 | 40 | 1500.00  
  
Ja es veu que les dades són de difrents tipus. Si tot fóra de text no hi
hauria problema. Però si considerem les dades com a numèriques enteres o reals
no ens serveixen els Stream de caràcters (Reader i Writer). Per tant hem
d'anar a InputStream i OutputStream, però seria molt dur treballar directament
amb bytes. Hauríem de saber exactament quants bytes ocupa cada dada: **int**
utilitza 4 bytes; **short** utilitza 2 bytes, **byte** utilitza 1 byte, i
**long** utilitza 8 bytes; en el cas dels reals, **float** utilitza 4 bytes i
**double** utilitza 8 bytes. Massa varietat i massa fiena recordar-los tots.
Ens falta doncs una ajuda per a poder guardar i recuperar dades d'aquestos
diferents tipus.

Aquesta funcionalitat ens la proporciona la parella **DataInputStream** i
**DataOutputStream** , que són decoradors dels Stream i que disposen de
mètodes per a guardar o recuperar dades de diferents tipus, sense haver de
saber el format intern de cadascun ni quants bytes ocupen. En la següent taula
tenim uns quants mètodes d'aquestos:

**DataInputStream** | Explicació | **DataOutputStream**  
---|---|---  
byte **readByte**() | Un byte | void **writeByte**(int)  
short **readShort**() | Un enter xicotet (2 bytes) | void **writeShort**(short)  
int **readInt**() | Un enter (4 bytes) | void **writeInt**(int)  
long **readLong**() | Un enter llarg (8 bytes) | void **writeLong**(long)  
float **readFloat**() | Un real en simple precisió | void **writeFloat**(flaot)  
double **readDouble**() | Un real en doble precisió | void **writeDouble**(double)  
char **readChar**() | Un caràcter Unicode (16 bits) | void **writeChar**(int)  
String **readUTF**() | Una cadena de caràcters UTF-8 i la converteix en String (16 bits) | void **writeUTF**(String)  
  
Anem a veure l'exemple, en el qual guardarem en un fitxer anomenat
**Empleats.dat** les dades dels 4 empleats. Per comoditat ens els definirem en
arrays de 4 elements: un array per als noms, un altre per als departaments,
etc. Enganxeu el següent codi en un fitxer de Kotlin anomenat
**Exemple_3_1_1_CrearEmpleats.kt**

    
    
    package exemples
    
    import java.io.DataOutputStream
    import java.io.FileOutputStream
    
    fun main(args: Array<String>) {
        val f = DataOutputStream(FileOutputStream("Empleats.dat"))
    
        val noms = arrayOf("Andreu", "Bernat", "Clàudia", "Damià")
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

I aquesta seria la recuperació de les dades. Enganxeu el següent codi en un
fitxer de Kotlin anomenat **Exemple_3_1_2_ConsultarEmpleats.kt**

    
    
    package exemples
    
    import java.io.FileInputStream
    import java.io.DataInputStream
    
    fun main(args: Array<String>) {
        val f = DataInputStream(FileInputStream("Empleats.dat"))
    
        while (f.available() > 0) {
            System.out.println("Número: " + f.readInt())
            System.out.println("Nom: " + f.readUTF())
            System.out.println("Depart: " + f.readInt())
            System.out.println("Edat: " + f.readInt())
            System.out.println("Sou: " + f.readDouble())
            System.out.println()
        }
        f.close()
    }

 
Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

