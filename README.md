<!doctype html>
<html lang="fa" dir="rtl" class="h-full">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>جعبه ابزار حرفه‌ای - ماتریکس</title>
    <script src="/_sdk/element_sdk.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Courier New', 'Vazirmatn', monospace;
            transition: all 0.5s ease;
        }
        /* Matrix Rain Effect */
        
        #matrix-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            opacity: 0.15;
            pointer-events: none;
        }
        /* Glitch Animation */
        
        @keyframes glitch {
            0%,
            100% {
                transform: translate(0);
            }
            20% {
                transform: translate(-2px, 2px);
            }
            40% {
                transform: translate(-2px, -2px);
            }
            60% {
                transform: translate(2px, 2px);
            }
            80% {
                transform: translate(2px, -2px);
            }
        }
        
        .glitch {
            animation: glitch 0.3s infinite;
        }
        /* Neon Glow */
        
        .neon-text {
            text-shadow: 0 0 10px currentColor, 0 0 20px currentColor, 0 0 30px currentColor;
        }
        
        .neon-border {
            box-shadow: 0 0 10px currentColor, 0 0 20px currentColor, inset 0 0 10px currentColor;
        }
        /* Floating Particles */
        
        @keyframes float {
            0%,
            100% {
                transform: translateY(0px) rotate(0deg);
            }
            50% {
                transform: translateY(-20px) rotate(180deg);
            }
        }
        
        .floating-particle {
            position: absolute;
            opacity: 0.3;
            pointer-events: none;
            animation: float 6s ease-in-out infinite;
        }
        /* Scan Line Effect */
        
        @keyframes scan {
            0% {
                top: 0;
            }
            100% {
                top: 100%;
            }
        }
        
        .scan-line {
            position: fixed;
            width: 100%;
            height: 2px;
            background: linear-gradient(transparent, currentColor, transparent);
            animation: scan 8s linear infinite;
            pointer-events: none;
            z-index: 9999;
            opacity: 0.5;
        }
        /* Tool Card Hover Effects */
        
        .tool-card {
            transition: all 0.3s ease;
            cursor: pointer;
            backdrop-filter: blur(10px);
            position: relative;
            overflow: hidden;
        }
        
        .tool-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(transparent, rgba(255, 255, 255, 0.1), transparent);
            transform: rotate(45deg);
            transition: all 0.5s;
        }
        
        .tool-card:hover::before {
            left: 100%;
        }
        
        .tool-card:hover {
            transform: translateY(-8px) scale(1.05);
        }
        /* Modal Styles */
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 10000;
            overflow-y: auto;
            backdrop-filter: blur(10px);
        }
        
        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
            animation: fadeIn 0.3s ease;
        }
        
        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }
        
        .modal-content {
            background: rgba(0, 0, 0, 0.9);
            border-radius: 20px;
            padding: 30px;
            max-width: 600px;
            width: 90%;
            max-height: 90%;
            overflow-y: auto;
            position: relative;
            border: 2px solid;
        }
        /* Theme Selector */
        
        .theme-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        
        .theme-card {
            padding: 20px;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        
        .theme-card:hover {
            transform: scale(1.1);
            box-shadow: 0 0 30px currentColor;
        }
        
        .theme-card.active {
            box-shadow: 0 0 40px currentColor;
            transform: scale(1.05);
        }
        /* Calculator Buttons */
        
        .calc-btn {
            padding: 25px;
            font-size: 24px;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s;
            min-height: 70px;
            font-weight: bold;
        }
        
        .calc-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px currentColor;
        }
        
        .calc-btn:active {
            transform: scale(0.95);
        }
        /* Scrollbar */
        
         ::-webkit-scrollbar {
            width: 10px;
        }
        
         ::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.5);
        }
        
         ::-webkit-scrollbar-thumb {
            background: currentColor;
            border-radius: 5px;
        }
        /* Page Indicator */
        
        .page-indicator {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin: 20px 0;
        }
        
        .page-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .page-dot.active {
            background: currentColor;
            transform: scale(1.5);
            box-shadow: 0 0 15px currentColor;
        }
        /* Input Styles */
        
        input,
        select,
        textarea {
            border-radius: 10px;
            border: 2px solid;
            padding: 12px;
            font-size: 16px;
            width: 100%;
            margin: 10px 0;
            font-family: inherit;
            background: rgba(0, 0, 0, 0.5);
            color: inherit;
            transition: all 0.3s;
        }
        
        input:focus,
        select:focus,
        textarea:focus {
            outline: none;
            box-shadow: 0 0 15px currentColor;
        }
        
        button {
            border-radius: 10px;
            padding: 12px 24px;
            font-size: 16px;
            cursor: pointer;
            border: 2px solid;
            transition: all 0.3s;
            font-family: inherit;
            font-weight: bold;
        }
        
        button:hover {
            box-shadow: 0 0 20px currentColor;
            transform: translateY(-2px);
        }
        /* Pulse Animation */
        
        @keyframes pulse {
            0%,
            100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.05);
            }
        }
        
        .pulse {
            animation: pulse 2s ease-in-out infinite;
        }
        /* Theme Button Fixed Position */
        
        .theme-toggle-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            z-index: 9998;
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            border: 2px solid;
            transition: all 0.3s;
            backdrop-filter: blur(10px);
        }
        
        .theme-toggle-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 0 30px currentColor;
        }
    </style>
    <style>
        @view-transition {
            navigation: auto;
        }
    </style>
    <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
</head>

<body class="h-full">
    <canvas id="matrix-canvas"></canvas>
    <div class="scan-line"></div>
    <div id="app" class="h-full w-full overflow-auto" style="position: relative; z-index: 1;">
        <!-- Floating Particles -->
        <div class="floating-particle" style="top: 10%; left: 10%; font-size: 30px;">
            ⚡
        </div>
        <div class="floating-particle" style="top: 20%; right: 15%; font-size: 25px; animation-delay: 1s;">
            💻
        </div>
        <div class="floating-particle" style="bottom: 15%; left: 20%; font-size: 35px; animation-delay: 2s;">
            🔐
        </div>
        <div class="floating-particle" style="bottom: 20%; right: 10%; font-size: 28px; animation-delay: 3s;">
            ⚙️
        </div>
        <!-- Header -->
        <header class="p-6" style="background: rgba(0,0,0,0.7); backdrop-filter: blur(10px); border-bottom: 2px solid;">
            <div class="max-w-7xl mx-auto">
                <h1 id="siteTitle" class="text-4xl font-bold text-center mb-4 neon-text">🛠️ جعبه ابزار حرفه‌ای</h1>
                <div class="flex gap-4 justify-center flex-wrap"><button id="pageBtn" class="font-bold px-6 py-3 rounded-xl neon-border"> 📄 تغییر صفحه </button>
                </div>
                <div class="page-indicator">
                    <div class="page-dot active" data-page="1"></div>
                    <div class="page-dot" data-page="2"></div>
                </div>
            </div>
        </header>
        <!-- Page 1 -->
        <main id="page1" class="p-6 max-w-7xl mx-auto">
            <h2 id="page1Title" class="text-3xl font-bold text-center mb-8 neon-text">ابزارهای اصلی</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- Calculator -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('calculator')">
                    <div class="text-5xl text-center mb-3">
                        🔢
                    </div>
                    <h3 class="text-xl font-bold text-center">ماشین حساب</h3>
                </div>
                <!-- Unit Converter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('unitConverter')">
                    <div class="text-5xl text-center mb-3">
                        📏
                    </div>
                    <h3 class="text-xl font-bold text-center">تبدیل واحد</h3>
                </div>
                <!-- Timer -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('timer')">
                    <div class="text-5xl text-center mb-3">
                        ⏰
                    </div>
                    <h3 class="text-xl font-bold text-center">تایمر</h3>
                </div>
                <!-- Note -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('note')">
                    <div class="text-5xl text-center mb-3">
                        📝
                    </div>
                    <h3 class="text-xl font-bold text-center">یادداشت</h3>
                </div>
                <!-- Percentage -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('percentage')">
                    <div class="text-5xl text-center mb-3">
                        📊
                    </div>
                    <h3 class="text-xl font-bold text-center">محاسبه درصد</h3>
                </div>
                <!-- Loan Calculator -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('loan')">
                    <div class="text-5xl text-center mb-3">
                        💰
                    </div>
                    <h3 class="text-xl font-bold text-center">محاسبه وام</h3>
                </div>
                <!-- Tip Calculator -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('tip')">
                    <div class="text-5xl text-center mb-3">
                        🧾
                    </div>
                    <h3 class="text-xl font-bold text-center">محاسبه ��نعام</h3>
                </div>
                <!-- Currency Converter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('currency')">
                    <div class="text-5xl text-center mb-3">
                        💱
                    </div>
                    <h3 class="text-xl font-bold text-center">تبدیل ارز</h3>
                </div>
                <!-- Base64 -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('base64')">
                    <div class="text-5xl text-center mb-3">
                        🔐
                    </div>
                    <h3 class="text-xl font-bold text-center">رمزن��اری</h3>
                </div>
                <!-- Stopwatch -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('stopwatch')">
                    <div class="text-5xl text-center mb-3">
                        ⏱️
                    </div>
                    <h3 class="text-xl font-bold text-center">کرنومتر</h3>
                </div>
                <!-- World Clock -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('worldClock')">
                    <div class="text-5xl text-center mb-3">
                        🌍
                    </div>
                    <h3 class="text-xl font-bold text-center">ساعت جهانی</h3>
                </div>
                <!-- BMI -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('bmi')">
                    <div class="text-5xl text-center mb-3">
                        ⚖️
                    </div>
                    <h3 class="text-xl font-bold text-center">محاسبه BMI</h3>
                </div>
                <!-- Age Calculator -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('age')">
                    <div class="text-5xl text-center mb-3">
                        🎂
                    </div>
                    <h3 class="text-xl font-bold text-center">محاسبه سن</h3>
                </div>
                <!-- Word Counter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('wordCount')">
                    <div class="text-5xl text-center mb-3">
                        📄
                    </div>
                    <h3 class="text-xl font-bold text-center">شمارش کلمات</h3>
                </div>
                <!-- Password Generator -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('password')">
                    <div class="text-5xl text-center mb-3">
                        🔑
                    </div>
                    <h3 class="text-xl font-bold text-center">تولید رمز</h3>
                </div>
                <!-- QR Code -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('qrcode')">
                    <div class="text-5xl text-center mb-3">
                        📱
                    </div>
                    <h3 class="text-xl font-bold text-center">ساخت QR</h3>
                </div>
                <!-- Random Number -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('random')">
                    <div class="text-5xl text-center mb-3">
                        🎲
                    </div>
                    <h3 class="text-xl font-bold text-center">عدد تصادفی</h3>
                </div>
                <!-- Dice -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('dice')">
                    <div class="text-5xl text-center mb-3">
                        🎯
                    </div>
                    <h3 class="text-xl font-bold text-center">تاس</h3>
                </div>
                <!-- Color Picker -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('colorPicker')">
                    <div class="text-5xl text-center mb-3">
                        🎨
                    </div>
                    <h3 class="text-xl font-bold text-center">انتخاب رنگ</h3>
                </div>
                <!-- Counter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('counter')">
                    <div class="text-5xl text-center mb-3">
                        🔢
                    </div>
                    <h3 class="text-xl font-bold text-center">شمارشگر</h3>
                </div>
            </div>
        </main>
        <!-- Page 2 -->
        <main id="page2" class="p-6 max-w-7xl mx-auto" style="display: none;">
            <h2 id="page2Title" class="text-3xl font-bold text-center mb-8 neon-text">ابزارهای پیشرفته</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- Morse Code -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('morse')">
                    <div class="text-5xl text-center mb-3">
                        📡
                    </div>
                    <h3 class="text-xl font-bold text-center">مورس</h3>
                </div>
                <!-- Text Hash -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('hash')">
                    <div class="text-5xl text-center mb-3">
                        🔒
                    </div>
                    <h3 class="text-xl font-bold text-center">هش متن</h3>
                </div>
                <!-- Binary Converter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('binary')">
                    <div class="text-5xl text-center mb-3">
                        💻
                    </div>
                    <h3 class="text-xl font-bold text-center">تبدیل باینری</h3>
                </div>
                <!-- Hex Converter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('hex')">
                    <div class="text-5xl text-center mb-3">
                        🔢
                    </div>
                    <h3 class="text-xl font-bold text-center">تبدیل مبنا</h3>
                </div>
                <!-- RGB to HEX -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('rgbHex')">
                    <div class="text-5xl text-center mb-3">
                        🌈
                    </div>
                    <h3 class="text-xl font-bold text-center">RGB به HEX</h3>
                </div>
                <!-- Temperature -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('temperature')">
                    <div class="text-5xl text-center mb-3">
                        ���️
                    </div>
                    <h3 class="text-xl font-bold text-center">دماسنج</h3>
                </div>
                <!-- Speed Test -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('speedTest')">
                    <div class="text-5xl text-center mb-3">
                        ��
                    </div>
                    <h3 class="text-xl font-bold text-center">تست سرعت</h3>
                </div>
                <!-- JSON Formatter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('json')">
                    <div class="text-5xl text-center mb-3">
                        📋
                    </div>
                    <h3 class="text-xl font-bold text-center">فرمت JSON</h3>
                </div>
                <!-- Markdown -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('markdown')">
                    <div class="text-5xl text-center mb-3">
                        ✍️
                    </div>
                    <h3 class="text-xl font-bold text-center">Markdown</h3>
                </div>
                <!-- Gradient -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('gradient')">
                    <div class="text-5xl text-center mb-3">
                        🎨
                    </div>
                    <h3 class="text-xl font-bold text-center">گرادیانت</h3>
                </div>
                <!-- Box Shadow -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('boxShadow')">
                    <div class="text-5xl text-center mb-3">
                        📦
                    </div>
                    <h3 class="text-xl font-bold text-center">سا��ه CSS</h3>
                </div>
                <!-- Text Case -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('textCase')">
                    <div class="text-5xl text-center mb-3">
                        🔤
                    </div>
                    <h3 class="text-xl font-bold text-center">تبدیل حرو��</h3>
                </div>
                <!-- URL Encoder -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('urlEncode')">
                    <div class="text-5xl text-center mb-3">
                        🔗
                    </div>
                    <h3 class="text-xl font-bold text-center">رمزنگاری URL</h3>
                </div>
                <!-- Lorem Ipsum -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('lorem')">
                    <div class="text-5xl text-center mb-3">
                        📝
                    </div>
                    <h3 class="text-xl font-bold text-center">متن نمونه</h3>
                </div>
                <!-- IP Address -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('ip')">
                    <div class="text-5xl text-center mb-3">
                        🌐
                    </div>
                    <h3 class="text-xl font-bold text-center">آی‌پی من</h3>
                </div>
                <!-- Image to Base64 -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('imageBase64')">
                    <div class="text-5xl text-center mb-3">
                        🖼️
                    </div>
                    <h3 class="text-xl font-bold text-center">تصویر به Base64</h3>
                </div>
                <!-- Text Diff -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('textDiff')">
                    <div class="text-5xl text-center mb-3">
                        📊
                    </div>
                    <h3 class="text-xl font-bold text-center">مقایسه متن</h3>
                </div>
                <!-- UUID Generator -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('uuid')">
                    <div class="text-5xl text-center mb-3">
                        🆔
                    </div>
                    <h3 class="text-xl font-bold text-center">تولید UUID</h3>
                </div>
                <!-- Epoch Converter -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('epoch')">
                    <div class="text-5xl text-center mb-3">
                        🕐
                    </div>
                    <h3 class="text-xl font-bold text-center">تبدیل Epoch</h3>
                </div>
                <!-- Data Size -->
                <div class="tool-card p-6 rounded-xl neon-border" onclick="openTool('dataSize')">
                    <div class="text-5xl text-center mb-3">
                        💾
                    </div>
                    <h3 class="text-xl font-bold text-center">تبدیل داده</h3>
                </div>
            </div>
        </main>
        <!-- Footer -->
        <footer class="mt-12 p-8 text-center" style="background: rgba(0,0,0,0.7); backdrop-filter: blur(10px); border-top: 2px solid;">
            <div class="max-w-4xl mx-auto">
                <a href="https://instagram.com/amirali_bafti_1390" target="_blank" rel="noopener noreferrer" class="inline-block mb-4">
                    <svg class="pulse" width="60" height="60" viewbox="0 0 24 24" fill="none"><lineargradient id="instagramGradient" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" style="stop-color:#833ab4" />
        <stop offset="50%" style="stop-color:#fd1d1d" />
        <stop offset="100%" style="stop-color:#fcb045" />
       </lineargradient> <rect x="2" y="2" width="20" height="20" rx="5" stroke="url(#instagramGradient)" stroke-width="2" fill="none" /> <circle cx="12" cy="12" r="4" stroke="url(#instagramGradient)" stroke-width="2" fill="none" /> <circle cx="18" cy="6" r="1.5" fill="url(#instagramGradient)" />
      </svg></a>
                <h3 id="creatorName" class="text-2xl font-bold mb-2 neon-text">AmirAli Bafti</h3>
                <p class="text-xl mb-1" style="direction: ltr;">امیرعلی بافتی</p>
                <p id="instagramId" class="text-lg mb-2 opacity-80">@amirali_bafti_1390</p>
                <p id="phoneNumber" class="text-lg opacity-80" style="direction: ltr;">📞 09926274950</p>
            </div>
        </footer>
        <!-- Tool Modal -->
        <div id="toolModal" class="modal">
            <div class="modal-content neon-border" id="modalContent"><button onclick="closeModal()" style="position: absolute; top: 15px; left: 15px; width: 40px; height: 40px; border-radius: 50%; font-size: 20px;">✕</button>
                <div id="toolContent"></div>
            </div>
        </div>
        <!-- Secret Page Modal -->
        <div id="secretModal" class="modal">
            <div class="modal-content neon-border" style="max-width: 500px; text-align: center;"><button onclick="closeSecretPage()" style="position: absolute; top: 15px; left: 15px; width: 40px; height: 40px; border-radius: 50%; font-size: 20px;">✕</button>
                <h2 class="text-3xl font-bold mb-6 neon-text">🌟 پیام مخفی 🌟</h2>
                <div style="max-height: 400px; overflow-y: auto; padding: 20px; background: rgba(0,0,0,0.5); border-radius: 10px; line-height: 2;" class="neon-border">
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافت��</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-4">امیر علی بافتی</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold mb-2" style="direction: ltr;">AmirAli Bafti</p>
                    <p class="text-2xl font-bold" style="direction: ltr;">AmirAli Bafti</p>
                </div>
            </div>
        </div>
        <!-- Theme Selector Modal -->
        <div id="themeModal" class="modal">
            <div class="modal-content neon-border" style="max-width: 900px;"><button onclick="closeThemeModal()" style="position: absolute; top: 15px; left: 15px; width: 40px; height: 40px; border-radius: 50%; font-size: 20px;">✕</button>
                <h2 class="text-3xl font-bold mb-6 text-center neon-text">🎨 انتخاب ��م هکینگ</h2>
                <div class="theme-grid" id="themeGrid"></div>
            </div>
        </div>
    </div>
    <!-- Theme Toggle Button --><button class="theme-toggle-btn neon-border" onclick="openThemeModal()"> 🎨 تغییر تم </button>
    <!-- Secret Button --><button onclick="openSecretPage()" style="position: fixed; bottom: 30px; left: 30px; z-index: 9998; width: 15px; height: 15px; border-radius: 50%; font-size: 8px; cursor: pointer; border: 2px solid; transition: all 0.3s; backdrop-filter: blur(10px); opacity: 0.7;"
        class="neon-border" onmouseover="this.style.opacity='1'; this.style.transform='scale(1.15)'" onmouseout="this.style.opacity='0.7'; this.style.transform='scale(1)'"> ���� </button>
    <script>
        // Matrix Themes - 30 Professional Hacking Themes
        const themes = [{
            name: 'ماتریکس کلاسیک',
            bg: '#000000',
            surface: '#001a00',
            text: '#00ff00',
            primary: '#00cc00',
            secondary: '#00ff41'
        }, {
            name: 'سایبر آبی',
            bg: '#000a1a',
            surface: '#001a33',
            text: '#00ccff',
            primary: '#0099cc',
            secondary: '#33d4ff'
        }, {
            name: 'هکر قرمز',
            bg: '#1a0000',
            surface: '#330000',
            text: '#ff0000',
            primary: '#cc0000',
            secondary: '#ff3333'
        }, {
            name: 'ار��وانی تاریک',
            bg: '#0d001a',
            surface: '#1a0033',
            text: '#cc00ff',
            primary: '#9900cc',
            secondary: '#d633ff'
        }, {
            name: 'نئون صورتی',
            bg: '#1a0013',
            surface: '#330026',
            text: '#ff00aa',
            primary: '#cc0088',
            secondary: '#ff33bb'
        }, {
            name: 'فیروزه‌ای',
            bg: '#001a1a',
            surface: '#003333',
            text: '#00ffff',
            primary: '#00cccc',
            secondary: '#33ffff'
        }, {
            name: 'نارنجی آتشین',
            bg: '#1a0d00',
            surface: '#331a00',
            text: '#ff8800',
            primary: '#cc6600',
            secondary: '#ffaa33'
        }, {
            name: 'زرد برق',
            bg: '#1a1a00',
            surface: '#333300',
            text: '#ffff00',
            primary: '#cccc00',
            secondary: '#ffff33'
        }, {
            name: 'یاسی شبح',
            bg: '#13001a',
            surface: '#260033',
            text: '#bb00ff',
            primary: '#8800cc',
            secondary: '#cc33ff'
        }, {
            name: 'آبی عمیق',
            bg: '#00001a',
            surface: '#000033',
            text: '#3366ff',
            primary: '#0044cc',
            secondary: '#5588ff'
        }, {
            name: 'سبز لایم',
            bg: '#0d1a00',
            surface: '#1a3300',
            text: '#88ff00',
            primary: '#66cc00',
            secondary: '#aaff33'
        }, {
            name: 'قرمز خونین',
            bg: '#1a0008',
            surface: '#330010',
            text: '#ff0044',
            primary: '#cc0033',
            secondary: '#ff3366'
        }, {
            name: 'آبی یخی',
            bg: '#001a1a',
            surface: '#00334d',
            text: '#00ddff',
            primary: '#00aacc',
            secondary: '#33eeff'
        }, {
            name: 'بنفش شاهی',
            bg: '#0f001a',
            surface: '#1f0033',
            text: '#9933ff',
            primary: '#6600cc',
            secondary: '#bb66ff'
        }, {
            name: 'سبز فسفری',
            bg: '#001a0d',
            surface: '#00331a',
            text: '#00ff88',
            primary: '#00cc66',
            secondary: '#33ffaa'
        }, {
            name: 'صورتی هات',
            bg: '#1a0010',
            surface: '#330020',
            text: '#ff0099',
            primary: '#cc0077',
            secondary: '#ff33aa'
        }, {
            name: 'آبی کبالت',
            bg: '#00081a',
            surface: '#001033',
            text: '#0066ff',
            primary: '#0044cc',
            secondary: '#3388ff'
        }, {
            name: 'زرد لیمویی',
            bg: '#1a1a08',
            surface: '#333310',
            text: '#ddff00',
            primary: '#aacc00',
            secondary: '#eeff33'
        }, {
            name: 'قرمز نئون',
            bg: '#1a0003',
            surface: '#330006',
            text: '#ff003c',
            primary: '#cc002e',
            secondary: '#ff3366'
        }, {
            name: 'سبز را��یواکتیو',
            bg: '#0a1a00',
            surface: '#143300',
            text: '#39ff14',
            primary: '#2ecc11',
            secondary: '#5aff3d'
        }, {
            name: 'آبی الکتریک',
            bg: '#000f1a',
            surface: '#001f33',
            text: '#00aaff',
            primary: '#0088cc',
            secondary: '#33bbff'
        }, {
            name: '��رغوانی گالاکتیک',
            bg: '#10001a',
            surface: '#200033',
            text: '#aa00ff',
            primary: '#8800cc',
            secondary: '#cc33ff'
        }, {
            name: 'نارنجی مسی',
            bg: '#1a0a00',
            surface: '#331400',
            text: '#ff6600',
            primary: '#cc5200',
            secondary: '#ff8833'
        }, {
            name: 'فیروزه‌ای تاریک',
            bg: '#001315',
            surface: '#00262a',
            text: '#00e5ff',
            primary: '#00b8cc',
            secondary: '#33eeff'
        }, {
            name: 'صورتی فلورسنت',
            bg: '#1a0015',
            surface: '#33002a',
            text: '#ff00dd',
            primary: '#cc00aa',
            secondary: '#ff33ee'
        }, {
            name: 'سبز مالاکیت',
            bg: '#081a0a',
            surface: '#103314',
            text: '#00ff66',
            primary: '#00cc52',
            secondary: '#33ff88'
        }, {
            name: 'قرمز مات',
            bg: '#1a0505',
            surface: '#330a0a',
            text: '#ff2222',
            primary: '#cc1111',
            secondary: '#ff5555'
        }, {
            name: 'آبی سیگنال',
            bg: '#00111a',
            surface: '#002233',
            text: '#0099ff',
            primary: '#0077cc',
            secondary: '#33aaff'
        }, {
            name: 'بنفش شفقی',
            bg: '#0a001a',
            surface: '#140033',
            text: '#8800ff',
            primary: '#6600cc',
            secondary: '#aa33ff'
        }, {
            name: 'سبز تاکسیک',
            bg: '#0f1a00',
            surface: '#1e3300',
            text: '#99ff00',
            primary: '#77cc00',
            secondary: '#bbff33'
        }];

        let currentThemeIndex = 1; // Start with Cyber Blue
        let currentPage = 1;
        let stopwatchInterval = null;
        let stopwatchTime = 0;
        let timerInterval = null;
        let counterValue = 0;

        const defaultConfig = {
            background_color: themes[1].bg,
            surface_color: themes[1].surface,
            text_color: themes[1].text,
            primary_action: themes[1].primary,
            secondary_action: themes[1].secondary,
            font_family: 'Courier New',
            font_size: 16,
            site_title: 'جعبه ابزار حرفه‌ای',
            page1_title: 'ابزارهای اصلی',
            page2_title: 'ابزارهای پیشرفته',
            creator_name: 'AmirAli Bafti',
            instagram_id: '@amirali_bafti_1390',
            phone_number: '09926274950'
        };

        // Matrix Rain Animation
        function initMatrix() {
            const canvas = document.getElementById('matrix-canvas');
            const ctx = canvas.getContext('2d');

            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;

            const chars = '01アイウエオカキクケコサシスセソタチツテトナニヌネノハヒフヘホ���ミムメモヤユヨラリルレロワヲン';
            const fontSize = 16;
            const columns = canvas.width / fontSize;
            const drops = Array(Math.floor(columns)).fill(1);

            function draw() {
                ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
                ctx.fillRect(0, 0, canvas.width, canvas.height);

                ctx.fillStyle = currentTheme.text;
                ctx.font = fontSize + 'px monospace';

                for (let i = 0; i < drops.length; i++) {
                    const text = chars[Math.floor(Math.random() * chars.length)];
                    ctx.fillText(text, i * fontSize, drops[i] * fontSize);

                    if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
                        drops[i] = 0;
                    }
                    drops[i]++;
                }
            }

            setInterval(draw, 33);

            window.addEventListener('resize', () => {
                canvas.width = window.innerWidth;
                canvas.height = window.innerHeight;
            });
        }

        // Theme Functions
        function applyTheme(themeIndex) {
            currentThemeIndex = themeIndex;
            const theme = themes[themeIndex];

            document.body.style.background = theme.bg;
            document.body.style.color = theme.text;

            document.querySelectorAll('header, footer').forEach(el => {
                el.style.borderColor = theme.text;
                el.style.color = theme.text;
            });

            document.querySelectorAll('.tool-card, .neon-border, .theme-card').forEach(el => {
                el.style.borderColor = theme.text;
                el.style.color = theme.text;
            });

            document.querySelectorAll('.neon-text').forEach(el => {
                el.style.color = theme.text;
            });

            document.querySelector('.scan-line').style.color = theme.text;

            document.querySelectorAll('.floating-particle').forEach(el => {
                el.style.color = theme.text;
            });

            document.getElementById('pageBtn').style.background = theme.primary;
            document.getElementById('pageBtn').style.color = '#fff';
            document.getElementById('pageBtn').style.borderColor = theme.primary;

            document.querySelector('.theme-toggle-btn').style.background = theme.primary;
            document.querySelector('.theme-toggle-btn').style.color = '#fff';
            document.querySelector('.theme-toggle-btn').style.borderColor = theme.primary;

            document.querySelectorAll('.page-dot.active').forEach(dot => {
                dot.style.background = theme.text;
                dot.style.boxShadow = `0 0 15px ${theme.text}`;
            });

            document.querySelectorAll('input, select, textarea').forEach(el => {
                el.style.borderColor = theme.text;
                el.style.color = theme.text;
            });

            if (document.getElementById('themeModal').classList.contains('active')) {
                renderThemeGrid();
            }

            localStorage.setItem('selectedTheme', themeIndex);
        }

        function openThemeModal() {
            document.getElementById('themeModal').classList.add('active');
            renderThemeGrid();
        }

        function closeThemeModal() {
            document.getElementById('themeModal').classList.remove('active');
        }

        function renderThemeGrid() {
            const grid = document.getElementById('themeGrid');
            grid.innerHTML = themes.map((theme, index) => `
                <div class="theme-card ${index === currentThemeIndex ? 'active' : ''}" 
                     onclick="selectTheme(${index})"
                     style="background: ${theme.bg}; color: ${theme.text}; border-color: ${theme.text};">
                    <div style="font-size: 24px; margin-bottom: 10px;">⚡</div>
                    <div style="font-size: 14px; font-weight: bold;">${theme.name}</div>
                </div>
            `).join('');
        }

        function selectTheme(index) {
            applyTheme(index);
            closeThemeModal();
        }

        // Secret Page Functions
        function openSecretPage() {
            document.getElementById('secretModal').classList.add('active');
        }

        function closeSecretPage() {
            document.getElementById('secretModal').classList.remove('active');
        }

        // Get current theme
        const currentTheme = themes[currentThemeIndex];

        // Page Navigation
        document.getElementById('pageBtn').addEventListener('click', () => {
            currentPage = currentPage === 1 ? 2 : 1;
            document.getElementById('page1').style.display = currentPage === 1 ? 'block' : 'none';
            document.getElementById('page2').style.display = currentPage === 2 ? 'block' : 'none';
            document.querySelectorAll('.page-dot').forEach((dot, index) => {
                dot.classList.toggle('active', index + 1 === currentPage);
            });
        });

        document.querySelectorAll('.page-dot').forEach(dot => {
            dot.addEventListener('click', (e) => {
                const page = parseInt(e.target.dataset.page);
                currentPage = page;
                document.getElementById('page1').style.display = page === 1 ? 'block' : 'none';
                document.getElementById('page2').style.display = page === 2 ? 'block' : 'none';
                document.querySelectorAll('.page-dot').forEach((d, i) => {
                    d.classList.toggle('active', i + 1 === page);
                });
            });
        });

        // Tool Functions
        function openTool(toolName) {
            const modal = document.getElementById('toolModal');
            const content = document.getElementById('toolContent');

            const tools = {
                calculator: `
                    <h2 class="text-4xl font-bold mb-8 text-center neon-text">🔢 ماشین حساب هکری</h2>
                    <div style="max-width: 500px; margin: 0 auto; background: rgba(0,0,0,0.7); padding: 30px; border-radius: 20px;" class="neon-border">
                        <input type="text" id="calcDisplay" readonly style="font-size: 48px; text-align: left; margin-bottom: 20px; background: rgba(0,0,0,0.9); text-shadow: 0 0 10px currentColor; font-family: 'Courier New', monospace; padding: 20px; height: 100px; font-weight: bold;" value="0" class="neon-border">
                        <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 15px;">
                            <button class="calc-btn neon-border" onclick="clearCalc()">C</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('/')">÷</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('*')">����</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('-')">−</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('7')">7</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('8')">8</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('9')">9</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('+')">+</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('4')">4</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('5')">5</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('6')">6</button>
                            <button class="calc-btn neon-border" onclick="calculateCalc()" style="grid-row: span 2;">=</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('1')">1</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('2')">2</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('3')">3</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('0')" style="grid-column: span 2;">0</button>
                            <button class="calc-btn neon-border" onclick="appendCalc('.')">.</button>
                        </div>
                    </div>
                `,
                unitConverter: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">��� تبدیل واحد</h2>
                    <select id="unitType" onchange="updateUnitOptions()">
                        <option value="length">طول</option>
                        <option value="weight">وزن</option>
                        <option value="temperature">دما</option>
                    </select>
                    <input type="number" id="unitInput" placeholder="مقدار">
                    <select id="unitFrom"></select>
                    <select id="unitTo"></select>
                    <button onclick="convertUnit()">تبدیل</button>
                    <div id="unitResult" class="mt-4 text-xl font-bold text-center neon-text"></div>
                `,
                timer: `
                    <h2 class="text-2xl font-bold mb-4 text-center neon-text">⏰ تایمر ��کری</h2>
                    <div style="text-align: center;">
                        <input type="number" id="timerMinutes" placeholder="دقیقه" min="0" style="width: 100px;">
                        <input type="number" id="timerSeconds" placeholder="ثانیه" min="0" max="59" style="width: 100px;">
                        <div id="timerDisplay" class="text-5xl font-bold my-6 neon-text">00:00</div>
                        <button onclick="startTimer()">شروع</button>
                        <button onclick="pauseTimer()">توقف</button>
                        <button onclick="resetTimer()">ریست</button>
                    </div>
                `,
                note: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📝 یادداشت رمزی</h2>
                    <textarea id="noteText" rows="10" placeholder="یادداشت خود را بنویسید..."></textarea>
                    <button onclick="saveNote()">ذخیره محلی</button>
                    <button onclick="loadNote()">بارگذاری</button>
                `,
                percentage: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📊 محاسبه درصد</h2>
                    <input type="number" id="percentNum" placeholder="عدد">
                    <input type="number" id="percentOf" placeholder="از">
                    <button onclick="calculatePercent()">محاسبه</button>
                    <div id="percentResult" class="mt-4 text-xl font-bold text-center neon-text"></div>
                `,
                loan: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">💰 محاسبه وام</h2>
                    <input type="number" id="loanAmount" placeholder="مبلغ وام">
                    <input type="number" id="loanRate" placeholder="نرخ سود (%)">
                    <input type="number" id="loanMonths" placeholder="مدت (ماه)">
                    <button onclick="calculateLoan()">محاسبه</button>
                    <div id="loanResult" class="mt-4 text-lg"></div>
                `,
                tip: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🧾 محاسبه انعام</h2>
                    <input type="number" id="billAmount" placeholder="مبلغ صورتحساب">
                    <input type="number" id="tipPercent" placeholder="درصد انعام" value="15">
                    <input type="number" id="people" placeholder="تعد��د نفرات" value="1">
                    <button onclick="calculateTip()">محاسبه</button>
                    <div id="tipResult" class="mt-4 text-lg"></div>
                `,
                currency: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">💱 تبدیل ارز</h2>
                    <input type="number" id="currencyAmount" placeholder="مقدار">
                    <select id="currencyFrom">
                        <option value="USD">د��ار</option>
                        <option value="EUR">یورو</option>
                        <option value="GBP">پوند</option>
                        <option value="IRR">ریال</option>
                    </select>
                    <select id="currencyTo">
                        <option value="IRR">ریال</option>
                        <option value="USD">دلار</option>
                        <option value="EUR">یورو</option>
                        <option value="GBP">پوند</option>
                    </select>
                    <button onclick="convertCurrency()">تبدیل</button>
                    <div id="currencyResult" class="mt-4 text-xl font-bold text-center neon-text"></div>
                `,
                base64: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🔐 رمزنگاری Base64</h2>
                    <textarea id="base64Input" rows="5" placeholder="متن خود را وارد کنید"></textarea>
                    <button onclick="encodeBase64()">رمزنگاری</button>
                    <button onclick="decodeBase64()">رمزگشایی</button>
                    <textarea id="base64Output" rows="5" readonly style="margin-top: 10px;"></textarea>
                `,
                stopwatch: `
                    <h2 class="text-2xl font-bold mb-4 text-center neon-text">⏱️ کرنومتر</h2>
                    <div style="text-align: center;">
                        <div id="stopwatchDisplay" class="text-5xl font-bold my-6 neon-text">00:00:00</div>
                        <button onclick="startStopwatch()">شروع</button>
                        <button onclick="pauseStopwatch()">توقف</button>
                        <button onclick="resetStopwatch()">ریست</button>
                    </div>
                `,
                worldClock: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🌍 ساع�� جهانی</h2>
                    <div id="worldClockList" class="space-y-4"></div>
                `,
                bmi: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">⚖️ محاسبه BMI</h2>
                    <input type="number" id="bmiWeight" placeholder="وزن (کی��وگرم)">
                    <input type="number" id="bmiHeight" placeholder="قد (سانتیمتر)">
                    <button onclick="calculateBMI()">محاسبه</button>
                    <div id="bmiResult" class="mt-4 text-lg"></div>
                `,
                age: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🎂 محاس��ه سن</h2>
                    <input type="date" id="birthDate">
                    <button onclick="calculateAge()">محاسبه</button>
                    <div id="ageResult" class="mt-4 text-lg"></div>
                `,
                wordCount: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📄 شمارش کلمات</h2>
                    <textarea id="wordCountText" rows="8" placeholder="متن خود را وارد کنید" oninput="countWords()"></textarea>
                    <div id="wordCountResult" class="mt-4 text-lg"></div>
                `,
                password: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🔑 تولید رمز عبور</h2>
                    <input type="number" id="passwordLength" placeholder="طول رمز" value="12" min="4" max="50">
                    <label><input type="checkbox" id="includeNumbers" checked> اعداد</label>
                    <label><input type="checkbox" id="includeSymbols" checked> نمادها</label>
                    <button onclick="generatePassword()">تولید</button>
                    <input type="text" id="generatedPassword" readonly style="margin-top: 10px; font-family: monospace;">
                `,
                qrcode: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📱 ساخت کد QR</h2>
                    <input type="text" id="qrText" placeholder="متن یا لی��ک">
                    <button onclick="generateQR()">ساخت</button>
                    <div id="qrcode" class="mt-4"></div>
                `,
                random: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🎲 عدد تصادفی</h2>
                    <input type="number" id="randomMin" placeholder="حداقل" value="1">
                    <input type="number" id="randomMax" placeholder="حداکثر" value="100">
                    <button onclick="generateRandom()">تولید</button>
                    <div id="randomResult" class="mt-4 text-5xl font-bold text-center neon-text"></div>
                `,
                dice: `
                    <h2 class="text-2xl font-bold mb-4 text-center neon-text">🎯 تاس دیجیتال</h2>
                    <div style="text-align: center;">
                        <div id="diceResult" class="text-9xl my-6">🎲</div>
                        <button onclick="rollDice()" style="padding: 15px 40px; font-size: 20px;">پرت��ب تاس</button>
                    </div>
                `,
                colorPicker: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🎨 انتخاب رنگ</h2>
                    <input type="color" id="colorInput" style="width: 100%; height: 100px; cursor: pointer;" oninput="updateColor()">
                    <div id="colorInfo" class="mt-4 text-lg"></div>
                `,
                counter: `
                    <h2 class="text-2xl font-bold mb-4 text-center neon-text">🔢 شمارشگر</h2>
                    <div style="text-align: center;">
                        <div id="counterValue" class="text-6xl font-bold my-6 neon-text">0</div>
                        <button onclick="incrementCounter()" style="padding: 15px 30px;">+</button>
                        <button onclick="decrementCounter()" style="padding: 15px 30px;">-</button>
                        <button onclick="resetCounter()" style="padding: 15px 30px;">ریست</button>
                    </div>
                `,
                morse: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📡 تبدیل مورس</h2>
                    <textarea id="morseInput" rows="5" placeholder="متن انگلی��ی"></textarea>
                    <button onclick="textToMorse()">به مورس</button>
                    <button onclick="morseToText()">از مورس</button>
                    <textarea id="morseOutput" rows="5" readonly style="margin-top: 10px;"></textarea>
                `,
                hash: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🔒 هش متن</h2>
                    <textarea id="hashInput" rows="5" placeholder="متن خود را وارد کنید"></textarea>
                    <button onclick="hashText()">تولید هش</button>
                    <input type="text" id="hashOutput" readonly style="margin-top: 10px; font-family: monospace;">
                `,
                binary: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">💻 تبدیل باینری</h2>
                    <textarea id="binaryInput" rows="5" placeholder="متن یا باینری"></textarea>
                    <button onclick="textToBinary()">به باینری</button>
                    <button onclick="binaryToText()">از باینری</button>
                    <textarea id="binaryOutput" rows="5" readonly style="margin-top: 10px;"></textarea>
                `,
                hex: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🔢 تبدیل مبنا</h2>
                    <input type="number" id="hexInput" placeholder="عدد">
                    <button onclick="convertToHex()">تبدیل</button>
                    <div id="hexOutput" class="mt-4 text-lg"></div>
                `,
                rgbHex: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🌈 تبدیل RGB به HEX</h2>
                    <input type="number" id="rgbR" placeholder="R (0-255)" min="0" max="255">
                    <input type="number" id="rgbG" placeholder="G (0-255)" min="0" max="255">
                    <input type="number" id="rgbB" placeholder="B (0-255)" min="0" max="255">
                    <button onclick="rgbToHex()">تبدیل</button>
                    <div id="rgbHexResult" class="mt-4 text-lg"></div>
                `,
                temperature: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🌡️ تبدیل دما</h2>
                    <input type="number" id="tempInput" placeholder="دما">
                    <select id="tempFrom">
                        <option value="C">سلسیوس</option>
                        <option value="F">فارنهایت</option>
                        <option value="K">کلوین</option>
                    </select>
                    <select id="tempTo">
                        <option value="F">فارنهایت</option>
                        <option value="C">سلسیوس</option>
                        <option value="K">کلوین</option>
                    </select>
                    <button onclick="convertTemperature()">تبدیل</button>
                    <div id="tempResult" class="mt-4 text-xl font-bold text-center neon-text"></div>
                `,
                speedTest: `
                    <h2 class="text-2xl font-bold mb-4 text-center neon-text">⚡ تست سرعت کلیک</h2>
                    <div style="text-align: center;">
                        <div id="clickCount" class="text-5xl font-bold my-6 neon-text">0</div>
                        <p class="mb-4">10 ثانیه زما�� داری!</p>
                        <button id="speedTestBtn" onclick="startSpeedTest()" style="padding: 20px 40px; font-size: 20px;">شروع تست</button>
                    </div>
                `,
                json: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📋 فرمت کننده JSON</h2>
                    <textarea id="jsonInput" rows="8" placeholder='{"key": "value"}'></textarea>
                    <button onclick="formatJSON()">فرمت کن</button>
                    <textarea id="jsonOutput" rows="8" readonly style="margin-top: 10px;"></textarea>
                `,
                markdown: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">✍️ ویرایشگر Markdown</h2>
                    <textarea id="markdownInput" rows="8" placeholder="# سرتیتر خود را بنویسید" oninput="renderMarkdown()"></textarea>
                    <div id="markdownOutput" style="border: 2px solid; padding: 20px; border-radius: 10px; min-height: 100px; background: rgba(0,0,0,0.5); margin-top: 10px;"></div>
                `,
                gradient: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🎨 ساخت گرادیانت</h2>
                    <input type="color" id="gradColor1" value="#3b82f6">
                    <input type="color" id="gradColor2" value="#8b5cf6">
                    <select id="gradDirection">
                        <option value="to right">افقی</option>
                        <option value="to bottom">عمودی</option>
                        <option value="to bottom right">مورب</option>
                    </select>
                    <button onclick="generateGradient()">ساخت</button>
                    <div id="gradientPreview" style="height: 150px; border-radius: 10px; margin-top: 10px; border: 2px solid;"></div>
                    <input type="text" id="gradientCode" readonly style="margin-top: 10px; font-family: monospace;">
                `,
                boxShadow: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📦 ساخت سایه CSS</h2>
                    <input type="range" id="shadowX" min="-50" max="50" value="10" oninput="updateShadow()">
                    <label>X: <span id="shadowXVal">10</span></label>
                    <input type="range" id="shadowY" min="-50" max="50" value="10" oninput="updateShadow()">
                    <label>Y: <span id="shadowYVal">10</span></label>
                    <input type="range" id="shadowBlur" min="0" max="100" value="20" oninput="updateShadow()">
                    <label>Blur: <span id="shadowBlurVal">20</span></label>
                    <input type="color" id="shadowColor" value="#000000" oninput="updateShadow()">
                    <div id="shadowPreview" style="width: 200px; height: 200px; background: rgba(255,255,255,0.1); margin: 20px auto; border-radius: 10px;"></div>
                    <input type="text" id="shadowCode" readonly style="font-family: monospace;">
                `,
                textCase: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🔤 تبدیل حروف</h2>
                    <textarea id="textCaseInput" rows="5" placeholder="متن ان��لیسی خود را وارد ک��ید"></textarea>
                    <button onclick="toUpperCase()">بزرگ</button>
                    <button onclick="toLowerCase()">��وچک</button>
                    <button onclick="toCapitalize()">حرف اول بزرگ</button>
                    <textarea id="textCaseOutput" rows="5" readonly style="margin-top: 10px;"></textarea>
                `,
                urlEncode: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🔗 رمزنگاری URL</h2>
                    <textarea id="urlInput" rows="5" placeholder="URL یا متن"></textarea>
                    <button onclick="encodeURL()">رمزنگاری</button>
                    <button onclick="decodeURL()">رمزگشایی</button>
                    <textarea id="urlOutput" rows="5" readonly style="margin-top: 10px;"></textarea>
                `,
                lorem: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📝 تولید متن نمونه</h2>
                    <input type="number" id="loremCount" placeholder="تعداد پاراگراف" value="3" min="1" max="10">
                    <button onclick="generateLorem()">تولید</button>
                    <textarea id="loremOutput" rows="10" readonly style="margin-top: 10px;"></textarea>
                `,
                ip: `
                    <h2 class="text-2xl font-bold mb-4 text-center neon-text">🌐 آدرس IP شما</h2>
                    <div style="text-align: center;">
                        <div id="ipAddress" class="text-3xl font-bold my-6 neon-text">در حال بارگذاری...</div>
                        <button onclick="getIP()">بارگذاری مجدد</button>
                    </div>
                `,
                imageBase64: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🖼️ تبدیل ت��ویر به Base64</h2>
                    <p class="mb-4 text-sm">توجه: فقط برای تصاویر کوچک مناسب است</p>
                    <input type="file" id="imageInput" accept="image/*" onchange="convertImageToBase64()">
                    <textarea id="base64Image" rows="8" readonly style="margin-top: 10px; font-family: monospace;"></textarea>
                `,
                textDiff: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">📊 مقایسه متن</h2>
                    <textarea id="diffText1" rows="5" placeholder="متن اول"></textarea>
                    <textarea id="diffText2" rows="5" placeholder="متن دوم"></textarea>
                    <button onclick="compareText()">مقایسه</button>
                    <div id="diffResult" class="mt-4 text-lg"></div>
                `,
                uuid: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🆔 تولید UUID</h2>
                    <button onclick="generateUUID()">تولید</button>
                    <input type="text" id="uuidOutput" readonly style="margin-top: 10px; font-family: monospace;">
                `,
                epoch: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">🕐 تبدیل Epoch</h2>
                    <input type="number" id="epochInput" placeholder="Epoch timestamp">
                    <button onclick="epochToDate()">به تاریخ</button>
                    <input type="datetime-local" id="dateInput" style="margin-top: 10px;">
                    <button onclick="dateToEpoch()">به Epoch</button>
                    <div id="epochResult" class="mt-4 text-lg font-bold text-center neon-text"></div>
                `,
                dataSize: `
                    <h2 class="text-2xl font-bold mb-4 neon-text">💾 تبدیل حجم داده</h2>
                    <input type="number" id="dataSizeInput" placeholder="مقدار">
                    <select id="dataSizeFrom">
                        <option value="B">بایت</option>
                        <option value="KB">کیلوبایت</option>
                        <option value="MB">مگابایت</option>
                        <option value="GB">گیگابایت</option>
                    </select>
                    <select id="dataSizeTo">
                        <option value="MB">مگابایت</option>
                        <option value="B">بایت</option>
                        <option value="KB">کیلوبایت</option>
                        <option value="GB">گیگابایت</option>
                    </select>
                    <button onclick="convertDataSize()">تبدیل</button>
                    <div id="dataSizeResult" class="mt-4 text-xl font-bold text-center neon-text"></div>
                `
            };

            content.innerHTML = tools[toolName] || '<p>ابزار یافت نشد</p>';
            modal.classList.add('active');

            if (toolName === 'worldClock') updateWorldClock();
            if (toolName === 'colorPicker') updateColor();
            if (toolName === 'ip') getIP();
            if (toolName === 'unitConverter') updateUnitOptions();
        }

        function closeModal() {
            document.getElementById('toolModal').classList.remove('active');
            if (timerInterval) clearInterval(timerInterval);
            if (stopwatchInterval) clearInterval(stopwatchInterval);
        }

        // Calculator functions
        function clearCalc() {
            document.getElementById('calcDisplay').value = '0';
        }

        function appendCalc(val) {
            const display = document.getElementById('calcDisplay');
            if (display.value === '0') display.value = val;
            else display.value += val;
        }

        function calculateCalc() {
            const display = document.getElementById('calcDisplay');
            try {
                display.value = eval(display.value.replace('×', '*').replace('÷', '/'));
            } catch {
                display.value = 'خطا';
            }
        }

        // Unit converter
        function updateUnitOptions() {
            const type = document.getElementById('unitType').value;
            const units = {
                length: ['متر', 'کیلومتر', 'سانتیمتر', '��یلیمتر'],
                weight: ['کیلوگرم', 'گ��م', 'میلیگرم', 'تن'],
                temperature: ['سلسیوس', 'فارنهایت', 'کلوین']
            };
            const options = units[type].map(u => `<option value="${u}">${u}</option>`).join('');
            document.getElementById('unitFrom').innerHTML = options;
            document.getElementById('unitTo').innerHTML = options;
        }

        function convertUnit() {
            const value = parseFloat(document.getElementById('unitInput').value);
            const type = document.getElementById('unitType').value;
            const from = document.getElementById('unitFrom').value;
            const to = document.getElementById('unitTo').value;
            let result = value;

            if (type === 'length') {
                const toMeter = {
                    'متر': 1,
                    'کیلومتر': 1000,
                    'سانتیمتر': 0.01,
                    'میلیمتر': 0.001
                };
                result = value * toMeter[from] / toMeter[to];
            } else if (type === 'weight') {
                const toKg = {
                    'کیلوگرم': 1,
                    'گرم': 0.001,
                    'میلیگرم': 0.000001,
                    'تن': 1000
                };
                result = value * toKg[from] / toKg[to];
            }

            document.getElementById('unitResult').textContent = `${result.toFixed(4)} ${to}`;
        }

        // Timer
        function startTimer() {
            const minutes = parseInt(document.getElementById('timerMinutes').value) || 0;
            const seconds = parseInt(document.getElementById('timerSeconds').value) || 0;
            let totalSeconds = minutes * 60 + seconds;

            if (timerInterval) clearInterval(timerInterval);

            timerInterval = setInterval(() => {
                if (totalSeconds <= 0) {
                    clearInterval(timerInterval);
                    document.getElementById('timerDisplay').textContent = '00:00';
                    return;
                }
                totalSeconds--;
                const m = Math.floor(totalSeconds / 60);
                const s = totalSeconds % 60;
                document.getElementById('timerDisplay').textContent =
                    `${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}`;
            }, 1000);
        }

        function pauseTimer() {
            if (timerInterval) clearInterval(timerInterval);
        }

        function resetTimer() {
            if (timerInterval) clearInterval(timerInterval);
            document.getElementById('timerDisplay').textContent = '00:00';
            document.getElementById('timerMinutes').value = '';
            document.getElementById('timerSeconds').value = '';
        }

        // Note
        function saveNote() {
            localStorage.setItem('quickNote', document.getElementById('noteText').value);
        }

        function loadNote() {
            document.getElementById('noteText').value = localStorage.getItem('quickNote') || '';
        }

        // Percentage
        function calculatePercent() {
            const num = parseFloat(document.getElementById('percentNum').value);
            const of = parseFloat(document.getElementById('percentOf').value);
            const result = (num / of) * 100;
            document.getElementById('percentResult').textContent = `${result.toFixed(2)}%`;
        }

        // Loan
        function calculateLoan() {
            const amount = parseFloat(document.getElementById('loanAmount').value);
            const rate = parseFloat(document.getElementById('loanRate').value) / 100 / 12;
            const months = parseInt(document.getElementById('loanMonths').value);
            const payment = (amount * rate * Math.pow(1 + rate, months)) / (Math.pow(1 + rate, months) - 1);
            const total = payment * months;
            document.getElementById('loanResult').innerHTML = `
                <p><strong>پرداخت ماهانه:</strong> ${payment.toFixed(0)} تومان</p>
                <p><strong>کل پرداختی:</strong> ${total.toFixed(0)} تومان</p>
                <p><strong>کل سود:</strong> ${(total - amount).toFixed(0)} تومان</p>
            `;
        }

        // Tip
        function calculateTip() {
            const bill = parseFloat(document.getElementById('billAmount').value);
            const tipPercent = parseFloat(document.getElementById('tipPercent').value);
            const people = parseInt(document.getElementById('people').value);
            const tip = bill * (tipPercent / 100);
            const total = bill + tip;
            const perPerson = total / people;
            document.getElementById('tipResult').innerHTML = `
                <p><strong>انعام:</strong> ${tip.toFixed(0)} تومان</p>
                <p><strong>کل:</strong> ${total.toFixed(0)} تومان</p>
                <p><strong>هر نفر:</strong> ${perPerson.toFixed(0)} تومان</p>
            `;
        }

        // Currency
        function convertCurrency() {
            const rates = {
                USD: 1,
                EUR: 0.92,
                GBP: 0.79,
                IRR: 42000
            };
            const amount = parseFloat(document.getElementById('currencyAmount').value);
            const from = document.getElementById('currencyFrom').value;
            const to = document.getElementById('currencyTo').value;
            const result = (amount * rates[to]) / rates[from];
            document.getElementById('currencyResult').textContent = result.toFixed(2);
        }

        // Base64
        function encodeBase64() {
            const text = document.getElementById('base64Input').value;
            document.getElementById('base64Output').value = btoa(unescape(encodeURIComponent(text)));
        }

        function decodeBase64() {
            try {
                const text = document.getElementById('base64Input').value;
                document.getElementById('base64Output').value = decodeURIComponent(escape(atob(text)));
            } catch {
                document.getElementById('base64Output').value = 'خطا در رمزگشایی';
            }
        }

        // Stopwatch
        function startStopwatch() {
            if (stopwatchInterval) return;
            stopwatchInterval = setInterval(() => {
                stopwatchTime++;
                updateStopwatchDisplay();
            }, 10);
        }

        function pauseStopwatch() {
            if (stopwatchInterval) {
                clearInterval(stopwatchInterval);
                stopwatchInterval = null;
            }
        }

        function resetStopwatch() {
            pauseStopwatch();
            stopwatchTime = 0;
            updateStopwatchDisplay();
        }

        function updateStopwatchDisplay() {
            const ms = stopwatchTime % 100;
            const s = Math.floor(stopwatchTime / 100) % 60;
            const m = Math.floor(stopwatchTime / 6000);
            document.getElementById('stopwatchDisplay').textContent =
                `${String(m).padStart(2, '0')}:${String(s).padStart(2, '0')}:${String(ms).padStart(2, '0')}`;
        }

        // World Clock
        function updateWorldClock() {
            const cities = [{
                name: 'تهران',
                offset: 3.5
            }, {
                name: 'دبی',
                offset: 4
            }, {
                name: 'لندن',
                offset: 0
            }, {
                name: 'نیویو��ک',
                offset: -5
            }, {
                name: 'توکیو',
                offset: 9
            }];
            const html = cities.map(city => {
                const date = new Date();
                const utc = date.getTime() + (date.getTimezoneOffset() * 60000);
                const cityTime = new Date(utc + (3600000 * city.offset));
                return `
                    <div style="background: rgba(0,0,0,0.5); padding: 15px; border-radius: 10px; border: 2px solid;" class="neon-border">
                        <h3 class="text-xl font-bold">${city.name}</h3>
                        <p class="text-2xl">${cityTime.toLocaleTimeString('fa-IR')}</p>
                    </div>
                `;
            }).join('');
            document.getElementById('worldClockList').innerHTML = html;
        }

        // BMI
        function calculateBMI() {
            const weight = parseFloat(document.getElementById('bmiWeight').value);
            const height = parseFloat(document.getElementById('bmiHeight').value) / 100;
            const bmi = weight / (height * height);
            let category = '';
            if (bmi < 18.5) category = 'ک��بود وزن';
            else if (bmi < 25) category = 'وزن طبیعی';
            else if (bmi < 30) category = 'اض��فه و��ن';
            else category = 'چاقی';
            document.getElementById('bmiResult').innerHTML = `
                <p><strong>BMI:</strong> ${bmi.toFixed(1)}</p>
                <p><strong>وضعیت:</strong> ${category}</p>
            `;
        }

        // Age
        function calculateAge() {
            const birth = new Date(document.getElementById('birthDate').value);
            const today = new Date();
            let years = today.getFullYear() - birth.getFullYear();
            let months = today.getMonth() - birth.getMonth();
            let days = today.getDate() - birth.getDate();

            if (days < 0) {
                months--;
                days += new Date(today.getFullYear(), today.getMonth(), 0).getDate();
            }
            if (months < 0) {
                years--;
                months += 12;
            }

            document.getElementById('ageResult').innerHTML = `
                <p><strong>سن:</strong> ${years} سال، ${months} ماه�� ${days} روز</p>
                <p><strong>کل روزها:</strong> ${Math.floor((today - birth) / 86400000)} روز</p>
            `;
        }

        // Word Count
        function countWords() {
            const text = document.getElementById('wordCountText').value;
            const words = text.trim().split(/\s+/).filter(w => w.length > 0).length;
            const chars = text.length;
            const charsNoSpace = text.replace(/\s/g, '').length;
            document.getElementById('wordCountResult').innerHTML = `
                <p><strong>کلمات:</strong> ${words}</p>
                <p><strong>حروف (با فاصله):</strong> ${chars}</p>
                <p><strong>حروف (بدون فاصله):</strong> ${charsNoSpace}</p>
            `;
        }

        // Password
        function generatePassword() {
            const length = parseInt(document.getElementById('passwordLength').value);
            const includeNumbers = document.getElementById('includeNumbers').checked;
            const includeSymbols = document.getElementById('includeSymbols').checked;

            let chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
            if (includeNumbers) chars += '0123456789';
            if (includeSymbols) chars += '!@#$%^&*()_+-=[]{}|;:,.<>?';

            let password = '';
            for (let i = 0; i < length; i++) {
                password += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            document.getElementById('generatedPassword').value = password;
        }

        // QR Code
        function generateQR() {
            const text = document.getElementById('qrText').value;
            document.getElementById('qrcode').innerHTML = `
                <div style="border: 4px solid; padding: 20px; display: inline-block; border-radius: 10px;" class="neon-border">
                    <p style="word-break: break-all; max-width: 200px;">${text}</p>
                    <p style="text-align: center; margin-top: 10px; font-size: 12px;">QR Code Placeholder</p>
                </div>
            `;
        }

        // Random Number
        function generateRandom() {
            const min = parseInt(document.getElementById('randomMin').value);
            const max = parseInt(document.getElementById('randomMax').value);
            const random = Math.floor(Math.random() * (max - min + 1)) + min;
            document.getElementById('randomResult').textContent = random;
        }

        // Dice
        function rollDice() {
            const dice = ['⚀', '⚁', '⚂', '������', '⚄', '⚅'];
            const result = dice[Math.floor(Math.random() * 6)];
            document.getElementById('diceResult').textContent = result;
        }

        // Color Picker
        function updateColor() {
            const color = document.getElementById('colorInput').value;
            const r = parseInt(color.substr(1, 2), 16);
            const g = parseInt(color.substr(3, 2), 16);
            const b = parseInt(color.substr(5, 2), 16);
            document.getElementById('colorInfo').innerHTML = `
                <p><strong>HEX:</strong> ${color}</p>
                <p><strong>RGB:</strong> rgb(${r}, ${g}, ${b})</p>
            `;
        }

        // Counter
        function incrementCounter() {
            counterValue++;
            document.getElementById('counterValue').textContent = counterValue;
        }

        function decrementCounter() {
            counterValue--;
            document.getElementById('counterValue').textContent = counterValue;
        }

        function resetCounter() {
            counterValue = 0;
            document.getElementById('counterValue').textContent = counterValue;
        }

        // Morse Code
        const morseCode = {
            'A': '.-',
            'B': '-...',
            'C': '-.-.',
            'D': '-..',
            'E': '.',
            'F': '..-.',
            'G': '--.',
            'H': '....',
            'I': '..',
            'J': '.---',
            'K': '-.-',
            'L': '.-..',
            'M': '--',
            'N': '-.',
            'O': '---',
            'P': '.--.',
            'Q': '--.-',
            'R': '.-.',
            'S': '...',
            'T': '-',
            'U': '..-',
            'V': '...-',
            'W': '.--',
            'X': '-..-',
            'Y': '-.--',
            'Z': '--..',
            '0': '-----',
            '1': '.----',
            '2': '..---',
            '3': '...--',
            '4': '....-',
            '5': '.....',
            '6': '-....',
            '7': '--...',
            '8': '---..',
            '9': '----.',
            ' ': '/'
        };

        function textToMorse() {
            const text = document.getElementById('morseInput').value.toUpperCase();
            const morse = text.split('').map(c => morseCode[c] || '').join(' ');
            document.getElementById('morseOutput').value = morse;
        }

        function morseToText() {
            const reverseMorse = Object.fromEntries(Object.entries(morseCode).map(([k, v]) => [v, k]));
            const morse = document.getElementById('morseInput').value;
            const text = morse.split(' ').map(m => reverseMorse[m] || '').join('');
            document.getElementById('morseOutput').value = text;
        }

        // Hash
        function hashText() {
            const text = document.getElementById('hashInput').value;
            let hash = 0;
            for (let i = 0; i < text.length; i++) {
                hash = ((hash << 5) - hash) + text.charCodeAt(i);
                hash = hash & hash;
            }
            document.getElementById('hashOutput').value = Math.abs(hash).toString(16).padStart(16, '0');
        }

        // Binary
        function textToBinary() {
            const text = document.getElementById('binaryInput').value;
            const binary = text.split('').map(c => c.charCodeAt(0).toString(2).padStart(8, '0')).join(' ');
            document.getElementById('binaryOutput').value = binary;
        }

        function binaryToText() {
            try {
                const binary = document.getElementById('binaryInput').value;
                const text = binary.split(' ').map(b => String.fromCharCode(parseInt(b, 2))).join('');
                document.getElementById('binaryOutput').value = text;
            } catch {
                document.getElementById('binaryOutput').value = 'خطا';
            }
        }

        // Hex Converter
        function convertToHex() {
            const num = parseInt(document.getElementById('hexInput').value);
            document.getElementById('hexOutput').innerHTML = `
                <p><strong>HEX:</strong> 0x${num.toString(16).toUpperCase()}</p>
                <p><strong>Binary:</strong> ${num.toString(2)}</p>
                <p><strong>Octal:</strong> ${num.toString(8)}</p>
            `;
        }

        // RGB to HEX
        function rgbToHex() {
            const r = parseInt(document.getElementById('rgbR').value);
            const g = parseInt(document.getElementById('rgbG').value);
            const b = parseInt(document.getElementById('rgbB').value);
            const hex = '#' + [r, g, b].map(x => x.toString(16).padStart(2, '0')).join('');
            document.getElementById('rgbHexResult').innerHTML = `
                <p><strong>HEX:</strong> ${hex}</p>
                <div style="width: 100px; height: 100px; background: ${hex}; margin: 10px auto; border-radius: 10px; border: 2px solid;" class="neon-border"></div>
            `;
        }

        // Temperature
        function convertTemperature() {
            const value = parseFloat(document.getElementById('tempInput').value);
            const from = document.getElementById('tempFrom').value;
            const to = document.getElementById('tempTo').value;

            let celsius = value;
            if (from === 'F') celsius = (value - 32) * 5 / 9;
            if (from === 'K') celsius = value - 273.15;

            let result = celsius;
            if (to === 'F') result = celsius * 9 / 5 + 32;
            if (to === 'K') result = celsius + 273.15;

            document.getElementById('tempResult').textContent = `${result.toFixed(2)}°`;
        }

        // Speed Test
        let speedTestActive = false;
        let clickCount = 0;

        function startSpeedTest() {
            if (speedTestActive) return;
            speedTestActive = true;
            clickCount = 0;
            document.getElementById('clickCount').textContent = '0';
            document.getElementById('speedTestBtn').textContent = 'کلیک کن!';

            const btn = document.getElementById('speedTestBtn');

            btn.onclick = () => {
                if (speedTestActive) {
                    clickCount++;
                    document.getElementById('clickCount').textContent = clickCount;
                }
            };

            setTimeout(() => {
                speedTestActive = false;
                btn.textContent = `تمام شد! ${clickCount} کلیک در 10 ثانیه`;
                btn.onclick = startSpeedTest;
            }, 10000);
        }

        // JSON Formatter
        function formatJSON() {
            try {
                const json = JSON.parse(document.getElementById('jsonInput').value);
                document.getElementById('jsonOutput').value = JSON.stringify(json, null, 2);
            } catch {
                document.getElementById('jsonOutput').value = 'خطا: JSON نامعتبر';
            }
        }

        // Markdown
        function renderMarkdown() {
            const md = document.getElementById('markdownInput').value;
            let html = md
                .replace(/^# (.+)$/gm, '<h1 style="font-size: 24px; font-weight: bold; margin: 10px 0;">$1</h1>')
                .replace(/^## (.+)$/gm, '<h2 style="font-size: 20px; font-weight: bold; margin: 10px 0;">$1</h2>')
                .replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>')
                .replace(/\*(.+?)\*/g, '<em>$1</em>')
                .replace(/\n/g, '<br>');
            document.getElementById('markdownOutput').innerHTML = html;
        }

        // Gradient
        function generateGradient() {
            const color1 = document.getElementById('gradColor1').value;
            const color2 = document.getElementById('gradColor2').value;
            const direction = document.getElementById('gradDirection').value;
            const gradient = `linear-gradient(${direction}, ${color1}, ${color2})`;
            document.getElementById('gradientPreview').style.background = gradient;
            document.getElementById('gradientCode').value = `background: ${gradient};`;
        }

        // Box Shadow
        function updateShadow() {
            const x = document.getElementById('shadowX').value;
            const y = document.getElementById('shadowY').value;
            const blur = document.getElementById('shadowBlur').value;
            const color = document.getElementById('shadowColor').value;

            document.getElementById('shadowXVal').textContent = x;
            document.getElementById('shadowYVal').textContent = y;
            document.getElementById('shadowBlurVal').textContent = blur;

            const shadow = `${x}px ${y}px ${blur}px ${color}`;
            document.getElementById('shadowPreview').style.boxShadow = shadow;
            document.getElementById('shadowCode').value = `box-shadow: ${shadow};`;
        }

        // Text Case
        function toUpperCase() {
            const text = document.getElementById('textCaseInput').value;
            document.getElementById('textCaseOutput').value = text.toUpperCase();
        }

        function toLowerCase() {
            const text = document.getElementById('textCaseInput').value;
            document.getElementById('textCaseOutput').value = text.toLowerCase();
        }

        function toCapitalize() {
            const text = document.getElementById('textCaseInput').value;
            document.getElementById('textCaseOutput').value = text.replace(/\b\w/g, l => l.toUpperCase());
        }

        // URL Encode
        function encodeURL() {
            const text = document.getElementById('urlInput').value;
            document.getElementById('urlOutput').value = encodeURIComponent(text);
        }

        function decodeURL() {
            try {
                const text = document.getElementById('urlInput').value;
                document.getElementById('urlOutput').value = decodeURIComponent(text);
            } catch {
                document.getElementById('urlOutput').value = 'خطا';
            }
        }

        // Lorem Ipsum
        function generateLorem() {
            const count = parseInt(document.getElementById('loremCount').value);
            const lorem = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris.';
            document.getElementById('loremOutput').value = Array(count).fill(lorem).join('\n\n');
        }

        // IP Address
        function getIP() {
            document.getElementById('ipAddress').textContent = 'در حال ��ریافت...';
            fetch('https://api.ipify.org?format=json')
                .then(r => r.json())
                .then(data => {
                    document.getElementById('ipAddress').textContent = data.ip;
                })
                .catch(() => {
                    document.getElementById('ipAddress').textContent = 'خطا در دریافت IP';
                });
        }

        // Image to Base64
        function convertImageToBase64() {
            const file = document.getElementById('imageInput').files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = (e) => {
                document.getElementById('base64Image').value = e.target.result;
            };
            reader.readAsDataURL(file);
        }

        // Text Diff
        function compareText() {
            const text1 = document.getElementById('diffText1').value;
            const text2 = document.getElementById('diffText2').value;

            const same = text1 === text2;
            const lengthDiff = Math.abs(text1.length - text2.length);

            document.getElementById('diffResult').innerHTML = `
                <p><strong>یکسان:</strong> ${same ? 'بله' : 'خیر'}</p>
                <p><strong>طول متن 1:</strong> ${text1.length}</p>
                <p><strong>طول متن 2:</strong> ${text2.length}</p>
                <p><strong>اختلاف طول:</strong> ${lengthDiff}</p>
            `;
        }

        // UUID
        function generateUUID() {
            const uuid = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, c => {
                const r = Math.random() * 16 | 0;
                const v = c === 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
            document.getElementById('uuidOutput').value = uuid;
        }

        // Epoch
        function epochToDate() {
            const epoch = parseInt(document.getElementById('epochInput').value);
            const date = new Date(epoch * 1000);
            document.getElementById('epochResult').textContent = date.toLocaleString('fa-IR');
        }

        function dateToEpoch() {
            const date = new Date(document.getElementById('dateInput').value);
            const epoch = Math.floor(date.getTime() / 1000);
            document.getElementById('epochResult').textContent = epoch;
        }

        // Data Size
        function convertDataSize() {
            const value = parseFloat(document.getElementById('dataSizeInput').value);
            const from = document.getElementById('dataSizeFrom').value;
            const to = document.getElementById('dataSizeTo').value;

            const sizes = {
                B: 1,
                KB: 1024,
                MB: 1024 * 1024,
                GB: 1024 * 1024 * 1024
            };
            const result = (value * sizes[from]) / sizes[to];

            document.getElementById('dataSizeResult').textContent = `${result.toFixed(4)} ${to}`;
        }

        // Element SDK Integration
        async function onConfigChange(config) {
            const fontFamily = config.font_family || defaultConfig.font_family;
            const fontSize = config.font_size || defaultConfig.font_size;

            document.body.style.fontFamily = `${fontFamily}, Courier New, monospace`;
            document.body.style.fontSize = `${fontSize}px`;

            document.getElementById('siteTitle').textContent = '🛠️ ' + (config.site_title || defaultConfig.site_title);
            document.getElementById('page1Title').textContent = config.page1_title || defaultConfig.page1_title;
            document.getElementById('page2Title').textContent = config.page2_title || defaultConfig.page2_title;
            document.getElementById('creatorName').textContent = config.creator_name || defaultConfig.creator_name;
            document.getElementById('instagramId').textContent = config.instagram_id || defaultConfig.instagram_id;
            document.getElementById('phoneNumber').textContent = '📞 ' + (config.phone_number || defaultConfig.phone_number);
        }

        if (window.elementSdk) {
            window.elementSdk.init({
                defaultConfig,
                onConfigChange,
                mapToCapabilities: (config) => ({
                    recolorables: [],
                    borderables: [],
                    fontEditable: {
                        get: () => config.font_family || defaultConfig.font_family,
                        set: (value) => {
                            config.font_family = value;
                            window.elementSdk.setConfig({
                                font_family: value
                            });
                        }
                    },
                    fontSizeable: {
                        get: () => config.font_size || defaultConfig.font_size,
                        set: (value) => {
                            config.font_size = value;
                            window.elementSdk.setConfig({
                                font_size: value
                            });
                        }
                    }
                }),
                mapToEditPanelValues: (config) => new Map([
                    ['site_title', config.site_title || defaultConfig.site_title],
                    ['page1_title', config.page1_title || defaultConfig.page1_title],
                    ['page2_title', config.page2_title || defaultConfig.page2_title],
                    ['creator_name', config.creator_name || defaultConfig.creator_name],
                    ['instagram_id', config.instagram_id || defaultConfig.instagram_id],
                    ['phone_number', config.phone_number || defaultConfig.phone_number]
                ])
            });

            onConfigChange(window.elementSdk.config);
        }

        // Initialize
        initMatrix();

        // Load saved theme
        const savedTheme = localStorage.getItem('selectedTheme');
        if (savedTheme) {
            applyTheme(parseInt(savedTheme));
        } else {
            applyTheme(1); // Default to Cyber Blue
        }
    </script>
    <script>
        (function() {
            function c() {
                var b = a.contentDocument || a.contentWindow.document;
                if (b) {
                    var d = b.createElement('script');
                    d.innerHTML = "window.__CF$cv$params={r:'9afdf867f3a1dcc9',t:'MTc2NjA1MzY3NC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";
                    b.getElementsByTagName('head')[0].appendChild(d)
                }
            }
            if (document.body) {
                var a = document.createElement('iframe');
                a.height = 1;
                a.width = 1;
                a.style.position = 'absolute';
                a.style.top = 0;
                a.style.left = 0;
                a.style.border = 'none';
                a.style.visibility = 'hidden';
                document.body.appendChild(a);
                if ('loading' !== document.readyState) c();
                else if (window.addEventListener) document.addEventListener('DOMContentLoaded', c);
                else {
                    var e = document.onreadystatechange || function() {};
                    document.onreadystatechange = function(b) {
                        e(b);
                        'loading' !== document.readyState && (document.onreadystatechange = e, c())
                    }
                }
            }
        })();
    </script>
</body>

</html>
