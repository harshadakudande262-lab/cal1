<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: #eaf0f8;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .calculator {
            background: #222;
            padding: 20px;
            border-radius: 15px;
            width: 280px;
        }

        #display {
            width: 100%;
            height: 60px;
            box-sizing: border-box;
            margin-bottom: 15px;
            font-size: 28px;
            text-align: right;
            padding: 10px;
            border: none;
            border-radius: 8px;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
        }

        button {
            height: 55px;
            font-size: 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            background: #444;
            color: white;
        }

        button:hover {
            background: #666;
        }

        .operator {
            background: #ff9500;
        }

        .clear {
            background: #d94343;
        }
    </style>
</head>

<body>

<div class="calculator">
    <input type="text" id="display" value="0" readonly>

    <div class="buttons">
        <button class="clear" onclick="clearDisplay()">AC</button>
        <button onclick="deleteLast()">DEL</button>
        <button onclick="append('%')">%</button>
        <button class="operator" onclick="append('/')">÷</button>

        <button onclick="append('7')">7</button>
        <button onclick="append('8')">8</button>
        <button onclick="append('9')">9</button>
        <button class="operator" onclick="append('*')">×</button>

        <button onclick="append('4')">4</button>
        <button onclick="append('5')">5</button>
        <button onclick="append('6')">6</button>
        <button class="operator" onclick="append('-')">−</button>

        <button onclick="append('1')">1</button>
        <button onclick="append('2')">2</button>
        <button onclick="append('3')">3</button>
        <button class="operator" onclick="append('+')">+</button>

        <button onclick="append('0')">0</button>
        <button onclick="append('.')">.</button>
        <button onclick="calculate()" style="grid-column: span 2; background: #2196F3;">=</button>
    </div>
</div>

<script>
    const display = document.getElementById("display");

    function append(value) {
        if (display.value === "Error") {
            display.value = "0";
        }

        if (value === "%") {
            if (display.value !== "0") {
                display.value = String(Number(display.value) / 100);
            }
            return;
        }

        if (display.value === "0" && !"+-*/.".includes(value)) {
            display.value = value;
        } else {
            display.value += value;
        }
    }

    function clearDisplay() {
        display.value = "0";
    }

    function deleteLast() {
        display.value = display.value.slice(0, -1) || "0";
    }

    function calculate() {
        try {
            const expression = display.value;

            if (!/^[0-9+*/.() -]+$/.test(expression)) {
                throw new Error("Invalid input");
            }

            const result = Function(
                '"use strict"; return (' + expression + ')'
            )();

            display.value = Number.isFinite(result)
                ? String(Number(result.toPrecision(10)))
                : "Error";
        } catch {
            display.value = "Error";
        }
    }
</script>

</body>
</html>
