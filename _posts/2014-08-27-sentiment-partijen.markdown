---
layout: post
title:  "Titel"
date:   2014-08-27 11:31:52
tags: NLP, sentiment, politics
comments: true
---

<p class="first">Eén van de populairste toepassingen van taaltechnologie in de afgelopen
jaren is <em>sentiment analysis</em>. Sentiment analysis laat toe om automatisch te bepalen
of een zin een positieve of negatieve mening uitdrukt. Voor bedrijven is dit een goudmijn: 
dankzij de vele meningen op blogs en sociale media als Twitter en Facebook, kunnen zij 
<a href="http://www.nytimes.com/2009/08/24/technology/internet/24emotion.html?_r=1">snel 
een inzicht krijgen in hoe klanten reageren</a> op hun producten, campagnes, enzovoort. Ook 
de media en politiek zijn geïnteresseerd, omdat zij zo in real-time de 
<a href="http://usatoday30.usatoday.com/news/politics/story/2012-08-01/twitter-political-index/56649678/1">
populariteit van politici</a> kunnen meten.</p>

<p class="nomargin">Voor dit experiment gebruiken we de <a href="http://www.clips.ua.ac.be/pages/pattern-nl">Pattern</a>
software, ontwikkeld door de Universiteit van Antwerpen. Pattern haalt een accuraatheid van 82%
op zinnen in boekrecensies. Dat is verre van perfect, maar sentiment analysis is 
dan ook geen eenvoudige opgave. Kijk bijvoorbeeld naar de volgende zinnen en hun automatische scores, gerangschikt van meest positief naar meest negatief:</p>

<blockquote>
Dat is een uitstekend idee. (0.9)<br />
Dat is een goed idee. (0.55)<br />
Dat is geen slecht idee. (0.35)<br />
Dat is geen goed idee. (-0.275)<br />
Dat is een slecht idee. (-0.7)<br />
Dat is een heel slecht idee. (-1.0)<br />
</blockquote>

<p class="noindent">Om die scores te berekenen, moet de software met heel wat factoren rekening houden. Bepaalde
woorden (“goed”, “slecht”, “uitstekend”) hebben een inherente positieve of negatieve betekenis,
terwijl andere woorden die betekenis kunnen versterken (“heel”) of juist omdraaien (“niet”). Zoals 
de scores bewijzen, doet Pattern dat in deze zinnen met brio.</p> 


<p>Eigenlijk is een verkiezingsprogramma een vorm van recensie. Enerzijds geeft een partij haar
visie op de maatschappij: ze haalt aan waar die goed georganiseerd is, en waar ze 
verbeterd zou kunnen worden. Anderzijds beoordeelt een programma ook de aftredende regering:
het legt uit waar die goed of slecht werk heeft verricht, en wat de partij anders zou doen. Door
die meningen te analyseren, kunnen we dus leren wat een partij over onze maatschappij denkt, 
en hoe ze zich tegenover de aftredende regering verhoudt. 
</p>

<div id="chart_div" style="width: 700px; height: 500px;"></div>

<p class="nomargin">
Ten eerste: CD&V levert drie keer het meest positieve partijprogramma af. 

Bij alledrie de verkiezingen was de CD&V de belangrijkste partij in de aftredende regering. 
Ze was het met andere woorden aan zichzelf verplicht om een positief verkiezingsprogramma te
schrijven. Zoniet haalde ze de verdiensten van haar eigen regering onderuit. Die houding
merk je bijvoorbeeld in één van de meest positieve zinnen uit het programma van de CD&V voor
de afgelopen verkiezingen: </p>

<blockquote>
En dit is vandaag het prachtige resultaat dat we hebben behaald. (1.0)
</blockquote>

<p class="noindent">Hoewel het geen algemeen patroon is, zie je bijvoorbeeld bij de verkiezingen
van 2009 mooi dat de partijen uit de aftredende Vlaamse regering (CD&V, Sp.a en Open Vld) de meest
positieve verkiezingsprogramma's schrijven. 
</p>

<p class="nomargin">
Ten tweede: Vlaams Belang heeft een patent op het meest negatieve partijprogramma. Als eeuwige
oppositiepartij levert het Vlaams Belang de meeste kritiek. Die is enerzijds gericht op het werk
van de aftredende regering, en anderzijds op het algemene functioneren van onze maatschappij. Dat blijkt
bijvoorbeeld uit twee van de meest negatieve zinnen uit het verkiezingsprogramma van 2014:</p>

<blockquote>
Wordt dan al gereageerd op misdrijven, dan gebeurt dit in vele gevallen veel te laat en ondermaats. (-0.7)<br />
De Europese Unie is echter een supranationale instelling die bemoeizuchtig en expansionistisch handelt en steeds meer macht naar zich toetrekt. (-0.65)<br />
</blockquote>

<p>
Ten derde: met uitzondering van Groen presenteerden alle partijen het negatiefste regeerakkoord
voor de federale verkiezingen van 2010. Vooral het contrast met de Vlaamse verkiezingen van 2009
is opvallend; dat met de samenvallende verkiezingen van 2014 is iets minder uitgesproken. De redenen hiervoor zijn moeilijker te achterhalen. Beschouwen de meeste partijen het Belgische niveau
als problematischer dan het Vlaamse? Of zijn de moeilijkste bewindsdomeinen toevallig federaal? 
Een diepere politieke analyse van de verkiezingsprogramma’s is nodig om het antwoord bloot te leggen.
</p>

<p>Ten slotte moeten we hier zoals steeds waarschuwen voor een blinde aanvaarding van 
automatisch behaalde resultaten. De aangehaalde voorbeelden tonen dat Pattern de polariteit van
een zin meestal juist inschat, maar de software is natuurlijk niet perfect. Zo kregen
de volgende zinnen de verkeerde polariteit mee:</p>

<blockquote>
Het Vlaams Belang hekelt al jaren de mensonterende wijze waarop geïnterneerden in dit land behandeld worden. (0.84)
De belastingdruk op arbeid moet dalen van 42% naar 40% in 2020. (-0.65)
</blockquote>

<script type="text/javascript" src="https://www.google.com/jsapi?autoload={'modules':[{'name':'visualization','version':'1','packages':['corechart']}]}"></script>

<script type="text/javascript">
      google.setOnLoadCallback(drawChart);
      function drawChart() {
        var data = google.visualization.arrayToDataTable([
          ['Year', 'Groen', 'Sp.a', 'CD&V','Open Vld','N-VA', 'Vlaams Belang'],
          ['2009',  0.074, 0.109, 0.122, 0.085, 0.077, 0.058],
          ['2010',  0.083, 0.052, 0.088, 0.041, 0.059, 0.038],
          ['2014',  0.070, 0.085, 0.101, 0.093, 0.071, 0.042],
        ]);

        var options = {
            colors: ['green','red','orange','blue','yellow','brown']
        };

        var chart = new google.visualization.LineChart(document.getElementById('chart_div'));

        chart.draw(data, options);
      }
</script>
