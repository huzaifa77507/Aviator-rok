# Aviator-rok
flutter create aviator_app cd aviator_app
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zaika-E-Bhopal</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #f8f9fa;
        }
        .hero {
            background: url('https://images.unsplash.com/photo-1606841966341-09a4e1e2e83f') center/cover no-repeat;
            padding: 60px 0;
            text-align: center;
            color: white;
            text-shadow: 0 0 10px rgba(0,0,0,0.5);
        }
        .menu-item {
            background: white;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .cart {
            background: #e9f7ef;
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
        }
        .biryani-img {
            max-width: 100%;
            height: auto;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        .quantity-input {
            width: 60px;
            display: inline-block;
        }
        .modal-content {
            border-radius: 10px;
        }
        .btn-primary {
            background-color: #e67e22;
            border-color: #e67e22;
        }
        .btn-primary:hover {
            background-color: #d35400;
            border-color: #d35400;
        }
        .btn-danger {
            background-color: #c0392b;
            border-color: #c0392b;
        }
    </style>
</head>
<body>
    <div class="hero">
        <h1 class="display-4">Zaika-E-Bhopal</h1>
        <p class="lead">Order your favorite biryani online!</p>
    </div>
    <div class="container my-5">
        <img src="/api/artifacts/c1de0657-8cde-440c-897a-ae919edbcc4d" alt="Biryani" class="c1de0657-8cde-440c-897a-ae919edbcc4d">
        <h2 class="mb-4">Menu</h2>
        <div class="row">
            <div class="col-md-6">
                <div class="menu-item">
                    <h5>Half Plate Biryani</h5>
                    <p>₹40</p>
                    <div class="input-group mb-2">
                        <input type="number" class="form-control quantity-input" id="qty-half-plate" value="1" min="1">
                        <button class="btn btn-primary" onclick="addToCart('Half Plate Biryani', 40, 'qty-half-plate')">Add to Cart</button>
                    </div>
                </div>
            </div>
            <div class="col-md-6">
                <div class="menu-item">
                    <h5>Full Plate Biryani</h5>
                    <p>₹60</p>
                    <div class="input-group mb-2">
                        <input type="number" class="form-control quantity-input" id="qty-full-plate" value="1" min="1">
                        <button class="btn btn-primary" onclick="addToCart('Full Plate Biryani', 60, 'qty-full-plate')">Add to Cart</button>
                    </div>
                </div>
            </div>
            <div class="col-md-6">
                <div class="menu-item">
                    <h5>Half Kg Biryani</h5>
                    <p>₹85</p>
                    <div class="input-group mb-2">
                        <input type="number" class="form-control quantity-input" id="qty-half-kg" value="1" min="1">
                        <button class="btn btn-primary" onclick="addToCart('Half Kg Biryani', 85, 'qty-half-kg')">Add to Cart</button>
                    </div>
                </div>
            </div>
            <div class="col-md-6">
                <div class="menu-item">
                    <h5>1 Kg Biryani</h5>
                    <p>₹170</p>
                    <div class="input-group mb-2">
                        <input type="number" class="form-control quantity-input" id="qty-1-kg" value="1" min="1">
                        <button class="btn btn-primary" onclick="addToCart('1 Kg Biryani', 170, 'qty-1-kg')">Add to Cart</button>
                    </div>
                </div>
            </div>
        </div>

        <div class="cart">
            <h2>Cart</h2>
            <ul id="cart-items" class="list-group mb-3"></ul>
            <p><strong>Total: ₹<span id="cart-total">0</span></strong></p>
            <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#checkoutModal" onclick="openCheckout()">Checkout</button>
        </div>
    </div>

    <!-- Checkout Modal -->
    <div class="modal fade" id="checkoutModal" tabindex="-1" aria-labelledby="checkoutModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="checkoutModalLabel">Checkout</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="mb-3">
                        <label for="name" class="form-label">Name</label>
                        <input type="text" class="form-control" id="name" placeholder="Enter your name">
                    </div>
                    <div class="mb-3">
                        <label for="address" class="form-label">Delivery Address</label>
                        <textarea class="form-control" id="address" placeholder="Enter your address"></textarea>
                    </div>
                    <div class="mb-3">
                        <label for="phone" class="form-label">Phone Number</label>
                        <input type="tel" class="form-control" id="phone" placeholder="Enter your phone number">
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary" onclick="placeOrder()">Place Order</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        let cart = JSON.parse(localStorage.getItem('cart')) || [];
        let total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);

        function addToCart(item, price, qtyId) {
            const quantity = parseInt(document.getElementById(qtyId).value);
            if (quantity < 1) {
                alert('Please enter a valid quantity.');
                return;
            }
            cart.push({ item, price, quantity });
            total += price * quantity;
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCart();
        }

        function removeFromCart(index) {
            total -= cart[index].price * cart[index].quantity;
            cart.splice(index, 1);
            localStorage.setItem('cart', JSON.stringify(cart));
            updateCart();
        }

        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            const cartTotal = document.getElementById('cart-total');
            cartItems.innerHTML = '';
            cart.forEach((item, index) => {
                const li = document.createElement('li');
                li.className = 'list-group-item d-flex justify-content-between align-items-center';
                li.innerHTML = `${item.item} (x${item.quantity}) - ₹${item.price * item.quantity}
                    <button class="btn btn-danger btn-sm" onclick="removeFromCart(${index})">Remove</button>`;
                cartItems.appendChild(li);
            });
            cartTotal.textContent = total;
        }

        function openCheckout() {
            if (cart.length === 0) {
                alert('Your cart is empty.');
                const modal = bootstrap.Modal.getInstance(document.getElementById('checkoutModal'));
                modal.hide();
            }
        }

        function placeOrder() {
            const name = document.getElementById('name').value;
            const address = document.getElementById('address').value;
            const phone = document.getElementById('phone').value;
            if (!name || !address || !phone) {
                alert('Please fill in all fields.');
                return;
            }
            if (cart.length === 0) {
                alert('Your cart is empty.');
                return;
            }
            alert(`Order placed successfully!\nName: ${name}\nAddress: ${address}\nPhone: ${phone}\nTotal: ₹${total}`);
            cart = [];
            total = 0;
            localStorage.setItem('cart', JSON.stringify(cart));
            document.getElementById('name').value = '';
            document.getElementById('address').value = '';
            document.getElementById('phone').value = '';
            updateCart();
            const modal = bootstrap.Modal.getInstance(document.getElementById('checkoutModal'));
            modal.hide();
        }

        // Initialize cart on page load
        updateCart();
    </script>
</body>
</html>
