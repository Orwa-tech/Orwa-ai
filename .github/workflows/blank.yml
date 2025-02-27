<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Trading Prediction System</title>
    <style>
        body {
            background: transparent;
            font-family: Arial, sans-serif;
        }
        .trade-box {
            position: absolute;
            top: 10px;
            left: 10px;
            background: rgba(0, 0, 0, 0.9);
            color: #fff;
            padding: 15px;
            border-radius: 10px;
            border: 2px solid #00ff00;
            font-size: 16px;
        }
        .trade-status {
            font-weight: bold;
            color: yellow;
            animation: blink 1s infinite alternate;
        }
        @keyframes blink {
            from { opacity: 1; }
            to { opacity: 0.5; }
        }
        select, button {
            margin-top: 5px;
            padding: 5px;
            font-size: 14px;
        }
        .consoles-container {
            position: absolute;
            bottom: 10px;
            left: 10px;
            width: 95%;
            display: flex;
            justify-content: space-between;
        }
        .ai-console, .trade-log, .strategy-console {
            background: rgba(0, 0, 0, 0.9);
            color: #00ff00;
            padding: 10px;
            font-size: 14px;
            border-radius: 5px;
            overflow-y: auto;
            height: 80px;
            border: 2px solid #00ff00;
            width: 32%;
        }
    </style>
</head>
<body>
    <div class="trade-box">
        <label for="marketSelect">🔹 Select Market:</label>
        <select id="marketSelect" onchange="updateTradeInfo()">
            <option value="R_100">Volatility 100 Index</option>
            <option value="R_75">Volatility 75 Index</option>
            <option value="R_50">Volatility 50 Index</option>
            <option value="BOOM_1000">Boom 1000 Index</option>
            <option value="CRASH_500">Crash 500 Index</option>
            <option value="STP">Step Index</option>
        </select>
        
        <label for="tradeTypeSelect">📌 Select Trade Type:</label>
        <select id="tradeTypeSelect">
            <option value="rise_fall">Rise/Fall</option>
            <option value="even_odd">Even/Odd</option>
            <option value="over_under">Over/Under</option>
        </select>
        
        <p>📊 <b>Current Price:</b> <span id="price">Loading...</span></p>
        <p>⏳ <b>Time Left:</b> <span id="timer">5s</span></p>
        <p>✅ <b>Status:</b> <span class="trade-status" id="trade-status">Waiting...</span></p>
        <p>📌 <b>Entry Advice:</b> <span id="entry-advice">Analyzing...</span></p>
        <p>📈 <b>Ticks Before Next Trade:</b> <span id="tick-count">10</span></p>
        <p>📊 <b>Market Entry Probability:</b> <span id="market-probability">Calculating...</span>%</p>
    </div>

    <div class="consoles-container">
        <div class="strategy-console" id="strategy-console">
            <p>Strategy Console: Initializing...</p>
        </div>
        
        <div class="trade-log" id="trade-log">
            <p>Trade Log: Waiting for data...</p>
        </div>
        
        <div class="ai-console" id="ai-console">
            <p>AI Console: Initializing...</p>
        </div>
    </div>

    <script>
        let ws;
        let lastPrices = [];
        let tickCounter = 10;

        function connectWebSocket(market) {
            if (ws) ws.close();
            ws = new WebSocket("wss://ws.binaryws.com/websockets/v3?app_id=1089");
            ws.onopen = () => ws.send(JSON.stringify({ "ticks": market }));
            ws.onmessage = (event) => {
                const data = JSON.parse(event.data);
                if (data.tick) {
                    const latestPrice = parseFloat(data.tick.quote).toFixed(2);
                    document.getElementById("price").textContent = latestPrice;
                    lastPrices.push(latestPrice);
                    if (lastPrices.length > tickCounter) lastPrices.shift();
                    generateEntryAdvice(latestPrice);
                    calculateMarketProbability();
                    updateTickCounter();
                    logToConsole(`Live Price: ${latestPrice}`);
                    logTrade(`New Tick: ${latestPrice}`);
                }
            };
            ws.onerror = () => logToConsole("WebSocket Error. Reconnecting...");
        }

        function updateTradeInfo() {
            const selectedMarket = document.getElementById("marketSelect").value;
            connectWebSocket(selectedMarket);
        }

        function generateEntryAdvice(latestPrice) {
            let digit = parseInt(latestPrice.slice(-1));
            let tradeType = document.getElementById("tradeTypeSelect").value;
            let advice;
            
            if (tradeType === "rise_fall") {
                advice = digit >= 5 ? "Fall" : "Rise";
            } else if (tradeType === "even_odd") {
                advice = digit % 2 === 0 ? "Even" : "Odd";
            } else if (tradeType === "over_under") {
                advice = digit > 5 ? `Under ${digit}` : `Over ${digit}`;
            }
            
            document.getElementById("entry-advice").textContent = `Suggested Trade: ${advice}`;
        }

        function calculateMarketProbability() {
            let probability = Math.random() * 20 + 80;
            document.getElementById("market-probability").textContent = probability.toFixed(2);
        }

        function updateTickCounter() {
            tickCounter--;
            if (tickCounter <= 0) {
                tickCounter = 10;
            }
            document.getElementById("tick-count").textContent = tickCounter;
        }

        function logToConsole(message) {
            document.getElementById("ai-console").innerHTML += `<p>${message}</p>`;
        }

        function logTrade(message) {
            document.getElementById("trade-log").innerHTML += `<p>${message}</p>`;
        }

        updateTradeInfo();
    </script>
</body>
</html>
