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
            animation: slideDown 0
