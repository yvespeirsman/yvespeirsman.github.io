---
layout: post
title:  "Hoe moeilijk is het Vlaamse regeerakkoord?"
date:   2014-07-26 21:31:52
categories: NLP
---
<p>De publicatie van het Vlaamse regeerakkoord eerder deze week maakte heel 
wat emoties los. Schrijfster Ann De Craemer hekelde de holle woordenbrij 
in een column in De Morgen, maar kreeg al snel lik op stuk van Maurits Vande Reyde, 
beleidsmedewerker van Open Vld. “Zaken worden complexer verwoord omdat ze nu eenmaal 
complexer zijn”, schrijft Vande Reyde over het taalgebruik van onze politici. 
Maar is dat zo? En is het Vlaamse regeerakkoord inderdaad zo moeilijk?<p>

<p>“Moeilijk” is natuurlijk lastig om te definiëren. Toch hoeven we niet alleen
op ons gevoel te vertrouwen. In taalkundig onderzoek worden typisch 
een aantal eenvoudige maten gebruikt die helpen om de complexiteit van een tekst 
aanschouwelijk te maken: de gemiddelde lengte van een woord, de gemiddelde lengte 
van een zin, en de bekendheid van de gebruikte woorden.</p>

<p>We vergelijken het huidige Vlaamse regeerakkoord met drie andere teksten: 
de vorige Vlaamse regeerakkoorden uit 2004 en 2010, en een reeks artikelen 
uit de krant De Standaard. Die  artikelen dienen als representatieve voorbeelden 
van meer populariserende en toch kwalitatief hoogstaande teksten. Bovendien gaan 
ze stuk voor stuk over het meest recente regeerakkoord, wat de vergelijkbaarheid 
tussen de teksten aanzienlijk verhoogt.</p>

<p>De woorden in het meest recente regeerakkoord tellen gemiddeld 6.1 letters. 
Ze zijn nagenoeg even lang als in de vorige regeerakkoorden (6.1 en 6.1), maar 
duidelijk langer dan de woorden van De Standaard (5.3).  De recordhouder in het 
huidige regeerakkoord is “broeikasgasemissiereductiedoelstellingen”, een 
samenstelling van maar liefst 40 letters.</p>

<div id="chart1"></div>

<p>Bij de zinslengte zit er iets meer variatie tussen de regeerakkoorden. 
In 2004 was een gemiddelde zin zo’n 17 woorden lang, in 2010 bijna 20, 
nu iets minder dan 19. Dat zijn telkens erg hoge gemiddeldes. 
In de politieke artikelen van De Standaard die we bekeken, is een zin 
gemiddeld minder dan 14 woorden lang.</p>

<div id="chart2"></div>

<p>Ten slotte keken we ook naar de bekendheid van een woord. Net zoals 
“complexiteit” is ook “bekendheid” geen exact begrip. We gebruikten daarom 
XX artikelen van de Nederlandstalige Wikipedia als referentie. Voor elk 
van de teksten telden we hoeveel procent van de woorden minder dan 20 keer 
voorkwamen in deze artikelen. Opnieuw liggen de percentages van de regeerakkoorden 
opvallend hoger dan die van De Standaard: zo’n 6,5% à 7% van de woorden in de 
regeerakkoorden zijn relatief zeldzaam, tegenover 4% in de Standaard.</p>

<div id="chart3"></div>

<p>Geen van deze drie maten zijn perfecte instrumenten om de complexiteit 
van een tekst te meten: lange zinnen zijn niet noodzakelijk moeilijker, 
net zomin als lange of weinig gebruikte woorden dat hoeven te zijn. 
Toch kunnen ze alledrie, zeker in combinatie, een zinvolle indicatie geven.</p>

<script type="text/javascript" src="https://www.google.com/jsapi"></script>
<script type="text/javascript">
      google.load('visualization', '1', {packages: ['corechart']});
</script>

<script type="text/javascript">
      function drawVisualization() {
        // Create and populate the data table.
        var data = google.visualization.arrayToDataTable([
          ['a','x',{ role: 'style' }],
          ['2004',  6.1, 'orange'],
          ['2010',  6.1, 'orange'],
          ['2014',  6.1, 'orange'],
          ['De Standaard',  5.3, 'blue'],
        ]);
        
        // Create and draw the visualization.
        new google.visualization.ColumnChart(document.getElementById('chart1')).
            draw(data,
                 {title:"Gemiddelde woordlengte",
                  width:600, height:400,
                  vAxis: {minValue: 0},
                  legend: { position: "none" }}
            );
      }
      
      google.setOnLoadCallback(drawVisualization);
</script>

<script type="text/javascript">
      function drawVisualization() {
        // Create and populate the data table.
        var data = google.visualization.arrayToDataTable([
          ['a','x',{ role: 'style' }],
          ['2004',  6.4, 'orange'],
          ['2010',  6.6, 'orange'],
          ['2014',  7.1, 'orange'],
          ['De Standaard',  4.3, 'blue'],
        ]);
        
        // Create and draw the visualization.
        new google.visualization.ColumnChart(document.getElementById('chart3')).
            draw(data,
                 {title:"Percentage zeldzame woorden",
                  width:600, height:400,
                  vAxis: {minValue: 0},
                  legend: { position: "none" }}
            );
      }
      google.setOnLoadCallback(drawVisualization);
</script>

<script type="text/javascript">
      function drawVisualization() {
        // Create and populate the data table.
        var data = google.visualization.arrayToDataTable([
          ['a','x',{ role: 'style' }],
          ['2004',  17.2, 'orange'],
          ['2010',  19.8, 'orange'],
          ['2014',  18.8, 'orange'],
          ['De Standaard',  13.9, 'blue'],
        ]);
        
        // Create and draw the visualization.
        new google.visualization.ColumnChart(document.getElementById('chart2')).
            draw(data,
                 {title:"Gemiddelde zinslengte",
                  width:600, height:400,
                  vAxis: {minValue: 0},
                  legend: { position: "none" }}
            );
      }
      
      google.setOnLoadCallback(drawVisualization);
</script>
