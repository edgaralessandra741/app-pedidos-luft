<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>🎰 Caça-Níquel LUFT</title>
  <style>
    body {
      font-family: system-ui, -apple-system, sans-serif;
      text-align: center;
      background: linear-gradient(135deg, #1a1a2e, #16213e);
      color: #fff;
      padding: 20px 10px;
      min-height: 100vh;
      margin: 0;
      user-select: none;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .header {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 12px;
      margin-bottom: 10px;
      flex-wrap: wrap;
    }
    .logo {
      width: 50px;
      height: 50px;
      object-fit: contain;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.3);
    }
    h1 { 
      margin: 0; 
      font-size: 24px; 
      text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .machine {
      background: #0f3460;
      padding: 20px;
      border-radius: 16px;
      display: inline-block;
      box-shadow: 0 8px 24px rgba(0,0,0,0.5);
      margin-top: 10px;      max-width: 100%;
      border: 2px solid #16213e;
    }
    .reels {
      display: flex;
      justify-content: center;
      gap: 12px;
      margin: 20px 0;
      flex-wrap: wrap;
    }
    .reel {
      background: #fff;
      width: 90px; height: 90px;
      display: flex; align-items: center; justify-content: center;
      border-radius: 12px;
      box-shadow: inset 0 2px 6px rgba(0,0,0,0.3);
      overflow: hidden;
      border: 2px solid #ddd;
    }
    .reel img {
      width: 100%; height: 100%;
      object-fit: contain;
      pointer-events: none;
    }
    .reel.spinning { animation: pulse 0.08s infinite alternate; }
    @keyframes pulse { from { transform: scale(0.96); } to { transform: scale(1.02); } }
    
    .bet-control {
      margin: 15px 0;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 12px;
    }
    .bet-control button {
      width: 36px; height: 36px;
      font-size: 20px; font-weight: bold;
      border-radius: 50%; border: none;
      background: #16213e; color: #fff;
      cursor: pointer; transition: 0.2s;
      box-shadow: 0 2px 0 #0a0f1a;
    }
    .bet-control button:active { transform: translateY(2px); box-shadow: none; }
    .bet-control button:hover:not(:disabled) { background: #1a2845; }
    .bet-control button:disabled { opacity: 0.4; cursor: not-allowed; box-shadow: none; }
    .bet-control span { font-size: 18px; font-weight: bold; min-width: 110px; }
    
    #btnGirar {
      padding: 12px 24px; font-size: 16px; font-weight: bold;
      cursor: pointer; background: #e94560; color: #fff; border: none;      border-radius: 8px; transition: 0.2s;
      box-shadow: 0 4px 0 #b93a4e;
    }
    #btnGirar:active { transform: translateY(4px); box-shadow: none; }
    #btnGirar:hover:not(:disabled) { background: #ff6b81; }
    #btnGirar:disabled { background: #555; cursor: not-allowed; box-shadow: none; }
    
    .info { margin-top: 15px; font-size: 18px; font-weight: bold; }
    .msg { margin-top: 12px; font-weight: bold; min-height: 24px; font-size: 16px; }
    .win { color: #ffd700; text-shadow: 0 0 5px rgba(255, 215, 0, 0.5); } 
    .loss { color: #aaa; }

    /* Rodapé com crédito */
    .footer {
      margin-top: 20px;
      padding: 10px;
      font-size: 14px;
      color: #8892b0;
      text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
    }
    .footer strong {
      color: #ffd700;
    }
  </style>
</head>
<body>
  <div class="header">
    <img src="https://i.ibb.co/yFvGjf7p/img.jpg" alt="Logo LUFT" class="logo" onerror="this.style.display='none'">
    <h1>🎰 Caça-Níquel LUFT</h1>
  </div>
  
  <div class="machine">
    <div class="reels">
      <div class="reel"><img id="r1" src="" alt="slot" data-id=""></div>
      <div class="reel"><img id="r2" src="" alt="slot" data-id=""></div>
      <div class="reel"><img id="r3" src="" alt="slot" data-id=""></div>
    </div>

    <div class="bet-control">
      <button id="btnDiminuir" onclick="mudarAposta(-5)">−</button>
      <span>Aposta: R$ <span id="aposta">5</span></span>
      <button id="btnAumentar" onclick="mudarAposta(5)">+</button>
    </div>

    <button id="btnGirar" onclick="girar()">Girar (R$ 5)</button>
    <div class="info">Saldo: R$ <span id="saldo">100</span></div>
    <div class="msg" id="msg">Boa sorte! 🍀</div>
  </div>

  <!-- Rodapé com crédito do criador -->  <div class="footer">
    Criador: <strong>Edgar Marques</strong> © 2026
  </div>

  <script>
    // 🔗 Suas 6 imagens com IDs simples
    const imagens = [
      { url: 'https://i.ibb.co/sd4XfYF3/img.jpg', id: '1' },
      { url: 'https://i.ibb.co/yFvGjf7p/img.jpg', id: '2' },
      { url: 'https://i.ibb.co/LDrXVgpg/img.jpg', id: '3' },
      { url: 'https://i.ibb.co/3y0fDs5W/img.jpg', id: '4' },
      { url: 'https://i.ibb.co/sTkL42R/img.jpg', id: '5' },
      { url: 'https://i.ibb.co/wNDpsw2g/img.jpg', id: '6' }
    ];

    // Configurações iniciais
    document.getElementById('r1').src = imagens[0].url; document.getElementById('r1').dataset.id = '1';
    document.getElementById('r2').src = imagens[1].url; document.getElementById('r2').dataset.id = '2';
    document.getElementById('r3').src = imagens[2].url; document.getElementById('r3').dataset.id = '3';

    let saldo = 100;
    let aposta = 5;
    const minAposta = 5, maxAposta = 25;

    const r1=document.getElementById('r1'), r2=document.getElementById('r2'), r3=document.getElementById('r3');
    const btnGirar=document.getElementById('btnGirar'), saldoEl=document.getElementById('saldo'), msgEl=document.getElementById('msg');
    const apostaEl=document.getElementById('aposta'), btnAumentar=document.getElementById('btnAumentar'), btnDiminuir=document.getElementById('btnDiminuir');

    function aleatorio() { return Math.floor(Math.random() * imagens.length); }

    function mudarAposta(valor) {
      let nova = aposta + valor;
      if (nova >= minAposta && nova <= maxAposta) {
        aposta = nova;
        apostaEl.textContent = aposta;
        btnGirar.textContent = `Girar (R$ ${aposta})`;
        atualizarBotoesAposta();
      }
    }

    function atualizarBotoesAposta() {
      btnDiminuir.disabled = aposta <= minAposta || btnGirar.disabled;
      btnAumentar.disabled = aposta >= maxAposta || btnGirar.disabled;
    }

    function girar() {
      if(saldo < aposta) {
        msgEl.textContent = '💸 Saldo insuficiente! Atualize a página.';
        msgEl.className = 'msg loss'; 
        return;      }
      saldo -= aposta;
      saldoEl.textContent = saldo;
      btnGirar.disabled = true;
      atualizarBotoesAposta();
      msgEl.textContent = '🎰 Girando...'; 
      msgEl.className = 'msg';
      [r1,r2,r3].forEach(el => el.parentElement.classList.add('spinning'));

      let voltas = 0;
      const intervalo = setInterval(() => {
        const idx1 = aleatorio(), idx2 = aleatorio(), idx3 = aleatorio();
        r1.src = imagens[idx1].url; r1.dataset.id = imagens[idx1].id;
        r2.src = imagens[idx2].url; r2.dataset.id = imagens[idx2].id;
        r3.src = imagens[idx3].url; r3.dataset.id = imagens[idx3].id;
        voltas++;
        if(voltas >= 15) {
          clearInterval(intervalo);
          [r1,r2,r3].forEach(el => el.parentElement.classList.remove('spinning'));
          finalizar();
        }
      }, 80);
    }

    function finalizar() {
      const id1 = r1.dataset.id;
      const id2 = r2.dataset.id;
      const id3 = r3.dataset.id;

      let ganho = 0, texto = '', classe = 'msg';

      if(id1 === id2 && id2 === id3) {
        ganho = aposta * 10;
        texto = `🎉 JACKPOT! 3 iguais! +R$ ${ganho}`;
        classe += ' win';
      } else if(id1 === id2 || id2 === id3 || id1 === id3) {
        ganho = aposta * 2;
        texto = `✨ Par! 2 iguais! +R$ ${ganho}`;
        classe += ' win';
      } else {
        texto = '😔 Não foi dessa vez. Tente novamente!';
        classe += ' loss';
      }

      saldo += ganho;
      saldoEl.textContent = saldo;
      msgEl.textContent = texto;
      msgEl.className = classe;
      btnGirar.disabled = false;
      atualizarBotoesAposta();    }

    atualizarBotoesAposta();
  </script>
</body>
</html>
