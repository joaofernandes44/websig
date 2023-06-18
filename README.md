Trabalho Prático de Websig - 4506

Utilização do Leaflet/Marker Clusters para demonstrar os casos de Covid-19 na região do continente por 10 mil habitantes

Para este projecto foi utilizada a libraria Leaflet. O leaflet é uma libraria open-source de javascript bastante leve, que facilita a construção de mapas interativos baseados na web. Esta libraria destingue-se por ter a facilidade de utilização para os utilizadores, sendo que pode ser estendida com plugins open-source criados pela comunidade.

O setup:
1- Criar uma pasta do projecto
2- Criar pastas dentro do projecto chamadas "data" (onde se coloca os json), "dist" (ficheiros js), "img" (dados em forma de imagens) e por fim "js" onde se mete os ficheiros do leaflet.
3- Criar um ficheiro index.html - Este ficheiro irá ser a nossa base, onde irá ser criado o código responsável pelos mapas.


Código:

  	<div id="map"></div>
  	<script src ="data/casospor10khab.geojson"></script>
  	<script src ="data/casos.geojson"></script>

  Para importar os dados é necessário dar a source ao script, como anteriormente referido, para a pasta "data". Nestes casos é necessário entrar dentro dos geojsons para dar o nome à variável da seguinte forma:
  
  ![imagem](https://github.com/joaofernandes44/websig/assets/134423694/9e9223bd-c5be-40fb-b86a-d9bb1b046dbe)

  De seguida começa-se por meter o mapa de base, aquele onde vão entrar as variáveis que se pretendem meter. Este passo não é obrigatório, mas é recomendando da maneira as variáveis inseridas serem mais fáceis de visualizar.

   	var map = L.map('map').setView([38.749, -9.155], 13);
  	var osm = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
		maxZoom: 19,
		attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
	}).addTo(map);

Neste caso mete-se o openstreetmaps.

Cores associadas às classes de legenda:

	function getcor10k (d) {
  	return d > 30 ? '#BD0026' :
  			d > 10 ? '#FFAC1C' :
  			d > 1 ? '#FFBF00' :
  			d >= 0 ? '#228B22' :
  				'#FFFFFF';
Neste caso os dados foram divididos em 4 classes, acima de 30, acima de 10, acima de 1 e por fim igual a 0. Aqui as cores têm de ser em formato hexadecimal.

Para se definir o estilo dos dados é o seguinte bloco de código:
  	
   	function estilo10k(feature) {
  		return {
  			weight: 1,   -> Espessura das linhas
  			opacity: 1,  -> Transparências
  			color: 'white',  -> Cor
  			dashArray:'3',  -> Estilo do Traço
  			fillOpacity: 1.0 ,
  			fillColor: getcor10k(feature.properties.Casos_23_1)
  		};

Para se poder carregar na layer e dizer o valor é necessário o seguinte:

  	function zoomToFeature(e) {
  		map.fitBounds(e.target.getBounds());
  	}
  	
  	function emcadaConcelho(feature,layer) {
  		layer.on({
  			click: zoomToFeature
  		});
  	
  	}

  Para se poder adicionar a legenda é:
  
  
  	L.control.scale({imperial:false}).addTo(map);
	
Neste caso está em métrica, mas para se meter imperial bastava meter true.

Para se meter a legenda é o seguinte:

	var legend = L.control({position: 'bottomright'});
	legend.onAdd = function (map) {

	legend.onAdd = function (map) {

    		var div = L.DomUtil.create('div', 'info legend'),
        		grades = [0, 1, 10, 30],
        		labels = [];

          for (var i = 0; i < grades.length; i++) {
        		div.innerHTML +=
           		 '<i style="background:' + getcor10k(grades[i] + 1) + '"></i> ' +
           		 (grades[i] === 0 ? grades[i] : (grades[i] + 1) + (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+'))
    		}
return div;
	};

	legend.addTo(map);
 
  Aqui cria-se o numero de quantidade de intervalos para se poder criar, este é associado à legenda que mais acima foi associada às cores.

  Para se poder criar overlays é o seguinte código:

  	var baseMaps = {
    	"OpenStreetMap": osm  
	};

	var overlayMaps = {  
    "Casos de COVID po 10 mil hab": casos10kLayer
	};	

	var layerControl = L.control.layers(baseMaps, overlayMaps).addTo(map);	

No basemaps mete-se os mapas de base, neste caso o openstreetaos, enquanto que nos overlaymaps é as layers que ficam por de cima.

O resultado final é o seguinte: https://joaofernandes44.github.io/websig/




Neste readme não se encontra tudo explicito, apenas o especifico deste projecto.
Para mais informações:
https://leafletjs.com/examples.html




  



