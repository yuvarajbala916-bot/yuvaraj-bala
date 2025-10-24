<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Mini E-Commerce Cart</title>
  <style>
    body { font-family: Arial, sans-serif; padding:20px; }
    .product { border: 1px solid #ddd; padding:10px; margin:10px; display:inline-block; width:200px; }
    .cart { border:1px solid #aaa; padding:10px; margin-top:30px; }
  </style>
</head>
<body>

<h1>Products</h1>
<div id="productList"></div>

<h2>Your Cart (<span id="count">0</span> items)</h2>
<div class="cart" id="cartContents">
  Cart is empty.
</div>

<script>
  const PRODUCTS = [
    { id: 1, name: "Product A", price: 500 },
    { id: 2, name: "Product B", price: 1200 },
    { id: 3, name: "Product C", price: 750 }
  ];

  function formatRupee(amount) {
    return "₹" + amount.toLocaleString("en-IN");
  }

  function renderProducts() {
    const container = document.getElementById("productList");
    PRODUCTS.forEach(p => {
      const div = document.createElement("div");
      div.className = "product";
      div.innerHTML = `
        <h3>${p.name}</h3>
        <p>Price: ${formatRupee(p.price)}</p>
        <button onclick="addToCart(${p.id})">Add to Cart</button>
      `;
      container.appendChild(div);
    });
  }

  let cart = [];

  function updateCartUI() {
    document.getElementById("count").innerText = cart.reduce((s,i) => s + i.qty,0);
    const cartDiv = document.getElementById("cartContents");
    if(cart.length === 0) {
      cartDiv.innerHTML = "Cart is empty.";
      return;
    }
    let html = "<ul>";
    cart.forEach(item => {
      const p = PRODUCTS.find(p=>p.id===item.productId);
      html += `<li>${p.name} x ${item.qty} = ${formatRupee(p.price * item.qty)} 
        <button onclick="removeFromCart(${item.productId})">Remove</button>
        <button onclick="changeQty(${item.productId}, ${item.qty + 1})">+</button>
        <button onclick="changeQty(${item.productId}, ${item.qty - 1})" ${item.qty<=1 ? "disabled":""}>-</button>
      </li>`;
    });
    html += `</ul><strong>Total: ${formatRupee(cart.reduce((s,i) => {
      const p = PRODUCTS.find(p=>p.id===i.productId);
      return s + (p.price * i.qty);
    },0))}</strong>`;
    cartDiv.innerHTML = html;
  }

  function addToCart(productId) {
    const existing = cart.find(i=>i.productId===productId);
    if(existing) {
      existing.qty++;
    } else {
      cart.push({ productId, qty:1 });
    }
    updateCartUI();
  }

  function removeFromCart(productId) {
    cart = cart.filter(i=>i.productId!==productId);
    updateCartUI();
  }

  function changeQty(productId, newQty) {
    const item = cart.find(i=>i.productId===productId);
    if(!item) return;
    if(newQty < 1) return;
    item.qty = newQty;
    updateCartUI();
  }

  // initial render
  renderProducts();
  updateCartUI();
</script>

</body>
</html>
# yuvaraj-bala
