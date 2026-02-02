<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Badawi Farm</title>

<style>
body{
  margin:0;
  font-family:Arial,sans-serif;
  background:#0f172a;
  color:#fff;
}
.container{
  max-width:600px;
  margin:auto;
  padding:20px;
}

/* LOGO */
.logo{
  display:flex;
  align-items:center;
  justify-content:center;
  gap:10px;
  margin-bottom:5px;
}
.logo-icon{
  width:40px;
  height:40px;
  border-radius:50%;
  background:linear-gradient(135deg,#22c55e,#16a34a);
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:22px;
}
.logo-text{
  font-size:22px;
  font-weight:bold;
  letter-spacing:1px;
}

.subtitle{text-align:center;color:#cbd5e1;margin-bottom:15px}

.box{
  background:#1e293b;
  padding:15px;
  border-radius:12px;
}
label{margin-top:10px;display:block}
input,select{
  width:100%;
  padding:12px;
  margin-top:5px;
  border-radius:8px;
  border:none;
  font-size:16px;
}
.info{font-size:13px;color:#94a3b8;margin-top:5px}

.total{
  margin-top:15px;
  font-size:22px;
  font-weight:bold;
  text-align:center;
}
.note{
  background:#020617;
  margin-top:15px;
  padding:12px;
  border-radius:10px;
  font-size:14px;
  white-space:pre-line;
}

a.btn{
  display:block;
  margin-top:20px;
  padding:15px;
  text-align:center;
  background:#22c55e;
  color:#000;
  font-weight:bold;
  text-decoration:none;
  border-radius:10px;
}
a.btn.disabled{
  background:#475569;
  pointer-events:none;
}

footer{
  margin-top:25px;
  text-align:center;
  font-size:14px;
  color:#94a3b8;
}

.badge{
  display:flex;
  gap:8px;
  justify-content:center;
  margin-bottom:10px;
  flex-wrap:wrap;
}
.badge span{
  background:#020617;
  padding:6px 10px;
  border-radius:20px;
  font-size:12px;
}

/* STOK */
.stock{
  text-align:center;
  font-weight:bold;
  margin-bottom:15px;
  padding:10px;
  border-radius:10px;
}
.stock.on{background:#16a34a}
.stock.off{background:#dc2626}

/* ANIMASI */
.fade{
  animation:fadeIn .4s ease-in-out;
}
@keyframes fadeIn{
  from{opacity:0;transform:translateY(6px)}
  to{opacity:1;transform:translateY(0)}
}
</style>
</head>

<body>
<div class="container">

<!-- LOGO -->
<div class="logo">
  <div class="logo-icon">🥚</div>
  <div class="logo-text">BADAWI FARM</div>
</div>
<div class="subtitle">Telur Ayam Petelur • Grosir & Eceran</div>

<div class="badge">
  <span>🐔 Peternakan Sendiri</span>
  <span>✅ Fresh Harian</span>
  <span>🚚 Bisa Anter</span>
</div>

<div style="text-align:center;margin:10px 0 15px;padding:10px;background:#020617;border-radius:10px;font-size:14px;color:#cbd5e1">
⏰ <b>Jam Operasional</b><br>Setiap hari 10.00 – 22.00 WIB
</div>

<div id="statusStok" class="stock on">🟢 STOK TERSEDIA</div>

<div class="box">
  <label>Nama Pembeli</label>
  <input id="nama" placeholder="Contoh: Pak Budi" oninput="hitung()">

  <label>Jenis Pembelian</label>
  <select id="jenis" onchange="hitung()">
    <option value="eceran">Eceran (Rp 27.500 / KG)</option>
    <option value="grosir">Grosir (Rp 25.800 / KG)</option>
  </select>

  <label>Jumlah (KG)</label>
  <input type="number" id="jumlah" placeholder="Contoh: 10" oninput="hitung()">
  <div class="info">ℹ️ Minimal grosir 5 KG • Gratis ongkir ≥ 10 KG (area sekitar)</div>

  <label>Mode Pengiriman</label>
  <select id="kirim" onchange="hitung()">
    <option value="ambil">Ambil di Farm (Gratis)</option>
    <option value="anter">Anter ke Lokasi</option>
  </select>

  <div class="total fade" id="total">TOTAL: Rp 0</div>
  <div class="note fade" id="nota">🧾 Nota akan muncul di sini</div>
</div>

<a class="btn" id="wa">🛒 Pesan via WhatsApp</a>

<footer>
Pembayaran: Tunai • Bank • DANA • ShopeePay<br>
© Badawi Farm
</footer>

</div>

<script>
let stokTersedia=true;

function updateStok(){
  const s=statusStok;
  const b=wa;
  if(stokTersedia){
    s.className="stock on";
    s.innerText="🟢 STOK TERSEDIA";
    b.classList.remove("disabled");
  }else{
    s.className="stock off";
    s.innerText="🔴 STOK HABIS";
    b.classList.add("disabled");
    b.innerText="❌ Stok Habis";
  }
}

function hitung(){
  if(!stokTersedia)return;

  const nama=namaInput=nama.value||"-";
  const jenisVal=jenis.value;
  const qty=Number(jumlah.value||0);
  const kirimVal=kirim.value;
  const harga=jenisVal==="eceran"?27500:25800;

  if(jenisVal==="grosir" && qty>0 && qty<5){
    total.innerText="❌ Minimal Grosir 5 KG";
    nota.innerText="⚠️ Pembelian grosir minimal 5 KG";
    return;
  }

  let ongkir=0;
  let ketOngkir="Gratis";

  if(kirimVal==="anter"){
    if(qty>=10){
      ongkir=0;
      ketOngkir="GRATIS ONGKIR (Area Sekitar)";
    }else{
      ongkir=5000;
      ketOngkir="Rp 5.000";
    }
  }

  const subtotal=qty*harga;
  const totalBayar=subtotal+ongkir;

  total.innerText="TOTAL: Rp "+totalBayar.toLocaleString("id-ID");
  nota.innerText=
`🧾 NOTA BADAWI FARM
Nama        : ${namaInput}
Jenis       : ${jenisVal}
Jumlah      : ${qty} KG
Harga/KG    : Rp ${harga.toLocaleString("id-ID")}
Subtotal    : Rp ${subtotal.toLocaleString("id-ID")}
Pengiriman  : ${kirimVal==="ambil"?"Ambil di Farm":"Anter ke Lokasi"}
Ongkir      : ${ketOngkir}
TOTAL BAYAR : Rp ${totalBayar.toLocaleString("id-ID")}`;

  wa.href="https://wa.me/6282132698172?text="+encodeURIComponent(nota.innerText);
}

updateStok();
</script>

</body>
</html>
