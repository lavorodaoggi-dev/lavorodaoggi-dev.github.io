<!DOCTYPE html>
<html lang="fr" dir="ltr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>Facture Bot - Hmente DATA</title>
<style>
@page { size: A4; margin: 0; }
* { margin: 0; padding: 0; box-sizing: border-box; }

body {
    font-family: Arial, Helvetica, sans-serif;
    background: #eef2f7;
    color: #000;
    -webkit-font-smoothing: antialiased;
}

.container { max-width: 900px; margin: 0 auto; padding: 15px; }

.form-section {
    background: #fff;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.08);
    margin-bottom: 15px;
}

.form-section h2 {
    color: #1a5276;
    margin-bottom: 12px;
    font-size: 16px;
    border-bottom: 2px solid #3498db;
    padding-bottom: 6px;
}

.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; }

@media (max-width: 600px) {
    .grid-2, .grid-3 { grid-template-columns: 1fr; }
    .container { padding: 10px; }
    .form-section { padding: 15px; }
}

.form-group { margin-bottom: 10px; }
.form-group label {
    display: block;
    font-weight: 600;
    color: #2c3e50;
    margin-bottom: 4px;
    font-size: 13px;
}
.form-group input, .form-group textarea {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid #d5dbdb;
    border-radius: 8px;
    font-family: Arial, sans-serif;
    font-size: 14px;
    -webkit-appearance: none;
}
.form-group input:focus { outline: none; border-color: #3498db; }
.form-group textarea { resize: vertical; min-height: 50px; }

.items-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; min-width: 400px; }
.items-table th {
    background: #d4e6f1;
    color: #1a5276;
    padding: 10px;
    font-weight: 700;
    font-size: 13px;
    border: 1px solid #aed6f1;
}
.items-table td {
    padding: 8px;
    border: 1px solid #d5dbdb;
    font-size: 13px;
}
.items-table td input {
    width: 100%;
    padding: 8px;
    border: 1px solid #d5dbdb;
    border-radius: 6px;
    font-family: Arial, sans-serif;
    font-size: 13px;
}

.btn {
    padding: 10px 18px;
    border: none;
    border-radius: 8px;
    font-family: Arial, sans-serif;
    font-weight: 700;
    cursor: pointer;
    font-size: 14px;
    touch-action: manipulation;
}
.btn:active { opacity: 0.8; }
.btn-add { background: #27ae60; color: #fff; width: auto; display: inline-block; padding: 8px 14px; font-size: 13px; }
.btn-remove { background: #e74c3c; color: #fff; width: auto; padding: 5px 10px; font-size: 12px; }
.btn-generate { background: #2980b9; color: #fff; font-size: 16px; padding: 14px; display: block; width: 100%; margin-bottom: 15px; }
.btn-print { background: #1a5276; color: #fff; font-size: 16px; padding: 14px; display: block; width: 100%; margin-bottom: 15px; }

.summary-box {
    background: #eaf2f8;
    padding: 12px;
    border-radius: 8px;
    margin: 10px 0;
    text-align: center;
    font-size: 14px;
    font-weight: 700;
    color: #1a5276;
}

.invoice-preview {
    background: #fff;
    width: 210mm;
    min-height: 297mm;
    margin: 0 auto;
    position: relative;
    overflow: hidden;
    box-shadow: 0 0 30px rgba(0,0,0,0.15);
}

.top-ids {
    text-align: center;
    font-size: 10px;
    color: #333;
    padding: 8px 40px 4px;
    line-height: 1.6;
}
.top-ids span { display: inline-block; margin: 0 3px; }

.invoice-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    padding: 10px 50px 8px;
    border-bottom: 2px solid #1a5276;
}

.logo-left { display: flex; align-items: center; gap: 6px; }
.logo-script {
    font-family: 'Brush Script MT', 'Segoe Script', cursive;
    font-size: 36px;
    color: #1a5276;
    font-style: italic;
    line-height: 0.9;
}
.logo-sub-row {
    display: flex;
    align-items: center;
    margin-top: 2px;
    gap: 4px;
}
.logo-sub {
    font-size: 13px;
    font-weight: bold;
    color: #2c3e50;
    letter-spacing: 5px;
}
.logo-icon {
    width: 26px;
    height: 26px;
    border: 2px solid #1a5276;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}
.logo-icon svg { width: 14px; height: 14px; }

.company-right { text-align: right; margin-top: 8px; }
.company-name { font-size: 20px; color: #2e86ab; font-weight: 400; }
.company-bank { font-size: 10px; color: #666; margin-top: 2px; }

.invoice-body { padding: 20px 50px 15px; position: relative; z-index: 2; }

.invoice-meta {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    font-size: 13px;
    line-height: 1.7;
    color: #000;
}
.meta-left { text-align: left; }
.meta-left div { margin-bottom: 2px; }
.meta-right { text-align: right; }
.meta-right .client-name { font-size: 14px; color: #000; margin-bottom: 3px; }
.meta-right .client-ice { font-size: 12px; color: #333; }

.invoice-table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 0;
}
.invoice-table th {
    background: #d4e6f1;
    color: #1a5276;
    font-weight: bold;
    font-size: 13px;
    padding: 10px 8px;
    border: 1px solid #a0c4e0;
    text-align: center;
}
.invoice-table td {
    padding: 14px 8px;
    border: 1px solid #b0c4de;
    font-size: 13px;
    text-align: center;
    color: #000;
}
.invoice-table td:nth-child(2) { text-align: left; padding-left: 20px; }
.invoice-table tbody tr { background: #e8f0f8; }

.totals-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 0;
}
.totals-table td {
    padding: 10px 15px;
    border: 1px solid #b0c4de;
    font-size: 13px;
    font-weight: bold;
}
.totals-table td:first-child {
    background: #f0f5fa;
    color: #1a5276;
    width: 75%;
    text-align: left;
}
.totals-table td:last-child { text-align: center; width: 25%; color: #000; }
.totals-table tr:nth-child(2) td:first-child { background: #e8f0f8; }
.totals-table tr:nth-child(2) td:last-child { background: #e8f0f8; }
.totals-table tr:last-child td:first-child { background: #d4e6f1; color: #1a5276; }
.totals-table tr:last-child td:last-child { background: #d4e6f1; color: #1a5276; }

.invoice-footer {
    display: flex;
    justify-content: space-around;
    padding: 30px 50px 10px;
    text-align: center;
}
.footer-item { flex: 1; }
.footer-icon-box {
    width: 30px;
    height: 30px;
    background: #1a5276;
    margin: 0 auto 6px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 3px;
}
.footer-icon-box svg { width: 14px; height: 14px; fill: #fff; }
.footer-text { font-size: 10px; color: #333; line-height: 1.4; }

.bottom-bar {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 50px;
    display: flex;
}
.bottom-left { width: 52%; background: #2c3e50; clip-path: polygon(0 0, 100% 0, 95% 100%, 0 100%); position: relative; z-index: 2; }
.bottom-mid1 { width: 6%; background: #2980b9; transform: skewX(-20deg); margin-left: -12px; position: relative; z-index: 1; }
.bottom-mid2 { width: 6%; background: #1a5276; transform: skewX(-20deg); margin-left: -6px; position: relative; z-index: 1; }
.bottom-right { flex: 1; background: #154360; margin-left: -6px; position: relative; z-index: 2; }

.hidden { display: none !important; }

@media print {
    body { background: #fff; padding: 0; }
    .form-section, .btn-generate, .btn-print { display: none !important; }
    .invoice-preview {
        box-shadow: none;
        margin: 0;
        width: 100%;
    }
}

@media screen {
    .invoice-preview { box-shadow: 0 0 30px rgba(0,0,0,0.15); }
}
</style>
</head>
<body>

<div class="container">

<div class="form-section">
    <h2>📝 معلومات الشركة</h2>
    <div class="grid-3">
        <div class="form-group"><label>اسم الشركة</label><input id="sellerName" value="HMENTE DATA SARLAU"></div>
        <div class="form-group"><label>اللوجو (نص)</label><input id="logoText" value="Hmente"></div>
        <div class="form-group"><label>اللوجو (فرعي)</label><input id="logoSub" value="DATA"></div>
        <div class="form-group"><label>العنوان</label><input id="sellerAddress" value="77 RUE MOHAMED SMIHA CASABLANCA"></div>
        <div class="form-group"><label>الهاتف</label><input id="sellerPhone" value="+212 6 61 51 52 28"></div>
        <div class="form-group"><label>Email</label><input id="sellerEmail" value="hmentedata@gmail.com"></div>
        <div class="form-group"><label>RC</label><input id="sellerRC" value="715947"></div>
        <div class="form-group"><label>ICE</label><input id="sellerICE" value="003907357000016"></div>
        <div class="form-group"><label>PATENTE</label><input id="sellerPatente" value="32109886"></div>
        <div class="form-group"><label>IF</label><input id="sellerIF" value="71837880"></div>
        <div class="form-group"><label>CNSS</label><input id="sellerCNSS" value="6736395"></div>
        <div class="form-group"><label>RIB</label><input id="sellerRIB" value="007 780 0000102000005425 59"></div>
        <div class="form-group" style="grid-column: span 3;"><label>البنك</label><input id="sellerBank" value="CENTE D'AFFAIRE ATTIJARI WAFABANK"></div>
    </div>
</div>

<div class="form-section">
    <h2>👤 معلومات الزبون</h2>
    <div class="grid-2">
        <div class="form-group"><label>اسم الزبون</label><input id="buyerName" value="IMPRIMERIE MODERNE CASABLANCA"></div>
        <div class="form-group"><label>ICE</label><input id="buyerIce" value="0000838020000065"></div>
    </div>
</div>

<div class="form-section">
    <h2>📅 تفاصيل الفاتورة</h2>
    <div class="grid-3">
        <div class="form-group"><label>رقم الفاتورة</label><input id="invoiceNum" value="0501/26"></div>
        <div class="form-group"><label>التاريخ</label><input type="date" id="invoiceDate" value="2026-05-15"></div>
        <div class="form-group"><label>المكان</label><input id="invoicePlace" value="CASABLANCA"></div>
        <div class="form-group"><label>TVA %</label><input type="number" id="tvaRate" value="20" min="0" max="100"></div>
    </div>
</div>

<div class="form-section">
    <h2>🛒 البنود</h2>
    <div style="overflow-x:auto;">
        <table class="items-table" id="itemsTable">
            <tr><th>Quantité</th><th>Designation</th><th>P.U HT</th><th>Total HT</th><th></th></tr>
            <tr>
                <td><input type="number" class="qty" value="4" min="1" style="width:60px; text-align:center" onchange="calc()"></td>
                <td><input type="text" class="desc" value="HP 222A (W2220A)"></td>
                <td><input type="number" class="price" value="862.50" step="0.01" style="width:100px" onchange="calc()"></td>
                <td class="line-total" style="font-weight:700; text-align:center">0.00</td>
                <td><button class="btn btn-remove" onclick="removeRow(this)">×</button></td>
            </tr>
        </table>
    </div>
    <button class="btn btn-add" onclick="addRow()">➕ إضافة بند</button>
    <div class="summary-box" id="summaryBox">HT: 0.00 | TVA: 0.00 | TTC: 0.00</div>
</div>

<button class="btn btn-generate" onclick="generate()">🖨️ عرض الفاتورة</button>

<div id="invoicePreview" class="invoice-preview hidden">

    <div class="top-ids" id="previewTopIds">
        <span>RC: 715947</span> | <span>ICE : 003907357000016</span> | <span>PATENTE : 32109886</span> |
        <span>IF : 71837880</span> | <span>CNSS : 6736395</span> | <span>RIB : 007 780 0000102000005425 59</span>
    </div>

    <div class="invoice-header">
        <div class="logo-left">
            <div>
                <div class="logo-script" id="previewLogoText">Hmente</div>
                <div class="logo-sub-row">
                    <div class="logo-sub" id="previewLogoSub">DATA</div>
                    <div class="logo-icon">
                        <svg viewBox="0 0 24 24" fill="none" stroke="#1a5276" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/></svg>
                    </div>
                </div>
            </div>
        </div>
        <div class="company-right">
            <div class="company-name" id="previewCompanyName">HMENTE DATA SARLAU</div>
            <div class="company-bank" id="previewCompanyBank">CENTE D'AFFAIRE ATTIJARI WAFABANK</div>
        </div>
    </div>

    <div class="invoice-body">
        <div class="invoice-meta">
            <div class="meta-left">
                <div id="previewDatePlace">A CASABLANCA Le 15/05/2026</div>
                <div id="previewInvoiceNum">FACTURE N° : 0501/26</div>
            </div>
            <div class="meta-right">
                <div class="client-name" id="previewBuyerName">IMPRIMERIE MODERNE CASABLANCA</div>
                <div class="client-ice" id="previewBuyerIce">ICE: 0000838020000065</div>
            </div>
        </div>

        <table class="invoice-table">
            <thead>
                <tr>
                    <th>Quantité</th>
                    <th>Designation</th>
                    <th>P.U HT ( MAD)</th>
                    <th>Total HT ( MAD)</th>
                </tr>
            </thead>
            <tbody id="previewItemsBody"></tbody>
        </table>

        <table class="totals-table">
            <tr><td>TOTAL HT (MAD)</td><td id="previewTotalHT">0.00</td></tr>
            <tr><td>TVA <span id="previewTvaRate">20</span> % (MAD)</td><td id="previewTVA">0.00</td></tr>
            <tr><td>TOTAL TTC (MAD)</td><td id="previewTotalTTC">0.00</td></tr>
        </table>
    </div>

    <div class="invoice-footer">
        <div class="footer-item">
            <div class="footer-icon-box">
                <svg viewBox="0 0 24 24"><path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7z"/></svg>
            </div>
            <div class="footer-text" id="previewFooterAddress">77 RUE MOHAMED SMIHA<br>ETG 10 N°57 CASABLANCA</div>
        </div>
        <div class="footer-item">
            <div class="footer-icon-box">
                <svg viewBox="0 0 24 24"><path d="M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z"/></svg>
            </div>
            <div class="footer-text" id="previewFooterPhone">+212 6 61 51 52 28</div>
        </div>
        <div class="footer-item">
            <div class="footer-icon-box">
                <svg viewBox="0 0 24 24"><path d="M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>
            </div>
            <div class="footer-text" id="previewFooterEmail">hmentedata@gmail.com</div>
        </div>
    </div>

    <div class="bottom-bar">
        <div class="bottom-left"></div>
        <div class="bottom-mid1"></div>
        <div class="bottom-mid2"></div>
        <div class="bottom-right"></div>
    </div>

</div>

<button class="btn btn-print hidden" id="printBtn" onclick="window.print()">🖨️ طباعة / حفظ PDF</button>

</div>

<script>
function addRow() {
    var tbody = document.getElementById('itemsTable');
    var row = document.createElement('tr');
    row.innerHTML = '<td><input type="number" class="qty" value="1" min="1" style="width:60px; text-align:center" onchange="calc()"></td>' +
        '<td><input type="text" class="desc" placeholder="اسم المنتج"></td>' +
        '<td><input type="number" class="price" value="0" step="0.01" style="width:100px" onchange="calc()"></td>' +
        '<td class="line-total" style="font-weight:700; text-align:center">0.00</td>' +
        '<td><button class="btn btn-remove" onclick="removeRow(this)">×</button></td>';
    tbody.appendChild(row);
    calc();
}

function removeRow(btn) {
    var rows = document.querySelectorAll('#itemsTable tr');
    if (rows.length > 2) {
        btn.closest('tr').remove();
        calc();
    } else {
        alert('لا يمكن حذف آخر بند!');
    }
}

function calc() {
    var rows = document.querySelectorAll('#itemsTable tr');
    var totalHT = 0;
    for (var i = 1; i < rows.length; i++) {
        var q = parseFloat(rows[i].querySelector('.qty').value) || 0;
        var p = parseFloat(rows[i].querySelector('.price').value) || 0;
        var l = q * p;
        rows[i].querySelector('.line-total').textContent = l.toFixed(2);
        totalHT += l;
    }
    var v = parseFloat(document.getElementById('tvaRate').value) || 0;
    var tv = totalHT * (v / 100);
    document.getElementById('summaryBox').textContent = 'HT: ' + totalHT.toFixed(2) + ' | TVA (' + v + '%): ' + tv.toFixed(2) + ' | TTC: ' + (totalHT + tv).toFixed(2);
    return { totalHT: totalHT, tva: tv, ttc: totalHT + tv, rate: v };
}

function generate() {
    var totals = calc();

    document.getElementById('previewTopIds').innerHTML =
        '<span>RC: ' + document.getElementById('sellerRC').value + '</span> | ' +
        '<span>ICE : ' + document.getElementById('sellerICE').value + '</span> | ' +
        '<span>PATENTE : ' + document.getElementById('sellerPatente').value + '</span> | ' +
        '<span>IF : ' + document.getElementById('sellerIF').value + '</span> | ' +
        '<span>CNSS : ' + document.getElementById('sellerCNSS').value + '</span> | ' +
        '<span>RIB : ' + document.getElementById('sellerRIB').value + '</span>';

    document.getElementById('previewLogoText').textContent = document.getElementById('logoText').value;
    document.getElementById('previewLogoSub').textContent = document.getElementById('logoSub').value;
    document.getElementById('previewCompanyName').textContent = document.getElementById('sellerName').value;
    document.getElementById('previewCompanyBank').textContent = document.getElementById('sellerBank').value;

    var d = document.getElementById('invoiceDate').value;
    var place = document.getElementById('invoicePlace').value;
    var fd = d ? new Date(d).toLocaleDateString('fr-FR') : '';
    document.getElementById('previewDatePlace').textContent = 'A ' + place + ' Le ' + fd;
    document.getElementById('previewInvoiceNum').textContent = 'FACTURE N° : ' + document.getElementById('invoiceNum').value;
    document.getElementById('previewBuyerName').textContent = document.getElementById('buyerName').value;
    var ice = document.getElementById('buyerIce').value;
    document.getElementById('previewBuyerIce').textContent = ice ? 'ICE: ' + ice : '';

    var pb = document.getElementById('previewItemsBody');
    pb.innerHTML = '';
    var rows = document.querySelectorAll('#itemsTable tr');
    for (var i = 1; i < rows.length; i++) {
        var q = rows[i].querySelector('.qty').value;
        var desc = rows[i].querySelector('.desc').value;
        var p = rows[i].querySelector('.price').value;
        var l = rows[i].querySelector('.line-total').textContent;
        var tr = document.createElement('tr');
        tr.innerHTML = '<td>' + q + '</td><td style="text-align:left; padding-left:20px;">' + desc + '</td><td>' + parseFloat(p).toFixed(2) + '</td><td>' + l + '</td>';
        pb.appendChild(tr);
    }

    document.getElementById('previewTotalHT').textContent = totals.totalHT.toFixed(2);
    document.getElementById('previewTvaRate').textContent = totals.rate;
    document.getElementById('previewTVA').textContent = totals.tva.toFixed(2);
    document.getElementById('previewTotalTTC').textContent = totals.ttc.toFixed(2);

    document.getElementById('previewFooterAddress').innerHTML = document.getElementById('sellerAddress').value.replace(/, /g, '<br>');
    document.getElementById('previewFooterPhone').textContent = document.getElementById('sellerPhone').value;
    document.getElementById('previewFooterEmail').textContent = document.getElementById('sellerEmail').value;

    document.getElementById('invoicePreview').classList.remove('hidden');
    document.getElementById('printBtn').classList.remove('hidden');
    document.getElementById('invoicePreview').scrollIntoView({ behavior: 'smooth' });
}

calc();
</script>

</body>
</html>

