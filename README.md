<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>M7_Bootstrap</title>
    <link rel="stylesheet" href="global.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB" crossorigin="anonymous">
  </head>
  <body>
    <nav class="navbar bg-body-tertiary">
      <div class="container">
        <a class="navbar-brand" href="#">
          <img src="Asset_Tugas/Logo1.png" alt="Bootstrap" width="50">
          <img src="Asset_Tugas/Logo1.png" alt="Bootstrap" width="50">
        </a>
        <ul class="nav justify-content-end">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="index.html" style="color: grey; text-decoration: none;">Produk</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="Keranjang.html" style="color: black; text-decoration: none;"><b>Cart</b></a>
          </li>
        </ul>
      </div>
    </nav>

    <div class="container my-5">
      <h4 class="fw-bold mb-4">Keranjang Belanja</h4>
 
      <!-- Tempat list produk keranjang -->
      <div id="cart-container"></div>
 
      <!-- Total & tombol kosongkan -->
      <div class="total-bar">
        <h4 id="total-harga">Total: Rp0</h4>
        <button class="btn btn-outline-danger" onclick="kosongkanKeranjang()">Kosongkan Keranjang</button>
      </div>
    </div>

    <!-- FOOTER -->
    <div class="footer bg-body-tertiary">
      <div class="container text-center" style="padding-top: 30px;">
        <div class="row">
          <div class="col" style="text-align: left;">
            <div style="padding-bottom: 20px;">
              <img src="Asset_Tugas/Logo1.png" style="width: 50px;">
              <img src="Asset_Tugas/Logo2.png" style="width: 50px;">
            </div>
            <p style="font-size: 20px;"><b>UNIQIKILO Indonesia</b></p>
            <p style="font-size: 15px;">PT Fast Retailing Indonesia<br>
            Gedung Sopo Del Tower 1, Lantai 8<br>
            Jl. Jend. Sudirman No. 52-53, Jakarta 12190</p>
          </div>
          <div class="col" style="text-align: left;">
            <p style="font-size: 20px;"><b>Layanan Pelanggan</b></p>
            <p style="font-size: 15px;">Hubungi Kami</p>
            <p style="font-size: 15px;">Cara Berbelanja</p>
            <p style="font-size: 15px;">Pengiriman</p>
            <p style="font-size: 15px;">Pengembalian Produk</p>
            <p style="font-size: 15px;">Cek Status Pesanan</p>
          </div>
          <div class="col" style="text-align: left;">
            <p style="font-size: 20px;"><b>Ikuti Kami</b></p>
            <p style="font-size: 15px;">Instagram: @uniqikilo_id</p>
            <p style="font-size: 15px;">Facebook: UNIQIKILO Indonesia</p>
            <p style="font-size: 15px;">Tiktok: @uniqikilo.id</p>
            <p style="font-size: 15px;">Email: support@uniqikilo.co.id</p>
            <p style="font-size: 15px;">Customer Service: 1500-999</p>
          </div>
        </div>
        <hr>
      </div>
      <p style="text-align: center;">&copy; 2024 UNIQIKILO. All rights reserved.</p>
    </div>
    
    <script>
              function tampilkanKeranjang() {
        const keranjang = JSON.parse(localStorage.getItem("keranjang")) || [];
        const container = document.getElementById("cart-container");
        container.innerHTML = ""; // Kosongkan dulu
 
        if (keranjang.length === 0) {
          // Tampilkan pesan keranjang kosong
          container.innerHTML = `
            <div class="kosong-container">
              <h5>Keranjang Kamu Kosong</h5>
              <p>Yuk Belanja Dulu Di Halaman Produk</p>
              <a href="index.html" class="btn btn-dark">Belanja Sekarang</a>
            </div>
          `;
          document.getElementById("total-harga").textContent = "Total: Rp0";
          return;
        }
 
        // Hitung total harga
        let total = 0;
 
        // Loop setiap item dan buat card-nya
        keranjang.forEach(function(item, index) {
          const subtotal = item.harga * item.qty;
          total += subtotal;
 
          const div = document.createElement("div");
          div.classList.add("cart-item");
          div.innerHTML = `
            <img src="${item.gambar}" alt="${item.nama}">
            <div class="cart-item-info">
              <p class="nama">${item.nama}</p>
              <p class="ukuran">${item.ukuran}</p>
              <p class="harga-satuan">Rp${item.harga.toLocaleString("id-ID")}</p>
              <div class="qty-control mt-2">
                <button onclick="ubahQty(${index}, -1)">-</button>
                <span class="qty-angka">${item.qty}</span>
                <button onclick="ubahQty(${index}, +1)">+</button>
              </div>
            </div>
            <div class="cart-item-kanan">
              <p class="subtotal">Rp${subtotal.toLocaleString("id-ID")}</p>
              <button class="btn btn-danger btn-sm" onclick="hapusItem(${index})">Hapus</button>
            </div>
          `;
          container.appendChild(div);
        });
 
        // Tampilkan total
        document.getElementById("total-harga").textContent =
          "Total: Rp" + total.toLocaleString("id-ID");
      }
 
      // =============================================
      // UBAH QUANTITY (+ atau -)
      // =============================================
      function ubahQty(index, perubahan) {
        let keranjang = JSON.parse(localStorage.getItem("keranjang")) || [];
        keranjang[index].qty += perubahan;
 
        // Kalau qty jadi 0 atau kurang, hapus item
        if (keranjang[index].qty <= 0) {
          keranjang.splice(index, 1);
        }
 
        localStorage.setItem("keranjang", JSON.stringify(keranjang));
        tampilkanKeranjang(); // Refresh tampilan
      }
 
      // =============================================
      // HAPUS SATU ITEM
      // =============================================
      function hapusItem(index) {
        let keranjang = JSON.parse(localStorage.getItem("keranjang")) || [];
        keranjang.splice(index, 1); // Hapus item di posisi index
        localStorage.setItem("keranjang", JSON.stringify(keranjang));
        tampilkanKeranjang();
      }
 
      // =============================================
      // KOSONGKAN SELURUH KERANJANG
      // =============================================
      function kosongkanKeranjang() {
        localStorage.removeItem("keranjang");
        tampilkanKeranjang();
      }
 
      // Jalankan saat halaman dibuka
      tampilkanKeranjang();
    </script>
</html>
