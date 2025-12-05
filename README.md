# Arsh0500.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crypto Trade Calculator - Selling Fees Added</title>
    <style>
        :root {
            --primary-color: #007bff;
            --secondary-color: #f8f9fa;
            --text-color: #333;
            --success-color: #28a745;
            --danger-color: #dc3545;
            --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            --input-bg: #fffbe6; /* Light yellow for primary inputs */
        }

        body {
            font-family: var(--font-family);
            background-color: #e9ecef;
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            min-height: 100vh;
        }

        .calculator-container {
            background-color: #fff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            max-width: 900px;
            width: 100%;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 30px;
        }

        h1 {
            grid-column: 1 / -1;
            text-align: center;
            color: var(--primary-color);
            margin-bottom: 20px;
            font-size: 1.8em;
        }

        h2 {
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 5px;
            margin-bottom: 20px;
            color: var(--primary-color);
            font-size: 1.2em;
        }

        .section {
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }

        /* Input Fields Styling */
        .input-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            font-size: 0.95em;
        }

        .input-item label {
            flex: 2;
            margin-right: 10px;
            font-weight: 600;
        }

        .input-item input {
            flex: 3;
            padding: 10px;
            border: 2px solid var(--primary-color);
            border-radius: 5px;
            font-size: 1em;
            text-align: right;
            box-sizing: border-box;
            background-color: var(--input-bg);
        }
        
        .static-charge {
            padding: 5px 0;
            font-size: 0.9em;
            color: #6c757d;
            border-top: 1px dashed #e9ecef;
            margin-top: 10px;
        }


        /* Results Styling */
        .results-section {
            grid-column: 1 / -1; /* Span full width */
            background-color: var(--secondary-color);
            margin-top: 10px;
        }
        
        .result-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px dashed #ccc;
            font-size: 1.1em;
        }

        .result-item:last-child {
            border-bottom: none;
        }

        .result-item .label {
            font-weight: 500;
        }

        .result-item .value {
            font-weight: bold;
        }
        
        /* Highlight calculated charges */
        #calculatedCharges, #sellingChargeAmount {
            font-weight: bold;
            color: #ff6a00; /* Orange color for calculated fee */
        }


        .result-item.final {
            font-size: 1.3em;
            padding: 15px 0 5px 0;
            border-top: 2px solid #333;
            margin-top: 10px;
        }
        
        .final .value {
            color: var(--success-color);
        }
        .final.loss .value {
            color: var(--danger-color);
        }
        .final.zero .value {
            color: var(--text-color);
        }

        /* Responsive adjustments */
        @media (max-width: 768px) {
            .calculator-container {
                grid-template-columns: 1fr;
                gap: 20px;
                padding: 20px;
            }
        }
    </style>
</head>
<body>

    <div class="calculator-container">
        <h1>💰 Crypto Trade Calculator - Fixed Fees</h1>

        <div class="section">
            <h2><span class="currency-symbol">₹</span> Trade Parameters (Inputs)</h2>
            <div class="input-item">
                <label for="investment">Investment Amount (₹)</label>
                <input type="number" id="investment" value="100000.00" min="0" oninput="calculateTrade()">
            </div>
            <div class="input-item">
                <label for="buyPrice">Buy Price Per Coin (₹)</label>
                <input type="number" id="buyPrice" value="92.50" min="0" oninput="calculateTrade()">
            </div>
            <div class="input-item">
                <label for="sellPrice">Sell Price Per Coin (₹)</label>
                <input type="number" id="sellPrice" value="95.00" min="0" oninput="calculateTrade()">
            </div>
            <div class="input-item">
                <label for="sellChargePercentage">Selling Platform Charge (%)</label>
                <input type="number" id="sellChargePercentage" value="0.15" min="0" oninput="calculateTrade()">
            </div>

            <div class="static-charge">
                *Fixed Buy Fee Used: **0.45% (GST Inclusive 1.18)**
            </div>
            <div class="static-charge">
                *Fixed Coin Transfer Fee: **3.54 coins**
            </div>
        </div>
        
        <div class="section">
            <h2>📈 Calculated Trade Details</h2>
            
            <div class="result-item">
                <span class="label">Calculated Buy Charges (₹)</span>
                <span id="calculatedCharges" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Net Amount for Coins (₹)</span>
                <span id="netAmount" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Coins Acquired</span>
                <span id="coinsAcquired" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Effective Price Per Coin (₹)</span>
                <span id="effectivePrice" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Coins to be Sold</span>
                <span id="coinsSold" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Gross Revenue (₹)</span>
                <span id="grossRevenue" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Selling Platform Charges (₹)</span>
                <span id="sellingChargeAmount" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Net Revenue (₹)</span>
                <span id="netRevenue" class="value"></span>
            </div>
        </div>

        <div class="results-section">
            <h2>✅ Trade Summary</h2>

            <div class="result-item">
                <span class="label">Total Investment (₹)</span>
                <span id="finalInvestment" class="value"></span>
            </div>
            <div class="result-item">
                <span class="label">Net Revenue (₹)</span>
                <span id="finalNetRevenue" class="value"></span>
            </div>

            <div class="result-item final" id="profitRow">
                <span class="label">GROSS PROFIT / (LOSS)</span>
                <span id="grossProfit" class="value"></span>
            </div>
        </div>
    </div>

    <script>
        // --- CONSTANTS (Derived from your spreadsheet) ---
        const FEE_PERCENTAGE = 0.45;
        const TAX_MULTIPLIER = 1.18;      // 18% GST (1 + 0.18)
        const COIN_TRANSFER_FEE = 3.54;   // Fixed amount in coins

        // --- Formatting Functions ---
        const formatter = new Intl.NumberFormat('en-IN', {
            style: 'currency',
            currency: 'INR',
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
        });
        
        const coinFormatter = (value) => {
            return new Intl.NumberFormat('en-US', {
                minimumFractionDigits: 8,
                maximumFractionDigits: 8,
            }).format(value);
        };
        
        // --- Core Calculation Function ---
        function calculateTrade() {
            // 1. Get User Input Values
            const investment = parseFloat(document.getElementById('investment').value) || 0;
            const buyPrice = parseFloat(document.getElementById('buyPrice').value) || 0;
            const sellPrice = parseFloat(document.getElementById('sellPrice').value) || 0;
            const sellChargePercentage = parseFloat(document.getElementById('sellChargePercentage').value) || 0; // NEW INPUT

            let buyCharges = 0;
            let coinsAcquired = 0;
            let netAmount = 0;
            let effectivePrice = 0;
            let coinsToSold = 0;
            let grossRevenue = 0;
            let sellingPlatformCharges = 0;
            let netRevenue = 0;
            let grossProfit = 0;
            
            // --- BUY SIDE CALCULATIONS ---
            // Formula 1: Buy Charges = Investment * (FEE_PERCENTAGE / 100) * TAX_MULTIPLIER
            buyCharges = investment * (FEE_PERCENTAGE / 100) * TAX_MULTIPLIER;
            
            // Net Amount = Investment - Calculated Buy Charges
            netAmount = investment - buyCharges;

            if (buyPrice > 0) {
                // Formula 2: Coins Acquired = (Investment - Charges) / Buy Price Per Coin
                coinsAcquired = netAmount / buyPrice;

                // Formula 3: Effective Price = Investment / Coins Acquired
                if (coinsAcquired > 0) {
                    effectivePrice = investment / coinsAcquired;
                }
            }

            // --- SELL SIDE CALCULATIONS ---
            // Formula 4: Coins to be Sold = Coins Acquired - Coin Transfer Fee (CONSTANT)
            coinsToSold = coinsAcquired - COIN_TRANSFER_FEE;
            if (coinsToSold < 0) coinsToSold = 0;

            // Formula 5: Gross Revenue = Coins to be Sold * Sell Price Per Coin
            grossRevenue = coinsToSold * sellPrice;
            
            // Formula 6: Selling Platform Charge Amount = Gross Revenue * (Sell Charge % / 100)
            sellingPlatformCharges = grossRevenue * (sellChargePercentage / 100);

            // Formula 7: Net Revenue = Gross Revenue - Selling Platform Charge Amount
            netRevenue = grossRevenue - sellingPlatformCharges;

            // --- FINAL CALCULATION ---
            // Formula 8: Gross Profit = Net Revenue - Investment
            grossProfit = netRevenue - investment;


            // 5. Update Display Elements
            
            // Calculated Trade Details
            document.getElementById('calculatedCharges').textContent = formatter.format(buyCharges).replace('₹', '');
            document.getElementById('netAmount').textContent = formatter.format(netAmount).replace('₹', '');
            document.getElementById('coinsAcquired').textContent = coinFormatter(coinsAcquired);
            document.getElementById('effectivePrice').textContent = formatter.format(effectivePrice).replace('₹', '');
            document.getElementById('coinsSold').textContent = coinFormatter(coinsToSold);
            document.getElementById('grossRevenue').textContent = formatter.format(grossRevenue).replace('₹', '');
            document.getElementById('sellingChargeAmount').textContent = formatter.format(sellingPlatformCharges).replace('₹', '');
            document.getElementById('netRevenue').textContent = formatter.format(netRevenue).replace('₹', '');
            
            // Final Summary
            const profitRow = document.getElementById('profitRow');
            const grossProfitElement = document.getElementById('grossProfit');

            document.getElementById('finalInvestment').textContent = formatter.format(investment).replace('₹', '');
            document.getElementById('finalNetRevenue').textContent = formatter.format(netRevenue).replace('₹', '');
            grossProfitElement.textContent = formatter.format(grossProfit).replace('₹', '');
            
            // Apply styling and loss formatting
            profitRow.classList.remove('final', 'loss', 'zero');
            if (grossProfit > 0) {
                profitRow.classList.add('final');
            } else if (grossProfit < 0) {
                profitRow.classList.add('final', 'loss');
                // Display loss in parentheses
                grossProfitElement.textContent = `(${formatter.format(Math.abs(grossProfit)).replace('₹', '')})`;
            } else {
                profitRow.classList.add('final', 'zero');
            }
        }

        // Run the calculation once on page load to display initial values
        window.onload = calculateTrade;
    </script>

</body>
</html>
