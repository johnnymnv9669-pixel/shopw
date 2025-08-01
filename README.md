<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>ร้านค้าของคุณ</title>
<style>
  body {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    background: #f5f7fa;
    margin: 0; padding: 20px;
    color: #333;
  }
  .container {
    max-width: 960px;
    margin: auto;
  }
  h1 {
    text-align: center;
    margin-bottom: 24px;
  }
  .products-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(120px,1fr));
    gap: 16px;
  }
  .product-card {
    background: white;
    border-radius: 8px;
    padding: 12px;
    box-shadow: 0 2px 5px rgb(0 0 0 / 0.1);
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }
  .product-card img {
    width: 100%;
    height: 120px;
    object-fit: contain;
    border-radius: 4px;
    margin-bottom: 10px;
  }
  .product-name {
    font-weight: 600;
    margin-bottom: 6px;
    font-size: 1rem;
  }
  .product-price {
    color: #0070f3;
    font-weight: 700;
    margin-bottom: 10px;
  }
  .btn-order {
    background-color: #0070f3;
    color: white;
    border: none;
    border-radius: 6px;
    padding: 8px 12px;
    font-size: 0.9rem;
    cursor: pointer;
    text-decoration: none;
    display: inline-block;
    width: 100%;
  }
  .btn-order:hover {
    background-color: #005bb5;
  }

  @media (min-width: 480px) {
    .products-grid {
      grid-template-columns: repeat(3, 1fr);
    }
  }
</style>
</head>
<body>
  <div class="container">
    <h1>ร้านค้าของคุณ</h1>
    <div id="products" class="products-grid">
      <!-- สินค้าจะมาแสดงที่นี่ -->
    </div>
  </div>

  <script>
    // ใส่ URL Web App ของคุณตรงนี้
    const API_URL = "https://script.google.com/macros/s/AKfycbyuHls7E56wQ7nzItCA-UOcOUKQChl1J7QLVmx42-81zrerlqNyfOa6iX9lbdlHnPtE6g/exec";

    async function fetchProducts() {
      try {
        const res = await fetch(API_URL);
        if (!res.ok) throw new Error("ไม่สามารถโหลดข้อมูลสินค้าได้");
        const products = await res.json();
        renderProducts(products);
      } catch (e) {
        document.getElementById("products").innerHTML = <p style="color:red;">${e.message}</p>;
        console.error(e);
      }
    }

    function renderProducts(products) {
      const container = document.getElementById("products");
      container.innerHTML = "";

      products.forEach(p => {
        const card = document.createElement("div");
        card.className = "product-card";

        const imageUrl = p.ImageURL || "";
        const name = p.Name || "ไม่มีชื่อสินค้า";
        const price = p.Price || "0";
        const orderLink = p.OrderLink || "#";

        card.innerHTML = `
          <img src="${imageUrl}" alt="${name}" loading="lazy" />
          <div class="product-name">${name}</div>
          <div class="product-price">฿${price}</div>
          <a href="${orderLink}" target="_blank" class="btn-order">สั่งซื้อ</a>
        `;

        container.appendChild(card);
      });
    }

    fetchProducts();
  </script>
</body>
</html>
