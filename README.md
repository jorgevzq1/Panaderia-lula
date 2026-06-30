# Panaderia-lula <!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Panadería PRO</title>
<style>
body { font-family: Arial; background:#f4f4f4; padding:15px; }
h2 { background:#8b4513; color:white; padding:10px; }
.box { background:white; padding:12px; margin-bottom:12px; border-radius:10px; }
input, select { padding:6px; margin:4px; }
button { padding:7px 10px; background:#28a745; color:white; border:none; cursor:pointer; }
table { width:100%; border-collapse:collapse; margin-top:10px; }
th, td { border:1px solid #ccc; padding:6px; text-align:center; }
.danger { background:red; }
</style>
</head>

<body>

<h2>🍞 Panadería PRO</h2>

<div class="box">
<h3>📦 Inventario</h3>
<input id="name" placeholder="Pan">
<input id="stock" type="number" placeholder="Stock">
<input id="price" type="number" placeholder="Precio">
<button onclick="add()">Agregar</button>

<table id="inv">
<tr><th>Producto</th><th>Stock</th><th>Precio</th></tr>
</table>
</div>

<div class="box">
<h3>🧾 Venta</h3>
<select id="sel"></select>
<input id="qty" type="number" placeholder="Cantidad">
<select id="shift">
<option>***** 1</option>
<option>***** 2</option>
</select>
<button onclick="sell()">Registrar</button>
</div>

<div class="box">
<h3>📊 ****** del Día</h3>
<table id="sales">
<tr><th>Producto</th><th>Cant</th><th>Turno</th><th>Total</th></tr>
</table>
</div>

<div class="box">
<h3>💰 Resumen</h3>
<p id="total"></p>
<p id="profit"></p>
<button onclick="closeDay()" class="danger">Cerrar día</button>
<button onclick="exportCSV()">******** Excel</button>
</div>

<script>

let inv = JSON.parse(localStorage.getItem("inv")||"[]");
let sales = JSON.parse(localStorage.getItem("sales")||"[]");
let history = JSON.parse(localStorage.getItem("history")||"[]");

function save(){
localStorage.setItem("inv",JSON.stringify(inv));
localStorage.setItem("sales",JSON.stringify(sales));
localStorage.setItem("history",JSON.stringify(history));
}

function add(){
inv.push({
name:name.value,
stock:+stock.value,
price:+price.value
});
save(); render();
}

function sell(){
let p = inv.find(i=>i.name==sel.value);
let q = +qty.value;

if(!p || p.stock<q){ alert("Sin stock"); return; }

p.stock -= q;

sales.push({
name:p.name,
qty:q,
price:p.price,
shift:shift.value
});

save(); render();
}

function render(){

invTable();
salesTable();
summary();
selectFill();
}

function invTable(){
inv.innerHTML="<tr><th>Producto</th><th>Stock</th><th>Precio</th></tr>";
inv.forEach(i=>{
inv.innerHTML+=`<tr><td>${i.name}</td><td>${i.stock}</td><td>$${i.price}</td></tr>`;
});
}

function selectFill(){
sel.innerHTML="";
inv.forEach(i=>{
sel.innerHTML+=`<option>${i.name}</option>`;
});
}

function salesTable(){
sales.innerHTML="<tr><th>Producto</th><th>Cant</th><th>Turno</th><th>Total</th></tr>";
}

function summary(){
let total=0;

sales.forEach(s=>{
total+=s.qty*s.price;
sales.innerHTML+=`<tr>
<td>${s.name}</td>
<td>${s.qty}</td>
<td>${s.shift}</td>
<td>$${s.qty*s.price}</td>
</tr>`;
});

totalDiv.innerText="Ventas: $"+total;
profit.innerText="Ganancia: $"+total;
}

function closeDay(){
history.push({date:new Date().toLocaleDateString(),sales});
sales=[];
save(); render();
}

function exportCSV(){
let csv="Producto,Cantidad,Turno,Total\n";
sales.forEach(s=>{
csv+=`${s.name},${s.qty},${s.shift},${s.qty*s.price}\n`;
});

let blob=new Blob([csv],{type:"text/csv"});
let a=document.createElement("a");
a.href=URL.createObjectURL(blob);
a.download="ventas.csv";
a.click();
}

render();

</script>

<p id="total"></p>
<p id="profit"></p>


<div style="color: red; font-size: 12px; width: 600px; margin: 0 auto; text-align: center;">Prueba Word to HTML - <a href="https://wordtohtml.net/site/payment">P&aacute;sate a PRO</a>.</div>
</body>
</html>      
