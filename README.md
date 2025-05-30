<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Planilha Inteligente - Cálculo de Preço</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #f5f7fa;
      margin: 0;
      padding: 40px;
      color: #333;
    }
    h1 {
      text-align: center;
      color: #004080;
      margin-bottom: 30px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background-color: white;
      border-radius: 10px;
      overflow: hidden;
      box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    }
    th, td {
      padding: 12px;
      border: 1px solid #ddd;
      text-align: center;
    }
    th {
      background-color: #004080;
      color: white;
    }
    td input {
      width: 100%;
      padding: 6px;
      border: 1px solid #ccc;
      border-radius: 4px;
      text-align: right;
    }
    tfoot td {
      font-weight: bold;
      background-color: #f0f0f0;
    }
    .footer {
      text-align: center;
      margin-top: 40px;
      font-size: 14px;
      color: #666;
    }
    .add-row-btn {
      display: block;
      margin: 20px auto;
      padding: 10px 20px;
      background-color: #004080;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .add-row-btn:hover {
      background-color: #003060;
    }
    .ia-msg {
      font-size: 12px;
      color: #006400;
      text-align: center;
      margin-top: 5px;
    }
  </style>
</head>
<body>
  <h1>Planilha Inteligente de Precificação com IA</h1>

  <table id="priceTable">
    <thead>
      <tr>
        <th>Produto</th>
        <th>Preço de Compra (R$)</th>
        <th>Imposto (%)</th>
        <th>Frete (R$)</th>
        <th>Preço de Venda (R$)</th>
        <th>Data da Compra</th>
        <th>Valor Total (R$)</th>
        <th>Lucro (R$)</th>
        <th>IA Sugestão</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><input type="text" placeholder="Nome do Produto"></td>
        <td><input type="number" step="0.01" class="preco-compra"></td>
        <td><input type="number" step="0.01" class="imposto"></td>
        <td><input type="number" step="0.01" class="frete"></td>
        <td><input type="number" step="0.01" class="preco-venda"></td>
        <td><input type="date"></td>
        <td><input type="text" class="valor-total" readonly></td>
        <td><input type="text" class="lucro" readonly></td>
        <td><div class="ia-msg"></div></td>
      </tr>
    </tbody>
  </table>

  <button class="add-row-btn" onclick="adicionarLinha()">Adicionar Produto</button>

  <div class="footer">
    © 2025 BHSI PLANILHAS DE SUCESSO - Organize. Cresça. Vença.
  </div>

  <script>
    function calcularValorTotal(row) {
      const precoCompra = parseFloat(row.querySelector('.preco-compra').value) || 0;
      const imposto = parseFloat(row.querySelector('.imposto').value) || 0;
      const frete = parseFloat(row.querySelector('.frete').value) || 0;
      const precoVenda = parseFloat(row.querySelector('.preco-venda').value) || 0;

      const total = precoCompra + frete + (precoCompra * (imposto / 100));
      const lucro = precoVenda - total;

      row.querySelector('.valor-total').value = total.toFixed(2);
      row.querySelector('.lucro').value = lucro.toFixed(2);

      const iaMsg = row.querySelector('.ia-msg');
      if (!iaMsg) return;

      if (lucro < 0) {
        iaMsg.innerText = "⚠️ Alerta: Prejuízo! Reavalie o preço de venda.";
        iaMsg.style.color = '#b00020';
      } else if (lucro < total * 0.1) {
        iaMsg.innerText = "💡 Dica IA: Margem baixa, tente aumentar o preço de venda.";
        iaMsg.style.color = '#ff8c00';
      } else {
        iaMsg.innerText = "✅ Margem saudável.";
        iaMsg.style.color = '#006400';
      }
    }

    function aplicarEventos(row) {
      row.querySelectorAll('input').forEach(input => {
        input.addEventListener('input', () => calcularValorTotal(row));
      });
    }

    document.querySelectorAll('tbody tr').forEach(row => aplicarEventos(row));

    function adicionarLinha() {
      const tbody = document.querySelector('#priceTable tbody');
      const novaLinha = document.createElement('tr');
      novaLinha.innerHTML = `
        <td><input type="text" placeholder="Nome do Produto"></td>
        <td><input type="number" step="0.01" class="preco-compra"></td>
        <td><input type="number" step="0.01" class="imposto"></td>
        <td><input type="number" step="0.01" class="frete"></td>
        <td><input type="number" step="0.01" class="preco-venda"></td>
        <td><input type="date"></td>
        <td><input type="text" class="valor-total" readonly></td>
        <td><input type="text" class="lucro" readonly></td>
        <td><div class="ia-msg"></div></td>
      `;
      tbody.appendChild(novaLinha);
      aplicarEventos(novaLinha);
    }
  </script>
</body>
</html>
