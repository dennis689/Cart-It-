<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cart It - Fresh Fruits & Vegetables</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Roboto', sans-serif; background-color: #f5f5f5; }
        header { background-color: #2e7d32; color: white; padding: 10px 20px; display: flex; align-items: center; justify-content: space-between; }
        header h1 { font-size: 24px; }
        .search-cart { display: flex; align-items: center; gap: 15px; }
        .search-cart input[type="text"] { padding: 5px 10px; border-radius: 5px; border: none; }
        .search-cart button { padding: 5px 10px; border-radius: 5px; border: none; background-color: #81c784; color: white; cursor: pointer; }
        .cart-count { background: red; color: white; font-size: 12px; padding: 2px 6px; border-radius: 50%; position: relative; top: -10px; left: -10px; }
        .hero { display: flex; gap: 10px; overflow-x: auto; padding: 20px; }
        .hero img { border-radius: 10px; width: 300px; flex-shrink: 0; }
        section { padding: 20px; }
        h2 { margin-bottom: 10px; color: #2e7d32; }
        .products { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; }
        .product { background-color: white; padding: 10px; border-radius: 10px; text-align: center; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .product img { width: 100%; border-radius: 10px; }
        .product h3 { margin: 10px 0 5px; font-size: 16px; }
        .price { color: #388e3c; font-weight: bold; }
        .add-to-cart { background: #2e7d32; color: white; border: none; padding: 5px 10px; border-radius: 5px; margin-top: 5px; cursor: pointer; }
        footer { background-color: #2e7d32; color: white; text-align: center; padding: 15px; margin-top: 20px; }
        #cartModal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; }
        #cartContent { background: white; padding: 20px; border-radius: 10px; width: 300px; }
        #cartItems { margin-bottom: 10px; }
        .cart-item { display: flex; justify-content: space-between; margin: 5px 0; }
        .close-cart { background: red; color: white; border: none; padding: 5px 10px; cursor: pointer; }
    </style>
</head>
<body>
    <header>
        <h1>Cart It</h1>
        <div class="search-cart">
            <input type="text" placeholder="Search Products...">
            <button>🔍</button>
            <button id="cartButton">🛒 <span id="cartCount" class="cart-count">0</span></button>
        </div>
    </header>

    <div class="hero">
        <img src="https://via.placeholder.com/300x150?text=Daily+Fresh" alt="Daily Fresh">
        <img src="https://via.placeholder.com/300x150?text=Fresh+50%25+Off" alt="Fresh Deals">
        <img src="https://via.placeholder.com/300x150?text=Natural+Fruits" alt="Natural Fruits">
    </div>

    <section>
        <h2>Flash Deal - Lowest Price Guaranteed</h2>
        <div class="products" id="productList">
            <div class="product" data-name="Onion" data-price="1">
                <img src="https://via.placeholder.com/150?text=Onion" alt="Onion">
                <h3>Onion</h3>
                <p class="price">₹1 / 500 Gm</p>
                <button class="add-to-cart">Add to Cart</button>
            </div>
            <div class="product" data-name="Premium Bhindi" data-price="4">
                <img src="https://via.placeholder.com/150?text=Bhindi" alt="Bhindi">
                <h3>Premium Bhindi</h3>
                <p class="price">₹4 / 250 Gm</p>
                <button class="add-to-cart">Add to Cart</button>
            </div>
            <div class="product" data-name="Sponge Gourd" data-price="7">
                <img src="https://via.placeholder.com/150?text=Sponge+Gourd" alt="Sponge Gourd">
                <h3>Sponge Gourd</h3>
                <p class="price">₹7 / 250 Gm</p>
                <button class="add-to-cart">Add to Cart</button>
            </div>
            <div class="product" data-name="Desi Tomato" data-price="25">
                <img src="https://via.placeholder.com/150?text=Tomato" alt="Tomato">
                <h3>Desi Tomato</h3>
                <p class="price">₹25 / 500 Gm</p>
                <button class="add-to-cart">Add to Cart</button>
            </div>
            <div class="product" data-name="Spinach" data-price="8">
                <img src="https://via.placeholder.com/150?text=Spinach" alt="Spinach">
                <h3>Spinach</h3>
                <p class="price">₹8 / 250 Gm</p>
                <button class="add-to-cart">Add to Cart</button>
            </div>
            <div class="product" data-name="New Potato" data-price="15">
                <img src="https://via.placeholder.com/150?text=Potato" alt="Potato">
                <h3>New Potato</h3>
                <p class="price">₹15 / 500 Gm</p>
                <button class="add-to-cart">Add to Cart</button>
            </div>
        </div>
    </section>

    <footer>
        &copy; 2025 Cart It. All Rights Reserved.
    </footer>

    <div id="cartModal">
        <div id="cartContent">
            <h3>Your Cart</h3>
            <div id="cartItems"></div>
            <p><strong>Total: ₹<span id="cartTotal">0</span></strong></p>
            <button class="close-cart">Close</button>
        </div>
    </div>

    <script>
        const cart = [];
        const cartCount = document.getElementById('cartCount');
        const cartItemsContainer = document.getElementById('cartItems');
        const cartTotal = document.getElementById('cartTotal');
        const cartModal = document.getElementById('cartModal');

        document.querySelectorAll('.add-to-cart').forEach(button => {
            button.addEventListener('click', () => {
                const product = button.closest('.product');
                const name = product.getAttribute('data-name');
                const price = parseFloat(product.getAttribute('data-price'));

                const existing = cart.find(item => item.name === name);
                if (existing) {
                    existing.qty++;
                } else {
                    cart.push({ name, price, qty: 1 });
                }
                updateCart();
            });
        });

        function updateCart() {
            cartItemsContainer.innerHTML = '';
            let total = 0;
            cart.forEach(item => {
                total += item.price * item.qty;
                const div = document.createElement('div');
                div.classList.add('cart-item');
                div.textContent = `${item.name} x${item.qty} - ₹${item.price * item.qty}`;
                cartItemsContainer.appendChild(div);
            });
            cartTotal.textContent = total;
            cartCount.textContent = cart.reduce((sum, item) => sum + item.qty, 0);
        }

        document.getElementById('cartButton').addEventListener('click', () => {
            cartModal.style.display = 'flex';
        });

        document.querySelector('.close-cart').addEventListener('click', () => {
            cartModal.style.display = 'none';
        });

        window.addEventListener('click', e => {
            if (e.target === cartModal) {
                cartModal.style.display = 'none';
            }
        });
    </script>
</body>
</html>     
