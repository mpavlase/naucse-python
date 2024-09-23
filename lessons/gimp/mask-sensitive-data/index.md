# Maskování citlivých dat

Vyzkoušíme si to na tomto obrázku z Wikimedia: https://commons.wikimedia.org/wiki/File:%C5%A0koda_Felicia%2BFabia%2BOctavia.jpg

Před:
![Testovací obrázek](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/%C5%A0koda_Felicia%2BFabia%2BOctavia.jpg/800px-%C5%A0koda_Felicia%2BFabia%2BOctavia.jpg)

Po:
![Lepší postup ](static/spz-komplet.jpg)

## Nejjednodušeji: Vybrat obdélník a vymazat
* vybrat obdélník
* vymazat (tlačítko Delete)

![Nejrychlejší cesta](static/spz-nejrychleji.jpg)

## Lépe: Výběrem i podle barvy
* ručně obkroužit oblast (Laso)
* použít nástroj Výběr dle barvy, režim Průnik s aktuálním výběrem
  * práh cca 60
* Přidat alfa kanál
* **Jen vyzkoušet** (tady nevhodné): nástroj rozmazání přes průhledné oblasti - nevypadá to dobře
* Delete
* Vybrat nic (Ctrl + Shift + A)
* Klonování: Ctrl+klik levou myší na zdroj, pak drag & drop na kreslení podle zdroje

![Lepší postup ](static/spz-alfa-klonovani.jpg)
