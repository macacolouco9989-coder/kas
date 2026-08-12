<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel Aimbot · Silent</title>
    <!-- Ícone simples (font-awesome) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
        }

        body {
            background: #0b0d11;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 1.5rem;
        }

        /* ── painel principal ── */
        .panel {
            background: #14181f;
            width: 100%;
            max-width: 480px;
            padding: 2rem 1.8rem 2.2rem;
            border-radius: 2rem;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.7), 0 0 0 1px rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(2px);
            transition: 0.2s ease;
        }

        /* cabeçalho */
        .header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 2.2rem;
        }

        .header h1 {
            color: #eef2f6;
            font-size: 1.7rem;
            font-weight: 500;
            letter-spacing: -0.3px;
            display: flex;
            align-items: center;
            gap: 0.65rem;
        }

        .header h1 i {
            color: #3d7eff;
            font-size: 1.6rem;
            filter: drop-shadow(0 0 6px #3d7eff55);
        }

        .status-badge {
            background: #1f2a33;
            padding: 0.3rem 0.9rem;
            border-radius: 40px;
            font-size: 0.75rem;
            font-weight: 600;
            color: #9aaec9;
            letter-spacing: 0.3px;
            border: 1px solid #2a3744;
            white-space: nowrap;
        }

        .status-badge i {
            margin-right: 6px;
            color: #3d7eff;
        }

        /* ── cards de opções ── */
        .option-grid {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            margin: 2.2rem 0 2.8rem;
        }

        .option-card {
            background: #1b2129;
            border-radius: 1.6rem;
            padding: 1.2rem 1.4rem 1.4rem;
            border: 1px solid #2a333e;
            transition: all 0.15s ease;
            box-shadow: 0 6px 0 #0e1218;
        }

        .option-card:hover {
            border-color: #3d7eff55;
            background: #1f2832;
        }

        .card-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 0.9rem;
        }

        .card-header .label {
            display: flex;
            align-items: center;
            gap: 0.7rem;
            color: #d3deec;
            font-weight: 500;
            font-size: 1.1rem;
        }

        .card-header .label i {
            font-size: 1.2rem;
            color: #3d7eff;
            width: 1.6rem;
            text-align: center;
        }

        /* toggle switch (estilo único) */
        .toggle {
            width: 48px;
            height: 26px;
            background: #2a3644;
            border-radius: 40px;
            position: relative;
            cursor: pointer;
            transition: 0.2s;
            flex-shrink: 0;
            border: 1px solid #3b4a5a;
        }

        .toggle.active {
            background: #1d4ed8;
            border-color: #3d7eff;
            box-shadow: 0 0 12px #1d4ed866;
        }

        .toggle .knob {
            width: 20px;
            height: 20px;
            background: #eef2f6;
            border-radius: 50%;
            position: absolute;
            top: 2px;
            left: 2px;
            transition: 0.2s cubic-bezier(0.34, 1.2, 0.64, 1);
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.5);
        }

        .toggle.active .knob {
            left: 24px;
            background: #ffffff;
        }

        .card-desc {
            color: #8ca1bb;
            font-size: 0.85rem;
            margin-top: 0.2rem;
            padding-left: 2.4rem;
            border-left: 2px solid #2a3744;
            padding-left: 1rem;
            line-height: 1.4;
        }

        .card-desc i {
            color: #3d7eff;
            margin-right: 5px;
            opacity: 0.7;
        }

        /* ── botão de ação ── */
        .action-btn {
            background: #1d4ed8;
            border: none;
            width: 100%;
            padding: 1rem 0;
            border-radius: 3rem;
            font-weight: 600;
            font-size: 1.1rem;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.7rem;
            cursor: pointer;
            transition: 0.15s;
            border: 1px solid #3d7eff88;
            box-shadow: 0 8px 0 #0f2a6b, 0 4px 20px #1d4ed833;
            letter-spacing: 0.3px;
        }

        .action-btn i {
            font-size: 1.2rem;
        }

        .action-btn:active {
            transform: translateY(4px);
            box-shadow: 0 4px 0 #0f2a6b;
        }

        .action-btn:disabled {
            opacity: 0.6;
            transform: translateY(4px);
            box-shadow: 0 4px 0 #0f2a6b;
            pointer-events: none;
        }

        /* rodapé / silent indicator */
        .silent-footer {
            display: flex;
            justify-content: flex-end;
            margin-top: 1.4rem;
            color: #5f748f;
            font-size: 0.8rem;
            align-items: center;
            gap: 0.5rem;
            border-top: 1px solid #1f2a33;
            padding-top: 1.2rem;
        }

        .silent-footer i {
            color: #3d7eff;
            font-size: 0.9rem;
        }

        .silent-dot {
            display: inline-block;
            width: 8px;
            height: 8px;
            background: #3d7eff;
            border-radius: 10px;
            margin-right: 4px;
            box-shadow: 0 0 12px #3d7effaa;
            transition: 0.3s;
        }

        .silent-dot.inactive {
            background: #4a5f7a;
            box-shadow: none;
        }

        /* responsivo */
        @media (max-width: 420px) {
            .panel { padding: 1.5rem; }
            .header h1 { font-size: 1.4rem; }
        }
    </style>
</head>
<body>
<div class="panel">
    <!-- cabeçalho -->
    <div class="header">
        <h1><i class="fas fa-crosshairs"></i> Aimbot</h1>
        <span class="status-badge"><i class="fas fa-circle" style="color: #3d7eff;"></i> Silent</span>
    </div>

    <!-- opções do menu (aimbot + silent) -->
    <div class="option-grid">
        <!-- Aimbot -->
        <div class="option-card" id="aimbotCard">
            <div class="card-header">
                <span class="label"><i class="fas fa-bullseye"></i> Aimbot</span>
                <div class="toggle" id="aimbotToggle" role="button" tabindex="0" aria-label="Alternar Aimbot">
                    <div class="knob"></div>
                </div>
            </div>
            <div class="card-desc">
                <i class="fas fa-arrow-right"></i> Mira assistida · ajuste fino
            </div>
        </div>

        <!-- Silent (modo silencioso) -->
        <div class="option-card" id="silentCard">
            <div class="card-header">
                <span class="label"><i class="fas fa-ghost"></i> Silent</span>
                <div class="toggle" id="silentToggle" role="button" tabindex="0" aria-label="Alternar Silent">
                    <div class="knob"></div>
                </div>
            </div>
            <div class="card-desc">
                <i class="fas fa-arrow-right"></i> Disparo invisível · sem movimentos bruscos
            </div>
        </div>
    </div>

    <!-- botão central (simulação de ativação / status) -->
    <button class="action-btn" id="actionBtn">
        <i class="fas fa-play"></i> Aplicar configuração
    </button>

    <!-- rodapé com indicador Silent + aimbot -->
    <div class="silent-footer">
        <span><i class="fas fa-shield-alt"></i> Silent ativo: </span>
        <span id="silentStatusText" style="font-weight: 500; color: #c0d0e5;">desligado</span>
        <span class="silent-dot inactive" id="silentDot"></span>
        <span style="margin-left: 0.6rem;">|</span>
        <span style="margin-left: 0.6rem;"><i class="fas fa-crosshairs"></i> Aimbot: <span id="aimbotStatusText" style="font-weight:500; color:#c0d0e5;">desligado</span></span>
        <span class="silent-dot inactive" id="aimbotDot" style="margin-left: 4px;"></span>
    </div>
</div>

<script>
    (function() {
        "use strict";

        // --- elementos DOM ---
        const aimbotToggle = document.getElementById('aimbotToggle');
        const silentToggle = document.getElementById('silentToggle');
        const actionBtn = document.getElementById('actionBtn');

        // textos e dots
        const aimbotStatusText = document.getElementById('aimbotStatusText');
        const silentStatusText = document.getElementById('silentStatusText');
        const aimbotDot = document.getElementById('aimbotDot');
        const silentDot = document.getElementById('silentDot');

        // estado interno
        let aimbotEnabled = false;
        let silentEnabled = false;

        // --- funções de atualização de UI ---
        function updateUI() {
            // Aimbot
            if (aimbotEnabled) {
                aimbotToggle.classList.add('active');
                aimbotStatusText.textContent = 'ligado';
                aimbotDot.className = 'silent-dot';  // ativo (cor padrão)
            } else {
                aimbotToggle.classList.remove('active');
                aimbotStatusText.textContent = 'desligado';
                aimbotDot.className = 'silent-dot inactive';
            }

            // Silent
            if (silentEnabled) {
                silentToggle.classList.add('active');
                silentStatusText.textContent = 'ligado';
                silentDot.className = 'silent-dot';
            } else {
                silentToggle.classList.remove('active');
                silentStatusText.textContent = 'desligado';
                silentDot.className = 'silent-dot inactive';
            }

            // Atualiza o botão com dica visual do estado (somente para feedback)
            if (aimbotEnabled || silentEnabled) {
                actionBtn.innerHTML = `<i class="fas fa-check-circle"></i> Configuração ativa`;
                actionBtn.style.background = '#1a5c3a';
                actionBtn.style.borderColor = '#2e9b5e';
                actionBtn.style.boxShadow = '0 8px 0 #0f3d25, 0 4px 20px #1d4ed833';
            } else {
                actionBtn.innerHTML = `<i class="fas fa-play"></i> Aplicar configuração`;
                actionBtn.style.background = '#1d4ed8';
                actionBtn.style.borderColor = '#3d7eff88';
                actionBtn.style.boxShadow = '0 8px 0 #0f2a6b, 0 4px 20px #1d4ed833';
            }
        }

        // --- alternar toggles (clique) ---
        aimbotToggle.addEventListener('click', function(e) {
            e.stopPropagation();
            aimbotEnabled = !aimbotEnabled;
            updateUI();
        });

        silentToggle.addEventListener('click', function(e) {
            e.stopPropagation();
            silentEnabled = !silentEnabled;
            updateUI();
        });

        // atalho: tecla Enter/Space para acessibilidade (opcional)
        aimbotToggle.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' || e.key === ' ') {
                e.preventDefault();
                aimbotToggle.click();
            }
        });
        silentToggle.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' || e.key === ' ') {
                e.preventDefault();
                silentToggle.click();
            }
        });

        // --- botão "Aplicar" (simula salvamento / ação) ---
        actionBtn.addEventListener('click', function() {
            // apenas feedback visual + notificação no console (simulação)
            const aimbotState = aimbotEnabled ? 'ATIVADO' : 'desativado';
            const silentState = silentEnabled ? 'ATIVADO' : 'desativado';

            // feedback tátil no botão (breve)
            actionBtn.disabled = true;
            actionBtn.innerHTML = `<i class="fas fa-spinner fa-pulse"></i> Aplicando...`;
            setTimeout(() => {
                actionBtn.disabled = false;
                // restaura conforme estado atual
                if (aimbotEnabled || silentEnabled) {
                    actionBtn.innerHTML = `<i class="fas fa-check-circle"></i> Configuração ativa`;
                } else {
                    actionBtn.innerHTML = `<i class="fas fa-play"></i> Aplicar configuração`;
                }
                // exibe no console (simulação de log)
                console.log(`[PAINEL] Aimbot: ${aimbotState} | Silent: ${silentState}`);
                // (Opcional) pequeno alerta visual, mas sem alert() para não incomodar
                // Mudamos o texto do rodapé temporariamente? 
                // Vamos piscar o status.
                const originalText = silentStatusText.textContent;
                silentStatusText.style.transition = '0.1s';
                silentStatusText.style.color = '#a0d0ff';
                setTimeout(() => {
                    silentStatusText.style.color = '#c0d0e5';
                }, 300);
            }, 400);
        });

        // --- inicialização ---
        // (ambos começam desligados)
        updateUI();

        // pequeno detalhe: ao carregar, mostra que silent está inativo, aimbot inativo
        console.log('Painel Aimbot + Silent carregado. Use os toggles e o botão "Aplicar".');
    })();
</script>
</body>
</html>
