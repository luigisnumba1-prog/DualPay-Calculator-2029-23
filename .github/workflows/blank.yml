<!DOCTYPE html>
<html lang="bg">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DualPay Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=Comfortaa:wght@400;700&family=Montserrat:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdn.tailwindcss.com"></script>
<style>
  /* Tailwind overrides */
  body {
    font-family: 'Montserrat', sans-serif;
    background: linear-gradient(135deg, #f7f0ff 25%, #ffffff 25%, #ffffff 50%, #f7f0ff 50%, #f7f0ff 75%, #ffffff 75%, #ffffff 100%);
    background-size: 40px 40px;
  }
  h1, h2, h3 {
    font-family: 'Comfortaa', cursive;
  }
  .glass {
    backdrop-filter: blur(10px);
    background-color: rgba(255, 255, 255, 0.3);
    box-shadow: 0 8px 32px rgba(0,0,0,0.2);
    border-radius: 20px;
  }
  .float {
    animation: floatAnim 6s ease-in-out infinite;
  }
  @keyframes floatAnim {
    0% { transform: translate(0,0) rotate(0deg) scale(1); }
    50% { transform: translate(10px, -10px) rotate(5deg) scale(1.05); }
    100% { transform: translate(0,0) rotate(0deg) scale(1); }
  }
</style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

<div class="glass p-6 max-w-md w-full space-y-4 relative">
  <!-- Floating background symbols -->
  <div class="absolute inset-0 pointer-events-none">
    <div class="float text-gray-200 text-4xl opacity-20 absolute top-2 left-4">€</div>
    <div class="float text-gray-200 text-4xl opacity-20 absolute top-16 right-6">лв</div>
    <div class="float text-gray-200 text-4xl opacity-20 absolute bottom-10 left-10">€</div>
    <div class="float text-gray-200 text-4xl opacity-20 absolute bottom-4 right-8">лв</div>
  </div>

  <!-- Logo -->
  <div class="flex justify-center mb-4">
    <canvas id="logoCanvas" width="80" height="80" class="rounded-full"></canvas>
  </div>

  <h1 class="text-2xl text-center text-purple-700 font-bold">DualPay Calculator</h1>

  <!-- Сума за плащане -->
  <div>
    <label class="block mb-1 text-gray-700">Сума за плащане</label>
    <div class="flex gap-2">
      <input id="amount" type="number" min="0" step="0.01" class="flex-1 p-2 rounded border border-gray-300 bg-purple-50" placeholder="0.00">
      <select id="currency" class="p-2 rounded border border-gray-300 bg-purple-50">
        <option value="BGN">BGN</option>
        <option value="EUR">EUR</option>
      </select>
    </div>
  </div>

  <!-- Платено в лв и € -->
  <div>
    <label class="block mb-1 text-gray-700">Платено в лв</label>
    <input id="paidBGN" type="number" min="0" step="0.01" class="w-full p-2 rounded border border-gray-300 bg-purple-50" placeholder="0.00">
  </div>
  <div>
    <label class="block mb-1 text-gray-700">Платено в €</label>
    <input id="paidEUR" type="number" min="0" step="0.01" class="w-full p-2 rounded border border-gray-300 bg-purple-50" placeholder="0.00">
  </div>

  <!-- Ресто -->
  <div class="space-y-2">
    <label class="block mb-1 text-gray-700">Ресто</label>
    <div id="changeBox" class="p-2 rounded border border-gray-300 bg-white transition-all duration-300 hidden">
      <div class="flex items-center justify-between">
        <span id="changeAmount" class="font-bold text-gray-800">0.00</span>
        <div>
          <button data-currency="BGN" class="px-2 py-1 rounded bg-blue-500 text-white">BGN</button>
          <button data-currency="EUR" class="px-2 py-1 rounded bg-blue-500 text-white">EUR</button>
        </div>
      </div>
      <div class="text-sm text-gray-500 mt-1" id="equivalent"></div>
    </div>
    <div id="errorBox" class="p-2 rounded bg-red-100 text-red-700 hidden">Недостатъчна сума!</div>
  </div>

  <!-- Footer -->
  <div class="text-center text-sm text-gray-500 mt-4">
    Курс: 1 EUR = 1.95583 BGN<br>
    © ТВОЕТО ИМЕ
  </div>
</div>

<script>
  // Canvas logo
  const canvas = document.getElementById('logoCanvas');
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = '#4F46E5'; // наситено морско синьо
  ctx.fillRect(0,0,80,80);
  ctx.fillStyle = 'white';
  ctx.font = 'bold 28px Comfortaa';
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  ctx.fillText('€ ⇄ лв', 40, 40);

  // Calculation logic
  const amountInput = document.getElementById('amount');
  const currencySelect = document.getElementById('currency');
  const paidBGN = document.getElementById('paidBGN');
  const paidEUR = document.getElementById('paidEUR');
  const changeBox = document.getElementById('changeBox');
  const changeAmount = document.getElementById('changeAmount');
  const equivalent = document.getElementById('equivalent');
  const errorBox = document.getElementById('errorBox');
  const changeButtons = changeBox.querySelectorAll('button');

  const RATE = 1.95583;

  function calculate() {
    const totalAmount = parseFloat(amountInput.value) || 0;
    const currency = currencySelect.value;
    let paid = 0;
    paid += (parseFloat(paidBGN.value) || 0);
    paid += (parseFloat(paidEUR.value) || 0) * RATE;

    const totalInBGN = currency === 'BGN' ? totalAmount : totalAmount * RATE;
    const change = paid - totalInBGN;

    if (change < 0) {
      changeBox.classList.add('hidden');
      errorBox.classList.remove('hidden');
    } else {
      errorBox.classList.add('hidden');
      changeBox.classList.remove('hidden');
      let displayCurrency = 'BGN';
      changeAmount.innerText = change.toFixed(2) + ' ' + displayCurrency;
      equivalent.innerText = '≈ ' + (change / RATE).toFixed(2) + ' EUR';
      changeButtons.forEach(btn => btn.addEventListener('click', () => {
        displayCurrency = btn.getAttribute('data-currency');
        if(displayCurrency === 'BGN') {
          changeAmount.innerText = change.toFixed(2) + ' BGN';
          equivalent.innerText = '≈ ' + (change / RATE).toFixed(2) + ' EUR';
        } else {
          const eurValue = change / RATE;
          changeAmount.innerText = eurValue.toFixed(2) + ' EUR';
          equivalent.innerText = '≈ ' + (eurValue * RATE).toFixed(2) + ' BGN';
        }
      }));
    }
  }

  [amountInput, currencySelect, paidBGN, paidEUR].forEach(el => {
    el.addEventListener('input', calculate);
  });

  // PWA manifest & service worker
  const manifest = {
    name: "DualPay Calculator",
    short_name: "DualPay",
    start_url: ".",
    display: "standalone",
    background_color: "#f7f0ff",
    theme_color: "#4F46E5",
    icons: [
      {
        src: canvas.toDataURL(),
        sizes: "80x80",
        type: "image/png"
      }
    ]
  };

  const manifestBlob = new Blob([JSON.stringify(manifest)], {type: 'application/json'});
  const manifestURL = URL.createObjectURL(manifestBlob);
  const link = document.createElement('link');
  link.rel = 'manifest';
  link.href = manifestURL;
  document.head.appendChild(link);

  // Service Worker
  if ('serviceWorker' in navigator) {
    const swBlob = new Blob([`
      self.addEventListener('install', e => { self.skipWaiting(); });
      self.addEventListener('fetch', e => { e.respondWith(fetch(e.request).catch(() => caches.match(e.request))); });
    `], {type: 'application/javascript'});
    const swURL = URL.createObjectURL(swBlob);
    navigator.serviceWorker.register(swURL);
  }
</script>
</body>
</html>
