xml
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🛒 N&G Temperos - Carrinho + Catálogo (iOS OK)</title>
    <style>
        :root {
            --primary: #D2691E; --secondary: #8B4513; --accent: #CD853F; --yellow: #FFD700;
            --success: #25D366; --danger: #DC3545; --bg-gradient: linear-gradient(135deg, #8B4513 0%, #D2691E 50%, #CD853F 100%);
            --card-bg: rgba(255,255,255,0.95);
        }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Inter', sans-serif; background: var(--bg-gradient); color: #2C1810; }

        .header { text-align: center; background: rgba(255,255,255,0.98); padding: 25px; border-radius: 25px; margin: 20px auto; max-width: 1200px; box-shadow: 0 20px 60px rgba(0,0,0,0.25); }
        .logo-section { display: flex; flex-direction: column; align-items: center; gap: 15px; }
        .logo-img { width: 160px; height: 110px; object-fit: contain; border-radius: 15px; box-shadow: 0 10px 30px rgba(0,0,0,0.2); }
        .header h1 { font-size: 2.2rem; background: linear-gradient(45deg, var(--yellow), var(--primary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; font-weight: 800; }
        .whatsapp-main { display: inline-flex; align-items: center; gap: 12px; font-size: 1.3rem; font-weight: 800; color: var(--success); text-decoration: none; background: white; padding: 14px 24px; border-radius: 30px; box-shadow: 0 8px 25px rgba(37,211,102,0.3); margin: 10px 0; }
        .whatsapp-main:hover { transform: translateY(-3px); box-shadow: 0 12px 35px rgba(37,211,102,0.4); }

        .controls { display: flex; flex-wrap: wrap; gap: 15px; justify-content: center; margin: 20px 0; padding: 0 20px; }
        .search-box { padding: 14px 22px; border: 3px solid rgba(255,255,255,0.4); border-radius: 30px; font-size: 16px; width: 300px; background: rgba(255,255,255,0.9); }
        .filter-group { display: flex; gap: 8px; flex-wrap: wrap; }
        .filter-btn { padding: 12px 18px; background: rgba(255,255,255,0.9); border: 2px solid transparent; border-radius: 25px; font-weight: 600; transition: all 0.3s; cursor: pointer; }
        .filter-btn.active, .filter-btn:hover { background: var(--yellow); color: #8B4513; border-color: var(--yellow); }

        .cart-btn { background: var(--success); color: white; position: fixed; bottom: 30px; right: 30px; width: 70px; height: 70px; border-radius: 50%; font-size: 1.5rem; box-shadow: 0 10px 30px rgba(37,211,102,0.4); z-index: 1000; border: none; cursor: pointer; transition: all 0.3s; }
        .cart-btn:hover { transform: scale(1.1); box-shadow: 0 15px 40px rgba(37,211,102,0.5); }
        .cart-badge { position: absolute; top: -8px; right: -8px; background: var(--danger); color: white; border-radius: 50%; width: 28px; height: 28px; display: flex; align-items: center; justify-content: center; font-size: 0.85rem; font-weight: 800; }

        .catalog { display: grid; grid-template-columns: repeat(auto-fill, minmax(340px, 1fr)); gap: 25px; max-width: 1300px; margin: 0 auto 100px auto; padding: 0 20px; }
        .spice-card { background: var(--card-bg); border-radius: 20px; overflow: hidden; box-shadow: 0 15px 45px rgba(0,0,0,0.2); transition: all 0.4s; }
        .spice-card:hover { transform: translateY(-10px); box-shadow: 0 25px 60px rgba(0,0,0,0.3); }
        .spice-image { height: 200px; position: relative; display: flex; align-items: center; justify-content: center; font-size: 4rem; color: white; }
        .spice-image::before { content: attr(data-name); position: absolute; bottom: 15px; left: 15px; font-size: 1rem; font-weight: 800; color: white; background: rgba(0,0,0,0.8); padding: 6px 12px; border-radius: 15px; }
        .spice-badge { position: absolute; top: 10px; right: 10px; background: var(--yellow); color: #8B4513; padding: 5px 10px; border-radius: 12px; font-size: 0.8rem; font-weight: 700; }
        .spice-info { padding: 22px; }
        .spice-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
        .spice-name { font-size: 1.35rem; font-weight: 800; color: var(--secondary); }
        .spice-weight { background: var(--yellow); color: #8B4513; padding: 4px 10px; border-radius: 15px; font-size: 0.85rem; font-weight: 700; }
        .spice-desc { color: #5D4037; margin-bottom: 15px; font-size: 0.95rem; }
        .spice-actions { display: flex; gap: 10px; }
        .btn { padding: 12px 20px; border: none; border-radius: 12px; font-weight: 600; cursor: pointer; transition: all 0.3s; flex: 1; font-size: 1rem; }
        .btn-add { background: var(--success); color: white; }
        .btn-add:hover { background: #128C7E; transform: translateY(-1px); }

        .cart-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 2000; }
        .cart-content { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: white; width: 90%; max-width: 600px; max-height: 90vh; overflow-y: auto; border-radius: 20px; box-shadow: 0 25px 80px rgba(0,0,0,0.5); }
        .cart-header { padding: 25px 30px; background: linear-gradient(135deg, var(--yellow), var(--primary)); color: #8B4513; text-align: center; border-radius: 20px 20px 0 0; position: relative; }
        .cart-items { padding: 20px 30px; }
        .cart-item { display: flex; justify-content: space-between; align-items: center; padding: 15px 0; border-bottom: 1px solid #eee; }
        .cart-total { font-size: 1.3rem; font-weight: 800; text-align: center; padding: 20px 30px; background: #f8f9fa; border-radius: 0 0 20px 20px; color: var(--secondary); }
        .btn-close, .btn-clear, .btn-checkout { padding: 12px 24px; border: none; border-radius: 12px; font-weight: 700; cursor: pointer; transition: all 0.3s; margin: 5px; }
        .btn-close { background: #6C757D; color: white; position: absolute; top: 15px; right: 20px; font-size: 1.2rem; }
        .btn-clear { background: var(--danger); color: white; }
        .btn-checkout { background: var(--success); color: white; font-size: 1.1rem; width: 100%; margin-top: 10px; }

        @media (max-width: 768px) { .catalog { grid-template-columns: 1fr; } .controls { flex-direction: column; } }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap" rel="stylesheet">
</head>
<body>
    <div class="header">
        <div class="logo-section">
            <img src="N-GTemperos.jpg" alt="N&G Temperos" class="logo-img">
            <h1>🛒 N&G Temperos Premium</h1>
        </div>
        <a href="https://wa.me/5511969764296?text=Olá%20N&G%20Temperos!%20Vim%20do%20catálogo%20🛒" class="whatsapp-main" target="_blank">
            📱 (11) 96976-4296 WhatsApp Pedidos
        </a>
        <p>CNPJ: 45.598.294/0001-33 • Entrega São Paulo</p>
    </div>

    <div class="controls">
        <input type="text" class="search-box" id="searchBox" placeholder="🔍 Buscar tempero...">
        <div class="filter-group">
            <button class="filter-btn active" data-filter="all">Todos (27)</button>
            <button class="filter-btn" data-filter="especiarias">Especiarias (12)</button>
            <button class="filter-btn" data-filter="ervas">Ervas (7)</button>
            <button class="filter-btn" data-filter="misturas">Misturas (6)</button>
            <button class="filter-btn" data-filter="outros">Outros (2)</button>
        </div>
    </div>

    <div class="catalog" id="catalog"></div>

    <button class="cart-btn" id="cartBtn" onclick="toggleCart()">
        🛒<span class="cart-badge" id="cartCount" style="display:none;">0</span>
    </button>

    <div class="cart-modal" id="cartModal">
        <div class="cart-content">
            <div class="cart-header">
                <h2>🛒 Meu Pedido N&G Temperos</h2>
                <button class="btn-close" onclick="toggleCart()">✕</button>
            </div>
            <div class="cart-items" id="cartItems"></div>
            <div class="cart-total">
                <div><strong>Total: <span id="cartTotal">0 itens</span></strong></div>
                <button class="btn-checkout" onclick="checkout()">🚚 Enviar Pedido WhatsApp</button>
                <button class="btn-clear" onclick="clearCart()">🗑️ Limpar Lista</button>
            </div>
        </div>
    </div>

    <script>
        const spices = [
            {name:"Bicarbonato",weight:"40g",category:"outros",desc:"Para bolos leves e pães"},
            {name:"Cravo",weight:"10g",category:"especiarias",desc:"Aroma intenso para doces"},
            {name:"Canela Casca",weight:"10g",category:"especiarias",desc:"Sobremesas tradicionais"},
            {name:"Canela Pó",weight:"20g",category:"especiarias",desc:"Café, frutas e carnes"},
            {name:"Açafrão",weight:"20g",category:"especiarias",desc:"Arroz e frango dourado"},
            {name:"Cominho",weight:"20g",category:"especiarias",desc:"Feijão e grelhados"},
            {name:"Temp. Baiano",weight:"30g",category:"misturas",desc:"Sabor nordestino autêntico"},
            {name:"Pega Marido",weight:"20g",category:"misturas",desc:"Carnes irresistíveis"},
            {name:"Camomila",weight:"10g",category:"ervas",desc:"Chá calmante digestivo"},
            {name:"Ana Maria Braga",weight:"20g",category:"misturas",desc:"Pizzas e massas perfeitas"},
            {name:"Edu Guedes",weight:"20g",category:"misturas",desc:"Tempero de chef profissional"},
            {name:"Chimirry",weight:"20g",category:"misturas",desc:"Churrasco argentino brasileiro"},
            {name:"Lemon Pepper",weight:"20g",category:"misturas",desc:"Peixes e frango grelhado"},
            {name:"Pimenta Reino",weight:"20g",category:"especiarias",desc:"O rei da cozinha"},
            {name:"Folha Louro",weight:"5g",category:"ervas",desc:"Ensopados e feijão"},
            {name:"Defumada",weight:"15g",category:"especiarias",desc:"Churrasco com fumaça"},
            {name:"Doce",weight:"15g",category:"especiarias",desc:"Frango e ovos mexidos"},
            {name:"Picante",weight:"15g",category:"especiarias",desc:"Molhos e sopas quentes"},
            {name:"Orégano",weight:"10g",category:"ervas",desc:"Tomate e massas italianas"},
            {name:"Colorau",weight:"40g",category:"especiarias",desc:"Cor brasileira para arroz"},
            {name:"Alho Frito",weight:"10g",category:"outros",desc:"Crocante para saladas"},
            {name:"Erva Doce",weight:"15g",category:"ervas",desc:"Doces e peixes leves"},
            {name:"Boldo",weight:"10g",category:"ervas",desc:"Digestivo para carnes"},
            {name:"Alecrim",weight:"10g",category:"ervas",desc:"Batata e assados"}
        ];

        let cart = JSON.parse(localStorage.getItem('ngCart')) || [];
        let currentSpices = [...spices];

        const catalogEl = document.getElementById('catalog');
        const cartModal = document.getElementById('cartModal');
        const cartCount = document.getElementById('cartCount');
        const cartItems = document.getElementById('cartItems');
        const cartTotal = document.getElementById('cartTotal');
        const searchBox = document.getElementById('searchBox');
        const filterBtns = document.querySelectorAll('.filter-btn');

        function updateCartUI() {
            const count = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartCount.textContent = count;
            cartCount.style.display = count > 0 ? 'flex' : 'none';
            localStorage.setItem('ngCart', JSON.stringify(cart));
        }

        function addToCart(spiceIndex) {
            const existing = cart.find(item => item.index === spiceIndex);
            if (existing) {
                existing.quantity += 1;
            } else {
                cart.push({ index: spiceIndex, quantity: 1, spice: spices[spiceIndex] });
            }
            updateCartUI();
            showNotification(spices[spiceIndex].name + " adicionado!");
        }

        function renderCatalog(list) {
            catalogEl.innerHTML = list.map((spice, index) => `
                <div class="spice-card" data-index="${index}" data-category="${spice.category}">
                    <div class="spice-image" style="background: linear-gradient(135deg, #D2691E, #CD853F);" data-name="${spice.name}"></div>
                    <div class="spice-info">
                        <div class="spice-header">
                            <div class="spice-name">${spice.name}</div>
                            <div class="spice-weight">${spice.weight}</div>
                        </div>
                        <div class="spice-desc">${spice.desc}</div>
                        <div class="spice-actions">
                            <button class="btn btn-add" data-index="${index}">➕ Adicionar ao Carrinho</button>
                        </div>
                    </div>
                </div>
            `).join('');
            attachButtonListeners();
        }

        function renderCart() {
            if (cart.length === 0) {
                cartItems.innerHTML = '<p style="text-align:center;padding:40px;color:#999;font-size:1.1rem;">Carrinho vazio 😢<br>Adicione temperos clicando em ➕.</p>';
                cartTotal.textContent = '0 itens';
                return;
            }
            cartItems.innerHTML = cart.map((item, i) => `
                <div class="cart-item">
                    <div style="flex:1;">
                        <strong>${item.spice.name}</strong><br>
                        <small style="color:#666;">${item.spice.weight} × ${item.quantity}</small>
                    </div>
                    <div style="display:flex;align-items:center;gap:8px;">
                        <button onclick="updateQuantity(${i}, -1)" style="background:#DC3545;color:white;border:none;padding:6px 12px;border-radius:6px;">-</button>
                        <span style="font-weight:700;min-width:35px;text-align:center;font-size:1.1rem;">${item.quantity}</span>
                        <button onclick="updateQuantity(${i}, 1)" style="background:#25D366;color:white;border:none;padding:6px 12px;border-radius:6px;">+</button>
                        <button onclick="removeFromCart(${i})" style="background:#6C757D;color:white;border:none;padding:6px 12px;border-radius:6px;">✕</button>
                    </div>
                </div>
            `).join('');
            const totalItems = cart.reduce((s, it) => s + it.quantity, 0);
            cartTotal.textContent = totalItems + (totalItems === 1 ? " item" : " itens");
        }

        function toggleCart() {
            cartModal.style.display = cartModal.style.display === 'block' ? 'none' : 'block';
            renderCart();
        }

        function updateQuantity(i, change) {
            cart[i].quantity += change;
            if (cart[i].quantity <= 0) cart.splice(i, 1);
            updateCartUI();
            renderCart();
        }

        function removeFromCart(i) {
            cart.splice(i, 1);
            updateCartUI();
            renderCart();
        }

        function clearCart() {
            cart = [];
            updateCartUI();
            renderCart();
        }

        function checkout() {
            if (cart.length === 0) return;
            const itemsList = cart.map(item => `${item.quantity}x ${item.spice.name} (${item.spice.weight})`).join('\n');
            const totalItems = cart.reduce((s, it) => s + it.quantity, 0);
            const msg = `🛒 PEDIDO N&G TEMPEROS\n\nItens:\n${itemsList}\n\nTotal: ${totalItems} item(s)\n\nOlá! Qual o valor e como pago?`;
            window.open('https://wa.me/5511969764296?text=' + encodeURIComponent(msg), '_blank');
            showNotification('Pedido enviado para o WhatsApp!');
        }

        function filterAndSort() {
            const term = searchBox.value.toLowerCase().trim();
            const activeFilter = document.querySelector('.filter-btn.active').dataset.filter;
            currentSpices = spices.filter(s =>
                s.name.toLowerCase().includes(term) &&
                (activeFilter === 'all' || s.category === activeFilter)
            );
            renderCatalog(currentSpices);
        }

        function showNotification(msg) {
            const n = document.createElement('div');
            n.style.cssText = 'position:fixed;top:20px;right:20px;background:#25D366;color:#fff;padding:12px 20px;border-radius:10px;font-weight:600;z-index:3000;box-shadow:0 10px 30px rgba(0,0,0,0.3);';
            n.textContent = msg;
            document.body.appendChild(n);
            setTimeout(() => { n.style.opacity = '0'; n.style.transform = 'translateX(100px)'; }, 2000);
            setTimeout(() => document.body.removeChild(n), 2600);
        }

        // LISTENERS DOS BOTÕES (CLICK + TOUCH PARA iOS)
        function attachButtonListeners() {
            const buttons = document.querySelectorAll('.btn-add');
            buttons.forEach(btn => {
                const index = parseInt(btn.dataset.index);
                const handler = (e) => {
                    e.preventDefault();
                    e.stopPropagation();
                    addToCart(index);
                };
                btn.onclick = handler;
                btn.ontouchstart = handler; // suporte iOS
            });
        }

        // FILTROS
        filterBtns.forEach(btn => {
            btn.addEventListener('click', () => {
                filterBtns.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                filterAndSort();
            });
        });
        searchBox.addEventListener('input', filterAndSort);

        // INICIALIZA
        updateCartUI();
        renderCatalog(spices);
    </script>
</body>
</html>
