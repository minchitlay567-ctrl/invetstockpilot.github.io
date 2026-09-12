<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>StockPilot Inventory V2</title>

<style>
* {
box-sizing: border-box;
}

body {
margin: 0;
font-family: Arial, sans-serif;
background: #f3f4f6;
color: #1f2937;
}

header {
background: #111827;
color: white;
padding: 18px 25px;
display: flex;
justify-content: space-between;
align-items: center;
}

header h1 {
margin: 0;
font-size: 24px;
}

header button {
background: #ef4444;
color: white;
border: none;
padding: 9px 14px;
border-radius: 6px;
cursor: pointer;
}

.container {
max-width: 1400px;
margin: auto;
padding: 20px;
}

.hidden {
display: none !important;
}

/* LOGIN */

#loginPage {
min-height: 100vh;
display: flex;
justify-content: center;
align-items: center;
background: linear-gradient(135deg,#111827,#374151);
}

.login-box {
background: white;
width: 350px;
padding: 35px;
border-radius: 12px;
box-shadow: 0 15px 40px rgba(0,0,0,.3);
}

.login-box h2 {
text-align: center;
margin-bottom: 25px;
}

/* NAVIGATION */

nav {
background: white;
padding: 10px 20px;
box-shadow: 0 2px 6px rgba(0,0,0,.1);
display: flex;
gap: 8px;
flex-wrap: wrap;
}

nav button {
border: none;
background: #e5e7eb;
padding: 10px 15px;
border-radius: 6px;
cursor: pointer;
}

nav button.active {
background: #2563eb;
color: white;
}

/* FORMS */

input,
select,
textarea {
width: 100%;
padding: 10px;
margin-top: 5px;
margin-bottom: 14px;
border: 1px solid #d1d5db;
border-radius: 6px;
}

label {
font-weight: bold;
font-size: 14px;
}

button.primary {
background: #2563eb;
color: white;
border: none;
padding: 11px 16px;
border-radius: 6px;
cursor: pointer;
}

button.success {
background: #16a34a;
color: white;
border: none;
padding: 11px 16px;
border-radius: 6px;
cursor: pointer;
}

button.warning {
background: #d97706;
color: white;
border: none;
padding: 11px 16px;
border-radius: 6px;
cursor: pointer;
}

button.danger {
background: #dc2626;
color: white;
border: none;
padding: 8px 12px;
border-radius: 5px;
cursor: pointer;
}

button.secondary {
background: #6b7280;
color: white;
border: none;
padding: 10px 14px;
border-radius: 6px;
cursor: pointer;
}

/* DASHBOARD */

.cards {
display: grid;
grid-template-columns: repeat(auto-fit,minmax(200px,1fr));
gap: 15px;
margin-bottom: 25px;
}

.card {
background: white;
padding: 20px;
border-radius: 10px;
box-shadow: 0 2px 8px rgba(0,0,0,.08);
}

.card h3 {
margin: 0;
font-size: 14px;
color: #6b7280;
}

.card .number {
font-size: 28px;
font-weight: bold;
margin-top: 10px;
}

/* TABLE */

.table-container {
overflow-x: auto;
background: white;
border-radius: 10px;
}

table {
width: 100%;
border-collapse: collapse;
}

th,
td {
padding: 12px;
border-bottom: 1px solid #e5e7eb;
text-align: left;
}

th {
background: #f9fafb;
}

.product-photo {
width: 55px;
height: 55px;
object-fit: cover;
border-radius: 7px;
}

.low-stock {
color: #dc2626;
font-weight: bold;
}

.in-stock {
color: #16a34a;
}

/* GRID */

.form-grid {
display: grid;
grid-template-columns: repeat(2,1fr);
gap: 15px;
}

.panel {
background: white;
padding: 20px;
border-radius: 10px;
margin-bottom: 20px;
box-shadow: 0 2px 8px rgba(0,0,0,.08);
}

/* SCANNER */

#scannerBox {
position: relative;
max-width: 500px;
}

#scannerVideo {
width: 100%;
border-radius: 10px;
background: black;
}

.scanner-line {
position: absolute;
top: 50%;
left: 10%;
width: 80%;
height: 2px;
background: red;
}

/* ALERT */

.alert {
padding: 12px;
border-radius: 7px;
margin-bottom: 10px;
background: #fee2e2;
color: #991b1b;
}

/* MOBILE */

@media(max-width:700px) {
.form-grid {
grid-template-columns: 1fr;
}

header {
flex-direction: column;
gap: 10px;
align-items: flex-start;
}
}
</style>
</head>

<body>

<!-- LOGIN -->

<div id="loginPage">

<div class="login-box">

<h2>📦 StockPilot</h2>

<label>Username</label>
<input id="loginUsername" placeholder="Username">

<label>Password</label>
<input id="loginPassword" type="password" placeholder="Password">

<button class="primary" style="width:100%" onclick="login()">
Login
</button>

<p style="text-align:center;color:#6b7280;font-size:13px">
Default password: admin123
</p>

</div>

</div>


<!-- APPLICATION -->

<div id="app" class="hidden">

<header>

<h1>📦 StockPilot Inventory</h1>

<div>
<span id="currentUser"></span>
<button onclick="logout()">Logout</button>
</div>

</header>


<nav>

<button onclick="showPage('dashboard')" class="active">
Dashboard
</button>

<button onclick="showPage('inventory')">
Inventory
</button>

<button onclick="showPage('transactions')">
Transactions
</button>

<button onclick="showPage('suppliers')">
Suppliers
</button>

<button onclick="showPage('scanner')">
Barcode Scanner
</button>

<button onclick="showPage('alerts')">
Low Stock
</button>

<button onclick="showPage('settings')">
Settings
</button>

</nav>


<div class="container">

<!-- DASHBOARD -->

<section id="dashboard" class="page">

<h2>Dashboard</h2>

<div class="cards">

<div class="card">
<h3>Products</h3>
<div class="number" id="totalProducts">0</div>
</div>

<div class="card">
<h3>Units in Stock</h3>
<div class="number" id="totalUnits">0</div>
</div>

<div class="card">
<h3>Inventory Cost</h3>
<div class="number" id="inventoryCost">$0.00</div>
</div>

<div class="card">
<h3>Retail Value</h3>
<div class="number" id="retailValue">$0.00</div>
</div>

<div class="card">
<h3>Potential Profit</h3>
<div class="number" id="potentialProfit">$0.00</div>
</div>

<div class="card">
<h3>Low Stock Items</h3>
<div class="number" id="lowStockCount">0</div>
</div>

</div>


<div class="panel">

<h3>Recent Transactions</h3>

<div class="table-container">

<table>

<thead>
<tr>
<th>Date</th>
<th>Product</th>
<th>Type</th>
<th>Quantity</th>
<th>User</th>
</tr>
</thead>

<tbody id="recentTransactions"></tbody>

</table>

</div>

</div>

</section>


<!-- INVENTORY -->

<section id="inventory" class="page hidden">

<h2>Inventory</h2>

<div class="panel">

<h3>Add / Edit Product</h3>

<input type="hidden" id="editId">

<div class="form-grid">

<div>
<label>Product Name</label>
<input id="productName">
</div>

<div>
<label>Barcode</label>
<input id="productBarcode">

<button type="button"
class="secondary"
onclick="openScannerForProduct()">
📷 Scan Barcode
</button>

</div>

<div>
<label>SKU</label>
<input id="productSku">
</div>

<div>
<label>Supplier</label>
<select id="productSupplier"></select>
</div>

<div>
<label>Purchase Cost</label>
<input id="purchaseCost" type="number" step="0.01">
</div>

<div>
<label>Selling Price</label>
<input id="sellingPrice" type="number" step="0.01">
</div>

<div>
<label>Quantity</label>
<input id="productQuantity" type="number">
</div>

<div>
<label>Reorder Point</label>
<input id="reorderPoint" type="number" value="5">
</div>

<div>
<label>Category</label>
<input id="productCategory">
</div>

<div>
<label>Location</label>
<input id="productLocation">
</div>

</div>

<label>Product Photo</label>
<input id="productPhoto" type="file" accept="image/*">

<img id="photoPreview"
class="product-photo hidden">

<br><br>

<button class="success" onclick="saveProduct()">
Save Product
</button>

<button class="secondary" onclick="clearProductForm()">
Clear
</button>

</div>


<div class="panel">

<input
id="inventorySearch"
placeholder="🔎 Search products by name, barcode, SKU, supplier..."
oninput="renderInventory()">

<br>

<button class="primary" onclick="exportCSV()">
Export CSV
</button>

</div>


<div class="table-container">

<table>

<thead>

<tr>
<th>Photo</th>
<th>Product</th>
<th>Barcode</th>
<th>SKU</th>
<th>Supplier</th>
<th>Cost</th>
<th>Price</th>
<th>Qty</th>
<th>Status</th>
<th>Actions</th>
</tr>

</thead>

<tbody id="inventoryTable"></tbody>

</table>

</div>

</section>


<!-- TRANSACTIONS -->

<section id="transactions" class="page hidden">

<h2>Inventory Transactions</h2>

<div class="panel">

<div class="form-grid">

<div>

<label>Transaction Type</label>

<select id="transactionType">

<option value="IN">IN — Receive Stock</option>

<option value="OUT">OUT — Issue Stock</option>

</select>

</div>

<div>

<label>Product</label>

<select id="transactionProduct"></select>

</div>

<div>

<label>Quantity</label>

<input id="transactionQuantity" type="number" min="1">

</div>

<div>

<label>Reason</label>

<input id="transactionReason"
placeholder="Receiving, sale, damage, adjustment...">

</div>

</div>

<button class="success"
onclick="processTransaction()">

Save Transaction

</button>

</div>


<div class="table-container">

<table>

<thead>

<tr>
<th>Date</th>
<th>Product</th>
<th>Type</th>
<th>Quantity</th>
<th>Reason</th>
<th>User</th>
</tr>

</thead>

<tbody id="transactionTable"></tbody>

</table>

</div>

</section>


<!-- SUPPLIERS -->

<section id="suppliers" class="page hidden">

<h2>Suppliers</h2>

<div class="panel">

<div class="form-grid">

<div>
<label>Supplier Name</label>
<input id="supplierName">
</div>

<div>
<label>Contact Person</label>
<input id="supplierContact">
</div>

<div>
<label>Phone</label>
<input id="supplierPhone">
</div>

<div>
<label>Email</label>
<input id="supplierEmail">
</div>

</div>

<button class="success" onclick="saveSupplier()">
Add Supplier
</button>

</div>


<div class="table-container">

<table>

<thead>

<tr>
<th>Supplier</th>
<th>Contact</th>
<th>Phone</th>
<th>Email</th>
<th>Products</th>
<th>Action</th>
</tr>

</thead>

<tbody id="supplierTable"></tbody>

</table>

</div>

</section>


<!-- SCANNER -->

<section id="scanner" class="page hidden">

<h2>Barcode Scanner</h2>

<div class="panel">

<p>
Point your phone camera at a barcode.
</p>

<div id="scannerBox">

<video id="scannerVideo"
autoplay
playsinline></video>

<div class="scanner-line"></div>

</div>

<br>

<button class="primary" onclick="startScanner()">
📷 Start Scanner
</button>

<button class="danger" onclick="stopScanner()">
Stop Scanner
</button>

<p id="scannerResult"></p>

</div>

</section>


<!-- LOW STOCK -->

<section id="alerts" class="page hidden">

<h2>Low Stock Alerts</h2>

<div id="lowStockList"></div>

</section>


<!-- SETTINGS -->

<section id="settings" class="page hidden">

<h2>Settings</h2>

<div class="panel">

<h3>Change Password</h3>

<input
id="newPassword"
type="password"
placeholder="New password">

<button class="primary"
onclick="changePassword()">

Change Password

</button>

</div>


<div class="panel">

<h3>Backup & Restore</h3>

<button class="primary"
onclick="backupData()">

Download Backup

</button>

<br><br>

<input
type="file"
id="restoreFile"
accept=".json">

<button class="warning"
onclick="restoreData()">

Restore Backup

</button>

</div>


<div class="panel">

<h3>Danger Zone</h3>

<button class="danger"
onclick="deleteEverything()">

Delete All Inventory Data

</button>

</div>

</section>

</div>

</div>


<script>

/* =========================
DATA
========================= */

let products =
JSON.parse(localStorage.getItem("stockpilot_products") || "[]");

let suppliers =
JSON.parse(localStorage.getItem("stockpilot_suppliers") || "[]");

let transactions =
JSON.parse(localStorage.getItem("stockpilot_transactions") || "[]");

let password =
localStorage.getItem("stockpilot_password") || "admin123";

let currentUser = "";

let scannerStream = null;
let scannerTimer = null;

let scannerTarget = null;


/* =========================
LOGIN
========================= */

function login() {

const username =
document.getElementById("loginUsername").value.trim();

const pass =
document.getElementById("loginPassword").value;

if (!username) {
alert("Enter username.");
return;
}

if (pass !== password) {
alert("Incorrect password.");
return;
}

currentUser = username;

document.getElementById("loginPage")
.classList.add("hidden");

document.getElementById("app")
.classList.remove("hidden");

document.getElementById("currentUser")
.textContent = "User: " + currentUser;

refreshAll();
}


function logout() {

stopScanner();

currentUser = "";

document.getElementById("app")
.classList.add("hidden");

document.getElementById("loginPage")
.classList.remove("hidden");

}


/* =========================
NAVIGATION
========================= */

function showPage(page) {

document.querySelectorAll(".page")
.forEach(p => p.classList.add("hidden"));

document.getElementById(page)
.classList.remove("hidden");

document.querySelectorAll("nav button")
.forEach(b => b.classList.remove("active"));

if (page === "scanner") {
stopScanner();
}

refreshAll();
}


/* =========================
SAVE DATA
========================= */

function saveData() {

localStorage.setItem(
"stockpilot_products",
JSON.stringify(products)
);

localStorage.setItem(
"stockpilot_suppliers",
JSON.stringify(suppliers)
);

localStorage.setItem(
"stockpilot_transactions",
JSON.stringify(transactions)
);

}


/* =========================
PRODUCT PHOTO
========================= */

document
.getElementById("productPhoto")
.addEventListener("change", function() {

const file = this.files[0];

if (!file) return;

const reader = new FileReader();

reader.onload = function(e) {

const img =
document.getElementById("photoPreview");

img.src = e.target.result;
img.classList.remove("hidden");

};

reader.readAsDataURL(file);

});


/* =========================
SAVE PRODUCT
========================= */

function saveProduct() {

const name =
document.getElementById("productName").value.trim();

const barcode =
document.getElementById("productBarcode").value.trim();

const sku =
document.getElementById("productSku").value.trim();

const supplier =
document.getElementById("productSupplier").value;

const cost =
Number(document.getElementById("purchaseCost").value || 0);

const price =
Number(document.getElementById("sellingPrice").value || 0);

const quantity =
Number(document.getElementById("productQuantity").value || 0);

const reorder =
Number(document.getElementById("reorderPoint").value || 0);

const category =
document.getElementById("productCategory").value;

const location =
document.getElementById("productLocation").value;

const editId =
document.getElementById("editId").value;

if (!name) {
alert("Product name is required.");
return;
}

if (barcode) {

const duplicate =
products.find(p =>
p.barcode === barcode &&
p.id != editId
);

if (duplicate) {
alert("That barcode is already being used.");
return;
}
}


const photo =
document.getElementById("photoPreview").src || "";


if (editId) {

const product =
products.find(p => p.id == editId);

if (!product) return;

product.name = name;
product.barcode = barcode;
product.sku = sku;
product.supplier = supplier;
product.cost = cost;
product.price = price;
product.qty = quantity;
product.reorder = reorder;
product.category = category;
product.location = location;

if (photo && !photo.startsWith("data:")) {
// Keep existing photo
} else if (photo) {
product.photo = photo;
}

alert("Product updated.");

} else {

products.push({

id: Date.now().toString(),

name,
barcode,
sku,
supplier,
cost,
price,
qty: quantity,
reorder,
category,
location,
photo:
photo.startsWith("data:")
? photo
: ""

});

alert("Product added.");

}

saveData();

clearProductForm();

refreshAll();

}


/* =========================
CLEAR FORM
========================= */

function clearProductForm() {

document.getElementById("editId").value = "";

document.getElementById("productName").value = "";

document.getElementById("productBarcode").value = "";

document.getElementById("productSku").value = "";

document.getElementById("purchaseCost").value = "";

document.getElementById("sellingPrice").value = "";

document.getElementById("productQuantity").value = "";

document.getElementById("reorderPoint").value = "5";

document.getElementById("productCategory").value = "";

document.getElementById("productLocation").value = "";

document.getElementById("productPhoto").value = "";

document.getElementById("photoPreview").src = "";

document.getElementById("photoPreview")
.classList.add("hidden");

}


/* =========================
EDIT PRODUCT
========================= */

function editProduct(id) {

const p = products.find(x => x.id === id);

if (!p) return;

document.getElementById("editId").value = p.id;

document.getElementById("productName").value = p.name;

document.getElementById("productBarcode").value = p.barcode;

document.getElementById("productSku").value = p.sku;

document.getElementById("purchaseCost").value = p.cost;

document.getElementById("sellingPrice").value = p.price;

document.getElementById("productQuantity").value = p.qty;

document.getElementById("reorderPoint").value = p.reorder;

document.getElementById("productCategory").value = p.category;

document.getElementById("productLocation").value = p.location;

document.getElementById("productSupplier").value = p.supplier;

if (p.photo) {

document.getElementById("photoPreview").src = p.photo;

document.getElementById("photoPreview")
.classList.remove("hidden");

}

window.scrollTo({
top: 0,
behavior: "smooth"
});

}


/* =========================
DELETE PRODUCT
========================= */

function deleteProduct(id) {

if (!confirm("Delete this product?")) return;

products =
products.filter(p => p.id !== id);

saveData();

refreshAll();

}


/* =========================
INVENTORY TABLE
========================= */

function renderInventory() {

const search =
document.getElementById("inventorySearch")
?.value
.toLowerCase() || "";

const table =
document.getElementById("inventoryTable");

table.innerHTML = "";

products
.filter(p => {

const text =
`${p.name} ${p.barcode} ${p.sku} ${p.supplier}`
.toLowerCase();

return text.includes(search);

})
.forEach(p => {

const low =
p.qty <= p.reorder;

const row =
document.createElement("tr");

row.innerHTML = `

<td>
${
p.photo
? `<img src="${p.photo}" class="product-photo">`
: "📦"
}
</td>

<td>${escapeHtml(p.name)}</td>

<td>${escapeHtml(p.barcode)}</td>

<td>${escapeHtml(p.sku)}</td>

<td>${escapeHtml(p.supplier)}</td>

<td>$${p.cost.toFixed(2)}</td>

<td>$${p.price.toFixed(2)}</td>

<td>${p.qty}</td>

<td class="${low ? "low-stock" : "in-stock"}">
${low ? "⚠ LOW STOCK" : "IN STOCK"}
</td>

<td>

<button
class="primary"
onclick="editProduct('${p.id}')">
Edit
</button>

<button
class="danger"
onclick="deleteProduct('${p.id}')">
Delete
</button>

</td>
`;

table.appendChild(row);

});

}


/* =========================
TRANSACTIONS
========================= */

function processTransaction() {

const type =
document.getElementById("transactionType").value;

const productId =
document.getElementById("transactionProduct").value;

const quantity =
Number(
document.getElementById("transactionQuantity").value
);

const reason =
document.getElementById("transactionReason").value;

const product =
products.find(p => p.id === productId);

if (!product) {

alert("Select a product.");

return;
}

if (quantity <= 0) {

alert("Enter a valid quantity.");

return;
}


if (type === "OUT" &&
quantity > product.qty) {

alert(
`Not enough stock. Current quantity: ${product.qty}`
);

return;
}


if (type === "IN") {

product.qty += quantity;

} else {

product.qty -= quantity;

}


transactions.unshift({

id: Date.now().toString(),

productId: product.id,

productName: product.name,

type,

quantity,

reason,

user: currentUser,

date: new Date().toLocaleString()

});


saveData();

document.getElementById("transactionQuantity").value = "";

document.getElementById("transactionReason").value = "";

refreshAll();

alert("Transaction saved.");

}


/* =========================
TRANSACTION TABLE
========================= */

function renderTransactions() {

const table =
document.getElementById("transactionTable");

table.innerHTML = "";

transactions.forEach(t => {

const row =
document.createElement("tr");

row.innerHTML = `

<td>${t.date}</td>

<td>${escapeHtml(t.productName)}</td>

<td>
${
t.type === "IN"
? '<span class="in-stock">IN</span>'
: '<span class="low-stock">OUT</span>'
}
</td>

<td>${t.quantity}</td>

<td>${escapeHtml(t.reason || "")}</td>

<td>${escapeHtml(t.user || "")}</td>

`;

table.appendChild(row);

});

}


/* =========================
SUPPLIERS
========================= */

function saveSupplier() {

const name =
document.getElementById("supplierName").value.trim();

if (!name) {

alert("Supplier name is required.");

return;
}

suppliers.push({

id: Date.now().toString(),

name,

contact:
document.getElementById("supplierContact").value,

phone:
document.getElementById("supplierPhone").value,

email:
document.getElementById("supplierEmail").value

});

saveData();

document.getElementById("supplierName").value = "";

document.getElementById("supplierContact").value = "";

document.getElementById("supplierPhone").value = "";

document.getElementById("supplierEmail").value = "";

refreshAll();

}


function renderSuppliers() {

const table =
document.getElementById("supplierTable");

table.innerHTML = "";

suppliers.forEach(s => {

const count =
products.filter(p => p.supplier === s.name).length;

const row =
document.createElement("tr");

row.innerHTML = `

<td>${escapeHtml(s.name)}</td>

<td>${escapeHtml(s.contact)}</td>

<td>${escapeHtml(s.phone)}</td>

<td>${escapeHtml(s.email)}</td>

<td>${count}</td>

<td>

<button
class="danger"
onclick="deleteSupplier('${s.id}')">

Delete

</button>

</td>

`;

table.appendChild(row);

});

}


function deleteSupplier(id) {

if (!confirm("Delete this supplier?")) return;

suppliers =
suppliers.filter(s => s.id !== id);

saveData();

refreshAll();

}


/* =========================
SUPPLIER DROPDOWN
========================= */

function renderSupplierDropdown() {

const select =
document.getElementById("productSupplier");

select.innerHTML =
'<option value="">-- No Supplier --</option>';

suppliers.forEach(s => {

const option =
document.createElement("option");

option.value = s.name;

option.textContent = s.name;

select.appendChild(option);

});

}


/* =========================
TRANSACTION PRODUCT DROPDOWN
========================= */

function renderTransactionDropdown() {

const select =
document.getElementById("transactionProduct");

select.innerHTML =
'<option value="">-- Select Product --</option>';

products.forEach(p => {

const option =
document.createElement("option");

option.value = p.id;

option.textContent =
`${p.name} — ${p.barcode || "No Barcode"} — Qty: ${p.qty}`;

select.appendChild(option);

});

}


/* =========================
LOW STOCK
========================= */

function renderLowStock() {

const box =
document.getElementById("lowStockList");

box.innerHTML = "";

const low =
products.filter(p => p.qty <= p.reorder);

if (low.length === 0) {

box.innerHTML =
`<div class="panel">
✅ No low-stock products.
</div>`;

return;
}

low.forEach(p => {

const order =
Math.max(0, (p.reorder * 2) - p.qty);

const div =
document.createElement("div");

div.className = "alert";

div.innerHTML = `

<strong>${escapeHtml(p.name)}</strong><br>

Current Stock: ${p.qty}<br>

Reorder Point: ${p.reorder}<br>

Suggested Order: ${order}

`;

box.appendChild(div);

});

}


/* =========================
DASHBOARD
========================= */

function updateDashboard() {

let units = 0;
let cost = 0;
let retail = 0;

products.forEach(p => {

units += p.qty;

cost += p.qty * p.cost;

retail += p.qty * p.price;

});

document.getElementById("totalProducts")
.textContent = products.length;

document.getElementById("totalUnits")
.textContent = units;

document.getElementById("inventoryCost")
.textContent = "$" + cost.toFixed(2);

document.getElementById("retailValue")
.textContent = "$" + retail.toFixed(2);

document.getElementById("potentialProfit")
.textContent =
"$" + (retail - cost).toFixed(2);

document.getElementById("lowStockCount")
.textContent =
products.filter(p => p.qty <= p.reorder).length;


const recent =
document.getElementById("recentTransactions");

recent.innerHTML = "";

transactions
.slice(0,10)
.forEach(t => {

recent.innerHTML += `

<tr>

<td>${t.date}</td>

<td>${escapeHtml(t.productName)}</td>

<td>${t.type}</td>

<td>${t.quantity}</td>

<td>${escapeHtml(t.user)}</td>

</tr>

`;

});

}


/* =========================
BARCODE SCANNER
========================= */

async function startScanner(targetInput = null) {

scannerTarget = targetInput;

const video =
document.getElementById("scannerVideo");

if (!("BarcodeDetector" in window)) {

alert(
"Barcode scanning is not supported by this browser. Try a recent Chrome or Edge browser."
);

return;

}

try {

scannerStream =
await navigator.mediaDevices.getUserMedia({

video: {
facingMode: {
ideal: "environment"
}
},

audio: false

});

video.srcObject = scannerStream;

const detector =
new BarcodeDetector({

formats: [
"code_128",
"code_39",
"ean_13",
"ean_8",
"upc_a",
"upc_e",
"itf",
"qr_code"
]

});

scannerTimer =
setInterval(async () => {

try {

const codes =
await detector.detect(video);

if (codes.length > 0) {

const value =
codes[0].rawValue;

handleBarcode(value);

}

} catch(e) {

console.log(e);

}

}, 300);

} catch(error) {

alert(
"Unable to access camera. Make sure camera permission is allowed."
);

}

}


function stopScanner() {

if (scannerTimer) {

clearInterval(scannerTimer);

scannerTimer = null;

}

if (scannerStream) {

scannerStream
.getTracks()
.forEach(track => track.stop());

scannerStream = null;

}

}


function handleBarcode(barcode) {

stopScanner();

document.getElementById("scannerResult")
.textContent =
"Scanned Barcode: " + barcode;


if (scannerTarget) {

document.getElementById(scannerTarget)
.value = barcode;

scannerTarget = null;

return;
}


const product =
products.find(p => p.barcode === barcode);

if (product) {

document.getElementById("transactionProduct")
.value = product.id;

showPage("transactions");

alert(
"Product found: " + product.name
);

} else {

alert(
"Barcode scanned, but no product was found."
);

}

}


function openScannerForProduct() {

showPage("scanner");

startScanner("productBarcode");

}


/* =========================
CSV EXPORT
========================= */

function exportCSV() {

let csv =
"Name,Barcode,SKU,Supplier,Cost,Price,Quantity,Reorder,Category,Location\n";

products.forEach(p => {

csv += [

p.name,
p.barcode,
p.sku,
p.supplier,
p.cost,
p.price,
p.qty,
p.reorder,
p.category,
p.location

]
.map(csvEscape)
.join(",") + "\n";

});


const blob =
new Blob([csv], {
type: "text/csv"
});

const url =
URL.createObjectURL(blob);

const a =
document.createElement("a");

a.href = url;

a.download =
"stockpilot_inventory.csv";

a.click();

URL.revokeObjectURL(url);

}


function csvEscape(value) {

return `"${String(value || "")
.replace(/"/g,'""')}"`;

}


/* =========================
BACKUP
========================= */

function backupData() {

const data = {

products,
suppliers,
transactions,
password

};

const blob =
new Blob(
[JSON.stringify(data,null,2)],
{type:"application/json"}
);

const url =
URL.createObjectURL(blob);

const a =
document.createElement("a");

a.href = url;

a.download =
"stockpilot_backup.json";

a.click();

URL.revokeObjectURL(url);

}


/* =========================
RESTORE
========================= */

function restoreData() {

const file =
document.getElementById("restoreFile")
.files[0];

if (!file) {

alert("Select a backup file.");

return;
}

const reader =
new FileReader();

reader.onload = function(e) {

try {

const data =
JSON.parse(e.target.result);

products =
data.products || [];

suppliers =
data.suppliers || [];

transactions =
data.transactions || [];

if (data.password) {
password = data.password;
localStorage.setItem(
"stockpilot_password",
password
);
}

saveData();

refreshAll();

alert("Backup restored successfully.");

} catch(error) {

alert("Invalid backup file.");

}

};

reader.readAsText(file);

}


/* =========================
CHANGE PASSWORD
========================= */

function changePassword() {

const newPassword =
document.getElementById("newPassword")
.value;

if (newPassword.length < 4) {

alert(
"Password must be at least 4 characters."
);

return;
}

password = newPassword;

localStorage.setItem(
"stockpilot_password",
password
);
