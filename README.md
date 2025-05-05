<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>الیکٹریکل لوڈ کیلکولیٹر</title>
  <style>
    body {
      font-family: 'Noto Nastaliq Urdu', 'Segoe UI', sans-serif;
      background-color: #f0f0f5;
      padding: 20px;
      max-width: 600px;
      margin: auto;
      direction: rtl;
      text-align: right;
    }

    h2 {
      text-align: center;
      color: #2c3e50;
    }

    label {
      margin-top: 15px;
      display: block;
      font-weight: bold;
      color: #34495e;
    }

    input, select {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border-radius: 6px;
      border: 1px solid #ccc;
      box-sizing: border-box;
    }

    .result {
      background-color: #fff;
      border: 1px solid #ccc;
      padding: 15px;
      border-radius: 6px;
      margin-top: 20px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
    }

    .result strong {
      color: #16a085;
    }

    @media (max-width: 600px) {
      body {
        padding: 10px;
      }
    }
  </style>
</head>
<body>
  <h2>الیکٹریکل لوڈ کیلکولیٹر</h2>

  <label for="power">پاور (واٹ):</label>
  <input type="number" id="power" placeholder="مثلاً 3000">

  <label for="supplyType">پاور سپلائی کی قسم:</label>
  <select id="supplyType">
    <option value="single">سنگل فیز</option>
    <option value="three">تھری فیز</option>
    <option value="dc">ڈی سی</option>
  </select>

  <label for="voltage">وولٹیج (وی):</label>
  <input type="number" id="voltage" placeholder="اگر خالی چھوڑیں تو خودکار سیٹ ہوگا">

  <label for="pf">پاور فیکٹر:</label>
  <input type="number" id="pf" step="0.01" value="0.8">

  <label for="distance">فاصلہ (میٹر):</label>
  <input type="number" id="distance" placeholder="مثلاً 15">

  <div class="result" id="output">
    یہاں نتیجہ ظاہر ہوگا...
  </div>

  <script>
    const powerInput = document.getElementById("power");
    const supplyType = document.getElementById("supplyType");
    const voltageInput = document.getElementById("voltage");
    const pfInput = document.getElementById("pf");
    const distanceInput = document.getElementById("distance");
    const outputDiv = document.getElementById("output");

    function calculate() {
      const power = parseFloat(powerInput.value);
      const type = supplyType.value;
      let voltage = parseFloat(voltageInput.value);
      const pf = parseFloat(pfInput.value);
      const distance = parseFloat(distanceInput.value);

      if (isNaN(power) || isNaN(pf) || isNaN(distance)) {
        outputDiv.innerHTML = "براہ کرم تمام فیلڈز مکمل کریں۔";
        return;
      }

      if (!voltage || voltage === 0) {
        voltage = type === "single" ? 220 : type === "three" ? 400 : 220;
        voltageInput.value = voltage;
      }

      let current = 0;

      if (type === "single") {
        current = power / (voltage * pf);
      } else if (type === "three") {
        current = power / (Math.sqrt(3) * voltage * pf);
      } else {
        current = power / voltage;
      }

      current = Math.ceil(current * 100) / 100;

      const breakerOptions = [6, 10, 16, 20, 25, 32, 40, 50, 63, 80, 100];
      const breaker = breakerOptions.find(b => b >= current) || "بہت زیادہ کرنٹ";

      let cableSize = Math.ceil(current / 6);
      if (distance > 20) {
        cableSize = Math.ceil(cableSize * 1.2);
      }

      outputDiv.innerHTML = `
        <strong>کرنٹ:</strong> ${current} ایمپیئر<br>
        <strong>تجویز کردہ بریکر:</strong> ${breaker} ایمپیئر<br>
        <strong>تجویز کردہ کیبل سائز:</strong> ${cableSize} mm²<br>
        <hr>
        <em>تفصیل:</em><br>
        پاور: ${power} واٹ، وولٹیج: ${voltage}V، فاصلہ: ${distance}m<br>
        کرنٹ ${current}A کے لیے ${breaker}A بریکر اور ${cableSize}mm² کی کیبل موزوں ہے۔
      `;
    }

    powerInput.addEventListener("input", calculate);
    supplyType.addEventListener("change", calculate);
    voltageInput.addEventListener("input", calculate);
    pfInput.addEventListener("input", calculate);
    distanceInput.addEventListener("input", calculate);
  </script>
</body>
</html># Electric-Calculator-
