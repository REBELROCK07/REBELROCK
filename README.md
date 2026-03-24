<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sticker Shop - Fun Stickers Collection</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 40px;
        }

        .header h1 {
            font-size: 3em;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 10px;
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .sticker-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 30px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .sticker-card {
            background: white;
            border-radius: 20px;
            padding: 25px;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .sticker-card:hover {
            transform: translateY(-10px) scale(1.05);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }

        .sticker-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: linear-gradient(90deg, #ff6b6b, #feca57, #48dbfb, #ff9ff3);
        }

        .sticker-emoji {
            font-size: 4em;
            margin-bottom: 15px;
            display: block;
            filter: drop-shadow(2px 2px 4px rgba(0,0,0,0.2));
        }

        .sticker-name {
            font-size: 1.5em;
            font-weight: bold;
            color: #333;
            margin-bottom: 10px;
        }

        .sticker-desc {
            color: #666;
            font-size: 1em;
            margin-bottom: 20px;
        }

        .price {
            font-size: 1.8em;
            font-weight: bold;
            color: #ff6b6b;
            margin-bottom: 15px;
        }

        .buy-btn {
            background: linear-gradient(45deg, #ff6b6b, #ff8e8e);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            font-size: 1.1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(255,107,107,0.4);
        }

        .buy-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(255,107,107,0.6);
        }

        .categories {
            text-align: center;
            margin-bottom: 40px;
        }

        .category-btn {
            background: rgba(255,255,255,0.2);
            color: white;
            border: 2px solid rgba(255,255,255,0.3);
            padding: 10px 25px;
            margin: 0 10px;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
        }

        .category-btn:hover,
        .category-btn.active {
            background: white;
            color: #667eea;
        }

        @media (max-width: 768px) {
            .sticker-grid {
                grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
                gap: 20px;
            }
            
            .header h1 {
                font-size: 2.2em;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>🎉 Sticker Shop 🎉</h1>
        <p>Collect the coolest stickers for your messages!</p>
    </div>

    <div class="categories">
        <button class="category-btn active" onclick="filterStickers('all')">All</button>
        <button class="category-btn" onclick="filterStickers('animals')">Animals</button>
        <button class="category-btn" onclick="filterStickers('emoji')">Emoji</button>
        <button class="category-btn" onclick="filterStickers('nature')">Nature</button>
    </div>

    <div class="sticker-grid" id="stickerGrid">
        <!-- Stickers will be generated here -->
    </div>

    <script>
        const stickers = [
            { id: 1, emoji: '🐶', name: 'Happy Dog', desc: 'A cheerful pup!', price: '$0.99', category: 'animals' },
            { id: 2, emoji: '🐱', name: 'Cool Cat', desc: 'Meow! 😸', price: '$0.99', category: 'animals' },
            { id: 3, emoji: '😂', name: 'Laugh', desc: 'LOL moment', price: '$0.99', category: 'emoji' },
            { id: 4, emoji: '🤔', name: 'Thinking', desc: 'Deep thoughts', price: '$0.99', category: 'emoji' },
            { id: 5, emoji: '🌸', name: 'Flower', desc: 'Beautiful bloom', price: '$0.99', category: 'nature' },
            { id: 6, emoji: '🌿', name: 'Leaf', desc: 'Fresh & green', price: '$0.99', category: 'nature' },
            { id: 7, emoji: '🦁', name: 'Lion King', desc: 'Roar! 👑', price: '$1.99', category: 'animals' },
            { id: 8, emoji: '🎉', name: 'Party', desc: 'Let\'s celebrate!', price: '$0.99', category: 'emoji' },
            { id: 9, emoji: '🌈', name: 'Rainbow', desc: 'Colorful magic', price: '$1.49', category: 'nature' }
        ];

        let currentFilter = 'all';

        function renderStickers(filter = 'all') {
            const grid = document.getElementById('stickerGrid');
            grid.innerHTML = '';

            const filtered = filter === 'all' ? stickers : stickers.filter(s => s.category === filter);

            filtered.forEach(sticker => {
                const card = document.createElement('div');
                card.className = 'sticker-card';
                card.innerHTML = `
                    <span class="sticker-emoji">${sticker.emoji}</span>
                    <div class="sticker-name">${sticker.name}</div>
                    <div class="sticker-desc">${sticker.desc}</div>
                    <div class="price">${sticker.price}</div>
                    <button class="buy-btn" onclick="addToCart('${sticker.name}')">Add to Cart</button>
                `;
                grid.appendChild(card);
            });
        }

        function filterStickers(category) {
            currentFilter = category;
            document.querySelectorAll('.category-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            renderStickers(category);
        }

        function addToCart(stickerName) {
            alert(`${stickerName} added to cart! 🛒`);
        }

        // Initial render
        renderStickers();
    </script>
</body>
</html>
