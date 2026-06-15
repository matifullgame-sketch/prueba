<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Tótem Petrizzio RVM</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Helvetica Neue', Arial, sans-serif;
        }
        body {
            background-color: #f4f4f6;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .totem-container {
            width: 380px;
            background-color: #1a1a1a;
            border-radius: 24px;
            padding: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
            border: 4px solid #333;
        }
        .screen {
            background-color: #ffffff;
            width: 100%;
            height: 480px;
            border-radius: 12px;
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
            color: #111;
        }
        .screen-header {
            background-color: #000000;
            color: #ffffff;
            padding: 15px;
            text-align: center;
            font-size: 14pt;
            font-weight: bold;
            letter-spacing: 2px;
        }
        .screen-content {
            padding: 20px;
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .welcome-title {
            font-size: 18pt;
            text-align: center;
            margin-top: 40px;
            color: #000;
            font-weight: 300;
        }
        .welcome-subtitle {
            font-size: 10pt;
            text-align: center;
            color: #666;
            margin-bottom: 40px;
        }
        .btn-primary {
            background-color: #000;
            color: #fff;
            border: none;
            padding: 14px;
            border-radius: 6px;
            font-size: 11pt;
            cursor: pointer;
            width: 100%;
            transition: background 0.2s;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: bold;
        }
        .btn-primary:hover {
            background-color: #333;
        }
        .eco-guide {
            background-color: #f0edf5;
            border-left: 4px solid #7d6bb3;
            padding: 12px;
            font-size: 9pt;
            color: #4a4263;
            border-radius: 0 6px 6px 0;
            line-height: 1.4;
        }
        .counter-box {
            background-color: #f8f9fa;
            border: 1px solid #e1e4e6;
            border-radius: 8px;
            padding: 12px;
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .counter-label {
            font-size: 10pt;
            font-weight: bold;
        }
        .counter-value {
            font-size: 14pt;
            font-weight: bold;
            color: #7d6bb3;
        }
        .btn-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 15px;
        }
        .btn-secondary {
            background-color: #fff;
            color: #000;
            border: 1px solid #000;
            padding: 10px;
            border-radius: 6px;
            font-size: 9pt;
            cursor: pointer;
            font-weight: bold;
        }
        .btn-secondary:hover {
            background-color: #f0f0f0;
        }
        .btn-disabled {
            background-color: #ccc !important;
            color: #777 !important;
            cursor: not-allowed !important;
            border: none !important;
        }
        .hardware-slots {
            margin-top: 15px;
            border-top: 2px dashed #444;
            padding-top: 15px;
            display: flex;
            justify-content: space-around;
        }
        .hw-label {
            color: #888;
            font-size: 8pt;
            text-align: center;
            text-transform: uppercase;
        }
        .hw-opening {
            width: 100px;
            height: 25px;
            background-color: #050505;
            border-radius: 12px;
            margin: 5px auto;
            border: 1px solid #333;
            box-shadow: inset 0 2px 5px rgba(0,0,0,0.8);
        }
        .hw-dispenser {
            width: 140px;
            height: 45px;
            background-color: #050505;
            border-radius: 6px;
            margin: 5px auto;
            position: relative;
            border: 1px solid #333;
        }
        .dispensed-item {
            width: 80%;
            height: 15px;
            background-color: #7d6bb3;
            color: white;
            font-size: 7pt;
            text-align: center;
            line-height: 15px;
            position: absolute;
            bottom: 5px;
            left: 10%;
            border-radius: 3px;
            animation: slideDown 0.5s ease-out;
            display: none;
            font-weight: bold;
        }
        @keyframes slideDown {
            0% { transform: translateY(-20px); opacity: 0; }
            100% { transform: translateY(0); opacity: 1; }
        }
    </style>
</head>
<body>

<div class="totem-container">
    <!-- PANTALLA TOUCH DE LA RVM -->
    <div class="screen">
        <div class="screen-header">PETRIZZIO ECO</div>
        
        <!-- PANTALLA 1: BIENVENIDA -->
        <div id="screen-welcome" class="screen-content">
            <div>
                <div class="welcome-title">REVIERTE EL CICLO</div>
                <div class="welcome-subtitle">Deposita tus residuos modulares y obtén tu recompensa al instante.</div>
            </div>
            <button class="btn-primary" onclick="changeScreen('deposit')">Comenzar</button>
            <div class="eco-guide">
                <strong>Eco-Guía Petrizzio:</strong> El envase principal trío resiste unas 4 recargas. Si notas desgaste en sus anclajes deslizantes, recíclalo hoy para crear un Objeto de Larga Vida.
            </div>
        </div>

        <!-- PANTALLA 2: DEPÓSITO ACTIVO -->
        <div id="screen-deposit" class="screen-content" style="display: none;">
            <div>
                <div style="font-size: 11pt; font-weight: bold; margin-bottom: 15px; text-align: center;">PANEL DE DEPÓSITO AUTÓNOMO</div>
                
                <div class="counter-box">
                    <span class="counter-label">Bandejas de Refill:</span>
                    <span id="count-bandejas" class="counter-value">0</span>
                </div>
                <div class="counter-box">
                    <span class="counter-label">Envases Principales:</span>
                    <span id="count-envases" class="counter-value">0</span>
                </div>
                
                <p style="font-size: 8pt; color: #666; text-align: center; margin: 10px 0;">
                    Simula la inserción de piezas con los botones de abajo.
                </p>
            </div>
            
            <button id="btn-confirmar" class="btn-primary btn-disabled" disabled onclick="changeScreen('canje')">Confirmar Depósito</button>
        </div>

        <!-- PANTALLA 3: SELECCIÓN DE RECOMPENSA -->
        <div id="screen-canje" class="screen-content" style="display: none;">
            <div>
                <div style="font-size: 11pt; font-weight: bold; margin-bottom: 12px; text-align: center;">SELECCIONA TU INCENTIVO</div>
                
                <div style="margin-bottom: 15px;">
                    <button id="btn-prize-refill" class="btn-primary btn-disabled" disabled style="margin-bottom: 8px;" onclick="dispense('Refill Cosmético')">
                        Canjear Refill Gratis<br><span style="font-size: 8pt; font-weight: normal;">(Requiere 2 Bandejas)</span>
                    </button>
                    
                    <button id="btn-prize-objeto" class="btn-primary btn-disabled" disabled onclick="dispense('Objeto Larga Vida')">
                        Canjear Objeto de Larga Vida<br><span style="font-size: 8pt; font-weight: normal;">(1 Envase o 4 Bandejas)</span>
                    </button>
                </div>
            </div>
            <button class="btn-secondary" style="width: 100%; border: none; color: #666;" onclick="resetAll()">Cancelar</button>
        </div>

        <!-- PANTALLA 4: DISPENSANDO -->
        <div id="screen-dispensing" class="screen-content" style="display: none; justify-content: center; align-items: center; text-align: center;">
            <div>
                <div style="font-size: 24pt; margin-bottom: 15px;">⬇️</div>
                <div style="font-size: 14pt; font-weight: bold; margin-bottom: 10px;" id="dispense-msg">¡PROCESANDO PREMIO!</div>
                <div style="font-size: 10pt; color: #27ae60; font-weight: bold; background-color: #e8f8f0; padding: 10px; border-radius: 6px;" id="retire-alert">
                    POR FAVOR, RETIRA TU PRODUCTO EN LA PARTE INFERIOR
                </div>
            </div>
        </div>
    </div>

    <!-- ELEMENTOS MECÁNICOS SIMULADOS DEL TÓTEM (HARDWARE EXTERNO) -->
    <div class="hardware-slots">
        <div>
            <div class="hw-label">Entrada Residuos</div>
            <div class="hw-opening"></div>
            <div style="display: flex; gap: 4px; justify-content: center;">
                <button class="btn-secondary" id="hw-in-bandeja" style="padding: 4px 6px; font-size: 7pt;" onclick="simulateInsert('bandeja')" disabled>+ Bandeja</button>
                <button class="btn-secondary" id="hw-in-envase" style="padding: 4px 6px; font-size: 7pt执行" onclick="simulateInsert('envase')" disabled>+ Envase</button>
            </div>
        </div>
        <div>
            <div class="hw-label">Compuerta de Salida</div>
            <div class="hw-dispenser">
                <div id="physical-product" class="dispensed-item">REFILL</div>
            </div>
            <div style="text-align: center;">
                <button class="btn-secondary" id="btn-collect" style="padding: 4px 8px; font-size: 7pt; display: none; background-color: #27ae60; color: white; border: none;" onclick="collectProduct()">[Tomar Producto]</button>
            </div>
        </div>
    </div>
</div>

<script>
    let bCount = 0;
    let eCount = 0;

    function changeScreen(screenName) {
        document.getElementById('screen-welcome').style.display = 'none';
        document.getElementById('screen-deposit').style.display = 'none';
        document.getElementById('screen-canje').style.display = 'none';
        document.getElementById('screen-dispensing').style.display = 'none';

        document.getElementById('hw-in-bandeja').disabled = true;
        document.getElementById('hw-in-envase').disabled = true;

        if(screenName === 'welcome') {
            document.getElementById('screen-welcome').style.display = 'flex';
        } else if(screenName === 'deposit') {
            document.getElementById('screen-deposit').style.display = 'flex';
            document.getElementById('hw-in-bandeja').disabled = false;
            document.getElementById('hw-in-envase').disabled = false;
        } else if(screenName === 'canje') {
            document.getElementById('screen-canje').style.display = 'flex';
            updatePrizeButtons();
        } else if(screenName === 'dispensing') {
            document.getElementById('screen-dispensing').style.display = 'flex';
        }
    }

    function simulateInsert(type) {
        if(type === 'bandeja') {
            bCount++;
            document.getElementById('count-bandejas').innerText = bCount;
        } else if(type === 'envase') {
            eCount++;
            document.getElementById('count-envases').innerText = eCount;
        }

        if(bCount > 0 || eCount > 0) {
            let btn = document.getElementById('btn-confirmar');
            btn.classList.remove('btn-disabled');
            btn.disabled = false;
        }
    }

    function updatePrizeButtons() {
        let btnRefill = document.getElementById('btn-prize-refill');
        let btnObjeto = document.getElementById('btn-prize-objeto');

        if(bCount >= 2) {
            btnRefill.classList.remove('btn-disabled');
            btnRefill.disabled = false;
        } else {
            btnRefill.classList.add('btn-disabled');
            btnRefill.disabled = true;
        }

        if(eCount >= 1 || bCount >= 4) {
            btnObjeto.classList.remove('btn-disabled');
            btnObjeto.disabled = false;
        } else {
            btnObjeto.classList.add('btn-disabled');
            btnObjeto.disabled = true;
        }
    }

    function dispense(prizeName) {
        changeScreen('dispensing');
        document.getElementById('dispense-msg').innerText = "DISPENSANDO: " + prizeName.toUpperCase();
        
        // Animación física del producto cayendo en la compuerta inferior
        let pProd = document.getElementById('physical-product');
        pProd.innerText = prizeName === 'Refill Cosmético' ? 'REFILL' : 'OBJETO ECO';
        pProd.style.display = 'block';
        
        document.getElementById('btn-collect').style.display = 'inline-block';
    }

    function collectProduct() {
        // Al retirar el producto se limpia y vuelve al inicio
        document.getElementById('physical-product').style.display = 'none';
        document.getElementById('btn-collect').style.display = 'none';
        resetAll();
    }

    function resetAll() {
        bCount = 0;
        eCount = 0;
        document.getElementById('count-bandejas').innerText = "0";
        document.getElementById('count-envases').innerText = "0";
        let btn = document.getElementById('btn-confirmar');
        btn.classList.add('btn-disabled');
        btn.disabled = true;
        changeScreen('welcome');
    }
</script>

</body>
</html>
