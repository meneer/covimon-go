# Covimon-Go
Architectuur en Functioneel Ontwerp
Het Covimon-Go systeem bestaat uit de volgende componenten:

•	Een mobiele app voor eindgebruikers
•	Een Autorisatie-server
•	Een Applicatieserver met Geo-database
•	Een wetenschapsmodule

# Use cases:
## Installatie van de app

De app is te downloaden via de reguliere app-stores voor Apple en Android (incl. f-droid).
De gebruiker downloadt de app en installeert de app zoals gebruikelijk voor het betreffende platform. Installatie vergt geen inlog, de identiteit is niet noodzakelijk.

## Registratie persoonlijke Covid-status
Na starten van de app, is de gebruiker verplicht om de eigen Covid-status bij te werken. Zonder eigen Covid-status heeft de app geen toegang tot de overige functies.
De Covid-status kan worden bepaald door het inschakelen van diverse vinkjes bij verschillende vragen. De vragen zijn vergelijkbaar met die van de OLVG-Corona app.

Op basis van de vinkjes bepaalt de app de Covid-status, namelijk Geen verschijnselen, Verdacht, Mogelijk Besmet. De status Besmet (100%) en de status Hersteld (0%) zijn niet door de gebruiker zelf in te voeren.  De gebruiker kan de Besmet-status (100% ) en de Schoon-status (0%) vastleggen door met de app de QR-code van het testresultaat te scannen.
De status wordt door de app omgezet in een cijfercode: 10, 25, 75.
De app zal dagelijks de vragenlijst voorleggen aan de gebruiker.

Als een gebruiker op een locatie is geweest waar eerder al een besmet persoon aanwezig is geweest, waardoor de kans bestaat dat iemand besmet wordt, dan zal de Covid-status in de app worden eengepast conform bovenstaand schema.

## Verplaatsen – lezen covid-status locatie
Uitgangspositie: een gebruiker heeft de eigen Covid-status in de app vastgelegd.

Als iemand zichzelf verplaatst, zal de app de geografische coördinaten, die door de sensor van het mobiele toestel worden bepaald, gebruiken om van die betreffende locatie vanuit de centrale geografische database de Covid-status van die locatie op te halen. Daarbij worden alle records van die locatie uit de afgelopen 24 uur opgehaald. De waarde van elke opgehaald covid-status wordt vervolgens getransponeert volgens de volgende methode:
Waarde langer dan 12 uur  geleden geregistreerd: status=status*10%
Waarde langer dan 4 uur  geleden geregistreerd: status=status*25%
Waarde langer dan 1 uur  geleden geregistreerd: status=status*50%
Waarde langer dan een half uur  geleden geregistreerd: status=status*90%
Waarde minder dan een half uur  geleden geregistreerd: status=status – deze status impliceert dat er iemand met een COVID-status minder dan een half uur geleden op dezelfde locatie is geweest of er misschien zelfs nog is. Dat betekent dat er een reële kans op besmetting kan worden aangenomen.
Als de kans heel groot is, dan zal de app de covid-status van de gebruiker in de app zelf verhogen, conform het bovenstaande schema.
Deze bepaling wordt voor elke opgehaald record uitgevoerd. De hoogste waarde wordt middels een kleurcode aan de gebruiker gepresenteerd. Een score van 100% zal via een notificatie aan de gebruiker worden gemeld.
Nog te bepalen:
•	Hoe lang moet een gebruiker op een locatie zijn om de gegevens op te halen. Het met een auto of fiets verplaatsen zal wellicht een te korte blootstellingsfactor inhouden.
•	Kleurcodes.
•	Granulariteit locatie

## Verplaatsen – registratie covid-status op locatie
Uitgangspositie: een gebruiker heeft de eigen Covid-status in de app vastgelegd.
Als een gebruiker een covid-status van > 25% heeft, zal de app de status registreren in de geografische database. De app zal de status in de database registreren op de coördinaten zoals bepaald door de geo-sensor van de smartphone. Daarbij zal ook het tijdstip worden vastgelegd.
De status van 0%, Schoon, alsmede de status 100% kan momenteel niet worden vastgelegd. Die status is alleen te bepalen als iemand negatief of positief getest is. Als die waarde zonder test in te vullen is, dan zou iemand de app kunnen gebruiken om locaties ten onrechte als risicovol te laten registreren.
De status Besmet en de status Hersteld is uitsluitend door een BIG-geregistreerde medewerker in te voeren. Zie de betreffende use case.

De app registreert uitsluitend:
Tijdstip
Covid-status (10 – 75%)

Nog te bepalen:
•	Hoe lang moet een gebruiker op een locatie zijn om de gegevens op te halen. Het met een auto of fiets verplaatsen zal wellicht een te korte blootstellingsfactor inhouden.

## BIG-registratie covid-status in de app
De autorisatieserver is verantwoordelijk voor verificatie van de BIG-status.
Registratie door een BIG-geregistreerde medewerker vindt als volgt plaats:
Medewerker zoekt in de eigen omgeving de Covid-status van cliënt op. De app toont een QR code. De gebruiker scant met de app de QR code waarin de 100% Besmet of 100% Hersteld status staat vermeld. Hierna zal in de app de Covid-status worden bijgewerkt.
Wetenschapsmodule
De wetenschapsmodule is beschikbaar voor wetenschappelijk ommderzoek. Daarbij biedt deze module toegang tot een specifieke API op grond waarvan aanvullende opvragingen vanuit de database kunnen worden gedaan.
De autorisatie om de betreffende API te benutten wordt afgedwongen via de autorisatieserver. Uitsluitend personen met in de autorisatieserver gedefinieerde attributen van gedefinieerde identity providers kunnen de API benaderen.

# Technologie
## App
De app heeft twee autorisaties nodig:
•	lezen geo-grafische coördinaten van het toestel
•	netwerktoegang
•	toegang tot de camera (voor scannen QR-code GGD testresultaat)
De enige functionele interface is een koppeling met een API van de applicatieserver (weliswaar indirect via de autorisatieserver).
Voorziene uitbreidingen
•	Na zelfdiagnose doorverwijzing naar GGD
•	Status Besmet/Hersteld registratie door GGD

## Applicatieserver/Database
De applicatieserver wordt uitsluitend via een beperkt aantal API’s ontsloten. Daarbij kunnen aanvragen uitsluitend vanaf de autorisatieserver worden ingediend.
De database dient te zijn geoptimaliseerd voor time series analyses, een time series database lijkt daarom de aangewezen soort.
De database gebruikt drie velden en heeft drie primaire sleutels: geo-coördinaten, tijd en Covid-Status.
Voor regulier gebruik worden alleen geo en tijd gebruikt. Voor wetenschappelijke doeleinden wordt ook Covid-status gebruikt.

De database levert op aanvraag van de app standaard de records van de afgelopen 24 uur op aan de autorisatieserver die ze teruglevert aan de app. Als er nog geen gegevens aanwezig zijn, wordt een Schoon-attribuut teruggeleverd.
Tegelijkertijd schrijf het dbms op diezelfde geolocatie een record weg met daarin Tijd en Covid-status, mist de status hoger is dan 25%.
De API voor wetenschappelijk onderzoek moet nog verder worden verfijnd. Te denken valt aan opleveren alle gegevens op een bepaald tijdstip of op een bepaalde grotere locatie ten behoeve van big-data analyse.
Voor deze API zal een afzonderlijke overeenkomst moeten worden opgesteld om in ieder geval de policy van de autorisatieserver te definiëren.

## Autorisatieserver
De autorisatieserver is een Identity Provider die uitsluitend tot doel heeft om Oauth requests te verzamelen en door te sturen naar de database/applicatieserver. Hiermee wordt gewaarborgd dat alleen vertrouwde communicatie plaatsvindt. Deze autorisatieserver is een open source component, die in ieder geval geclustered beschikbaar moet zijn om de load aan te kunnen en de beschikbaarheid te garanderen die noodzakelijk is.
De autorisatieserver ontvangt een toegangsaanvraag van de app. De app moet een vertrouwde app zijn die via een regulier kanaal op het toestel van de gebruiker is geïnstalleerd.
Het bericht dat vanaf de app naar de autorisatieserver wordt verzonden bevat het attribuut Geo-locatie, het attribuut Tijd en het attribuut Covid-Status.
Bij opvraag van de status van een locatie zullen Geo en Tijd worden gebruik, de autorisatieserver stuurt deze attributen namens de app door aan de database.

# Architectuur


Koppeling met BIG-register




