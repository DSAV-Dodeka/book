# Wat kun je ~~doen~~ leren?

Bij de .ComCom is enorm veel te doen en dus natuurlijk ook heel veel te leren. De taken kunnen in de volgende vier belangrijke delen worden opgesplitst:

- Design: de website moet er natuurlijk mooi uitzien. Dat is echter niet het enige, want de website moet ook gebruiksvriendelijk zijn. De user interface (UI), dus hoe de gebruiker omgaat met de website, en de user experience (UX), dus de ervaring van de gebruiker, moeten ook top zijn. UI/UX design is dus misschien nog wel belangrijker en daar is dus enorm veel over te leren.
- Content: de website bevat naast nieuws belangrijke informatie over o.a. hoe je lid wordt en wanneer de trainingstijden zijn, ook bijvoorbeeld informatie over huidige en toekomstige wedstrijden en activiteiten. Die moeten constant geüpdate worden. De website is een uithangbord voor de vereniging en speelt een belangrijke rol bij het aantrekken van nieuwe leden. Dit maakt de "content" (de tekst en plaatjes) erg belangrijk. Je zult expert worden in het doorspitten van de FOCUS-archieven en het schrijven van tekstjes.
- Programmeren: een groot deel van het werk dat bij de .ComCom wordt gedaan, is inderdaad simpelweg programmeren. Het is wel best anders dan je misschien gewend bent bij de vakken die je op de uni volgt. Het zijn namelijk geen Python plotjes of algoritmes in Java. Het gaat hier om het bouwen en onderhouden van een grote applicatie (die ondertussen 3+ jaar oud is) met meerdere componenten. Hieronder geven we nog meer detail.
- Systeemadministratie: dit is niet het meest sexy taakje, maar wel heel belangrijk. We kunnen namelijk ook privéinformatie. Daarnaast mogen de systemen niet zomaar uitvallen en moeten we de juiste keuze maken tussen aanbieders van servers. 

## Programmeren

Programmeren, coderen, developen, er zijn veel woorden voor wat wij doen. Je kunt het zelfs software _engineering_ of _architecture_ noemen. De volgende dingen bouwen wij allemaal:

- Een website, de "frontend", (JavaScript, HTML, CSS) met het React framework. Naast statische pagina's komen er ondertussen steeds meer dynamische pagina's bij, die up-to-date informatie ophalen van een server, die vervolgens moet worden gemanipuleerd. Daarnaast moet worden bijgehouden of je wel of niet bent ingelogd en welke onderdelen van de website je kunt zien. Het bestuur moet een tabel kunnen zien met informatie over alle leden, en dit aan kunnen passen. Als data verandert op de server, moet je dat ook meteen kunnen zien.
- Een server die een toegankelijk is via een API, de "backend" (geschreven in Python), die reageert op aanvragen (requests) van de website. Deze slaat alle wachtwoorden op in een vorm die wij niet kunnen lezen en kan bewijzen of je echt bent wie je zegt dat je bent. Daarnaast moet de backend informatie op kunnen halen uit een database. Hiervoor gebruiken we SQLite.

## Systeemadministratie

- Beheren van de server zelf (Ubuntu Linux), updates uitvoeren, de toegang veilighouden.
