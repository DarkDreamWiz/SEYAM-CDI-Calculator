<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Luffy's iOS Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 20px;
        }

        .calculator {
            width: 320px;
            background: #000;
            border-radius: 30px;
            padding: 20px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
        }

        .display {
            height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: flex-end;
            padding: 20px;
            color: white;
            margin-bottom: 10px;
        }

        .previous-operand {
            font-size: 20px;
            color: #a5a5a5;
            margin-bottom: 5px;
            min-height: 24px;
        }

        .current-operand {
            font-size: 48px;
            font-weight: 300;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
        }

        button {
            border: none;
            border-radius: 50%;
            height: 70px;
            font-size: 28px;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        button:active {
            transform: scale(0.95);
            opacity: 0.8;
        }

        .gray {
            background: #a5a5a5;
            color: black;
        }

        .gray:hover {
            background: #d4d4d4;
        }

        .dark-gray {
            background: #333;
            color: white;
        }

        .dark-gray:hover {
            background: #555;
        }

        .orange {
            background: #f1a33c;
            color: white;
        }

        .orange:hover {
            background: #f1b33c;
        }

        .zero {
            grid-column: span 2;
            border-radius: 35px;
            text-align: left;
            padding-left: 30px;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display">
            <div class="previous-operand"></div>
            <div class="current-operand">0</div>
        </div>
        <div class="buttons">
            <button class="gray" onclick="clearAll()">C</button>
            <button class="gray" onclick="toggleSign()">±</button>
            <button class="gray" onclick="percent()">%</button>
            <button class="orange" onclick="operate('/')">÷</button>
            
            <button class="dark-gray" onclick="appendNumber('7')">7</button>
            <button class="dark-gray" onclick="appendNumber('8')">8</button>
            <button class="dark-gray" onclick="appendNumber('9')">9</button>
            <button class="orange" onclick="operate('*')">×</button>
            
            <button class="dark-gray" onclick="appendNumber('4')">4</button>
            <button class="dark-gray" onclick="appendNumber('5')">5</button>
            <button class="dark-gray" onclick="appendNumber('6')">6</button>
            <button class="orange" onclick="operate('-')">−</button>
            
            <button class="dark-gray" onclick="appendNumber('1')">1</button>
            <button class="dark-gray" onclick="appendNumber('2')">2</button>
            <button class="dark-gray" onclick="appendNumber('3')">3</button>
            <button class="orange" onclick="operate('+')">+</button>
            
            <button class="dark-gray zero" onclick="appendNumber('0')">0</button>
            <button class="dark-gray" onclick="appendNumber('.')">.</button>
            <button class="orange" onclick="calculate()">=</button>
        </div>
    </div>

    <script>
        let currentOperand = '0';
        let previousOperand = '';
        let operation = null;
        let resetScreen = false;

        const currentDisplay = document.querySelector('.current-operand');
        const previousDisplay = document.querySelector('.previous-operand');

        function updateDisplay() {
            currentDisplay.textContent = currentOperand;
            previousDisplay.textContent = previousOperand;
        }

        function appendNumber(number) {
            if (currentOperand === '0' || resetScreen) {
                currentOperand = number;
                resetScreen = false;
            } else {
                currentOperand += number;
            }
            updateDisplay();
        }

        function operate(op) {
            if (operation !== null) calculate();
            operation = op;
            previousOperand = `${currentOperand} ${op}`;
            currentOperand = '0';
            updateDisplay();
        }

        function calculate() {
            let computation;
            const prev = parseFloat(previousOperand);
            const current = parseFloat(currentOperand);
            
            if (isNaN(prev) || isNaN(current)) return;

            switch (operation) {
                case '+':
                    computation = prev + current;
                    break;
                case '-':
                    computation = prev - current;
                    break;
                case '*':
                    computation = prev * current;
                    break;
                case '/':
                    computation = prev / current;
                    break;
                default:
                    return;
            }

            currentOperand = computation.toString();
            operation = null;
            previousOperand = '';
            resetScreen = true;
            updateDisplay();
        }

        function clearAll() {
            // Show SEYAM CDI instead of 0
            currentDisplay.textContent = "SEYAM CDI";
            currentOperand = '0';
            previousOperand = '';
            operation = null;
            
            // Return to 0 after 2 seconds
            setTimeout(() => {
                if (currentOperand === '0') {
                    currentDisplay.textContent = currentOperand;
                }
            }, 2000);
        }

        function toggleSign() {
            currentOperand = (parseFloat(currentOperand) * -1).toString();
            updateDisplay();
        }

        function percent() {
            currentOperand = (parseFloat(currentOperand) / 100).toString();
            updateDisplay();
        }

        // Keyboard support
        document.addEventListener('keydown', (event) => {
            if (event.key >= '0' && event.key <= '9') appendNumber(event.key);
            if (event.key === '.') appendNumber('.');
            if (event.key === '+') operate('+');
            if (event.key === '-') operate('-');
            if (event.key === '*') operate('*');
            if (event.key === '/') operate('/');
            if (event.key === 'Enter' || event.key === '=') calculate();
            if (event.key === 'Escape') clearAll();
            if (event.key === '%') percent();
        });
    </script>
</body>
</html>
