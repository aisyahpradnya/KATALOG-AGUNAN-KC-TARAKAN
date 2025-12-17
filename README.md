# KATALOG AGUNAN KC TARAKAN
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Manajemen Agunan Bankaltimtara</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
/* ===== Warna utama ===== */
:root {
    --soft-blue: #a8d0e6;
    --soft-yellow: #fef9c3;
    --soft-gray: #f1f3f5;
    --btn-blue: #4a90e2;
    --btn-yellow: #ffd54f;
    --btn-hover: #3c7ecf;
}

body { font-family: Arial, sans-serif; background: var(--soft-gray); margin:0; padding:0; }
.container { max-width: 900px; margin: 20px auto; background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
.hidden { display:none; }

/* ===== Header ===== */
header { display:flex; align-items:center; margin-bottom:20px; }
header img { height:50px; margin-right:15px; }
header h1 { color: var(--btn-blue); }

/* ===== Inputs & Buttons ===== */
input, select, textarea { width:100%; padding:10px; margin:5px 0; border-radius:6px; border:1px solid #ccc; }
button { padding:10px; border:none; border-radius:6px; cursor:pointer; margin:5px 0; font-weight:bold; }
button:hover { opacity:0.9; }
.btn-primary { background: var(--btn-blue); color:#fff; }
.btn-yellow { background: var(--btn-yellow); color:#333; }
.error { color:red; }

/* ===== Card Agunan ===== */
.agunan-card { border:1px solid #ccc; padding:10px; margin:10px 0; border-radius:12px; display:flex; position:relative; background: var(--soft-yellow); box-shadow: 0 2px 6px rgba(0,0,0,0.1);}
.agunan-images { display:flex; overflow-x:auto; max-width:200px; margin-right:10px; }
.agunan-images img { width:150px; height:100px; object-fit:cover; margin-right:5px; border-radius:6px; }
.agunan-info { flex:1; }
.agunan-info h4 { margin:0 0 5px 0; color: var(--btn-blue); }
.admin-btns { position:absolute; top:5px; right:5px; display:flex; flex-direction:column; }
.admin-btns button { margin:2px; font-size:12px; }

/* ===== Modal ===== */
.modal { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); justify-content:center; align-items:center; z-index:100; }
.modal-content { background:#fff; padding:20px; border-radius:12px; max-width:600px; width:90%; max-height:90%; overflow:auto; box-shadow:0 4px 15px rgba(0,0,0,0.2);}
.close { float:right; cursor:pointer; color:red; font-weight:bold; font-size:18px; }
.qrcode { margin-top:10px; }
.preview img { width:100px; margin:5px; border-radius:6px; }
</style>
</head>
<body>

<header>
<img src="https://upload.wikimedia.org/wikipedia/id/4/46/Logo_Bankaltimtara.png" alt="Bankaltimtara">
<h1>Manajemen Agunan</h1>
</header>

<!-- =================== LOGIN =================== -->
<div id="pageLogin" class="container">
<h2>Login Admin</h2>
<input type="text" id="loginUser" placeholder="Username">
<input type="password" id="loginPass" placeholder="Password">
<button class="btn-primary" onclick="loginAdmin()">Login Admin</button>
<p id="loginMsg" class="error"></p>
<hr>
<h3>Atau Masuk Sebagai Pengguna</h3>
<button class="btn-yellow" onclick="loginUser()">Masuk Sebagai Pengguna</button>
</div>

<!-- =================== INPUT AGUNAN =================== -->
<div id="pageInput" class="container hidden">
<h2>Input / Edit Agunan</h2>

<label>Jenis Agunan</label>
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
    <label>Panjang (m)</label><input type="number" id="panjang">
    <label>Lebar (m)</label><input type="number" id="lebar">
    <label>Luas (m²)</label><input type="number" id="luas">
    <label>Jenis Sertifikat</label><input type="text" id="sertifikat" placeholder="SHM, SHGB, dll">
</div>

<div id="alatSpec" style="display:none;">
    <label>Tipe</label><input type="text" id="tipe">
    <label>Merk</label><input type="text" id="merk">
    <label>Plate Number</label><input type="text" id="plate">
    <label>Tahun Pembuatan</label><input type="number" id="tahunBuat">
    <label>Tahun Invoice</label><input type="number" id="tahunInvoice">
</div>

<label>Alamat Agunan</label>
<textarea id="alamat" placeholder="Masukkan alamat lengkap"></textarea>

<label>Upload Gambar (maks 15)</label>
<input type="file" id="gambar" multiple accept="image/*">
<div class="preview" id="preview"></div>

<label>Nama Kontak</label><input type="text" id="namaKontak">
<label>Nomor WA</label><input type="text" id="waKontak" placeholder="628xxxxxxxxxx">

<label>Nilai Agunan (Rp)</label><input type="number" id="nilai">

<label>QR Code Lokasi</label>
<div id="qrcode"></div>

<button class="btn-primary" onclick="submitAgunan()">Simpan Agunan</button>
<button class="btn-yellow" onclick="goToIndex()">Beranda</button>
<p id="inputMsg" class="error"></p>
</div>

<!-- =================== BERANDA =================== -->
<div id="pageIndex" class="container hidden">
<h2>Beranda Agunan</h2>

<label>Pencarian</label>
<input type="text" id="searchInput" placeholder="Cari agunan..." oninput="renderAgunan()">

<label>Filter Jenis</label>
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
<input type="number" id="hargaMin" placeholder="Min" oninput="renderAgunan()">
<input type="number" id="hargaMax" placeholder="Max" oninput="renderAgunan()">

<button class="btn-yellow" onclick="logout()">Logout</button>
<button id="btnInputAgunan" class="btn-primary" onclick="goToInput()">Input Agunan</button>

<hr>
<div id="agunanList"></div>
</div>

<!-- =================== MODAL DETAIL =================== -->
<div id="modal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">×</span>
        <div id="modalBody"></div>
    </div>
</div>

<script>
// =================== LOGIN ===================
const ADMIN_USER = "220241005388";
const ADMIN_PASS = "220241005388";

function loginAdmin(){
    const u = document.getElementById("loginUser").value.trim();
    const p = document.getElementById("loginPass").value.trim();
    if(u===ADMIN_USER && p===ADMIN_PASS){
        localStorage.setItem("role","admin");
        showPage("pageIndex");
    } else {
        alert("Username atau password salah!");
    }
}

function loginUser(){
    localStorage.setItem("role","user");
    showPage("pageIndex");
}

// =================== PAGE SWITCH ===================
function showPage(id){
    document.getElementById("pageLogin").classList.add("hidden");
    document.getElementById("pageInput").classList.add("hidden");
    document.getElementById("pageIndex").classList.add("hidden");
    document.getElementById(id).classList.remove("hidden");

    const role = localStorage.getItem("role");
    document.getElementById("btnInputAgunan").style.display = role==="admin" ? "inline-block" : "none";
}

if(localStorage.getItem("role")) showPage("pageIndex");

// =================== INPUT AGUNAN ===================
function showSpecFields(){
    const jenis = document.getElementById("jenisAgunan").value;
    if(jenis==="bangunan" || jenis==="tanah" || jenis==="bangunan_tanah"){
        propSpec.style.display="block"; alatSpec.style.display="none";
    } else if(jenis==="alat_berat" || jenis==="mesin" || jenis==="kendaraan"){
        propSpec.style.display="none"; alatSpec.style.display="block";
    } else { propSpec.style.display="none"; alatSpec.style.display="none"; }
}

gambar.addEventListener('change', function(){
    preview.innerHTML = "";
    if(this.files.length>15){ alert("Maksimal 15 gambar"); this.value=""; return; }
    for(let i=0;i<this.files.length;i++){
        const reader = new FileReader();
        reader.onload = function(e){
            const img = document.createElement("img");
            img.src = e.target.result;
            preview.appendChild(img);
        }
        reader.readAsDataURL(this.files[i]);
    }
});

alamat.addEventListener('input',function(){
    qrcode.innerHTML="";
    if(alamat.value.trim()!==""){
        new QRCode(qrcode, alamat.value.trim());
    }
});

function submitAgunan(){
    const jenis = jenisAgunan.value;
    if(!jenis){ alert("Pilih jenis agunan"); return; }
    const data={jenis:jenis, spesifikasi:{}, alamat:alamat.value.trim(), gambar:[], kontak:{nama:namaKontak.value.trim(), wa:waKontak.value.trim()}, nilai: nilai.value};
    if(propSpec.style.display==="block"){
        data.spesifikasi.panjang = panjang.value;
        data.spesifikasi.lebar = lebar.value;
        data.spesifikasi.luas = luas.value;
        data.spesifikasi.sertifikat = sertifikat.value;
    } else if(alatSpec.style.display==="block"){
        data.spesifikasi.tipe = tipe.value;
        data.spesifikasi.merk = merk.value;
        data.spesifikasi.plate = plate.value;
        data.spesifikasi.tahunBuat = tahunBuat.value;
        data.spesifikasi.tahunInvoice = tahunInvoice.value;
    }
    if(gambar.files.length>0){
        let count=0;
        for(let i=0;i<gambar.files.length;i++){
            const reader = new FileReader();
            reader.onload=function(e){
                data.gambar.push(e.target.result);
                count++;
                if(count===gambar.files.length) saveAgunan(data);
            }
            reader.readAsDataURL(gambar.files[i]);
        }
    } else { saveAgunan(data); }
}

function saveAgunan(data){
    let list = JSON.parse(localStorage.getItem("agunanList")||"[]");
    if(data.index!=undefined){ list[data.index] = data; } else list.push(data);
    localStorage.setItem("agunanList", JSON.stringify(list));
    alert("Agunan berhasil disimpan!");
    renderAgunan();
    showPage("pageIndex");
}

// =================== BERANDA ===================
function renderAgunan(){
    let list = JSON.parse(localStorage.getItem("agunanList")||"[]");
    const search=document.getElementById("searchInput").value.toLowerCase();
    const jenisFilter=document.getElementById("filterJenis").value;
    const minHarga=parseFloat(document.getElementById("hargaMin").value);
    const maxHarga=parseFloat(document.getElementById("hargaMax").value);
    const role = localStorage.getItem("role");
    let html="";
    list.forEach((a,index)=>{
        const teksGabung = (a.jenis + " " + a.alamat + " " + a.kontak.nama).toLowerCase();
        let harga = parseFloat(a.nilai) || 0;
        if(teksGabung.includes(search) &&
           (jenisFilter==="" || a.jenis===jenisFilter) &&
           (isNaN(minHarga) || harga>=minHarga) &&
           (isNaN(maxHarga) || harga<=maxHarga)
        ){
            let imgs="";
            if(a.gambar.length>0){
                imgs=`<div class="agunan-images">`;
                a.gambar.forEach(g=>{ imgs+=`<img src="${g}" alt="Gambar">`; });
                imgs+=`</div>`;
            }
            let adminBtns="";
            if(role==="admin"){
                adminBtns=`<div class="admin-btns">
                    <button class="btn-primary" onclick="editAgunan(${index})">Edit</button>
                    <button class="btn-yellow" onclick="deleteAgunan(${index})">Hapus</button>
                </div>`;
            }
            html+=`
            <div class="agunan-card">
                ${imgs}
                <div class="agunan-info">
                    <h4>${a.jenis.toUpperCase()}</h4>
                    <p><strong>Alamat:</strong> ${a.alamat}</p>
                    <p><strong>Kontak:</strong> ${a.kontak.nama} (${a.kontak.wa})</p>
                    <p><strong>Nilai:</strong> Rp ${a.nilai}</p>
                    <button class="btn-primary" onclick="hubungiWA('${a.kontak.wa}')">Hubungi</button>
                    <button class="btn-yellow" onclick="showDetail(${index})">Detail Agunan</button>
                </div>
                ${adminBtns}
            </div>`;
        }
    });
    agunanList.innerHTML=html||"<p>Tidak ada agunan ditemukan.</p>";
}

function hubungiWA(wa){
    if(!wa) return alert("Nomor WA tidak tersedia!");
    let nomor = wa.replace(/[^0-9]/g,'');
    window.open("https://wa.me/"+nomor,"_blank");
}

function showDetail(index){
    let list=JSON.parse(localStorage.getItem("agunanList")||"[]");
    let a=list[index];
    let html=`<h3>${a.jenis.toUpperCase()}</h3>`;
    html+=`<p><strong>Alamat:</strong> ${a.alamat}</p>`;
    html+=`<p><strong>Kontak:</strong> ${a.kontak.nama} (${a.kontak.wa})</p>`;
    html+=`<p><strong>Nilai:</strong> Rp ${a.nilai}</p>`;
    html+=`<p><strong>Spesifikasi:</strong></p><ul>`;
    for(const key in a.spesifikasi){ html+=`<li>${key}: ${a.spesifikasi[key]}</li>`;}
    html+=`</ul>`;
    if(a.gambar.length>0){
        html+=`<div class="agunan-images">`;
        a.gambar.forEach(g=>{ html+=`<img src="${g}" alt="Gambar">`; });
        html+=`</div>`;
    }
    html+=`<div class="qrcode" id="modalQR"></div>`;
    modalBody.innerHTML=html;
    new QRCode(document.getElementById("modalQR"), a.alamat);
    modal.style.display="flex";
}

// =================== EDIT / DELETE ===================
function editAgunan(index){
    let list = JSON.parse(localStorage.getItem("agunanList")||"[]");
    const a = list[index];
    showPage("pageInput");
    jenisAgunan.value = a.jenis;
    showSpecFields();
    if(propSpec.style.display==="block"){
        panjang.value=a.spesifikasi.panjang||"";
        lebar.value=a.spesifikasi.lebar||"";
        luas.value=a.spesifikasi.luas||"";
        sertifikat.value=a.spesifikasi.sertifikat||"";
    } else if(alatSpec.style.display==="block"){
        tipe.value=a.spesifikasi.tipe||"";
        merk.value=a.spesifikasi.merk||"";
        plate.value=a.spesifikasi.plate||"";
        tahunBuat.value=a.spesifikasi.tahunBuat||"";
        tahunInvoice.value=a.spesifikasi.tahunInvoice||"";
    }
    alamat.value=a.alamat;
    namaKontak.value=a.kontak.nama;
    waKontak.value=a.kontak.wa;
    nilai.value=a.nilai;
    qrcode.innerHTML="";
    new QRCode(qrcode, a.alamat);
    preview.innerHTML="";
    a.gambar.forEach(g=>{
        const img=document.createElement("img");
        img.src=g;
        preview.appendChild(img);
    });
    a.index=index;
}

function deleteAgunan(index){
    if(confirm("Yakin ingin menghapus agunan ini?")){
        let list = JSON
