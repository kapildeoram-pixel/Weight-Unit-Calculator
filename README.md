<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Unit Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @media print {
            body { background: white !important; color: black !important; padding: 0 !important; }
            .no-print { display: none !important; }
            .print-card { border: none !important; box-shadow: none !important; max-width: 100% !important; margin: 0 !important; padding: 0 !important; }
            input, select { border: none !important; background: transparent !important; appearance: none; -webkit-appearance: none; pointer-events: none; padding-right: 0 !important; font-weight: bold; width: auto !important; text-align: right; }
            select { text-align-last: right; }
        }
    </style>
</head>
<body class="bg-slate-50 min-h-screen flex items-center justify-center p-4">

    <div class="print-card bg-white shadow-xl rounded-2xl p-6 w-full max-w-md border border-slate-200">
        <!-- <h2 class="text-xl font-bold text-slate-800 mb-6 text-center border-b pb-4">Weight Unit Calculator</h2> -->
        
        <!-- Vendor Section -->
        <div class="mb-6">
            <h3 class="text-xs font-semibold text-blue-600 uppercase tracking-wider mb-2">Vendor Point</h3>
            <div class="space-y-3 bg-blue-50/40 p-4 rounded-xl border border-blue-100/80">
                <div class="flex justify-between items-center">
                    <label class="text-slate-600 font-medium text-sm">Unit</label>
                    <select id="vendorUnit" onchange="calculatePrice()" class="bg-white border border-slate-300 rounded-lg px-2 py-1 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="Kg" selected>Kg</option>
                        <option value="g">g</option>
                    </select>
                </div>
                <div class="flex justify-between items-center">
                    <label class="text-slate-600 font-medium text-sm">Quantity</label>
                    <input type="number" id="vendorQty" value="1" step="any" oninput="calculatePrice()" class="w-24 bg-white border border-slate-300 rounded-lg px-2 py-1 text-sm text-right font-medium focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex justify-between items-center">
                    <label class="text-slate-600 font-medium text-sm">Price</label>
                    <input type="number" id="vendorPrice" value="0.05" step="any" oninput="calculatePrice()" class="w-24 bg-white border border-slate-300 rounded-lg px-2 py-1 text-sm text-right font-medium focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
            </div>
        </div>

        <!-- Customer Section -->
        <div class="mb-6">
            <h3 class="text-xs font-semibold text-emerald-600 uppercase tracking-wider mb-2">Customer Point</h3>
            <div class="space-y-3 bg-emerald-50/40 p-4 rounded-xl border border-emerald-100/80">
                <div class="flex justify-between items-center">
                    <label class="text-slate-600 font-medium text-sm">Unit</label>
                    <select id="customerUnit" onchange="calculatePrice()" class="bg-white border border-slate-300 rounded-lg px-2 py-1 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-emerald-500">
                        <option value="Kg" selected>Kg</option>
                        <option value="g">g</option>
                    </select>
                </div>
                <div class="flex justify-between items-center">
                    <label class="text-slate-600 font-medium text-sm">Quantity</label>
                    <input type="number" id="customerQty" value="2" step="any" oninput="calculatePrice()" class="w-24 bg-white border border-slate-300 rounded-lg px-2 py-1 text-sm text-right font-medium focus:outline-none focus:ring-2 focus:ring-emerald-500">
                </div>
                <div class="flex justify-between items-center border-t border-emerald-200/60 pt-3 mt-2">
                    <label class="text-slate-800 font-bold text-sm">Price (Result)</label>
                    <span id="resultPrice" class="text-xl font-extrabold text-emerald-700">0.10</span>
                </div>
            </div>
        </div>

        <!-- Print Action -->
        <button onclick="window.print()" class="no-print w-full bg-slate-800 hover:bg-slate-900 text-white font-medium py-2.5 px-4 rounded-xl transition-colors duration-150 shadow-sm flex items-center justify-center gap-2">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 17h2a2 2 0 002-2v-4a2 2 0 00-2-2H5a2 2 0 00-2 2v4a2 2 0 002 2h2m2 4h6a2 2 0 002-2v-4a2 2 0 00-2-2H9a2 2 0 00-2 2v4a2 2 0 002 2zm8-12V5a2 2 0 00-2-2H9a2 2 0 00-2 2v4h10z" />
            </svg>
            Save & Print
        </button>
    </div>

    <script>
        function calculatePrice() {
            const c2 = document.getElementById('vendorUnit').value;
            const c3 = parseFloat(document.getElementById('vendorQty').value) || 0;
            const c4 = parseFloat(document.getElementById('vendorPrice').value) || 0;
            
            const c7 = document.getElementById('customerUnit').value;
            const c8 = parseFloat(document.getElementById('customerQty').value) || 0;
            
            let result = 0;

            if (c3 === 0) {
                document.getElementById('resultPrice').innerText = "0.00";
                return;
            }

            // Exactly implements formula: =IF(AND(C2="g",C7="Kg"),(C4/C3)*1000*C8,IF(AND(C2="g",C7="g"),(C4/C3)*100*(C8/100),IF(AND(C2="Kg",C7="g"),(C4/1000)*C8,IF(AND(C2="Kg",C7="Kg"),C4*C8,"Error"))))
            if (c2 === "g" && c7 === "Kg") {
                result = (c4 / c3) * 1000 * c8;
            } else if (c2 === "g" && c7 === "g") {
                result = (c4 / c3) * 100 * (c8 / 100);
            } else if (c2 === "Kg" && c7 === "g") {
                result = (c4 / 1000) * c8;
            } else if (c2 === "Kg" && c7 === "Kg") {
                result = c4 * c8;
            }

            document.getElementById('resultPrice').innerText = result.toFixed(2);
        }

        // Initialize view matching spreadsheet default metrics
        calculatePrice();
    </script>
</body>
</html>
