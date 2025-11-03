<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calcolatore Punteggi Coppa Italia</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #ffffff;
            color: #333;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }
        
        .logo-container {
            text-align: center;
            margin-bottom: 20px;
        }
        
        .logo {
            max-width: 200px;
            height: auto;
        }
        
        .container {
            background-color: #ffffff;
            width: 100%;
            max-width: 500px;
            padding: 30px;
            border: 1px solid #e0e0e0;
        }
        
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 25px;
            font-size: 24px;
            font-weight: 400;
            border-bottom: 1px solid #e0e0e0;
            padding-bottom: 15px;
        }
        
        .race-type {
            margin-bottom: 25px;
        }
        
        .race-type h2 {
            font-size: 16px;
            margin-bottom: 10px;
            color: #555;
            font-weight: 500;
        }
        
        select {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #e0e0e0;
            font-size: 16px;
            background-color: #fff;
            transition: border-color 0.3s;
            cursor: pointer;
        }
        
        select:focus {
            border-color: #4CAF50;
            outline: none;
        }
        
        .input-group {
            margin-bottom: 20px;
        }
        
        .input-group h2 {
            font-size: 16px;
            margin-bottom: 15px;
            color: #555;
            font-weight: 500;
        }
        
        .position-input {
            display: flex;
            gap: 15px;
            align-items: center;
        }
        
        .input-field {
            flex: 1;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
            color: #666;
            font-size: 14px;
        }
        
        input {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #e0e0e0;
            font-size: 16px;
            transition: border-color 0.3s;
        }
        
        input:focus {
            border-color: #4CAF50;
            outline: none;
        }
        
        button {
            background-color: #fff;
            color: #333;
            border: 1px solid #e0e0e0;
            padding: 15px;
            font-size: 16px;
            font-weight: 500;
            cursor: pointer;
            width: 100%;
            transition: all 0.3s;
            margin-top: 10px;
        }
        
        button:hover {
            background-color: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }
        
        .result {
            margin-top: 25px;
            padding: 20px;
            background-color: #f9f9f9;
            text-align: center;
            border-top: 1px solid #e0e0e0;
        }
        
        .result h2 {
            color: #555;
            margin-bottom: 10px;
            font-weight: 500;
            font-size: 16px;
        }
        
        .points {
            font-size: 36px;
            font-weight: 300;
            color: #333;
            margin: 10px 0;
        }
        
        .footer {
            margin-top: 30px;
            text-align: center;
            color: #999;
            font-size: 14px;
        }
        
        @media (max-width: 480px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 20px;
            }
            
            .position-input {
                flex-direction: column;
                gap: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="logo-container">
        <img src="https://www.fiso.it/_files/societa_files/0000_logo.jpg" alt="Logo Coppa Italia" class="logo">
    </div>
    
    <div class="container">
        <h1>Calcolatore Punteggi Coppa Italia</h1>
        
        <div class="race-type">
            <h2>Tipo di Gara</h2>
            <select id="race-type-select">
                <option value="normale">Normale</option>
                <option value="finale">Finale</option>
            </select>
        </div>
        
        <div class="input-group">
            <h2>Posizione in Classifica</h2>
            <div class="position-input">
                <div class="input-field">
                    <label for="position">Posizione</label>
                    <input type="number" id="position" min="1" placeholder="Inserisci la posizione">
                </div>
            </div>
        </div>
        
        <button id="calculate-btn">Calcola Punteggio</button>
        
        <div class="result" id="result" style="display: none;">
            <h2>Punteggio Ottenuto</h2>
            <div class="points" id="points">0</div>
        </div>
    </div>
    
    <div class="footer">
        App sviluppata per la Coppa Italia
    </div>

    <script>
        // Tabella punteggi Coppa Italia
        const pointsTable = {
            1: 20,
            2: 17,
            3: 14,
            4: 12,
            5: 11,
            6: 10,
            7: 9,
            8: 8,
            9: 7,
            10: 6,
            11: 5,
            12: 4,
            13: 3,
            14: 2,
            15: 1,
            16: 1,
            17: 1
        };

        // Calcolo del punteggio
        document.getElementById('calculate-btn').addEventListener('click', function() {
            const position = parseInt(document.getElementById('position').value);
            const raceType = document.getElementById('race-type-select').value;
            
            // Verifica che la posizione sia valida
            if (isNaN(position) || position < 1) {
                alert("Inserisci una posizione valida");
                return;
            }
            
            // Calcola il punteggio base
            let points;
            if (position <= 17) {
                points = pointsTable[position];
            } else {
                points = 1; // Per le posizioni oltre la 17
            }
            
            // Se è una finale, raddoppia il punteggio
            if (raceType === 'finale') {
                points *= 2;
            }
            
            // Mostra il risultato
            document.getElementById('points').textContent = points;
            document.getElementById('result').style.display = 'block';
        });

        // Calcola automaticamente quando si preme Enter
        document.getElementById('position').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                document.getElementById('calculate-btn').click();
            }
        });
    </script>
</body>
</html>
