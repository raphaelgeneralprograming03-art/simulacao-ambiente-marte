<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>MHD - Propulsão e Coleta</title>
    <style>
        body { background: #000505; color: #00ffcc; font-family: monospace; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin:0; }
        .hud { border: 2px solid #00ffcc; background: rgba(0, 20, 20, 0.8); padding: 25px; width: 430px; border-radius: 8px; box-shadow: 0 0 20px rgba(0, 255, 204, 0.2); }
        h2 { text-align: center; border-bottom: 1px solid #00ffcc; padding-bottom: 8px; }
        .thruster-visual { height: 40px; background: #001a1a; border: 1px dashed #00ffcc; margin: 15px 0; display: flex; align-items: center; justify-content: center; position: relative; overflow: hidden; }
        .plasma-beam { height: 100%; width: 0%; background: linear-gradient(90deg, transparent, #00ffcc); position: absolute; left: 0; transition: width 0.3s ease; }
        .btn { width: 100%; padding: 10px; background: transparent; border: 2px solid #00ffcc; color: #00ffcc; font-weight: bold; cursor: pointer; margin-top: 10px; }
        .btn:hover { background: #00ffcc; color: #000; }
    </style>
</head>
<body>
    <div class="hud">
        <h2>MHD // PROPULSION_SYSTEM</h2>
        <div>Fluxo do Motor de Plasma:</div>
        <div class="thruster-visual"><div id="beam" class="plasma-beam"></div><span style="z-index: 2;" id="pct">0% POWER</span></div>
        <div style="margin: 15px 0;">
            <div>Coleta Magnética Funil Bussard: <span id="bussard" style="color:#fff;">Inativa</span></div>
            <div>Taxa Ionização Atmosférica: <span id="ion" style="color:#fff;">0.00 ppm</span></div>
        </div>
        <button class="btn" onclick="alternarPropulsao()">Ignição Micro-Propulsores RCS</button>
    </div>

    <script>
        let ativo = false;
        function alternarPropulsao() {
            ativo = !ativo;
            const beam = document.getElementById('beam');
            const pct = document.getElementById('pct');
            const bussard = document.getElementById('bussard');
            const ion = document.getElementById('ion');

            if(ativo) {
                beam.style.width = "100%";
                pct.innerText = "100% THRUST (RELATIVISTIC)";
                bussard.innerText = "CAPTURA_ATIVA (50m)";
                ion.innerText = "1450.75 ppm";
            } else {
                beam.style.width = "0%";
                pct.innerText = "0% POWER";
                bussard.innerText = "Inativa";
                ion.innerText = "0.00 ppm";
            }
        }
    </script>
</body>
</html>
