# Calculadora do salário líquido em Itália

> Do bruto ao líquido em Itália em 2026: contribuições do trabalhador, IRPEF com deduções, derramas regional e municipal, para Milão, Roma, Turim, Bolonha e Nápoles.


<section> <span class="eyebrow">Calculadora do líquido</span> <h1 id="quanto-leva-para-casa-o-seu-trabalhador-em-italia">Quanto leva para casa o seu trabalhador em Itália</h1> <p class="lede">Introduza o salário bruto anual e a cidade. A estimativa aplica as regras de 2026: contribuições do trabalhador, IRPEF com deduções de trabalho dependente e derramas locais. Útil para comparar com o líquido que o candidato teria em Portugal.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Salário bruto anual, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Cidade<select id="city"><option value="Milan">Milão</option><option value="Rome">Roma</option><option value="Turin">Turim</option><option value="Bologna">Bolonha</option><option value="Naples">Nápoles</option></select></label> <label for="inst">Pago em<select id="inst"><option value="13">13 prestações</option><option value="14">14 prestações</option><option value="12">12 (13.ª/14.ª em duodécimos)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Rubrica</th><th class="num">Por ano</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="como-funciona-o-calculo">Como funciona o cálculo</h2> <ol> <li>O trabalhador paga 9,19% de contribuições sobre o bruto, mais 1% sobre a parte acima de 56.224 € por ano.</li> <li>O IRPEF incide sobre o resto: 23% até 28.000 €, 33% de 28.000 € a 50.000 €, 43% acima.</li> <li>As deduções de trabalho dependente reduzem o imposto. São maiores nos salários baixos e chegam a zero nos 50.000 €. Entre 20.000 € e 40.000 € aplica-se uma dedução extra até 1.000 €.</li> <li>Somam-se as derramas regional e municipal, salvo se o rendimento estiver abaixo do limiar de isenção da cidade.</li> </ol> <p>Nada disto muda o que paga como empregador: veja a <a href="calculadora.html">calculadora de custos do empregador</a>.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardia","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lácio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piemonte","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0.8% to 1.2% by band; 0.8% used","exempt":11790},"Bologna":{"region":"Emília-Romanha","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Campânia","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−€' : '€') + Math.round(Math.abs(n)).toLocaleString('pt-PT');
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Salário bruto', g], ['Contribuições do trabalhador', -inps], ['IRPEF após deduções', -irpef], [`Derrama regional (${c.region})`, -reg], ['Derrama municipal', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Salário líquido</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `Cerca de ${eur(net / n)} líquidos em cada um dos ${n} recibos (${(net / g * 100).toFixed(0)}% do bruto).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://contrataremitalia.com/calculadora-salario-liquido.html
