# Dates Slovenia — brskalniški prototip

Preprost, odziven prototip za mobilni brskalnik. Uporablja samo HTML, CSS in JavaScript, brez namestitve paketov ali zunanjih storitev.

## Zagon

V mapi repozitorija zaženite:

```sh
python3 -m http.server 4173 --bind 127.0.0.1
```

Nato odprite `http://127.0.0.1:4173` v brskalniku. Za zaključek strežnika pritisnite `Ctrl+C`.

## Kaj lahko preizkusite

- Zavihke Domov, Razišči, Zemljevid, Shranjeno in Profil.
- Iskanje ter filtre po kategoriji, ceni, okolju in trajanju.
- Podrobnosti idej, shranjevanje v brskalniku, izbor mesta in interesov.
- Izbiro številčnih oznak v poenostavljenem sivinskem prikazu zemljevida.

Vse ideje in lokacijske oznake so demonstracijski primeri, ne preverjene ponudbe. Cene, razdalje, zemljevid, prijava, sinhronizacija in zaledni sistem niso vključeni. Shranjene ideje, mesto in interesi se hranijo v lokalnem `localStorage` brskalnika.

## Oblikovanje

Vmesnik je namenoma omejen na sive odtenke, besedilne oznake in preproste ploskve. Ne uporablja fotografij, ikon, animacij ali vizualnih učinkov.
