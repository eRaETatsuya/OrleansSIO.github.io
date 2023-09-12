<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Convertisseur Décimal vers Binaire</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }

        input {
            padding: 5px;
            width: 200px;
            font-size: 16px;
        }

        button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 5px 10px;
            font-size: 16px;
            cursor: pointer;
        }



        p {
            margin-top: 20px;
            font-size: 18px;
        }

        span {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>Convertisseur Décimal vers Binaire</h1>
    
    <input type="text" id="decimalInput" placeholder="Entrez un nombre décimal">
    <button onclick="convertirDecimalEnBinaire()">Convertir</button>
    
    <p>Résultat en binaire : <span id="resultatBinaire"></span></p>

    <script>
        function convertirDecimalEnBinaire() {
            const decimalInput = document.getElementById('decimalInput').value;
            const decimal = parseInt(decimalInput);

            if (isNaN(decimal)) {
                alert('Veuillez entrer un nombre décimal valide.');
                return;
            }

            let binaire = decimal.toString(2); // Conversion en binaire

            // Formatage du résultat en ajoutant un espace tous les 4 caractères
            const resultatFormate = binaire.replace(/(\d{4})(?=\d)/g, '$1 ');

            document.getElementById('resultatBinaire').textContent = resultatFormate;
        }
    </script>
</body>
</html>
| Décimal | Hexadécimal | Binaire | Décimal |
| ------- | ----------- | ------- | ------- |
| 0       | 0           | 0000    | 0       |
| 1       | 1           | 0001    | 1       |
| 2       | 2           | 0010    | 2       |
| 3       | 3           | 0011    | 3       |
| 4       | 4           | 0100    | 4       |
| 5       | 5           | 0101    | 5       |
| 6       | 6           | 0110    | 6       |
| 7       | 7           | 0111    | 7       |
| 8       | 8           | 1000    | 8       |
| 9       | 9           | 1001    | 9       |
| 10      | A           | 1010    | 10      |
| 11      | B           | 1011    | 11      |
| 12      | C           | 1100    | 12      |
| 13      | D           | 1101    | 13      |
| 14      | E           | 1110    | 14      |
| 15      | F           | 1111    | 15      |

