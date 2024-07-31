Accés a Dades

# 5.2 Driver org.json

Hi ha més d'una llibreria per a poder accedir i analitzar els documents json.

Per exemple, es comenten prou per Internet són **GSON** (de Google) i
**Jackson**.

Però nosaltres anem a utilitzar dues altres llibreries

  * Una, la del grup **org.json** que en principi serà la mateixa que s'utilitzarà en Android
  * Una altra, la de **Moshi** , que ens permetrà elaborar més el resultat ja que podrem fer una conversió entre Json i objectes de Java o de Kotlin

Pel que fa a la primera, en el moment de fer aquestos apunts l'última versió
estable és la **20210307** , és a dir, de juliol de 2018. En aquest enllaç
teniu d'on us la podeu baixar: <https://jar-
download.com/artifacts/org.json/json/20210307>. Una vegada baixada l'haurem
d'incorporar al nostre projecte (sobre el projecte **F4 -- > Modules -->
pestanya dependencies --> +**)

En ella trobem el més bàsic:

  * **JSONObject** : equivaldrà a un objecte
  * **JSONArray** : equivaldrà a un array
  * **JSONTokener** :ens permetrà analitzar el document Json, i tornarà l'objecte arrel. El mètode que utilitzarem serà sempre **nextValue()**.


Llicenciat sota la  [Llicència Creative Commons Reconeixement NoComercial
CompartirIgual 2.5](http://creativecommons.org/licenses/by-nc-sa/2.5/)

