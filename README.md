<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ALDO STORE PRO</title>

<style>
body{
margin:0;
font-family:Arial;
background:#2a5298;
color:white;
}

header{
text-align:center;
padding:15px;
background:#ff4757;
}

/* TABS */
.tabs{
text-align:center;
margin:10px;
}

.tabs button{
padding:10px;
margin:5px;
border:none;
border-radius:8px;
cursor:pointer;
}

/* PRODUCTS */
.products{
display:flex;
gap:15px;
overflow-x:auto;
padding:15px;
scroll-snap-type:x mandatory;
scroll-behavior:smooth;
}

.products::-webkit-scrollbar{
display:none;
}

.card{
min-width:250px;
flex:0 0 auto;
scroll-snap-align:start;
background:white;
color:black;
border-radius:12px;
overflow:hidden;
position:relative;
}

.card img{
width:100%;
height:180px;
object-fit:cover;
}

.card-body{padding:10px}

.price{color:red;font-weight:bold}

button.buy{
width:100%;
padding:10px;
margin-top:5px;
border:none;
border-radius:8px;
background:#25D366;
color:white;
cursor:pointer;
}

/* DELETE BUTTON */
.del{
position:absolute;
top:5px;
right:5px;
background:red;
color:white;
border:none;
border-radius:5px;
cursor:pointer;
}

/* OVERLAY */
.overlay{
position:fixed;
top:0;left:0;right:0;bottom:0;
background:rgba(0,0,0,0.85);
display:none;
justify-content:center;
align-items:center;
flex-direction:column;
z-index:999;
}

.overlay img,
.overlay video{
max-width:90%;
max-height:80%;
margin:10px;
border-radius:10px;
}

/* ADMIN */
.adminBox{
position:fixed;
top:0;left:0;right:0;bottom:0;
background:white;
color:black;
display:none;
flex-direction:column;
padding:20px;
z-index:1000;
}

.adminBox input,select{
margin-bottom:10px;
padding:8px;
}

.adminBox button{
padding:10px;
margin:5px;
border:none;
border-radius:8px;
cursor:pointer;
}
</style>
</head>

<body>

<header>
<h2>👟 ALDO STORE PRO</h2>
<p>Admin Panel + Save Products</p>
</header>

<!-- TABS -->
<div class="tabs">
<button onclick="filter('all')">All</button>
<button onclick="filter('shoes')">👟 Shoes</button>
<button onclick="filter('clothes')">👕 Clothes</button>
<button onclick="filter('accessories')">💍 Accessories</button>

<button onclick="openAdmin()">⚙️ Admin</button>
</div>

<!-- PRODUCTS -->
<div class="products" id="products"></div>

<!-- GALLERY -->
<div class="overlay" id="galleryBox" onclick="closeGallery()">
<div id="galleryContent"></div>
</div>

<!-- ADMIN -->
<div class="adminBox" id="adminBox">

<h2>⚙️ Admin Panel</h2>

<input id="pname" placeholder="Product Name">
<input id="pprice" placeholder="Price">

<select id="pcategory">
<option value="shoes">Shoes</option>
<option value="clothes">Clothes</option>
<option value="accessories">Accessories</option>
</select>

<input id="pimg" placeholder="Image URL">

<button onclick="addProduct()">➕ Add Product</button>
<button onclick="closeAdmin()">❌ Close</button>

</div>

<script>

/* ===== STORAGE ===== */
let products = JSON.parse(localStorage.getItem("products")) || [];

/* FILTER */
let currentFilter="all";
let currentProducts=[];

function filter(type){
currentFilter=type;
render();
}

/* SAVE */
function save(){
localStorage.setItem("products",JSON.stringify(products));
}

/* RENDER */
function render(){

let html="";

currentProducts = products.filter(p=>{
return currentFilter==="all" || p.category===currentFilter;
});

currentProducts.forEach((p,i)=>{

html+=`
<div class="card">

<button class="del" onclick="deleteProduct(${i})">X</button>

<img src="${p.media[0].src}">

<div class="card-body">
<h3>${p.name}</h3>
<p class="price">${p.price} €</p>

<button class="buy" onclick="order(${JSON.stringify(p.name)},${p.price})">
📱 WhatsApp
</button>

</div>
</div>
`;
});

document.getElementById("products").innerHTML=html;
}

render();

/* DELETE PRODUCT */
function deleteProduct(i){
products.splice(i,1);
save();
render();
}

/* WHATSAPP */
function order(name,price){

let msg = `🛒 ORDER:
Product: ${name}
Price: ${price}€`;

let url = "https://wa.me/393510798667?text=" + encodeURIComponent(msg);

window.open(url,"_blank");
}

/* ADMIN */
function openAdmin(){
document.getElementById("adminBox").style.display="flex";
}

function closeAdmin(){
document.getElementById("adminBox").style.display="none";
}

/* ADD PRODUCT */
function addProduct(){

let name=document.getElementById("pname").value;
let price=document.getElementById("pprice").value;
let category=document.getElementById("pcategory").value;
let img=document.getElementById("pimg").value;

if(!name||!price||!img){
alert("Fill all fields");
return;
}

products.push({
name:name,
price:Number(price),
category:category,
media:[{type:"image",src:img}]
});

save();
render();
closeAdmin();
}

/* INIT */
render();

</script>

</body>
</html>
