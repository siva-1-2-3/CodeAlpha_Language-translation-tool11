# CodeAlpha_Language-translation-tool11
# Language Translation Tool – CodeAlpha Internship Task 1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Language Translation Tool</title>

    <!-- Google Font -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">

    <style>
        * {
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }

        body {
            margin: 0;
            height: 100vh;
            background: linear-gradient(135deg, #667eea, #764ba2);
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .card {
            background: #ffffff;
            width: 460px;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2);
        }

        h2 {
            text-align: center;
            margin-bottom: 25px;
            color: #333;
            font-weight: 600;
        }

        label {
            font-size: 13px;
            font-weight: 500;
            color: #555;
            margin-bottom: 5px;
            display: block;
        }

        textarea {
            width: 100%;
            height: 100px;
            padding: 12px;
            border-radius: 10px;
            border: 1px solid #ddd;
            resize: none;
            font-size: 14px;
            margin-bottom: 15px;
        }

        textarea:focus, select:focus {
            outline: none;
            border-color: #667eea;
        }

        .row {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }

        select {
            flex: 1;
            padding: 10px;
            border-radius: 10px;
            border: 1px solid #ddd;
            font-size: 14px;
        }

        .translate-btn {
            width: 100%;
            padding: 12px;
            border-radius: 12px;
            border: none;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            font-size: 15px;
            font-weight: 500;
            cursor: pointer;
            transition: transform 0.2s ease;
        }

        .translate-btn:hover {
            transform: scale(1.02);
        }

        .output {
            background: #f7f8fc;
        }

        .actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .actions button {
            flex: 1;
            padding: 10px;
            border-radius: 10px;
            border: none;
            font-size: 13px;
            cursor: pointer;
            color: white;
        }

        .copy-btn {
            background: #28a745;
        }

        .speak-btn {
            background: #6f42c1;
        }

        footer {
            text-align: center;
            font-size: 11px;
            color: #888;
            margin-top: 20px;
        }
    </style>
</head>
<body>

<div class="card">
    <h2>🌐 Language Translation Tool</h2>

    <label>Enter Text</label>
    <textarea id="inputText" placeholder="Type text here..."></textarea>

    <div class="row">
        <div>
            <label>From</label>
            <select id="sourceLang">
                <option value="en">English</option>
                <option value="ta">Tamil</option>
                <option value="hi">Hindi</option>
                <option value="fr">French</option>
                <option value="es">Spanish</option>
            </select>
        </div>
        <div>
            <label>To</label>
            <select id="targetLang">
                <option value="ta">Tamil</option>
                <option value="en">English</option>
                <option value="hi">Hindi</option>
                <option value="fr">French</option>
                <option value="es">Spanish</option>
            </select>
        </div>
    </div>

    <button class="translate-btn" onclick="translateText()">Translate</button>

    <label style="margin-top:15px;">Translated Output</label>
    <textarea id="outputText" class="output" placeholder="Translated text will appear here..." readonly></textarea>

    <div class="actions">
        <button class="copy-btn" onclick="copyText()">Copy</button>
        <button class="speak-btn" onclick="speakText()">Speak</button>
    </div>

    <footer>
        API integration demo • Internship Task
    </footer>
</div>

<script>
    async function translateText() {
        const text = document.getElementById("inputText").value;
        const source = document.getElementById("sourceLang").value;
        const target = document.getElementById("targetLang").value;

        if (text.trim() === "") {
            alert("Please enter text to translate");
            return;
        }

        try {
            const response = await fetch(
                `https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=${source}&to=${target}`,
                {
                    method: "POST",
                    headers: {
                        "Ocp-Apim-Subscription-Key": "YOUR_API_KEY",+
                        "Ocp-Apim-Subscription-Region": "YOUR_REGION",
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify([{ Text: text }])
                }
            );

            const data = await response.json();
            document.getElementById("outputText").value = data[0].translations[0].text;
        } catch (err) {
            document.getElementById("outputText").value = "Live translation requires a valid API key.";
        }
    }

    function copyText() {
        const output = document.getElementById("outputText");
        output.select();
        document.execCommand("copy");
        alert("Copied to clipboard");
    }

    function speakText() {
        const text = document.getElementById("outputText").value;
        if (!text) return;
        const speech = new SpeechSynthesisUtterance(text);
        speechSynthesis.speak(speech);
    }
</script>

</body>
</html>


