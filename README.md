
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Manajemen Agunan Bankaltimtara</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
:root{
    --soft-blue: #a8d0e6; /* biru muda */
    --soft-yellow: #fff9c3; /* kuning muda */
    --btn-blue: #4a90e2;
    --btn-yellow: #ffd54f;
    --soft-gray: #fefcea;
}
body {
    font-family: Arial, sans-serif;
    background: var(--soft-yellow);
    padding: 0; margin: 0;
}
.hidden { display:none; }
.container {
    max-width:900px;
    margin:auto;
    background:#fff;
    padding:20px;
    border-radius:12px;
    box-shadow:0 4px 10px rgba(0,0,0,0.1);
}

/* ===== HEADER ===== */
header{
    display:flex;
    align-items:center;
    padding:10px 20px;
    background:var(--soft-blue);
    color:#333;
    border-bottom:2px solid var(--btn-blue);
}
header img{
    height:50px;
    margin-right:15px;
}
header h1{
    margin:0;
    font-size:24px;
    color:#333;
}

/* ===== INPUT / BUTTONS ===== */
input, select, textarea{
    width:100%;
    padding:10px;
    margin:5px 0;
    border-radius:8px;
    border:1px solid #ccc;
}
button{
    padding:10px;
    border:none;
    border-radius:8px;
    cursor:pointer;
    font-weight:bold;
    margin:5px 0;
}
button:hover{ opacity:0.9; }
.btn-primary{
    background: var(--soft-blue);
    color:#fff;
}
.btn-yellow{
    background: var(--soft-yellow);
    color:#333;
    border:1px solid #ccc;
}
.error{
    color:red;
}

/* ===== AGUNAN CARD ===== */
.agunan-card{
    border:1px solid var(--soft-blue);
    padding:15px;
    margin:10px 0;
    border-radius:12px;
    display:flex;
    position:relative;
    background:#fff;
    box-shadow:0 2px 6px rgba(0,0,0,0.1);
    transition: transform 0.3s ease;
}
.agunan-card:hover {
    transform: scale(1.02);
}
.agunan-images{
    display:flex;
    overflow-x:auto;
    max-width:200px;
    margin-right:10px;
}
.agunan-images img{
    width:150px;
    height:100px;
    object-fit:cover;
    margin-right:5px;
    border-radius:6px;
}
.agunan-info{
    flex:1;
}
.agunan-info h4{
    margin:0 0 5px 0;
    color: var(--soft-blue);
}
.admin-btns{
    position:absolute;
    top:5px;
    right:5px;
    display:flex;
    flex-direction:column;
}
.admin-btns button{
    margin:2px;
    font-size:12px;
}

/* ===== MODAL ===== */
.modal{
    display:none;
    position:fixed;
    top:0; left:0;
    width:100%;
    height:100%;
    background:rgba(0,0,0,0.5);
    justify-content:center;
    align-items:center;
    z-index:100;
}
.modal-content{
    background:#fff;
    padding:20px;
    border-radius:12px;
    max-width:600px;
    width:90%;
    max-height:90%;
    overflow:auto;
    box-shadow:0 4px 15px rgba(0,0,0,0.2);
}
.close{
    float:right;
    cursor:pointer;
    color:red;
    font-weight:bold;
    font-size:18px;
}
.preview img{
    width:100px;
    margin:5px;
    border-radius:6px;
}

/* ===== FLEX HORIZONTAL FOR RANGE INPUT ===== */
.range-filter {
    display:flex;
    gap:10px;
    margin-bottom:10px;
}
.range-filter input {
    flex: 1;
}

/* ===== BUTTONS LAYOUT ===== */
.buttons-row {
    display:flex;
    gap: 10px;
    flex-wrap: wrap;
}

/* ===== LABEL STYLING ===== */
label {
    font-weight: 600;
}

/* ===== LINKS BUTTON ===== */
.link-button {
    background:none;
    border:none;
    color: var(--btn-blue);
    cursor:pointer;
    text-decoration: underline;
    padding:0;
    font-size: 14px;
}

/* ===== SCROLLBAR FOR IMAGES ===== */
.agunan-images::-webkit-scrollbar {
    height: 6px;
}
.agunan-images::-webkit-scrollbar-thumb {
    background-color: var(--soft-blue);
    border-radius: 3px;
}
</style>
</head>
<body>

<header>
    <img src="logo-bankaltimtara.png" alt="Bankaltimtara Logo" />
    <h1>Manajemen Agunan Bankaltimtara</h1>
</header>

<!-- ========== LOGIN PAGE ========== -->
<div id="pageLogin" class="container">
    <h2>Login Admin</h2>
    <input type="text" id="loginUser" placeholder="Username" autocomplete="off" />
    <input type="password" id="loginPass" placeholder="Password" autocomplete="off" />
    <button class="btn-primary" onclick="loginAdmin()">Login Admin</button>
    <p id="loginMsg" class="error"></p>
    <hr />
    <h3>Atau Masuk Sebagai Pengguna</h3>
    <button class="btn-yellow" onclick="loginUser()">Masuk Sebagai Pengguna</button>
</div>

<!-- ========== INPUT AGUNAN PAGE ========== -->
<div id="pageInput" class="container hidden">
    <h2>Input / Edit Agunan</h2>

    <label for="jenisAgunan">Jenis Agunan</label>
    <select id="jenisAgunan" onchange="showSpecFields()">
        <option value="">Pilih jenis</option>
        <option value="bangunan">Bangunan</option>
        <option value="tanah">Tanah</option>
        <option value="bangunan_tanah">Bangunan & Tanah</option>
        <option value="alat_berat">Alat Berat</option>
        <option value="mesin">Mesin</option>
        <option value="kendaraan">Kendaraan</option>
    </select>

    <div id="propSpec" style="display:none;">
        <label>Panjang (m)</label>
        <input type="number" id="panjang" min="0" />
        <label>Lebar (m)</label>
        <input type="number" id="lebar" min="0" />
        <label>Luas (m²)</label>
        <input type="number" id="luas" min="0" />
        <label>Jenis Sertifikat</label>
        <input type="text" id="sertifikat" placeholder="SHM, SHGB, dll" />
    </div>

    <div id="alatSpec" style="display:none;">
        <label>Tipe</label>
        <input type="text" id="tipe" />
        <label>Merk</label>
        <input type="text" id="merk" />
        <label>Plate Number</label>
        <input type="text" id="plate" />
        <label>Tahun Pembuatan</label>
        <input type="number" id="tahunBuat" min="1900" max="2099" />
        <label>Tahun Invoice</label>
        <input type="number" id="tahunInvoice" min="1900" max="2099" />
    </div>

    <label>Alamat Agunan</label>
    <textarea id="alamat" placeholder="Masukkan alamat lengkap"></textarea>

    <label>Upload Gambar (maks 15)</label>
    <input type="file" id="gambar" multiple accept="image/*" />
    <div class="preview" id="preview"></div>

    <label>Nama Kontak</label>
    <input type="text" id="namaKontak" />
    <label>Nomor WA</label>
    <input type="text" id="waKontak" placeholder="628xxxxxxxxxx" />

    <label>Nilai Agunan (Rp)</label>
    <input type="number" id="nilai" min="0" />

    <label>QR Code Lokasi</label>
    <div id="qrcode"></div>

    <div class="buttons-row">
        <button class="btn-primary" onclick="submitAgunan()">Simpan Agunan</button>
        <button class="btn-yellow" onclick="goToIndex()">Beranda</button>
    </div>
    <p id="inputMsg" class="error"></p>
</div>

<!-- ========== BERANDA PAGE ========== -->
<div id="pageIndex" class="container hidden">
    <h2>Beranda Agunan</h2>

    <label for="searchInput">Pencarian</label>
    <input type="text" id="searchInput" placeholder="Cari agunan..." oninput="renderAgunan()" />

    <label for="filterJenis">Filter Jenis</label>
    <select id="filterJenis" onchange="renderAgunan()">
        <option value="">Semua</option>
        <option value="bangunan">Bangunan</option>
        <option value="tanah">Tanah</option>
        <option value="bangunan_tanah">Bangunan & Tanah</option>
        <option value="alat_berat">Alat Berat</option>
        <option value="mesin">Mesin</option>
        <option value="kendaraan">Kendaraan</option>
    </select>

    <label>Range Harga</label>
    <div class="range-filter">
        <input type="number" id="hargaMin" placeholder="Min" oninput="renderAgunan()" min="0" />
        <input type="number" id="hargaMax" placeholder="Max" oninput="renderAgunan()" min="0" />
    </div>

    <div class="buttons-row">
        <button onclick="logout()" class="btn-yellow">Logout</button>
        <button id="btnInputAgunan" class="btn-primary" onclick="goToInput()">Input Agunan</button>
    </div>

    <hr />
    <div id="agunanList"></div>
</div>

<!-- ========== MODAL DETAIL ========= -->
<div id="modal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">×</span>
        <div id="modalBody"></div>
    </div>
</div>

<script>
// ========== KONSTANTA LOGIN ==========
const ADMIN_USER = "220241005388";
const ADMIN_PASS = "220241005388";

// ========== LOGIN ==========
function loginAdmin(){
    const u = document.getElementById("loginUser").value.trim();
    const p = document.getElementById("loginPass").value.trim();
    if(u === ADMIN_USER && p === ADMIN_PASS){
        localStorage.setItem("role", "admin");
        clearLoginFields();
        showPage("pageIndex");
        renderAgunan();
    } else {
        alert("Username atau password salah!");
    }
}
function loginUser(){
    localStorage.setItem("role", "user");
    clearLoginFields();
    showPage("pageIndex");
    renderAgunan();
}
function clearLoginFields(){
    document.getElementById("loginUser").value = "";
    document.getElementById("loginPass").value = "";
}

// ========== HALAMAN & NAVIGASI ==========
function showPage(id){
    document.getElementById("pageLogin").classList.add("hidden");
    document.getElementById("pageInput").classList.add("hidden");
    document.getElementById("pageIndex").classList.add("hidden");
    document.getElementById(id).classList.remove("hidden");

    const role = localStorage.getItem("role");
    document.getElementById("btnInputAgunan").style.display = role === "admin" ? "inline-block" : "none";
}

// Otomatis cek login saat load
window.onload = function(){
    const roleNow = localStorage.getItem("role");
    if(roleNow){
        showPage("pageIndex");
        renderAgunan();
    } else {
        showPage("pageLogin");
    }
}

// ========== TAMPILKAN FORM SPESIFIKASI ==========
function showSpecFields(){
    const jenis = document.getElementById("jenisAgunan").value;
    const propSpec = document.getElementById("propSpec");
    const alatSpec = document.getElementById("alatSpec");
    if(jenis === "bangunan" || jenis === "tanah" || jenis === "bangunan_tanah"){
        propSpec.style.display = "block";
        alatSpec.style.display = "none";
    } else if(jenis === "alat_berat" || jenis === "mesin" || jenis === "kendaraan"){
        propSpec.style.display = "none";
        alatSpec.style.display = "block";
    } else {
        propSpec.style.display = "none";
        alatSpec.style.display = "none";
    }
}

// ========== PREVIEW GAMBAR ==========
const gambar = document.getElementById("gambar");
const preview = document.getElementById("preview");

gambar.addEventListener('change', function(){
    preview.innerHTML = "";
    if(this.files.length > 15){
        alert("Maksimal 15 gambar");
        this.value = "";
        return;
    }
    for(let i=0; i < this.files.length; i++){
        const reader = new FileReader();
        reader.onload = function(e){
            const img = document.createElement("img");
            img.src = e.target.result;
            preview.appendChild(img);
        }
        reader.readAsDataURL(this.files[i]);
    }
});

// ========== QR CODE ALAMAT ==========
const alamatInput = document.getElementById("alamat");
const qrcodeDiv = document.getElementById("qrcode");

alamatInput.addEventListener('input', function(){
    qrcodeDiv.innerHTML = "";
    if(alamatInput.value.trim() !== ""){
        new QRCode(qrcodeDiv, alamatInput.value.trim());
    }
});

// ========== SIMPAN AGUNAN ==========
function submitAgunan(){
    const jenis = document.getElementById("jenisAgunan").value;
    if(!jenis){
        alert("Pilih jenis agunan");
        return;
    }

    const data = {
        jenis: jenis,
        spesifikasi: {},
        alamat: alamatInput.value.trim(),
        gambar: [],
        kontak: {
            nama: document.getElementById("namaKontak").value.trim(),
            wa: document.getElementById("waKontak").value.trim()
        },
        nilai: document.getElementById("nilai").value.trim(),
    };

    const propSpec = document.getElementById("propSpec");
    const alatSpec = document.getElementById("alatSpec");

    if(propSpec.style.display === "block"){
        data.spesifikasi.panjang = document.getElementById("panjang").value.trim();
        data.spesifikasi.lebar = document.getElementById("lebar").value.trim();
        data.spesifikasi.luas = document.getElementById("luas").value.trim();
        data.spesifikasi.sertifikat = document.getElementById("sertifikat").value.trim();
    } else if(alatSpec.style.display === "block"){
        data.spesifikasi.tipe = document.getElementById("tipe").value.trim();
        data.spesifikasi.merk = document.getElementById("merk").value.trim();
        data.spesifikasi.plate = document.getElementById("plate").value.trim();
        data.spesifikasi.tahunBuat = document.getElementById("tahunBuat").value.trim();
        data.spesifikasi.tahunInvoice = document.getElementById("tahunInvoice").value.trim();
    }

    const files = gambar.files;
    if(files.length > 0){
        let count = 0;
        for(let i=0; i < files.length; i++){
            const reader = new FileReader();
            reader.onload = function(e){
                data.gambar.push(e.target.result);
                count++;
                if(count === files.length){
                    saveAgunan(data);
                }
            }
            reader.readAsDataURL(files[i]);
        }
    } else {
        saveAgunan(data);
    }
}

function saveAgunan(data){
    let list = JSON.parse(localStorage.getItem("agunanList") || "[]");
    if(data.index !== undefined){
        list[data.index] = data; // update
    } else {
        list.push(data); // new
    }
    localStorage.setItem("agunanList", JSON.stringify(list));
    alert("Agunan berhasil disimpan!");
    resetInputForm();
    renderAgunan();
    showPage("pageIndex");
}

// ========== RESET FORM INPUT ==========
function resetInputForm(){
    document.getElementById("jenisAgunan").value = "";
    showSpecFields();
    document.getElementById("panjang").value = "";
    document.getElementById("lebar").value = "";
    document.getElementById("luas").value = "";
    document.getElementById("sertifikat").value = "";
    document.getElementById("tipe").value = "";
    document.getElementById("merk").value = "";
    document.getElementById("plate").value = "";
    document.getElementById("tahunBuat").value = "";
    document.getElementById("tahunInvoice").value = "";
    alamatInput.value = "";
    qrcodeDiv.innerHTML = "";
    gambar.value = "";
    preview.innerHTML = "";
    document.getElement
