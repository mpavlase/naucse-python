## Cyklické importy

V domácích úkolech budeš rozdělovat piškvorkový projekt na několik modulů.
Tento text si doporučuju číst až když narazíš na příslušný úkol,
abys věděl{{a}} o čem tu je řeč.

Po rozdělení bude projekt vypadat třeba nějak takhle:
(Šipky mezi moduly znázorňují importování.)

```plain
┌──────────────────╮  ┌───────────────╮  ┌──────────────────╮
│      ai.py       │  │ piskvorky.py  │  │    hra.py        │
├──────────────────┤  ├───────────────┤  ├──────────────────┤
│                  │◀-│ import ai     │◀-│ import piskvorky │
├──────────────────┤  ├───────────────┤  ├──────────────────┤
│ def tah_pocitace │  │ def vyhodnot  │  │                  │
│                  │  │ def tah       │  │                  │
└──────────────────┘  │ def tah_hrace │  └──────────────────┘
                      │               │
                      └───────────────┘
```

Jenže funkce `tah_pocitace`
většinou potřebuje volat funkci `tah`.
Co s tím?
Můžeš importovat `ai` z `piskvorky` a zároveň
`piskvorky` z `ai`?

```plain
┌──────────────────╮  ┌───────────────╮
│      ai.py       │  │ piskvorky.py  │
├──────────────────┤  ├───────────────┤
│                  │◀-│ import ai     │
│ import piskvorky │-▶│               │
│                  │  │               │
│ def tah_pocitace │  │ def vyhodnot  │
│                  │  │ def tah       │
└──────────────────┘  │ def tah_hrace │
                      │               │
                      └───────────────┘
```

Můžeš se na to podívat z pohledu Pythonu,
který příkazy v souborech vykonává.
Když má importovat soubor `piskvorky.py`, začne ho
zpracovávat řádek po řádku,
když tu (docela brzo) narazí na příkaz `import ai`.
Otevře tedy soubor `ai.py`
a začne ho zpracovávat řádek po řádku.
Brzy narazí na příkaz `import piskvorky`. Co teď?

Aby nenastala situace podobná nekonečné smyčce –
jeden soubor by importoval druhý, druhý zase první,
a tak stále dokola –
udělá Python takový malý „podvod“:
když zjistí, že soubor `piskvorky.py`
už importuje, zpřístupní v modulu `ai`
modul `piskvorky` tak, jak ho
má: nekompletní, bez většiny funkcí co v něm mají
být nadefinované.
A až potom, co dokončí import `ai.py`,
se vrátí k souboru `piskvorky.py`
a pokračuje v provádění příkazů `def`, které v něm jsou.
Takový nekompletní modul může být občas užitečný,
ale ve většině případů se chová skoro
nepředvídatelně a tudíž nebezpečně.

Jinými slovy: když se dva moduly importují navzájem,
nemusí to fungovat podle očekávání.

Téhle situaci se budeš chtít vyvarovat.

Jak na to? Máš dvě možnosti.


## Organizace modulů podle závislostí

První možnost je definovat funkci `tah` v modulu `ai`
a používat ji odtamtud.
To je jednoduché, ale nerespektuje účel modulu
`ai`, který má obsahovat jenom logiku
vybírání tahu počítače, a ne pomocné funkce, které
můžou být potřeba i jinde.

```plain
┌──────────────────╮  ┌───────────────╮
│      ai.py       │  │ piskvorky.py  │
├──────────────────┤  ├───────────────┤
│                  │◀-│ import ai     │
│                  │  │               │
│ def tah_pocitace │  │ def vyhodnot  │
│ def tah          │  │ def tah_hrace │
│                  │  │               │
└──────────────────┘  └───────────────┘
```

## Pomocný modul

Druhá možnost je definovat nový, sdílený modul,
který se použije jak v `piskvorky.py` tak v `ai.py`.

Takový modul se často pojmenovává
`util.py` (z angl. *utility*, pomůcka, nástroj).

```plain
              ┌──────────────────╮
              │ util.py          │
              ├──────────────────┤
              │ def tah          │
              └──────────────────┘
                      ▲  ▲
                      │  │
┌──────────────────╮  │  │  ┌───────────────╮
│      ai.py       │  │  │  │ piskvorky.py  │
├──────────────────┤  │  │  ├───────────────┤
│ import util      │──┘  └──│ import util   │
│                  │◀───────│ import ai     │
│                  │        │               │
│ def tah_pocitace │        │ def vyhodnot  │
│                  │        │ def tah_hrace │
│                  │        │               │
└──────────────────┘        └───────────────┘
```

Nevýhoda pomocného modulu je ta,
že se z něj může stát neudržované „odkladiště“
všeho kódu, který byl jednou potřeba na dvou
nebo více místech.

Pro kterou z možností se rozhodnout, záleží
na situaci.
Programování není vždycky jen exaktní věda!
