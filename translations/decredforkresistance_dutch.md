## Gedetailleerde analyse van Decred's resistentie tegen het splitsen van de blockchain 

Het is geen geheim meer dat zuivere PoW netwerken kwetsbaar zijn voor splitsingen. We zijn getuige geweest van de creatie van verschillende "nieuwe" munten doordat een minderheidsgroep binnen het oorsponkelijke netwerk afsplitste, waaronder Ethereum Classic, Bitcoin Gold, Bitcoin Cash en Bitcoin SV. 

In dit bericht wordt uitgelegd hoe het Decred netwerk de afsplitsing van minderheidsgroepen voorkomt, gebaseerd op een analyse die oorspronkelijk op [Reddit werd gepubliceerd](https://www.reddit.com/r/decred/comments/7f9ie1/detailed_analysis_of_decred_fork_resistance/) door davecgh. Het beschrijft belangrijke aspecten van Decred's hybride Proof-of-Work (PoW) en Proof-of-Stake (PoS) consensussysteem en biedt een gedetailleerd overzicht van wat er zou gebeuren als een entiteit zou proberen om de Decred blockchain te splitsen.

Als je een opfrissers nodigd hebt waarom splitsingen best vermeden worden kan je [hier](https://blog.goodaudience.com/blockchain-forks-and-chain-splits-why-we-ould-avoid-them-f54c693a90f1) terecht.

### Voorbereidende kennis

Het Decred netwerk wordt beveiligd door zowel PoW miners als PoS stemmers. Het PoS stemsysteem werkt door bepaalde hoeveelheden munten vast te zetten in wat een _stem ticket_ wordt genoemd. Deze ticketten fungeren als de fundamentele bouwstenen waarmee stakeholders kunnen deelnemen aan het [bestuur van Decred](https://docs.decred.org/governance/introduction-to-decred-governance/).

Per blok zijn maximaal 20 nieuwe tickets beschikbaar. Eenmaal verworven is er een wachttijd van 256 blokken waarna het ticket in de live ticketpool wordt geplaatst. Deze pool (een verzameling van alle live tickets op de blockchain) heeft een streefdoel van 40960 tickets, maar kan groeien of krimpen tijdens de werking. De prijs voor een ticket (aka PoS difficulty) wordt aangepast via vraag en aanbod om de grootte van 40960 te behouden. Het algoritme dat de ticketprijs bepaald, wordt beschreven in [DCP0001](https://github.com/decred/dcps/blob/master/dcp-0001/dcp-0001.mediawiki).

Live tickets wachten in de pool om hun stem uit te brengen en het selectieproces kan onmogelijk door PoW miners gemanipuleerd worden. Het algoritme dat de selectie van tickets controleert is in de eerste plaats gebaseerd op de hash van de vorige blok, wat betekent dat het zowel pseudorandom als deterministisch is. Als je blok 100 bovenop blok 99 bouwt, zijn de tickets die in blok 100 moeten worden opgenomen bekend bij elke full node in het netwerk. De ticketselectie kan enkel worden gewijzigd door een nieuwe oplossing te vinden voor blok 99 met een andere hash, waardoor vervolgens een nieuwe reeks willekeurige tickets geselecteerd zou worden om te stemmen.

Elke blok worden 5 tickets geselecteerd om te stemmen. Ten minste 3 van de 5 stemmen moeten in de blok worden opgenomen, anders wordt deze niet door het netwerk geaccepteerd. Bovendien wordt de beloning voor PoW mijnwerkers verlaagd als er slechts 3 of 4 stemmen in een blok worden opgenomen, respectievelijk met 40% en 20%. Dit om mijnwerkers te ontmoedigen die stemmen zouden kunnen negeren om het systeem te bespelen.

Het is belangrijk op te merken dat stakeholders **aanwezig moeten zijn** op de main chain of een afsplitsing daarvan wanneer hun tickets zijn geselecteerd. Het kopen van een ticket betekent niet dat er ook automatisch gestemt wordt, uw wallet of Voting Service Provide (VSP) moet uw stem uitbrengen wanneer het ticket wordt geselecteerd. Dit onderscheid is van cruciaal belang, omdat het betekent dat de live ticket-pool op een chain afgesplitst door een minderheidsgroep grotendeels bestaat uit tickets zonder stemrecht, omdat de eigenaren zich op de main chain bevinden.

Een gedetailleerde behandeling van de theorie achter elk van deze aspecten valt buiten het bereik van dit artikel. In essentie heeft het voornamelijk te maken met bescherming tegen allerlei vijandige scenarios.

### Scenario, veronderstellingen en methodologie

Met al het voorgaande in gedachten stellen we een ons nu een scenario voor waarin een entiteit probeert een afsplitsing van het netwerk te creëren waar 75% van de stakeholders (zij die in het bezit zijn van tickets) het niet mee eens is.

We gaan ervan uit dat beide zijden van de gepoogde afsplitsing over evenveel PoW rekenkracht beschikken (dus 50% hashvermogen elks). Zoals vermeld bestaat de meerderheidsgroep uit 75% van de stakeholders, en de minderheidsgroep uit de overige 25%.

Verder nemen we aan dat de meest recente blok op het punt van de splitsing blok 99999 is. Dus beide zijden van de splitsing werken aan het vinden van blok 100000, één zijde volgens de regels van de minderheidsblockchain, de andere volgens die van de meerderheidsblockchain.

Tot slot, om de beschrijving te vereenvoudigen en het gemakkelijker te maken de logica te volgen, nemen we aan dat elk 4e ticket in de live ticketpool behoort tot de minderheidsgroep (omdat slechts 25% van de stakeholders in de minderheidsblockchain zit). Met andere woorden, ticketnummers 0, 4, 8, 12, 16, 20, ..., 40956 zijn tickets in de live ticket pool die stakeholders uit de minderheidsgroep vertegenwoordigen, terwijl de ticketnummers 1, 2, 3, 5, 6, 7, 9, ..., 40957, 40958, 40959 de stakeholders uit de meerderheidsgroep vertegenwoordigen.

Vergeet niet: stakeholders **moeten aanwezig zijn** op de relevante (afsplitsing van de) blockchain wanneer hun tickets worden geselecteerd om hun stem uit te brengen.

![](https://cdn-images-1.medium.com/max/1000/1*Y8OnkamQAVLjSd8Ch5eQUA.png)
*<p align="center">Illustratie van het voorgestelde scenario</p>*
### Stapsgewijze, verklarende beschrijving
Wat volgt is een stapsgewijze verklaring van de gebeurtenissen die zouden plaatsvinden in het hierboven beschreven scenario.

#### Blok 100000

* De hash power op beide blockchains zal proberen een nieuwe blok bovenop blok 99999 te bouwen.
* Om deze nieuwe blok op de minderheidsblockchain te bouwen, moet hij minimaal 3 stemmen uit de live ticketpool bevatten en de geselecteerde stemmen zijn afhankelijk van blok 99999.
* Blok 99999 stelt aan de rest van het netwerk dat blok 100000 gebouwd dient te worden met ticketnummers 17113, 17331, 21307, 21328 en 24903.
* Zoals we kunnen vaststellen behoren zijn 4 van deze 5 tickets toe aan stakeholders uit de meerderheidsgroep (ticketnummers 17113, 17331, 21307 en 24903), wat betekent dat ze hun stem zullen uitbrengen voor blok 100000 op de meerderheidsblockchain.
* De minderheidsgroep kan slechts 1 stem (ticketnummer 21328) verwerven en kan bijgevolg geen blok 100000 bouwen. In plaats daarvan moeten ze teruggaan en een nieuwe oplossing vinden om voor blok 99999 om zo een nieuwe set tickets te selecteren.

Op dit punt zien de blockchains er als volgt uit: Haakjes met de * in deze notatie duiden blokken aan waaraan wordt gewerkt.

```
... -> [99999] -> (100000*)
meerderheidsgroep van stakeholders (75%) zit op deze blockchain

\-> (99999a*)
minderheidsgroep van stakeholders (25%) zit op deze blockchain
```

Met andere woorden, op de blockchain van de meerderheid wordt er nu gewerkt aan blok 100000, terwijl de groep op de minderheidsblockchain vastloopt in de zoektocht naar een nieuwe oplossing voor blok 99999 in de hoop dat deze uiteindelijk wel minstens 3 ticketten in hun bezit uit de live ticketpool zal selecteren. Omdat, volgens ons gedachte-experiment, beide afsplitsingen over dezelfde hoeveelheid rekenkracht (hashpower) beschikken, kunnen we veilig aannemen dat zowel blok 100000 op de meerderheidsblockchain als de nieuw blok 99999 (noem deze 99999a) op de minderheidsblockchain rond dezelfde tijd worden gevonden .

#### Blok 100001

Op dit moment gebeurt het volgende:

* De rekenkracht (hashpower) op de meerderheidsblockchain zal proberen een nieuwe blok bovenop blok 100000 te bouwen. De stemmen die voor dit blok zijn vereist, zijn ticketnummers 563, 6766, 21009, 37394 en 37775.
* Deze keer behoren alle 5 tickets toe aan de stakeholders uit de meerderheidsgroep. Zij zullen bijgevolg hun stem geven voor blok 100000 op de meerderheidsblockchain, waardoor blok 100001 kan worden gebouwd.
* De minderheidsblockchain, nu met een nieuwe versie van blok 99999 (99999a), heeft een nieuwe hash en deze vereist de ticketnummers 1069, 8007, 16413, 19172 en 31821.
* De minderheidsblockchain kan nog altijd maar 1 stem (ticketnummer 19172) verwerven, en moet dus teruggaan en opnieuw een oplossing voor block 99999 zoeken die wel voldoende stemmen, die in hun bezit zijn, bevat.

Beide blockchains zien er nu zo uit:

```
... -> [99999] -> [100000] -> (100001*) 
meerderheidsgroep van stakeholders (75%) zit op deze blockchain

\-> (99999b*) 
minderheidsgroep van stakeholders (25%) zit op deze blockchain
```
Met andere woorden, de meerderheidsblockchain werkt nu aan blok 100001 terwijl de minderheidsgroep nog steeds vastzit en zoekt naar een nieuwe oplossing voor blok 99999 om een nieuwe selectie tickets te krijgen in de hoop dat ze deze keer wel in staat zullen zijn 3 stemmen te vergaren. Omdat, volgens ons gedachte-experiment, beide blockchains dezelfde rekenkracht (hashpower) hebben, kunnen we opnieuw veilig aannemen dat zowel blok 100001 op de meerderheidsblockchain als de nieuwe blok 99999 (99999b) op de minderheidsblockchain gemiddeld rond de dezelfde tijd gevonden worden.

#### Blok 100002

Wat er zich op dit punt afspeelt:

* De rekenkracht (hashpower) van de meerderheidsblockchain zal proberen een nieuwe blok te bouwen bovenop blok 100001. De stemmen die voor dit blok zijn vereist, hebben ticketnummers 174, 1999, 12808, 31928 en 38317.
* Dit keer zijn 3 van die 5 tickets in het bezit van de stakeholders uit de meerderheidsgroep (ticketnummers 174, 1999, 38317), wat betekent dat ze hun stemmen zullen geven voor blok 100001 op de meerderheidsblockchain, waardoor blok 100002 kan worden gebouwd.
* De minderheidsgroep, nu met een nieuwe versie van blok 99999 (99999b), heeft een nieuwe hash, die de volgende ticketnummers selecteerd: 4653, 15211, 29988, 35175 en 35665.
* De minderheidsblockchain kan nog altijd maar 1 stem (ticketnummer 29988) uit brengen, en moet dus teruggaan en weer een nieuwe oplossing zoeken voor blok 99999 om zo een nieuwe selectie tickets te bekomen.

Dat ziet er al volgt uit:

```
... -> [99999] -> [100000] -> [100001] -> (100002*)
meerderheidsgroep van stakeholders (75%) zit op deze blockchain

\-> (99999c*)
minderheidsgroep van stakeholders (25%) zit op deze blockchain
```

De meerderheidsgroep werkt nu dus aan blok 100002, terwijl de minderheidsgroep nog steeds vastzit en zoekt naar een nieuwe oplossing voor blok 99999 om een nieuwe selectie tickets te krijgen in de hoop dat ze van deze selectie wel 3 of meer tickets zullen bezitten.

![](https://cdn-images-1.medium.com/max/800/1*vnZv8fLSnjc6xIu6GbZh3Q.jpeg)

#### Spring verder naar blok 100010
Het reeds beschreven proces herhaalt zich totdat uiteindelijk een variant van blok 99999 op de minderheidsblockchain geluk heeft en 3 tickets selecteert die in het bezit van de minderheidsgroep zijn. Dit blijkt ongeveer 1 op de 10 pogingen te zijn. Dus, al we vooruitspringe op de blockchains naar het punt waarop dit gebeurt, zien we het volgende:

```
... -> [99999] -> [100000] -> [100001] -> [100002] -> ... -> [100009] -> (100010*)
meerderheidsgroep van stakeholders (75%) zit op deze blockchain

\-> [99999j] -> (100000a*)
minderheidsgroep van stakeholders (25%) zit op deze blockchain
```

Aangezien beide blockchains evenveel rekenkracht (hashpower) hebben, kan de minderheidsgroep op geen enkele manier de meerderheidsgroep in halen. Bovendien zal hetzelfde proces zich herhalen voor minderheidsgroep bij blok 100001. Zij zullen terug moeten gaan en nieuwe oplossingen voor blok 100000 moeten zoeken tot deze een ticketselectie met minimum 3 tickets in hun bezit bevat.

Bijgevolg zullen miners niet in de minderheidsgroep blijven omdat ze daar nauwelijks blokken oplossen en dus amper beloond worden. De minderheidsblockchain zal nooit winstgevend zijn en daarom zal alle rekenkracht (hashpower) uiteindelijk terugkeren naar de meerderheidsblockchain.

#### Frequent gehoorde bezwaren
***Wat als de minderheidsblockchain meer dan 10x de rekenkracht (hashpower) van de meerderheidsblockchain bezit?***

In theorie kan de minderheidsblockchain, vertegenwoordigd door 25% van de stakeholders,met 10x meer rekenkracht (hashpower) de meerderheidsblockchain bijbenen. Dit is echter geen realistisch scenario vanwege de economische stimulansen.

Minen op de minderheidsblockchain met 10x meer rekenkracht (hashpower) betekent uiteindelijk dat de miners slechts 1/10 van de blokbeloning krijgen die ze op de meerderheidsblockchain zouden krijgen, en dat is wanneer we alleen rekenkracht in beschouwing nemen. In ons scenario word hun beloning nog verder beperkt tot 1/10 van 60% omdat gemiddeld slechts 3 stemmen zullen kunnen worden opgenomen. Met andere woorden, miners zouden slechts 6% van de beloningen ontvangen die ze zouden verdienen door hun rekenkracht op de meerderheidsblockchain in te zetten. Of vanuit de andere hoek, miners zouden 94% minder ontvangen door te werken aan de minderheidsblockchain.

In getallen uitgedrukt betekent dit dat wanneer een miner 5% van de totale rekenkracht van het netwerk controleert, deze kan verwachten ongeveer 5% van de PoW beloning per blok te ontvangen, of 5% van ~ 13.89 ≈ 0.6945 DCR op dit moment (12/12/2018). Op de minderheidsblockchain zou de beloning ten eerste slechts 60% van ~ 13.89 ≈ 8.334 DCR bedragen, en vervolgens zou die 5% rekenkracht slechts 0,5% van het totale rekenkracht op de minderheidsblockchain zijn, dus 0,5% van ~ 8.334 ≈ 0.04167 DCR. Als we naar de cijfers kijken, zien we dat 0,04167 DCR inderdaad 6% is van 0,6945 DCR.

PoW minen is zeer competitief omdat het een zero-sum game is. De meeste miners, zelfs die met enorme voordelen zoals gratis elektriciteit, hebben een dunne marge en moeten vaak hopen op een toekomstige appreciatie om het verschil te maken. Gezien de verlaging van het inkomen met 94% zouden de meeste miners in feite betalen, i.p.v. verdienen, door aan de minderheidschain te werken.

***Kan iemand niet gewoon de consensus regels veranderen en zo de stakeholders negeren?***

Als de minderheidsblockchain gedurende een bepaalde periode het ticket-stem systeem zou uitgeschakelen, zou ze in staat zijn om blokken te produceren en af te splitsen van de meerderheidsblockchain. Hoewel dit theoretisch mogelijk is, zou dit het hybride systeem volledig vernietigen en zou de afgesplitste blockchain in feite terug een puur PoW netwerk zijn. Het zou ongetwijfeld Decred niet meer zijn.

In tegenstelling tot zuivere PoW munten waar niemand kan zeggen welke van de afgesplitste blockchains de 'echte' is, is dit bij Decred overduidelijk vanwege het aantoonbaar en geformaliseerd bestuurssysteem. Decred stakeholders maken de beslissing welke blockchain de 'echte' Decred is en dat doen ze on-chain en op een cryptografisch aantoonbare manier.

Stakeholders investeren in Decred met de verwachting dat belangrijke beslissingen over de consensus regels door de stakeholders zelf worden genomen. Het verwijderen van de autoriteit van de stakeholders zou hetzelfde zijn als het verwijderen van Proof-of-Work uit een pure PoW munt. Met andere woorden, het zou de veiligheid van het systeem volledig vernietigen. Hoeveel vertrouwen gaan investeerders kunnen opbrengen voor een munt die één van de belangrijkste kenmerken die het claimt te bieden negeert?

### Conclusie
Het hybride PoW-PoS consensussysteem van Decred maakt het afsplitsen van de blockchain uiterst moeilijk - zo niet onmogelijk - zonder goedkeuring van de meerderheid van de stakeholders. Deze dissertatie heeft aangetoond waarom een Classic-, Gold- of Cash-scenario hoogst onwaarschijnlijk is voor Decred.

De kosten om een minderheidsblockchain te onderhouden, zelfs met 10x van de rekenkracht, zijn aanzienlijk; miners kunnen een ernstige inkomensverlaging verwachten als ze besluiten om deel te nemen. Als alternatief is het mogelijk om het PoS-systeem te verwijderen of uit te schakelen en de Decred blockchain te splitsen zoals elk ander PoW netwerk. Dit verslaat echter het doel van Decred en het is twijfelachtig of iemand zo'n poging serieus zou nemen.

De basisprincipes van bestendigheid tegen deregelijke afsplitsingen (fork resistance) goed krijgen, is van cruciaal belang voor de levensduur. Het hybride PoW-PoS systeem zorgt ervoor dat kleine groepen de transactiestroom niet kunnen domineren of wijzigingen kunnen aanbrengen in Decred zonder dat de stakeholders het daarmee eens zijn. Het stimuleert coördinatie en samenwerking, waardoor Decred een uiterst sterk netwerk wordt dat is gebouwd om lang mee te gaan.

### Aanvullende lectuur
Deze post heeft het belangrijke onderwerp van bestendigheid tegen afsplitsingen (fork resistance) behandeld, maar er valt nog veel meer te ontdekken. Het hybride PoW-PoS systeem van Decred is bijvoorbeeld ook een superieur afschrikmiddel tegen aanvallen met een meerderheid van de rekenkracht (51%). Als je wilt weten hoe dit werkt, lees dan dit artikel door Zubair Zia: [Zubair Zia](https://medium.com/@zubairzia):

Ben je op zoek naar meer geavanceerde onderwerpen? Dan kan je bijvoorbeeld onderzoeken hoe Decred zijn netwerk soepel kan upgraden door te stemmen over wijzigingen in consensusregels, of hoe mensen voorstellen kunnen indienen op het off-chain bestuurssysteem genaamd Politeia. Als u de voorkeur geeft aan technische details, bekijk dan de Decred documentatie.(https://medium.com/decred/decreds-hybrid-protocol-a-superior-deterrent-to-majority-attacks-9421bf486292)


Kies een van de hier vermelde chatplatformen als je interactie wilt met de Decred gemeenschap. Wij zijn een pragmatische groep mensen - kom erbij!

### Dankwoord
Zonder de originele analyse van davecgh zou deze post waarschijnlijk niet bestaan. Bovendien hebben de recensie van Artikozel en de constructieve opmerkingen in de schrijverskamer dit bericht enorm verbeterd. De illustratie van het scenario is gemaakt door Zubair Zia. Bedankt allemaal!

Dit artikel werd oorsponkelijk gepubliceerd op 12/12/2018 op https://medium.com/decred/detailed-analysis-of-decred-fork-resistance-93022e0bcde7

Vrije vertaling door Jazzah
