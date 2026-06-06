<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Private Crypto Tracker</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen pb-32">

    <div class="bg-slate-800 text-center py-5 shadow-lg border-b border-slate-700 sticky top-0 z-50">
        <h1 class="text-xl font-bold tracking-wide text-emerald-400">📊 My Crypto Journal</h1>
        <p class="text-xs text-slate-400 mt-1">100% Private • Local Mobile Database</p>
    </div>

    <div class="max-w-md mx-auto px-4 mt-4 space-y-4">

        <div class="bg-slate-800 p-5 rounded-2xl shadow-md border border-slate-700">
            <div class="flex items-center space-x-2 border-b border-slate-700 pb-2 mb-3">
                <span class="text-xl">📥</span>
                <h2 class="text-base font-bold text-slate-200">नया ट्रांजैक्शन जोड़ें</h2>
            </div>
            <div class="space-y-3">
                <div>
                    <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">क्रिप्टो का नाम (Symbol)</label>
                    <select id="cryptoSymbol" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-3 text-sm font-medium text-white focus:outline-none focus:border-emerald-500">
                        <option value="ADA">Cardano (ADA)</option>
                        <option value="FET">Artificial Superintelligence Alliance (FET)</option>
                        <option value="BTC">Bitcoin (BTC)</option>
                        <option value="ETH">Ethereum (ETH)</option>
                        <option value="SOL">Solana (SOL)</option>
                    </select>
                </div>
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">क्वांटिटी (Qty)</label>
                        <input type="number" id="cryptoQty" placeholder="0.00" step="any" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-3 text-sm text-white focus:outline-none focus:border-emerald-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">बाइंग प्राइस ($)</label>
                        <input type="number" id="cryptoPrice" placeholder="USD में" step="any" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-3 text-sm text-white focus:outline-none focus:border-emerald-500">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">परचेस डेट</label>
                    <input type="date" id="cryptoDate" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-3 text-sm text-white focus:outline-none">
                </div>
                <button onclick="addTransaction()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3 rounded-xl transition mt-2">
                    Add to Portfolio
                </button>
            </div>
        </div>

        <div class="bg-slate-800 p-4 rounded-2xl border border-slate-700 flex justify-between items-center">
            <span class="text-xs font-semibold uppercase text-slate-400 tracking-wider">Live Rates Status:</span>
            <button onclick="fetchLivePrices()" class="bg-slate-700 hover:bg-slate-600 text-xs px-3 py-1.5 rounded-lg text-emerald-400 font-bold flex items-center space-x-1">
                <span>🔄</span> <span>Refresh Rates</span>
            </button>
        </div>

        <div id="cryptoCardsContainer" class="space-y-3">
            </div>

        <div class="bg-slate-800 p-5 rounded-2xl shadow-md border border-slate-700 space-y-3">
            <h3 class="text-sm font-bold text-slate-300 uppercase tracking-wider">Currency Exchange Configuration</h3>
            <div>
                <label class="block text-xs text-slate-400 mb-1">आज का डॉलर रेट डालें (1 USD = INR)</label>
                <input type="number" id="usdInrRate" value="83.50" step="0.05" oninput="updateCalculationsOnly()" class="w-full bg-slate-900 border border-slate-700 rounded-xl p-3 text-sm text-white focus:outline-none">
            </div>
        </div>

        <div class="bg-slate-800 p-4 rounded-2xl border border-slate-700">
            <details class="group">
                <summary class="list-none flex justify-between items-center font-bold text-sm text-slate-300 cursor-pointer">
                    <span>📝 सभी ट्रांजैक्शन्स की हिस्ट्री</span>
                    <span class="group-open:rotate-180 transition-transform">▼</span>
                </summary>
                <div id="historyTableContainer" class="mt-3 overflow-x-auto text-xs space-y-2 max-h-60 overflow-y-auto">
                    </div>
            </details>
        </div>

        <div class="bg-slate-950 text-white p-5 rounded-2xl shadow-2xl border border-slate-800 sticky bottom-4 z-40">
            <div class="space-y-2">
                <div class="flex justify-between items-center text-xs text-slate-400 font-medium">
                    <span>TOTAL TARGET VALUE (USD):</span>
                    <span id="grandTargetUsd" class="font-bold text-slate-200">$0.00</span>
                </div>
                <div class="border-t border-slate-800 my-1"></div>
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-[10px] uppercase tracking-wider text-slate-400 font-bold">Target Hit Final Value (INR)</p>
                        <p class="text-2xl font-black text-emerald-400 mt-0.5" id="grandTargetInr">₹0</p>
                    </div>
                    <button onclick="clearAllData()" class="bg-slate-900 hover:bg-red-950 text-slate-400 hover:text-red-400 text-xs px-3 py-2 rounded-xl border border-slate-800 transition font-medium">
                        Wipe All
                    </button>
                </div>
            </div>
        </div>

    </div>

    <script>
        // Mapping CoinGecko IDs
        const cgMapping = {
            "ADA": "cardano",
            "FET": "artificial-superintelligence-alliance",
            "BTC": "bitcoin",
            "ETH": "ethereum",
            "SOL": "solana"
        };

        let transactions = [];
        let livePrices = { "ADA": 0, "FET": 0, "BTC": 0, "ETH": 0, "SOL": 0 };
        let targetInputs = {};

        // Load data on start
        window.onload = function() {
            document.getElementById('cryptoDate').value = new Date().toISOString().split('T')[0];
            
            // Load transactions
            const savedTx = localStorage.getItem('my_private_crypto_tx');
            if(savedTx) transactions = JSON.parse(savedTx);

            // Load saved targets
            const savedTargets = localStorage.getItem('my_private_crypto_targets');
            if(savedTargets) targetInputs = JSON.parse(savedTargets);

            // Load saved USD/INR rate
            const savedRate = localStorage.getItem('my_private_usd_inr');
            if(savedRate) document.getElementById('usdInrRate').value = savedRate;

            fetchLivePrices();
        };

        // Fetch Live Rates
        function fetchLivePrices() {
            const ids = Object.values(cgMapping).join(',');
            const url = `https://api.coingecko.com/api/v3/simple/price?ids=${ids}&vs_currencies=usd`;

            fetch(url)
                .then(res => res.json())
                .then(data => {
                    Object.keys(cgMapping).forEach(sym => {
                        const id = cgMapping[sym];
                        if(data[id]) {
                            livePrices[sym] = data[id].usd;
                        }
                    });
                    renderAll();
                })
                .catch(err => {
                    console.log("API Error, using fallback or 0", err);
                    renderAll();
                });
        }

        // Add Transaction
        function addTransaction() {
            const sym = document.getElementById('cryptoSymbol').value;
            const qty = parseFloat(document.getElementById('cryptoQty').value) || 0;
            const price = parseFloat(document.getElementById('cryptoPrice').value) || 0;
            const date = document.getElementById('cryptoDate').value;

            if(qty <= 0 || price <= 0) {
                alert("Please enter valid quantity and price.");
                return;
            }

            transactions.push({ sym, qty, price, date });
            localStorage.setItem('my_private_crypto_tx', JSON.stringify(transactions));

            // Reset fields
            document.getElementById('cryptoQty').value = '';
            document.getElementById('cryptoPrice').value = '';
            
            renderAll();
        }

        // Remove Single Transaction Row
        function deleteTransaction(index) {
            if(confirm("इस ट्रांजैक्शन को डिलीट करें?")) {
                transactions.splice(index, 1);
                localStorage.setItem('my_private_crypto_tx', JSON.stringify(transactions));
                renderAll();
            }
        }

        // Save Target Box values instantly dynamically
        function handleTargetChange(sym, val) {
            targetInputs[sym] = parseFloat(val) || 0;
            localStorage.setItem('my_private_crypto_targets', JSON.stringify(targetInputs));
            updateCalculationsOnly();
        }

        // Full UI Render
        function renderAll() {
            const container = document.getElementById('cryptoCardsContainer');
            container.innerHTML = '';

            // Group transactions by token
            let summary = {};
            transactions.forEach(t => {
                if(!summary[t.sym]) {
                    summary[t.sym] = { totalQty: 0, totalSpent: 0 };
                }
                summary[t.sym].totalQty += t.qty;
                summary[t.sym].totalSpent += (t.qty * t.price);
            });

            // If no data
            if(Object.keys(summary).length === 0) {
                container.innerHTML = `<div class="text-center text-slate-500 py-6 text-sm">पोर्टफोलियो खाली है। नया ट्रांजैक्शन जोड़ें!</div>`;
            }

            // Loop through active tokens
            Object.keys(summary).forEach(sym => {
                const data = summary[sym];
                const avgPrice = data.totalSpent / data.totalQty;
                const livePrice = livePrices[sym] || 0;
                const currentVal = data.totalQty * livePrice;
                
                // Initialize target default if not set
                if(targetInputs[sym] === undefined) {
                    targetInputs[sym] = Math.round(avgPrice * 1.5 * 100) / 100; // 1.5x default
                }

                const cardHtml = `
                    <div class="bg-slate-800 p-4 rounded-2xl border border-slate-700 space-y-3 shadow-sm">
                        <div class="flex justify-between items-center border-b border-slate-700 pb-2">
                            <span class="text-base font-black text-emerald-400 tracking-wider">${sym}</span>
                            <div class="text-right">
                                <span class="text-[10px] text-slate-400 font-bold uppercase block">Live Price</span>
                                <span class="text-sm font-bold text-slate-200">$${livePrice.toLocaleString('en-US', {minimumFractionDigits: 2})}</span>
                            </div>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-2 text-xs">
                            <div>
                                <p class="text-slate-400">Total Holdings</p>
                                <p class="font-bold text-slate-200">${data.totalQty.toFixed(4)}</p>
                            </div>
                            <div>
                                <p class="text-slate-400">Invested Capital</p>
                                <p class="font-bold text-slate-200">$${data.totalSpent.toFixed(2)}</p>
                            </div>
                            <div>
                                <p class="text-slate-400">Average Price</p>
                                <p class="font-bold text-slate-300">$${avgPrice.toFixed(4)}</p>
                            </div>
                            <div>
                                <p class="text-slate-400">Current Value</p>
                                <p class="font-bold text-emerald-400">$${currentVal.toFixed(2)}</p>
                            </div>
                        </div>

                        <div class="bg-slate-900/60 p-2.5 rounded-xl border border-slate-700/50 flex justify-between items-center">
                            <span class="text-xs text-amber-400 font-bold">Target Price ($):</span>
                            <input type="number" step="any" value="${targetInputs[sym]}" 
                                oninput="handleTargetChange('${sym}', this.value)"
                                class="target-box-${sym} w-24 bg-slate-800 text-right border border-slate-600 rounded-lg p-1 text-xs font-bold text-white focus:outline-none focus:border-amber-400">
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', cardHtml);
            });

            // Render History
            renderHistory();
            updateCalculationsOnly();
        }

        function renderHistory() {
            const histContainer = document.getElementById('historyTableContainer');
            if(transactions.length === 0) {
                histContainer.innerHTML = `<p class="text-slate-500 text-center">No logs recorded.</p>`;
                return;
            }

            let html = `<table class="w-full text-left border-collapse">
                <thead>
                    <tr class="text-slate-400 border-b border-slate-700">
                        <th class="pb-1">Date</th><th class="pb-1">Coin</th><th class="pb-1">Qty</th><th class="pb-1">Buy($)</th><th class="pb-1 text-center">Action</th>
                    </tr>
                </thead>
                <tbody>`;
            
            transactions.forEach((t, idx) => {
                html += `<tr class="border-b border-slate-800 text-slate-300">
                    <td class="py-2">${t.date}</td>
                    <td class="font-bold text-emerald-400">${t.sym}</td>
                    <td>${t.qty}</td>
                    <td>$${t.price}</td>
                    <td class="text-center"><button onclick="deleteTransaction(${idx})" class="text-red-400 font-bold px-1">✕</button></td>
                </tr>`;
            });
            html += `</tbody></table>`;
            histContainer.innerHTML = html;
        }

        // Fast calculations for Realtime updating without full re-rendering
        function updateCalculationsOnly() {
            let totalTargetUsd = 0;
            const usdInrRate = parseFloat(document.getElementById('usdInrRate').value) || 83.50;
            localStorage.setItem('my_private_usd_inr', usdInrRate);

            // Group summary setup
            let summary = {};
            transactions.forEach(t => {
                if(!summary[t.sym]) summary[t.sym] = 0;
                summary[t.sym] += t.qty;
            });

            // Calc target sum
            Object.keys(summary).forEach(sym => {
                const qty = summary[sym];
                const targetPrice = targetInputs[sym] || 0;
                totalTargetUsd += (qty * targetPrice);
            });

            const totalTargetInr = totalTargetUsd * usdInrRate;

            // Update DOM element displays
            document.getElementById('grandTargetUsd').innerText = `$${totalTargetUsd.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`;
            document.getElementById('grandTargetInr').innerText = `₹${totalTargetInr.toLocaleString('en-IN', {maximumFractionDigits: 0})}`;
        }

        function clearAllData() {
            if(confirm("क्या आप सच में पूरा पोर्टफोलियो डेटा डिलीट करना चाहते हैं?")) {
                localStorage.clear();
                transactions = [];
                targetInputs = {};
                livePrices = { "ADA": 0, "FET": 0, "BTC": 0, "ETH": 0, "SOL": 0 };
                renderAll();
            }
        }
    </script>
</body>
</html>
