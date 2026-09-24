<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Módulo 1: Simulação Atmosférica de Marte - MHD/SAMS</title>
    <script src="https://jsdelivr.net"></script>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen p-8 font-sans">
    <div class="max-w-4xl mx-auto bg-slate-900 border border-red-900/40 rounded-xl p-6 shadow-2xl shadow-red-950/20">
        <header class="border-b border-red-800/30 pb-4 mb-6">
            <h1 class="text-2xl font-bold text-red-500 tracking-wide uppercase">SAMS/MHD - Mars Environment Simulator</h1>
            <p class="text-slate-400 text-sm">Compensação gravitacional ativa e compressão seletiva de CO₂ atmosférico.</p>
        </header>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- Controles -->
            <div class="space-y-6 bg-slate-950/50 p-4 rounded-lg border border-slate-800">
                <h2 class="text-lg font-semibold text-slate-300">Controles de Ambiente</h2>
                <div>
                    <label class="block text-xs font-mono text-slate-400 uppercase mb-2">Gravidade Marciana (m/s²)</label>
                    <input id="gravityRange" type="range" min="1" max="10" step="0.01" value="3.71" class="w-full h-1 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-red-500">
                    <div class="text-right text-xs font-mono text-red-400 mt-1"><span id="gravityVal">3.71</span> m/s²</div>
                </div>
                <div>
                    <label class="block text-xs font-mono text-slate-400 uppercase mb-2">Eficiência de Sucção MHD</label>
                    <input id="mhdRange" type="range" min="0" max="100" value="85" class="w-full h-1 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-red-500">
                    <div class="text-right text-xs font-mono text-red-400 mt-1"><span id="mhdVal">85</span>%</div>
                </div>
            </div>

            <!-- Telemetria -->
            <div class="space-y-4">
                <div class="bg-slate-950 p-4 rounded-lg border border-slate-800/80">
                    <span class="text-xs font-mono text-slate-500 uppercase">Micro-Pressão SAMS (Muscular)</span>
                    <div class="text-3xl font-mono font-bold text-emerald-400 mt-1" id="samsPressure">12.20 kPa</div>
                    <div class="w-full bg-slate-800 h-1.5 rounded-full mt-2 overflow-hidden">
                        <div id="samsBar" class="bg-emerald-500 h-full transition-all duration-300" style="width: 61%"></div>
                    </div>
                </div>

                <div class="bg-slate-950 p-4 rounded-lg border border-slate-800/80">
                    <span class="text-xs font-mono text-slate-500 uppercase">Taxa de Compressão de CO₂</span>
                    <div class="text-3xl font-mono font-bold text-blue-400 mt-1" id="co2Rate">15.0 : 1</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const gravityRange = document.getElementById('gravityRange');
        const mhdRange = document.getElementById('mhdRange');
        const gravityVal = document.getElementById('gravityVal');
        const mhdVal = document.getElementById('mhdVal');
        const samsPressure = document.getElementById('samsPressure');
        const samsBar = document.getElementById('samsBar');
        const co2Rate = document.getElementById('co2Rate');

        function updateSimulation() {
            const g = parseFloat(gravityRange.value);
            const mhd = parseFloat(mhdRange.value);

            gravityVal.innerText = g.toFixed(2);
            mhdVal.innerText = mhd;

            // Diferença para a gravidade da Terra (9.81 m/s²) determina a necessidade de contração do SAMS
            const deltaG = Math.max(0, 9.81 - g);
            const pressureKpa = deltaG * 2.0; // Fator de escala empírico
            samsPressure.innerText = `${pressureKpa.toFixed(2)} kPa`;
            
            const maxPressurePossible = 20; 
            const percentage = Math.min(100, (pressureKpa / maxPressurePossible) * 100);
            samsBar.style.width = `${percentage}%`;

            // Compressão MHD baseada na eficiência selecionada
            const compressionRatio = (mhd * 0.176).toFixed(1);
            co2Rate.innerText = `${compressionRatio} : 1`;
        }

        gravityRange.addEventListener('input', updateSimulation);
        mhdRange.addEventListener('input', updateSimulation);
        updateSimulation();
    </script>
</body>
</html>
