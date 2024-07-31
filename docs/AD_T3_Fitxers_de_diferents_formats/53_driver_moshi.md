Accés a Dades

# 5.3 Driver Moshi

El driver anterior tenia un funcionament molt senzill, però que obliga a
seguir en tot moment l'estructura que té el fitxer Json, buscar entre els
objectes i arrays cada vegada que es fa un accés.

Si són relativament pocs els accessos, és molt còmode, però si s'ha de fer un
accés continuat pot resultar més pesat.

Existeix una altra llibreria, que ens permet passar d'un fitxer Json a
objectes de Java o Kotlin, la qual cosa ens anirà bé quan siguen molts els
accessos, perquè en definitiva tindrem les dades en l'entorn natural del
nostre llenguatge: els objectes.

Es tracta de la llibreria **MOSHI** , i aquest és el lloc on poder trobar-la:

<https://github.com/square/moshi>

En el moment de fer aquestos apunts, va per la versió 1.14.0. Tanmateix en les
últimes versions dóna un error de compatibilitat. Ens conformarem per tant amb
una versió anterior. Podem trobar totes les versions en aquest enllaç:

<https://jar-download.com/artifacts/com.squareup.moshi/moshi>

Podem agafar per exemple versió 1.8.0:

<https://jar-download.com/artifacts/com.squareup.moshi/moshi/1.8.0/source-
code>

Veureu que hi ha dos drivers, el propi de **Moshi** i un altre driver anomenat
**okio**. Haurem d'incorporar els dos al nostre projecte.

Recordeu que la manera d'incorporar els drivers al projecte IntelliJ és amb:

**Sobre el projecte F4 (Project Structure) -- > Modules --> pestanya
dependencies --> +**


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

