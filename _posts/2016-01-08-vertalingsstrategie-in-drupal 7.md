---
layout: post
title: Vertalingsstrategie in Drupal 7
---
Dat Drupal volledig in het Engels werd opgemaakt, is gezien het internationale karakter van het CMS best logisch. 

Alle teksten, zoals veldnamen en labels, zijn dan ook standaard in deze taal. Dat betekent echter dat we vaak zelf nog 
heel wat werk hebben wanneer we een website in meerdere talen opstellen.

Hoe we dat op een gestructureerde manier oplossen, lees je in dit artikel.

 
## Bedankt, Drupal Community!

Door de meertaligheid in ons land maken we websites meestal in meerdere talen, waardoor we telkens alles moeten vertalen. 

Gelukkig is er de geweldige Drupal Community, die dit al voor een groot deel van de Drupal Core en de meest gebruikte modules heeft gedaan.

![Drupal translations page]({{ "/assets/Drupal_core_translation_downloads___Translations.jpg" | absolute_url }})

Voor alle teksten die we zelf aanmaken moeten we het vertaalwerk echter ook zelf nog uitvoeren. Bij grote projecten kan 
dit aantal al gauw aardig oplopen; een duidelijke strategie is dus noodzakelijk om gestructureerd aan het werk te gaan.

En dat vertaling belangrijk zijn, geeft deze screenshot in 1 beeld beter mee, dan 1000 woorden.

![Google translate]({{ "/assets/Google_Translate.jpg" | absolute_url }})
 

## Vertalen in Drupal, niet altijd vanzelfsprekend
 De normale gang van zaken is dat wanneer je een string print, je dit doet door gebruik te maken van de t() functie.
  Door de string hier als parameter mee te geven zorg je ervoor dat deze nadien vertaald kan worden. 

{% highlight php %}
print t("Foo to the Bar");
{% endhighlight %}

Vermits de standaardtaal Engels is zou je hier eigenlijk een Engelstalige tekst moeten gebruiken.

Dit zorgt echter voor 2 problemen:

- Engels wordt niet gebruikt
- Engels is de basis
 

## Engels wordt niet gebruikt

In sommige gevallen heeft een klant geen behoefte aan een Engelstalige website.

Stel dat we enkel een Nederlandstalige site moeten maken. Telkens we de t() functie gebruiken, zouden we de tekst dan 
zelf kunnen vertalen naar het Engels. De inhoud hiervoor komt immers vaak uit een wireframe of van de klant, en staat in 
het Nederlands.

Maar deze omzetting kost extra tijd. Bij een enkel labeltje is dit te verwaarlozen, maar voor een paar zinnen ben je toch 
al even bezig.

Een eenvoudigere oplossing is om gewoon alle strings in één keer in het Nederlands in te geven, maar dat wil zeggen dat 
je Nederlandse tekst gaat vermommen als Engels. En vanuit een developerstandpunt is dit not done.

Wat doe je dan als de klant een jaar later tóch een Engelstalige website wil? Of stel dat hij naast de Nederlandstalige 
website ook een Poolse site wil, dan zou de Poolse vertaler ook Nederlands moeten kunnen.

Dit zou een nogal kortzichtige aanpak zijn.

 

## Engels is de basis

Als de klant wél vanaf het begin een Engelstalige website wilt, dan is het ingeven van Engelstalige strings op het 
eerste zicht geen probleem. De developer moet immers geen onnodig vertaalwerk doen en de vertaler kan Engels als basis gebruiken.

De ‘problemen’ beginnen echter op het moment dat je een Engelstalig label toch anders wil weergeven; omdat “Send” 
bijvoorbeeld toch beter bij het contactformulier past dan “Submit”.

Als developer moet je dan in de code eerst op zoek gaan naar `t("Submit")`, dit aanpassen naar `t("Send")`, deze wijziging 
in git zetten en dan online publiceren.

Veel werk om een simpel labeltje te veranderen, niet?

En in veel gevallen stopt het daar niet. Aangezien ‘Submit’ als key voor de vertaling gebruikt werd, moet je nu ook 
alle vertalingen die al gebeurd waren opnieuw uitvoeren - de vorige ben je immers kwijt.

Voor een website met meerdere talen kruipt hier ook al aardig wat tijd in.

 

## De vertaal-placeholder

Om bovenstaande problemen te vermijden en om tijdens het ontwikkelen niet te veel met vertalingen bezig te moeten zijn, 
gebruiken wij vertaal placeholders.

Kort gezegd gaan we in plaats van de uiteindelijke string een omschrijving geven van wat er zou moeten komen.

In het geval van het contactformulier zouden we dan `t("Submit")` vervangen door 

{% highlight php %}
print t(‘CONTACT_FORM.SUBMIT_VALUE’);
{% endhighlight %}
 

Dit vertelt de uiteindelijke vertaler dat het om de value gaat die op de submit button van het contactformulier komt.

De placeholder geeft in dit geval naast de uiteindelijke waardeomschrijving “SUBMIT_VALUE” ook een context mee: 
“CONTACT_FORM”. Dit heeft als voordeel dat een vertaling kan worden aangepast afhankelijk van de context.
In sommige gevallen is dit natuurlijk niet nodig en kan je het contextgedeelte weglaten, waardoor een placeholder herbruikbaar wordt.

In dit geval is de placeholderwaarde veel langer dan wat er uiteindelijk zal komen: “Send”, “Verzenden”, … Maar stel dat 
dit formulier ook een begeleidend tekstje heeft van enkele zinnen. Hiervoor kan je dan een simpele placeholder gebruiken als:

{% highlight php %}
print t("CONTACT_FORM.INTRO_TEXT");
{% endhighlight %}

Extra argumenten kunnen ook nog steeds worden meegegeven, we plaatsen deze er gewoon achteraan bij:

{% highlight php %}
print t("WELKOM_MSG @username", array('@name' => format_username($account));
{% endhighlight %}

Het is aan de vertaler om deze dan op de juiste plaats te zetten.

De keuze om de placeholder uit te voeren in hoofdletters en met underscores is er eentje uit praktische overwegingen. 
Het valt namelijk goed op, wat het voor de vertaler makkelijker maakt om strings te vinden die nog vertaald moeten worden.

## Elke taal is een vertaling

Dit lost ons eerste probleem wel op, maar wat als een klant ook een Engelstalige site wil?

Wij gaan uit van het principe dat elke taal op de site een vertaling is, inclusief het Engels. Het standaard Engels (en) 
beschouwen we als niets meer dan een register van onze placeholders. Willen we een Engelstalige vertaling toevoegen, dan 
maken we gewoon een extra taal aan.

Je zou hiervoor bijvoorbeeld de bestaande taal “Engels,brits” kunnen gebruiken, maar wij kiezen er meestal voor om een 
nieuwe taal toe te voegen.

 
![Google translate]({{ "/assets/Selection_002.png" | absolute_url }})
 

Binnen deze nieuwe taal kan een vertaler doen wat hij wil met de placeholders, zonder dat een developer moet tussenkomen. 
En vermits Engels (en) steeds de fallback taal is moet hij ook alleen maar de placeholders vertalen.

Om ervoor te zorgen dat bezoekers van de site niet op de taal Engels (en) uitkomen, gebruiken we de Language access 
module om deze uit te schakelen voor bepaalde rollen.

Indien je om een bepaalde reden toch een andere taal als fallback wil gebruiken, kan je hiervoor ook de Language 
fallback module gebruiken.

 
## De voor- en nadelen
\+ Minder tijd verspillen aan ongebruikte vertalingen.

\+ De context is voor de vertaler al uit de placeholder af te leiden. Bij gebruik van po files kan een vertaling
 accurater gebeuren, zonder dat het specifieke gebruik op de site gekend moet zijn.

\+ Engelstalige strings kunnen aangepast worden zonder tussenkomst van de developer.

\- Extra taal nodig om Engels kunnen gebruiken.

\- Niet alles werkt met de placeholders. Je hebt dus normale strings en placeholders door elkaar.

 
## Drupal 8
Een aantal van deze problemen zijn in Drupal 8 opgelost door een verbeterde aanpak voor vertalingen.

