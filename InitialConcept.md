# covimon-go
Covid-19 Monitoring solution


## Het concept

De dag na de persconferentie van Hugo de Jonge waarin opeens de Corona apps ter sprake kwamen, heb ik op Twitter een hele discussie gevoerd over een wild idee. En het leek me wel een goed om die discussie hier te laten zien. Ik heb de tweets ongecensureerd in dit artikel geplakt. Alleen een paar typefouten verwijderd. Voor de liefhebbers: ik ben op Twitter te vinden als @meneer
= = =
## de app
Grappig van die apps van Hugo: we hebben het weer over privacy, de identiteit. Fout: we moeten uitgaan van toegang tot gegevens. Wanneer mag je erbij...?
Laten we eens kijken naar een bestaande oplossing: Pokemon-Go, of Waze Het gaat niet om de deelnemer, maar om de gegevens. Die worden vastgelegd op een kaart doordat een bepaald vertrouwd proces de kaartgegevens mag wijzigen. Via een API. Een API om de gegevens te lezen, alleen de relevante gegevens natuurlijk. En een API om gegevens te registreren. Alleen de relevante gegevens. En alleen als je de API mag gebruiken. Is dit concept bruikbaar?
Dus alleen een vertrouwde app mag de API benaderen. Dat is beveiligd immers. Ik roep maar even OAuth2 voor liefhebbers. Dus een API om te lezen, een API om te schrijven en een API voor de wetenschappers die statistische info willen gebruiken. Als ze dat mogen.

## En de vastgelegde gegevens?
“Op deze geolocatie heeft het afgelopen uur iemand met de status 'besmet' rondgehangen.” En dat gegevens is geregistreerd door een app waarin iemand zijn eigen status beheert.
Privacy? Nee, geen issue, want simpel een app downloaden. registreer zelf je Corona status. En tijdens je verplaatsing registreert de app automatisch locatie, tijd en status en leest ook automatisch van de locatie de nog relevante historische gegevens van die locatie. de API registreert geen andere gegevens dan de status op een bepaald moment.
Inloggen? Nee, waarom, we willen toch niet een identiteit kennen? Totaal niet relevant. Device ID? Nee, totaal niet relevant. De API hoeft maar een paar gegevens te beheren. Net als Pokemon Go. Dan kun je het nog leuk visualiseren ook.
Zo, probleem van Hugo de Jonge opgelost en de problemen van al die privacycollega's op hetzelfde moment.
Wat hebben we nodig?
•	Een geo database. Verrek, die hebben we vast al ergens, anders kopiëren we Pokemon-Go.
•	Een autorisatieserver (voor liefhebbers: een OAuth server)
•	Een app. Eentje maar.

En een digitale identiteit zoals DigiD, iDIN, of Facebook? Lachen, nee hebben we niet nodig. Een een attributencache zoals Irma? Nog meer lachen, hebben we ook niet nodig.
En zullen we het dan Covimon-Go noemen? En als iemand er een (r) of (c) of TM of zo bij zet: afblijven dit hele concept is nu al Public Domain.
En de naam bedacht voor Covimon: COVId-19 MONitor :D

## Deze vragen kwamen al voorbij van de Twitter community:
V: En wie mag erbij. Ook degene die al veel gegevens heeft en zo het profiel kan aanvullen?\
A: De autorisatieprofielen worden op de autorisatieserver beheerd. En er zijn maar drie profielen: lezen locatie-info, schrijven locatie-info (beide voor de app) en lezen kaartinfo voor wetenschap. Maar daar zitten helemaal geen persoonsgegevens in.\
V: Voor elke stap in het proces moet de AVG gevolgd worden. Niet alleen de output. Maar wat bedoel je met herdefiniëren?\
A: Wat willen we oplossen? Het fundamentele probleem. Dus niet we moeten privacy borgen. Dat is alleen maar een randvoorwaarde. Welk probleem willen we oplossen. Voor mij: hoe kan ik weten dat ik (wellicht) in contact kan zijn geweest met een geïnfecteerd persoon. 'Ik', 'waar' en 'wanneer' zijn de gegevens die relevant zijn. Als 'ik' gelijk is aan 'mijn mobiel' dan moeten we alleen nog 'waar' en 'wanneer' regelen. Zie mijn draadje voor een oplossing.\
V: De tijdsduur en mogelijke afstand van het contact zijn ook van belang zoals oa bij het onderzoek/experiment van @FryRsquared destijds naar voren kwam. Maar je komt met graaf al redelijk ver voor deze interacties en hoef je geen tijdstip en locatie op te slaan.\
A: Even andersom: in de geo database staan contactmomenten voor de betreffende locatie, verder registreren we nergens iets.\
V: Nou moeten we nog weten wie allemaal drager is van het virus. Daar hebben we geen benul van omdat er weinig getest wordt. En dan false positives. Aan het stoplicht waar je als fietser stond te wachten, stopt naast je een positieve automobilist, bv.\
A: Klopt. Vandaar de in de app de houder van de mobiel zelf symptomen en dergelijke moet beheren. een aanpassen. Statussen: Besmet, Verdacht, Blanco, Hersteld. Voldoende mogelijkheden.\
V: Dat klinkt minimaal als pseudoniem id plus timestamp plus lokatie plus besmettings-status. Dat zou ik zeker aanmerken als persoonsgegevens.\
A: Daarom is dat ook een onwenselijkheid. Wat je niet registreert, kun je niet kwijtraken.\
V: Wat zou er nog vanaf moeten? Pseudoniem id en tijdstempel?\
A: Pseudoniem. Misschien kan tijdstempel er zelfs ook af, dat kan de autorisatieserver wel toevoegen aan het token.\
V: De tijdstempel op de server bijvoegen is niet echt een verbetering in de vorm van privacybescherming. Zonder pseudoniem ID kan je niet meer. zeggen: persoon x blijkt nu besmet, waar is x in het verleden geweest zodat de bron te herleiden is.\
A: Maar dat persoon X = Besmet is hoe dan ook alleen bekend na diagnose. En als X=Besmet dan gelden al strikte regels. En elk gebruik van dat attribuut is hoe dan ook een privacyding. Is niet aan te ontkomen.\
V: En ik reageer juist op de opmerking dat privacy geen issue is. Ik dacht dat je daarmee bedoelde dat de AVG (en Tw?) volledig buiten scope zijn. Daar ben ik het niet mee eens. Het kan best binnen de regels.\
A: Mee eens, maar ik probeer juist om zo min mogelijk persoonsgegevens te gebruiken, om anoniem te zijn. En volgens mij kan dat grotendeels.\
V: Anoniem gaat je niet lukken. Anonieme output aan het eind van het proces misschien wel, afhankelijk van het doel.\
V: Wat ik me ook afvraag is de grid van de locatie bepaling wel nauwkeurig genoeg. Ervan uitgaande dat ik veilig ben > 1.5 M van een coronapatiënt.\
V: Ik verwacht dat datakwaliteit en daarmee de effectiviteit het grootste obstakel zal worden. Ik ben benieuwd hoe dat van tevoren gewaarborgd gaat worden.\
V: dat zijn zeker ook grote aandachtspunten.\
V: Veel mensen schijnen niet of nauwelijks klachten te hebben, dus in hoeverre is die informatie nuttig? Ik heb ernstige vraagtekens bij nut en noodzaak in vergelijking met de zwaarte van de maatregel (afhankelijk van de gekozen oplossing).\
A: Daarom is mijn idee leuker, laat mensen in gamification mode gaan. Verzamel virusbadges...\
V: Hoe zorg je dat alleen de "vertrouwde" app naar de API mag schrijven? Wat als men de API tokens van de app bloot weet te leggen of device location mockt. Dit is met Pokemon Go in het verleden ook gebeurd. Is de data die men naar de API schrijft dan wel te vertrouwen?\
A: Er zijn vast echte experts die dat kunnen regelen :) Misschien initiële registratie die alleen de vertrouwde geregistreerde app toegang geeft tot de Oauth server. Dat token hoeft niet verder te komen dan tot die server, hoeft niet te worden gelogd.\
V: Ik draai Lineage op een oude Samsung, Playstore doet het dus niet en zo'n app zeer waarschijnlijk ook niet...\
A: Waarom zou je google services moeten gebruiken? mijn app-idee gaat over gps data, of netwerk locatiegegevens. Kan lineage toch wel bij?\
V: Goed verhaal natuurlijk, maar wat heb je dan aan die data? Een moment heeft een device een ander device gezien. Zonder unieke identifiers kan je dan toch niks met die data, dan wordt het toch gewoon een vergaarbak van zooi?\
A: Ja, maar als we nu de status "verdacht' op een locatie door de tijd heen verlagen tot 'blanco'... dus elk uur iets minder spannend, met kleurtjes of zo, steeds minder rood. Na een dag zou die locatie misschien wel groen zijn, Visualiseren, gamification!!! :D\
V: Ok dan zou dus de app als doel hebben een heatmap van nederland te maken? Ik begreep meer dat ze vanuit de app een contacten onderzoek willen starten.\
A: Dat zoeken ze zelf maar uit als ze dat nodig vinden. Ik wil social distancing spannend maken :-) Maar alle gekheid op een stokje... contactenonderzoek is per definitie een privacyprobleem. Ik kan me voorstellen dat een app dat faciliteert met de heatmap.\
V: Je kan natuurlijk een app maken die begint te zeuren als je op 2 meter van een ander komt :) Das altijd leuk. Vooral als je dan custom sounds kan gebruiken\
V: Ik snap wat je technisch allemaal bedoelt, maar kun je beschrijven hoe in jouw opzet een later geconstateerde besmetting dan terugwerkend gepropageerd wordt?\
A: Geen idee. Misschien moeten we een random unieke identifier van de geïnstalleerde app op het mobiel registreren en ook dat attribuut aan de database toevoegen.\
V: Alle privacy aspecten komen om de hoek kijken bij het later terug matchen als een besmetting geconstateerd wordt, die weer gepropageerd moet worden naar de contacten uit het verleden. Daardoor is er altijd een link tussen jou als individu en de besmette random persoon.\
Het zou mij overigens niks verbazen als er allerlei aanvullende privacy problemen op komen zetten doordat RIVM/Overheid wel wil weten hoe het nu staat met de besmettingen en de verspreiding. Kortom, toegang door anderen zal altijd wenselijk zijn.\
A: Yep, vandaar mijn API-3 voor wetenschap. Maar die zal nooit meer info kunnen krijgen dan locatie, datum, status. Is meer dan genoeg voor de basisfuncties.\
V: Nou ja, dat is dus wel waar het om draait: Dat de overheid niet een centrale database krijgt met identiteiten gekoppeld aan gezondheid. Zoals jij het hier uitlegt, als het zo gemaakt wordt, zou ik het wel installeren op mijn telefoon.\
A: Open Source natuurlijk, je wilt zien wat er vastgelegd kan worden. Leve de API infrastructuur!\
V: Aangezien die app medische gegevens verwerkt: zou het een CE mark nodig hebben? (zie https://www.nweurope.eu/media/3201/ce-marking_infographic-medical-apps.pdf )\
A: Volgens mijn uitwerking: nee :-)\
V: en dan moet je alle geïnfecteerden vangen? En je eigen IC verdedigen? :-)\
V: Begrijpelijk pokemon, velen doen vrijwillig mee in een locatiespel. Dezelfde aanpak zou nu een privacy probleem zijn? Dat snap ik niet.\

Tot zover de discussie op Twitter. Vooralsnog lijkt Covimon-Go niet te gaan vliegen. Het concept bevat geen spannende technische protocollen of zware cryptografische technieken. Maar misschien is het cnocept met de gamification wel aansprekend genoeg om geadopteerd te worden door het publiek. Ik zou zeggen: laat maar komen jullie reacties :)


