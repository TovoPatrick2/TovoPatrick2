<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulaire de Recueil de Données</title>
    <style>
        /* CSS comme précédemment */
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .form-container {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 600px;
        }
        h1 {
            text-align: center;
            margin-bottom: 20px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
        }
        input[type="text"],
        textarea {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        textarea {
            resize: vertical;
            height: 100px;
        }
        button {
            background-color: #007bff;
            color: #fff;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
        }
        button:hover {
            background-color: #0056b3;
        }
        .error {
            color: red;
            font-size: 0.875em;
        }
    </style>
</head>
<body>
    <div class="form-container">
        <h1>Formulaire de Recueil de Données</h1>
        <form id="dataForm">
            <div class="form-group">
                <label for="sigle">Sigle:</label>
                <input type="text" id="sigle" name="sigle" required>
                <div id="sigleError" class="error"></div>
            </div>
            <div class="form-group">
                <label for="projet">Projet:</label>
                <input type="text" id="projet" name="projet" required>
                <div id="projetError" class="error"></div>
            </div>
            <div class="form-group">
                <label for="production">Production:</label>
                <input type="text" id="production" name="production" required>
                <div id="productionError" class="error"></div>
            </div>
            <div class="form-group">
                <label for="faute">Faute:</label>
                <textarea id="faute" name="faute"></textarea>
                <div id="fauteError" class="error"></div>
            </div>
            <button type="submit">Soumettre</button>
        </form>
    </div>

    <script>
        document.getElementById('dataForm').addEventListener('submit', function(event) {
            event.preventDefault();
            
            // Clear previous errors
            const errorElements = document.querySelectorAll('.error');
            errorElements.forEach(element => element.textContent = '');

            // Form fields
            const sigle = document.getElementById('sigle').value.trim();
            const projet = document.getElementById('projet').value.trim();
            const production = document.getElementById('production').value.trim();
            const faute = document.getElementById('faute').value.trim();
            
            let isValid = true;

            // Validate fields
            if (sigle === '') {
                document.getElementById('sigleError').textContent = 'Le sigle est requis.';
                isValid = false;
            }
            if (projet === '') {
                document.getElementById('projetError').textContent = 'Le projet est requis.';
                isValid = false;
            }
            if (production === '') {
                document.getElementById('productionError').textContent = 'La production est requise.';
                isValid = false;
            }

            // If valid, proceed with form submission
            if (isValid) {
                // Prepare data for POST request
                const formData = {
                    sigle: sigle,
                    projet: projet,
                    production: production,
                    faute: faute
                };

                // Send POST request
                fetch('https://script.google.com/macros/s/AKfycbxm77y6RtQfmxdiMyAdwtUfKOYAE13Mj9ekw9sb3H74y_8Cw_xMuBVY6gwn1W1fU0w/exec', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(formData)
                })
                .then(response => response.text())
                .then(result => {
                    alert('Formulaire soumis avec succès !');
                    // Optionally reset the form
                    document.getElementById('dataForm').reset();
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            }
        });
    </script>
</body>
</html>
