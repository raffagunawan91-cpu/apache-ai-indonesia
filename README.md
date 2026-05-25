# apache-ai-indonesia<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Apache AI Indonesia - Chat</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            background: #0d0d0d;
            color: #e0e0e0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            height: 100vh;
            overflow: hidden;
        }

        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #0d0d0d; }
        ::-webkit-scrollbar-thumb { background: #333; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #555; }

        .topbar {
            height: 55px;
            background: #111;
            border-bottom: 1px solid #222;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 15px;
            position: fixed;
            top: 0; left: 0; right: 0;
            z-index: 100;
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 10px;
            text-decoration: none;
        }

        .brand-logo {
            width: 32px;
            height: 32px;
            background: linear-gradient(135deg, #00d4ff, #0066cc);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            font-size: 18px;
        }

        .brand-text { display: flex; flex-direction: column; }
        .brand h1 {
            font-size: 18px;
            color: #fff;
            font-weight: 600;
            letter-spacing: 0.5px;
            line-height: 1.2;
        }
        .brand .creator {
            font-size: 10px;
            color: #555;
            letter-spacing: 0.5px;
            margin-top: -2px;
        }

        .topbar-btns {
            display: flex;
            gap: 8px;
        }

        .icon-btn {
            width: 36px;
            height: 36px;
            border: 1px solid #333;
            background: #1a1a1a;
            color: #aaa;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: 0.2s;
        }

        .icon-btn:hover {
            background: #252525;
            color: #fff;
            border-color: #444;
        }

        .sidebar {
            position: fixed;
            top: 55px;
            left: 0;
            bottom: 0;
            width: 260px;
            background: #111;
            border-right: 1px solid #222;
            transform: translateX(-100%);
            transition: transform 0.25s ease;
            z-index: 99;
            padding: 15px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }

        .sidebar.open { transform: translateX(0); }

        .overlay {
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.6);
            z-index: 98;
            display: none;
        }

        .overlay.show { display: block; }

        .new-chat {
            width: 100%;
            padding: 10px;
            background: #1a1a1a;
            border: 1px solid #333;
            color: #fff;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            margin-bottom: 20px;
            transition: 0.2s;
        }

        .new-chat:hover {
            border-color: #00d4ff;
            color: #00d4ff;
        }

        .history-title {
            font-size: 11px;
            text-transform: uppercase;
            color: #666;
            margin-bottom: 10px;
            letter-spacing: 1px;
        }

        .history-item {
            padding: 8px 10px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 13px;
            color: #999;
            margin-bottom: 4px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            transition: 0.15s;
        }

        .history-item:hover {
            background: #1a1a1a;
            color: #ccc;
        }

        .history-item.active {
            background: rgba(0, 212, 255, 0.08);
            color: #00d4ff;
            border-left: 2px solid #00d4ff;
        }

        .sidebar-footer {
            margin-top: auto;
            padding-top: 15px;
            border-top: 1px solid #222;
            text-align: center;
        }

        .sidebar-footer .credit {
            font-size: 11px;
            color: #444;
        }

        .sidebar-footer .credit-name {
            color: #00d4ff;
            font-weight: 600;
            font-size: 12px;
        }

        .sidebar-url {
            margin-top: 8px;
            font-size: 10px;
            color: #333;
            word-break: break-all;
        }

        .sidebar-url a {
            color: #444;
            text-decoration: none;
            transition: 0.3s;
        }

        .sidebar-url a:hover {
            color: #00d4ff;
        }

        .main {
            margin-top: 55px;
            height: calc(100vh - 55px);
            display: flex;
            flex-direction: column;
            position: relative;
        }

        @media (min-width: 1024px) {
            .main { margin-left: 260px; }
            .sidebar { transform: translateX(0); }
            .menu-btn { display: none !important; }
            .overlay { display: none !important; }
        }

        .chat-box {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            padding-bottom: 140px;
        }

        .welcome {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100%;
            text-align: center;
            padding: 40px 20px;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .welcome-icon {
            width: 70px;
            height: 70px;
            background: linear-gradient(135deg, #00d4ff, #0055aa);
            border-radius: 18px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 35px;
            margin-bottom: 20px;
            box-shadow: 0 0 25px rgba(0, 212, 255, 0.2);
        }

        .welcome h2 {
            font-size: 28px;
            margin-bottom: 4px;
            color: #fff;
        }

        .welcome .subtitle {
            font-size: 15px;
            color: #00d4ff;
            margin-bottom: 6px;
            font-weight: 500;
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        .welcome .creator-tag {
            font-size: 13px;
            color: #666;
            margin-bottom: 10px;
        }

        .welcome p {
            color: #888;
            font-size: 15px;
            max-width: 450px;
            line-height: 1.6;
            margin-bottom: 35px;
        }

        .welcome-url {
            font-size: 12px;
            color: #444;
            margin-bottom: 30px;
            font-family: 'Courier New', monospace;
        }

        .welcome-url a {
            color: #555;
            text-decoration: none;
            border-bottom: 1px solid #333;
            transition: 0.3s;
        }

        .welcome-url a:hover {
            color: #00d4ff;
            border-color: #00d4ff;
        }

        .suggestions {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 10px;
            max-width: 700px;
            width: 100%;
        }

        .suggestion {
            background: #161616;
            border: 1px solid #252525;
            border-radius: 10px;
            padding: 15px;
            cursor: pointer;
            text-align: left;
            transition: 0.2s;
        }

        .suggestion:hover {
            background: #1c1c1c;
            border-color: #333;
            transform: translateY(-2px);
        }

        .suggestion .icon { font-size: 22px; margin-bottom: 6px; display: block; }
        .suggestion .title { font-size: 14px; font-weight: 600; color: #ddd; margin-bottom: 4px; }
        .suggestion .desc { font-size: 12px; color: #666; line-height: 1.4; }

        .msg {
            display: flex;
            gap: 12px;
            margin-bottom: 24px;
            animation: fadeIn 0.3s ease;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            width: 100%;
        }

        .msg-avatar {
            width: 34px;
            height: 34px;
            border-radius: 50%;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 15px;
            font-weight: bold;
        }

        .user-msg .msg-avatar { background: #333; color: #fff; }
        .ai-msg .msg-avatar { 
            background: linear-gradient(135deg, #00d4ff, #0066cc); 
            color: white;
            box-shadow: 0 0 10px rgba(0, 212, 255, 0.25);
        }

        .msg-body { flex: 1; min-width: 0; }

        .msg-name {
            font-size: 13px;
            font-weight: 600;
            color: #ccc;
            margin-bottom: 4px;
        }

        .msg-bubble {
            background: #161616;
            border: 1px solid #252525;
            border-radius: 12px;
            padding: 12px 16px;
            font-size: 15px;
            line-height: 1.7;
            color: #bbb;
            word-wrap: break-word;
        }

        .user-msg .msg-bubble {
            background: #1e1e1e;
            border-color: #333;
            color: #eee;
        }

        .msg-bubble code {
            background: #0a0a0a;
            padding: 2px 6px;
            border-radius: 4px;
            font-family: 'Courier New', monospace;
            font-size: 13px;
            color: #00d4ff;
            border: 1px solid #222;
        }

        .msg-bubble pre {
            background: #0a0a0a;
            border: 1px solid #222;
            border-radius: 8px;
            padding: 14px;
            overflow-x: auto;
            margin: 10px 0;
            font-family: 'Courier New', monospace;
            font-size: 13px;
            line-height: 1.5;
        }

        .msg-bubble pre code {
            background: none;
            border: none;
            padding: 0;
            color: #aaa;
        }

        .typing {
            display: flex;
            gap: 4px;
            padding: 6px 0;
        }

        .typing span {
            width: 7px;
            height: 7px;
            background: #00d4ff;
            border-radius: 50%;
            animation: bounce 1.4s infinite ease-in-out both;
        }

        .typing span:nth-child(1) { animation-delay: -0.32s; }
        .typing span:nth-child(2) { animation-delay: -0.16s; }

        @keyframes bounce {
            0%, 80%, 100% { transform: scale(0); }
            40% { transform: scale(1); }
        }

        .input-area {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: #0d0d0d;
            border-top: 1px solid #222;
            padding: 12px 15px 10px;
            z-index: 50;
        }

        @media (min-width: 1024px) {
            .input-area { left: 260px; }
        }

        .input-wrap {
            max-width: 800px;
            margin: 0 auto;
            position: relative;
        }

        .chat-input {
            width: 100%;
            background: #161616;
            border: 1px solid #2a2a2a;
            border-radius: 14px;
            padding: 13px 45px 13px 16px;
            font-size: 15px;
            color: #fff;
            font-family: inherit;
            outline: none;
            resize: none;
            min-height: 50px;
            max-height: 150px;
            line-height: 1.5;
        }

        .chat-input::placeholder { color: #555; }

        .chat-input:focus {
            border-color: #00d4ff;
            box-shadow: 0 0 0 2px rgba(0, 212, 255, 0.1);
        }

        .send {
            position: absolute;
            right: 8px;
            bottom: 7px;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border: none;
            background: linear-gradient(135deg, #00d4ff, #0066cc);
            color: white;
            font-size: 18px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: 0.2s;
            box-shadow: 0 0 12px rgba(0, 212, 255, 0.3);
        }

        .send:hover { transform: scale(1.08); }
        .send:active { transform: scale(0.95); }
        .send:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }

        .hint {
            text-align: center;
            margin-top: 4px;
            font-size: 11px;
            color: #444;
        }

        .footer-credit {
            text-align: center;
            margin-top: 2px;
            font-size: 11px;
            color: #333;
            letter-spacing: 0.5px;
        }

        .footer-credit .name {
            color: #444;
            font-weight: 600;
        }

        .footer-credit .name:hover {
            color: #00d4ff;
            transition: 0.3s;
        }

        .footer-url {
            text-align: center;
            margin-top: 2px;
            font-size: 10px;
            color: #2a2a2a;
            font-family: 'Courier New', monospace;
        }

        .footer-url a {
            color: #333;
            text-decoration: none;
            transition: 0.3s;
        }

        .footer-url a:hover {
            color: #00d4ff;
        }

        .msg-actions {
            margin-top: 6px;
            opacity: 0;
            transition: 0.2s;
        }

        .msg:hover .msg-actions { opacity: 1; }

        .msg-btn {
            background: transparent;
            border: none;
            color: #555;
            font-size: 12px;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 4px;
            transition: 0.15s;
        }

        .msg-btn:hover { background: #222; color: #aaa; }

        @media (max-width: 768px) {
            .welcome h2 { font-size: 22px; }
            .welcome .subtitle { font-size: 13px; }
            .welcome .creator-tag { font-size: 12px; }
            .suggestions { grid-template-columns: 1fr; }
            .msg { gap: 10px; }
            .msg-avatar { width: 30px; height: 30px; font-size: 13px; }
            .msg-bubble { font-size: 14px; padding: 10px 13px; }
            .chat-input { font-size: 16px; }
            .msg-actions { opacity: 1; }
            .brand .creator { display: none; }
            .welcome-url { font-size: 11px; }
        }

        .toast {
            position: fixed;
            bottom: 120px;
            left: 50%;
            transform: translateX(-50%);
            background: #1a1a1a;
            border: 1px solid #333;
            color: #fff;
            padding: 10px 20px;
            border-radius: 8px;
            font-size: 13px;
            z-index: 9999;
            animation: fadeIn 0.3s ease;
            pointer-events: none;
        }

        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div class="topbar">
        <div style="display:flex; align-items:center; gap:10px;">
            <button class="icon-btn menu-btn" onclick="toggleMenu()">☰</button>
            <a href="https://apache-ai-indonesia.com" target="_blank" class="brand">
                <div class="brand-logo">A</div>
                <div class="brand-text">
                    <h1>Apache AI</h1>
                    <span class="creator">by Muhammad Raffa Gunawan</span>
                </div>
            </a>
        </div>
        <div class="topbar-btns">
            <button class="icon-btn" onclick="startNewChat()" title="Chat baru">✎</button>
            <button class="icon-btn" onclick="alert('Tema gelap aktif')" title="Tema">◐</button>
        </div>
    </div>

    <div class="overlay" id="overlay" onclick="toggleMenu()"></div>

    <div class="sidebar" id="sidebar">
        <button class="new-chat" onclick="startNewChat(); toggleMenu();">+ Chat Baru</button>
        <div class="history-title">Riwayat</div>
        <div id="historyList">
            <div class="history-item active" onclick="loadChat(-1)">💬 Percakapan Baru</div>
        </div>
        
        <div class="sidebar-footer">
            <div class="credit">Dibuat oleh</div>
            <div class="credit-name">Muhammad Raffa Gunawan</div>
            <div class="sidebar-url">
                <a href="https://apache-ai-indonesia.com" target="_blank">apache-ai-indonesia.com</a>
            </div>
        </div>
    </div>

    <div class="main">
        <div class="chat-box" id="chatBox">
            
            <div class="welcome" id="welcome">
                <div class="welcome-icon">🤖</div>
                <h2>Apache AI</h2>
                <div class="subtitle">Indonesia</div>
                <div class="creator-tag">by Muhammad Raffa Gunawan</div>
                <div class="welcome-url">
                    <a href="https://apache-ai-indonesia.com" target="_blank">https://apache-ai-indonesia.com</a>
                </div>
                <p>Asisten pintar yang siap bantu nulis kode, jawab pertanyaan, atau sekadar ngobrol.</p>
                <div class="suggestions">
                    <div class="suggestion" onclick="quickAsk('Jelaskan tentang AI')">
                        <span class="icon">🧠</span>
                        <div class="title">Jelaskan AI</div>
                        <div class="desc">Penjelasan detail tentang kecerdasan buatan</div>
                    </div>
                    <div class="suggestion" onclick="quickAsk('Bantu saya coding Python')">
                        <span class="icon">💻</span>
                        <div class="title">Bantu Coding</div>
                        <div class="desc">Bantuin error atau bikin script baru</div>
                    </div>
                    <div class="suggestion" onclick="quickAsk('Tulis essay tentang teknologi')">
                        <span class="icon">✍️</span>
                        <div class="title">Tulis Essay</div>
                        <div class="desc">Bantuin nulis makalah atau artikel</div>
                    </div>
                    <div class="suggestion" onclick="quickAsk('Terjemahkan ke bahasa Inggris')">
                        <span class="icon">🌐</span>
                        <div class="title">Terjemahan</div>
                        <div class="desc">Terjemahin antar bahasa dengan natural</div>
                    </div>
                </div>
            </div>

            <div id="messages" class="hidden"></div>
        </div>

        <div class="input-area">
            <div class="input-wrap">
                <textarea 
                    id="input" 
                    class="chat-input" 
                    placeholder="Ketik pesan..." 
                    rows="1"
                    oninput="autoGrow(this)"
                    onkeydown="if(event.key==='Enter' && !event.shiftKey){event.preventDefault();sendMsg();}"
                ></textarea>
                <button class="send" id="sendBtn" onclick="sendMsg()">↑</button>
            </div>
            <div class="hint">Enter kirim • Shift+Enter baris baru</div>
            <div class="footer-credit">
                Apache AI Indonesia — <span class="name">by Muhammad Raffa Gunawan</span>
            </div>
            <div class="footer-url">
                <a href="https://apache-ai-indonesia.com" target="_blank">apache-ai-indonesia.com</a>
            </div>
        </div>
    </div>

    <script>
        var chats = [];
        var currentChat = { id: Date.now(), msgs: [] };
        var isAiTyping = false;
        var chatIdCounter = 1;

        var sidebar = document.getElementById('sidebar');
        var overlay = document.getElementById('overlay');
        var welcome = document.getElementById('welcome');
        var messagesDiv = document.getElementById('messages');
        var input = document.getElementById('input');
        var sendBtn = document.getElementById('sendBtn');
        var chatBox = document.getElementById('chatBox');
        var historyList = document.getElementById('historyList');

        function toggleMenu() {
            sidebar.classList.toggle('open');
            overlay.classList.toggle('show');
            if(sidebar.classList.contains('open') && window.innerWidth < 1024) {
                document.body.style.overflow = 'hidden';
            } else {
                document.body.style.overflow = '';
            }
        }

        function autoGrow(el) {
            el.style.height = 'auto';
            el.style.height = Math.min(el.scrollHeight, 150) + 'px';
        }

        function quickAsk(text) {
            input.value = text;
            autoGrow(input);
            sendMsg();
        }

        function sendMsg() {
            var text = input.value.trim();
            if(!text || isAiTyping) return;

            welcome.classList.add('hidden');
            messagesDiv.classList.remove('hidden');

            addMsg('user', text);

            input.value = '';
            input.style.height = 'auto';
            input.focus();

            showTyping();

            setTimeout(function() {
                hideTyping();
                var reply = generateReply(text);
                addMsg('ai', reply);
            }, 1200 + Math.random() * 1500);
        }

        function addMsg(role, text) {
            var msg = {
                id: Date.now() + Math.random(),
                role: role,
                text: text,
                time: new Date().toLocaleTimeString('id-ID', {hour:'2-digit', minute:'2-digit'})
            };
            currentChat.msgs.push(msg);

            var div = document.createElement('div');
            div.className = 'msg ' + (role === 'user' ? 'user-msg' : 'ai-msg');
            div.dataset.id = msg.id;

            var avatar = role === 'user' ? '👤' : '🤖';
            var name = role === 'user' ? 'Kamu' : 'Apache AI';

            var formatted = text
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                .replace(/\n/g, '<br>');

            div.innerHTML = 
                '<div class="msg-avatar">' + avatar + '</div>' +
                '<div class="msg-body">' +
                    '<div class="msg-name">' + name + ' <span style="color:#555; font-weight:normal; font-size:11px;">' + msg.time + '</span></div>' +
                    '<div class="msg-bubble">' + formatted + '</div>' +
                    '<div class="msg-actions">' +
                        '<button class="msg-btn" onclick="copyText(' + msg.id + ')">📋 Salin</button>' +
                        (role === 'ai' ? '<button class="msg-btn" onclick="retry(' + msg.id + ')">🔄 Ulangi</button>' : '') +
                    '</div>' +
                '</div>';

            messagesDiv.appendChild(div);
            scrollDown();

            if(currentChat.msgs.length === 1 && role === 'user') {
                currentChat.title = text.substring(0, 25) + (text.length > 25 ? '...' : '');
                updateHistory();
            }
        }

        function showTyping() {
            isAiTyping = true;
            sendBtn.disabled = true;

            var div = document.createElement('div');
            div.className = 'msg ai-msg';
            div.id = 'typingIndicator';
            div.innerHTML = 
                '<div class="msg-avatar">🤖</div>' +
                '<div class="msg-body">' +
                    '<div class="msg-name">Apache AI</div>' +
                    '<div class="msg-bubble">' +
                        '<div class="typing"><span></span><span></span><span></span></div>' +
                    '</div>' +
                '</div>';

            messagesDiv.appendChild(div);
            scrollDown();
        }

        function hideTyping() {
            var el = document.getElementById('typingIndicator');
            if(el) el.remove();
            isAiTyping = false;
            sendBtn.disabled = false;
        }

        function generateReply(userText) {
            var lower = userText.toLowerCase();
            
            if(lower.includes('halo') || lower.includes('hi')) {
                return 'Halo! Selamat datang di Apache AI Indonesia. Ada yang bisa saya bantu? 🇮🇩😊';
            }
            if(lower.includes('coding') || lower.includes('python') || lower.includes('javascript') || lower.includes('code')) {
                return 'Tentu, saya bisa bantu coding! Contohnya kalau kamu mau bikin fungsi sederhana di Python:\n\n<pre><code>def halo(nama):\n    return f"Halo, {nama}!"\n\nprint(halo("Apache"))</code></pre>\n\nMau dibantu apa lagi?';
            }
            if(lower.includes('terjemah') || lower.includes('translate')) {
                return 'Saya bisa terjemahin ke beberapa bahasa. Coba kirim teks yang mau diterjemahkan! 🌐';
            }
            if(lower.includes('siapa pembuat') || lower.includes('siapa yang buat') || lower.includes('creator') || lower.includes('developer')) {
                return 'Apache AI Indonesia dibuat oleh **Muhammad Raffa Gunawan**.\n\nKunjungi website resmi di: https://apache-ai-indonesia.com 🇮🇩';
            }
            if(lower.includes('ai') || lower.includes('kecerdasan buatan')) {
                return 'AI (Artificial Intelligence) itu teknologi yang bikin mesin bisa "berpikir" dan belajar dari data. Contohnya:\n\n• <strong>Machine Learning</strong> - belajar dari pola data\n• <strong>Deep Learning</strong> - neural networks yang kompleks\n• <strong>NLP</strong> - memahami bahasa manusia\n\nApache AI Indonesia dibuat oleh Muhammad Raffa Gunawan! 🤖🇮🇩';
            }
            
            var defaults = [
                'Menarik pertanyaannya! Saya pikir "' + userText + '" itu topik yang worth untuk dibahas lebih dalam.',
                'Oke, saya coba jelasin ya tentang "' + userText + '".\n\nSecara umum, ini tergantung konteksnya. Kalau kamu mau detailnya, coba tanyain lebih spesifik lagi.',
                'Hmm, saya ngerti maksudnya. Jadi "' + userText + '" itu... \n\nSaya coba rangkum: intinya ada beberapa faktor penting yang perlu diperhatiin. Mau saya jelasin lebih detail?',
                'Baik, saya bantu jawab tentang "' + userText + '".\n\nPertama-tama, kita perlu paham dasarnya dulu. Nanti saya bisa jelasin step by step.'
            ];
            return defaults[Math.floor(Math.random() * defaults.length)];
        }

        function scrollDown() {
            chatBox.scrollTo({ top: chatBox.scrollHeight, behavior: 'smooth' });
        }

        function copyText(id) {
            var msg = currentChat.msgs.find(function(m) { return m.id == id; });
            if(msg) {
                navigator.clipboard.writeText(msg.text).then(function() {
                    showToast('Disalin!');
                });
            }
        }

        function retry(id) {
            var idx = currentChat.msgs.findIndex(function(m) { return m.id == id; });
            if(idx > 0 && currentChat.msgs[idx-1].role === 'user') {
                var userText = currentChat.msgs[idx-1].text;
                currentChat.msgs.splice(idx, 1);
                var el = messagesDiv.querySelector('[data-id="' + id + '"]');
                if(el) el.remove();
                showTyping();
                setTimeout(function() {
                    hideTyping();
                    addMsg('ai', generateReply(userText));
                }, 1000);
            }
        }

        function startNewChat() {
            if(currentChat.msgs.length > 0) {
                chats.unshift(currentChat);
            }
            currentChat = { id: Date.now(), msgs: [] };
            messagesDiv.innerHTML = '';
            messagesDiv.classList.add('hidden');
            welcome.classList.remove('hidden');
            input.value = '';
            input.style.height = 'auto';
            updateHistory();
        }

        function updateHistory() {
            var html = '';
            var title = currentChat.title || 'Percakapan Baru';
            html += '<div class="history-item active" onclick="loadChat(-1)">💬 ' + title + '</div>';
            
            for(var i = 0; i < chats.length; i++) {
                var t = chats[i].title || 'Chat ' + (i+1);
                html += '<div class="history-item" onclick="loadChat(' + i + ')">💬 ' + t + '</div>';
            }
            
            historyList.innerHTML = html;
        }

        function loadChat(index) {
            if(index === -1) {
                if(currentChat.msgs.length === 0) {
                    messagesDiv.innerHTML = '';
                    messagesDiv.classList.add('hidden');
                    welcome.classList.remove('hidden');
                } else {
                    renderMessages(currentChat.msgs);
                }
            } else {
                renderMessages(chats[index].msgs);
            }
            toggleMenu();
        }

        function renderMessages(msgs) {
            welcome.classList.add('hidden');
            messagesDiv.classList.remove('hidden');
            messagesDiv.innerHTML = '';
            for(var i = 0; i < msgs.length; i++) {
                var m = msgs[i];
                var div = document.createElement('div');
                div.className = 'msg ' + (m.role === 'user' ? 'user-msg' : 'ai-msg');
                div.dataset.id = m.id;
                
                var avatar = m.role === 'user' ? '👤' : '🤖';
                var name = m.role === 'user' ? 'Kamu' : 'Apache AI';
                var formatted = m.text
                    .replace(/&/g, '&amp;')
                    .replace(/</g, '&lt;')
                    .replace(/>/g, '&gt;')
                    .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                    .replace(/\n/g, '<br>');
                
                div.innerHTML = 
                    '<div class="msg-avatar">' + avatar + '</div>' +
                    '<div class="msg-body">' +
                        '<div class="msg-name">' + name + ' <span style="color:#555; font-weight:normal; font-size:11px;">' + m.time + '</span></div>' +
                        '<div class="msg-bubble">' + formatted + '</div>' +
                        '<div class="msg-actions">' +
                            '<button class="msg-btn" onclick="copyText(' + m.id + ')">📋 Salin</button>' +
                            (m.role === 'ai' ? '<button class="msg-btn" onclick="retry(' + m.id + ')">🔄 Ulangi</button>' : '') +
                        '</div>' +
                    '</div>';
                
                messagesDiv.appendChild(div);
            }
            scrollDown();
        }

        function showToast(msg) {
            var t = document.createElement('div');
            t.className = 'toast';
            t.textContent = msg;
            document.body.appendChild(t);
            setTimeout(function() {
                t.style.opacity = '0';
                t.style.transition = '0.3s';
                setTimeout(function() { t.remove(); }, 300);
            }, 1800);
        }

        window.onload = function() {
            input.focus();
        };

        window.addEventListener('resize', function() {
            if(window.innerWidth >= 1024) {
                sidebar.classList.add('open');
                overlay.classList.remove('show');
            } else {
                sidebar.classList.remove('open');
            }
        });

        if(window.innerWidth >= 1024) {
            sidebar.classList.add('open');
        }

        var lastTouch = 0;
        document.addEventListener('touchend', function(e) {
            var now = Date.now();
            if(now - lastTouch <= 300) {
                e.preventDefault();
            }
            lastTouch = now;
        }, {passive: false});
    </script>
</body>
</html>
