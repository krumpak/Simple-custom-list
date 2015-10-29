# Simple-custom-list 

JavaScript & Bootstrap

<!doctype html>
<html>
<head>
<meta charset="UTF-8">
<title>Seznam navideznih prijateljev</title>
<link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">
<script>
function SeznamPrijateljev(){
	if (localStorage.getItem("prijatelji") === null) {
		var seznam = [];
		ShraniSeznam(seznam);
		IzpisiSeznam();
	}else{
		IzpisiSeznam();
	}
}
function PreberiSeznam(){
	var podatki = localStorage.getItem("prijatelji");
	return JSON.parse(podatki);
}
function IzpisiSeznam(){
	var NizPodatkov = PreberiSeznam();
	if( NizPodatkov[0] != '' && NizPodatkov.length > 0 ){
		var izpis='<ol>';
		for(var i = 0; i < NizPodatkov.length; i++){
			izpis += "<li>" + NizPodatkov[i] + " <span class='glyphicon glyphicon-trash' onClick='IzbrisiPrijatelja(" + i + ");'></span></li>";
		}
		izpis+='</ol>'
		document.getElementById("seznam").innerHTML=izpis;
		timer();
	} else {
		document.getElementById("seznam").innerHTML='<div class="alert alert-info">Seznam je prazen!</div>';
		timer();
	}
}
function IzbrisiPrijatelja(IdPrijatelja){
	var NizPodatkov = PreberiSeznam();
	var Odstranjen = NizPodatkov.splice(IdPrijatelja, 1);
	ShraniSeznam(NizPodatkov);
	IzpisiSeznam();
	document.getElementById("obvestila").innerHTML='<div class="alert alert-warning">Prijatelj je izbrisan!</div>';
}
function ShraniSeznam(NizPodatkov){
	localStorage.setItem("prijatelji", JSON.stringify(NizPodatkov));
}
function DodajPrijatelja(){
	var ImeNovegaPrijatelja = document.getElementById("NovPrijatelj").value;
	if( ImeNovegaPrijatelja != '' ){
		document.getElementById("NovPrijatelj").value = '';
		var ObstojeciPrijatelji = PreberiSeznam();
		ObstojeciPrijatelji.push(ImeNovegaPrijatelja);
		ShraniSeznam(ObstojeciPrijatelji);
		IzpisiSeznam();
		document.getElementById("obvestila").innerHTML='<div class="alert alert-success">Prijatelj je dodan na seznam!</div>';
	}else{
		document.getElementById("obvestila").innerHTML='<div class="alert alert-danger">Prazno ime, vpišite ime prijatelja!</div>';
		timer();
	}
}
function PritisniEnter(dogodek){
	var tipka=dogodek.keyCode || dogodek.which;
	if(tipka==13){ DodajPrijatelja(); }
}
function OdstraniVsePrijatelje(){
	ShraniSeznam([]);
	IzpisiSeznam();
	document.getElementById("obvestila").innerHTML='<div class="alert alert-warning">Vsi prijatelji so izbrisani!</div>';
}
function timer(){
  setTimeout(
    function() {
      document.getElementById('obvestila').innerHTML='';
    }, 5000);	
}
</script>
<style>
html, body { width: 100%; height: 100%; margin:0; padding:0; }
.container-fluid { padding-top: 50px; height: calc(100% - 25px); }
li { font-size: 25px; }
.glyphicon-trash { font-size:12px; }
#seznam { margin-top:0px; }
footer { text-align: center; } .copy-left { display: inline-block; -moz-transform: scaleX(-1); -o-transform: scaleX(-1); -webkit-transform: scaleX(-1); transform: scaleX(-1); }
</style>
</head>
<body onLoad="SeznamPrijateljev();">
<div class="container-fluid">
    <div class="row">
        <div class="col-md-4 col-md-offset-4">
            <div class="input-group">
                <input type="text" id="NovPrijatelj" class="form-control" placeholder="Dodaj ime prijatelja" onKeyPress="PritisniEnter(event);" />
                <div class="input-group-addon" onClick="DodajPrijatelja();"><span class="glyphicon glyphicon-plus"></span></div>
            </div>
            <div class="input-group-addon" onClick="OdstraniVsePrijatelje();"><span class="glyphicon glyphicon-trash"></span></div>
            <div id="obvestila"></div>
            <div id="seznam"></div>
        </div>
    </div>
</div><footer><span class="copy-left">©</span> 2015 Gorazd Krumpak</footer>
</body>
</html>
