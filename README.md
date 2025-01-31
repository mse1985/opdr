<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Торговая площадка</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f9f9f9;
        }
        header {
            background-color: #4CAF50;
            color: white;
            padding: 15px;
            text-align: center;
        }
        .container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin: 20px;
        }
        .product {
            background-color: white;
            border: 1px solid #ddd;
            border-radius: 5px;
            margin: 10px;
            width: 300px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .product img {
            width: 100%;
            border-bottom: 1px solid #ddd;
        }
        .product h2 {
            font-size: 20px;
            margin: 10px 0;
        }
        .product p {
            font-size: 16px;
            color: #555;
            margin: 10px;
        }
        .product button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 15px;
            margin: 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        .product button:hover {
            background-color: #45a049;
        }
        .cart {
            margin: 20px;
            padding: 10px;
            background-color: white;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .cart h2 {
            margin: 0;
        }
        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .remove-button {
            background-color: red;
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        .order-form, .registration-form {
            margin: 20px;
            padding: 10px;
            background-color: white;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .order-form input, .registration-form input {
            margin: 5px 0;
            padding: 10px;
            width: calc(100% - 22px);
        }
        .order-form button, .registration-form button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 15px;
            cursor: pointer;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Добро пожаловать на Торговую Площадку</h1>
    </header>
    <div class="container">
        <!-- Пример продукта -->
        <div class="product">
            <img src="https://via.placeholder.com/300x200" alt="Продукт 1">
            <h2>Продукт 1</h2>
            <p>Цена: 1000 руб.</p>
            <button onclick="addToCart('Продукт 1', 1000)">Добавить в корзину</button>
        </div>
        <div class="product">
            <img src="https://via.placeholder.com/300x200" alt="Продукт 2">
            <h2>Продукт 2</h2>
            <p>Цена: 2000 руб.</p>
            <button onclick="addToCart('Продукт 2', 2000)">Добавить в корзину</button>
        </div>
        <div class="product">
            <img src="https://via.placeholder.com/300x200" alt="Продукт 3">
            <h2>Продукт 3</h2>
            <p>Цена: 1500 руб.</p>
            <button onclick="addToCart('Продукт 3', 1500)">Добавить в корзину</button>
        </div>
    </div>

    <div class="cart">
        <h2>Корзина</h2>
        <ul id="cartItems"></ul>
        <p id="totalPrice">Общая стоимость: 0 руб.</p>
    </div>

    <div class="order-form" id="orderForm" style="display: none;">
        <h2>Оформление заказа</h2>
        <input type="text" id="customerName" placeholder="Ваше имя" required>
        <input type="text" id="customerAddress" placeholder="Ваш адрес" required>
        <button onclick="placeOrder()">Подтвердить заказ</button>
    </div>

    <div class="registration-form" id="registrationForm">
        <h2>Регистрация</h2>
        <input type="text" id="regName" placeholder="Ваше имя" required>
        <input type="email" id="regEmail" placeholder="Электронная почта" required>
        <input type="password" id="regPassword" placeholder="Пароль" required>
        <button onclick="registerUser()">Зарегистрироваться</button>
    </div>

    <script>
        let cart = [];
        let total = 0;

        function addToCart(productName, productPrice) {
            cart.push({ name: productName, price: productPrice });
            total += productPrice;
            updateCart();
            alert(productName + " добавлен в корзину!");
        }

        function removeFromCart(index) {
            total -= cart[index].price;
            cart.splice(index, 1);
            updateCart();
        }

        function updateCart() {
            const cartItems = document.getElementById('cartItems');
            cartItems.innerHTML = '';
            cart.forEach((item, index) => {
                const li = document.createElement('li');
                li.className = 'cart-item';
                li.textContent = item.name + ' - ' + item.price + ' руб.';
                const removeButton = document.createElement('button');
                removeButton.className = 'remove-button';
                removeButton.textContent = 'Удалить';
                removeButton.onclick = () => removeFromCart(index);
                li.appendChild(removeButton);
                cartItems.appendChild(li);
            });
            document.getElementById('totalPrice').textContent = 'Общая стоимость: ' + total + ' руб.';
            document.getElementById('orderForm').style.display = cart.length > 0 ? 'block' : 'none';
        }

        function placeOrder() {
            const name = document.getElementById('customerName').value;
            const address = document.getElementById('customerAddress').value;
            if (name && address) {
                alert(`Спасибо, ${name}! Ваш заказ на сумму ${total} руб. оформлен и будет доставлен по адресу: ${address}.`);
                cart = [];
                total = 0;
                updateCart();
                document.getElementById('customerName').value = '';
                document.getElementById('customerAddress').value = '';
            } else {
                alert('Пожалуйста, заполните все поля.');
            }
        }

        function registerUser() {
            const name = document.getElementById('regName').value;
            const email = document.getElementById('regEmail').value;
            const password = document.getElementById('regPassword').value;
            if (name && email && password) {
                alert(`Спасибо за регистрацию, ${name}!`);
                document.getElementById('regName').value = '';
                document.getElementById('regEmail').value = '';
                document.getElementById('regPassword').value = '';
            } else {
                alert('Пожалуйста, заполните все поля.');
            }
        }
    </script>
</body>
</html>
# opdr
