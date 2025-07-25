Surebet
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Calculadora de Surebet</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #eef3f8;
      padding: 40px 20px;
      display: flex;
      justify-content: center;
    }

    .container {
      background: #ffffff;
      padding: 30px 35px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      max-width: 600px;
      width: 100%;
    }

    h1 {
      text-align: center;
      color: #2c3e50;
      margin-bottom: 25px;
    }

    label {
      display: block;
      font-weight: 600;
      margin-bottom: 5px;
      color: #34495e;
    }

    .form-group {
      margin-bottom: 20px;
    }

    input {
      padding: 10px;
      width: 100%;
      font-size: 16px;
      border-radius: 8px;
      border: 1px solid #ccc;
      box-sizing: border-box;
    }

    .odd-input {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
    }

    .odd-input input {
      flex: 1;
    }

    .odd-input button {
      margin-left: 10px;
      background: none;
      border: none;
      cursor: pointer;
      font-size: 20px;
      color: #c0392b;
      padding: 5px;
    }

    .btn {
      width: 100%;
      padding: 12px;
      background-color: #2980b9;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      margin-top: 10px;
    }

    .btn:hover {
      background-color: #1c5984;
    }

    #resultado {
      margin-top: 25px;
      background: #ecf0f1;
      padding: 20px;
      border-radius: 8px;
      color: #2c3e50;
    }

    .valor-label {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .valor-label::before {
      content: "💰";
      font-size: 20px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>Calculadora de Surebet</h1>

    <div class="form-group">
      <label class="valor-label" for="total">Valor total investido (R$)</label>
      <input type="number" id="total" step="0.01" placeholder="Ex: 1000">
    </div>

    <div id="oddsContainer">
      <div class="odd-input">
        <input type="number" step="0.01" class="odd" placeholder="Odd 1">
      </div>
      <div class="odd-input">
        <input type="number" step="0.01" class="odd" placeholder="Odd 2">
      </div>
    </div>

    <button class="btn" onclick="adicionarOdd()">➕ Adicionar outra odd</button>
    <button class="btn" onclick="calcular()">🧮 Calcular</button>

    <div id="resultado"></div>
  </div>

  <script>
    function adicionarOdd() {
      const container = document.getElementById("oddsContainer");
      const div = document.createElement("div");
      div.className = "odd-input";

      div.innerHTML = `
        <input type="number" step="0.01" class="odd" placeholder="Nova Odd">
        <button onclick="removerOdd(this)" title="Remover odd">🗑️</button>
      `;

      container.appendChild(div);
    }

    function removerOdd(botao) {
      const linha = botao.parentNode;
      linha.remove();
    }

    function calcular() {
      const total = parseFloat(document.getElementById("total").value);
      const odds = Array.from(document.getElementsByClassName("odd"))
        .map(input => parseFloat(input.value))
        .filter(v => !isNaN(v) && v > 1);

      const resultadoDiv = document.getElementById("resultado");

      if (isNaN(total) || total <= 0 || odds.length < 2) {
        resultadoDiv.innerHTML = "<strong>⚠️ Insira ao menos duas odds válidas e o valor total.</strong>";
        return;
      }

      const inversos = odds.map(o => 1 / o);
      const somaInversos = inversos.reduce((acc, val) => acc + val, 0);

      if (somaInversos >= 1) {
        resultadoDiv.innerHTML = "<strong>❌ Não é uma oportunidade de surebet.</strong>";
        return;
      }

      let html = `<strong>✅ Surebet identificada!</strong><br><br>`;
      let retornos = [];

      odds.forEach((odd, i) => {
        const investimento = (1 / odd) / somaInversos * total;
        const retorno = investimento * odd;
        retornos.push(retorno);

        html += `🔹 Apostar <strong>R$ ${investimento.toFixed(2)}</strong> na Odd ${i + 1} (<strong>${odd}</strong>)<br>`;
      });

      const retornoGarantido = retornos[0];
      const lucro = retornoGarantido - total;
      const roi = (lucro / total) * 100;

      html += `<br>💰 Retorno garantido: <strong>R$ ${retornoGarantido.toFixed(2)}</strong><br>`;
      html += `📈 Lucro: <strong>R$ ${lucro.toFixed(2)}</strong> (${roi.toFixed(2)}% ROI)`;

      resultadoDiv.innerHTML = html;
    }
  </script>

</body>
</html>
