# eet

Node.js knihovna pro EET ([elektronickou evidenci tržeb](http://www.etrzby.cz/cs/technicka-specifikace)).

*Upozornění: berte prosím na vědomí, že je balíček ve vývoji (verze 0.x). API se sice už měnit nebude, nicméně chybí především více testů a dokumentace. Pull requesty samozřejmě uvítám:-)*

## Instalace 

Je nutné mít nainstalovaný Node.js v4+.

```
npm install eet
```

## Příklad

```javascript
const eet = require('eet')

// privatni klic a certifikat podnikatele
const options = {
  privateKey: '...',
  certificate: '...',
  playground: true
}

// polozky, ktere se posilaji do EET 
const items = {
  dicPopl: 'CZ1212121218',
  idPokl: '/5546/RO24',
  poradCis: '0/6460/ZQ42',
  datTrzby: new Date(),
  celkTrzba: 34113,
  idProvoz: '273'
}

// ziskani FIK (kod uctenky) pomoci async/await (Node.js 8+ / Babel)
const {fik} = await eet(options, items)

// ziskani FIK v Node.js 6+
eet(options, items).then(response => {
  // response.fik
})
```

## Převod .p12 na .pem

Balíček pracuje s klíči v textovém formátu, z binárního .p12 je lze převést např. pomocí balíčku [pem](https://github.com/andris9/pem):

```javascript
// npm install pem
const pem = require('pem')

const file = require('fs').readFileSync('cesta/k/souboru.p12')
const password = '' //pro testovací certifikáty EET je heslo 'eet'

pem.readPkcs12(file, {p12Password: password}, (err, result) => {
  if (err) ...
  // result.key je privátní klíč
  // result.cert je certifikát
})

```

## Nastavení

### eet (options, items)

* *options* - Volby pro odesílání požadavku (pro SOAP).
  * *options.privateKey* (string) - Privátní klíč.
  * *options.certificate* (string) - Certifikát.
  * *options.playground* (bool) - Posílat požadavky na playground? Def. false (ne).
  * *options.httpClient* - Viz [soap options](https://github.com/vpulim/node-soap#options), slouží pro testování.
* *items* - Položky, které se posílají do EET. Mají stejný název jako ve specifikaci EET, jen používají cammel case (tedy místo dic_popl se používá dicPopl).

## Časté chyby

### Neplatny podpis SOAP zpravy (4)

Na 99% půjde o problém s certifikátem, více je popsáno v issue [#1](https://github.com/JakubMrozek/eet/issues/1#issuecomment-256877574).


## Changelog

### v0.2 (30. 10. 2016)
- podpora verze Node.js 4+
- doplněna dokumentace (časté chyby a převod z .p12 na .pem pomocí balíčku `pem`)

### v0.1 (20. 10. 2016)
- první veřejná verze

## Licence

MIT
