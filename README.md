<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Detecção de oportunidades com cadeia de IAs | ADM0150 | Luís Felipe Azevedo Rodrigues</title>
<style>
:root{
  --bg:#ffffff; --fg:#1b1f24; --muted:#59636e; --line:#d6dbe1; --card:#f5f7f9;
  --accent:#0b5cad; --ok-bg:#dff3e4; --ok-fg:#14532d; --warn-bg:#fdecc8; --warn-fg:#6b4a00;
  --sup-bg:#e7e0f8; --sup-fg:#3b2a77; --orf-bg:#fbdcdc; --orf-fg:#7a1c1c; --pre-bg:#f1f3f5;
  --ph-bg:#fff3a3; --ph-fg:#4a3b00;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    --bg:#14171a; --fg:#e8eaed; --muted:#a0a8b1; --line:#2f353b; --card:#1c2024;
    --accent:#7db7ff; --ok-bg:#16361f; --ok-fg:#a6e3b4; --warn-bg:#40320c; --warn-fg:#f3d58a;
    --sup-bg:#2c2550; --sup-fg:#cdbff5; --orf-bg:#4a1f1f; --orf-fg:#f5b5b5; --pre-bg:#1c2024;
    --ph-bg:#5a4c00; --ph-fg:#fff3a3;
  }
}
:root[data-theme="dark"]{
  --bg:#14171a; --fg:#e8eaed; --muted:#a0a8b1; --line:#2f353b; --card:#1c2024;
  --accent:#7db7ff; --ok-bg:#16361f; --ok-fg:#a6e3b4; --warn-bg:#40320c; --warn-fg:#f3d58a;
  --sup-bg:#2c2550; --sup-fg:#cdbff5; --orf-bg:#4a1f1f; --orf-fg:#f5b5b5; --pre-bg:#1c2024;
  --ph-bg:#5a4c00; --ph-fg:#fff3a3;
}
*{box-sizing:border-box}
html{height:100%; scroll-padding-top:5rem}
body{margin:0; background:var(--bg); color:var(--fg); font:17px/1.6 Georgia,"Times New Roman",serif;
  padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px);}
header.top{position:sticky; top:0; z-index:10; background:var(--bg); border-bottom:1px solid var(--line);
  padding-top:env(safe-area-inset-top,0px)}
header.top .inner{max-width:960px; margin:0 auto; padding:.6rem 1rem; display:flex; flex-wrap:wrap; gap:.4rem .9rem; align-items:center}
.brand{font:600 .9rem system-ui,sans-serif; color:var(--muted); margin-right:auto}
nav.main{display:flex; flex-wrap:wrap; gap:.25rem}
nav.main a, .subnav a{font:500 .9rem system-ui,sans-serif; text-decoration:none; color:var(--fg);
  padding:.35rem .7rem; border-radius:999px; border:1px solid var(--line)}
nav.main a[aria-current="page"], .subnav a[aria-current="page"]{background:var(--accent); color:#fff; border-color:var(--accent)}
main{max-width:960px; margin:0 auto; padding:1.2rem 1rem 3rem}
.view{display:none}
.view.active{display:block}
h1{font:700 1.9rem/1.25 system-ui,sans-serif; margin:.6rem 0 .4rem}
h2{font:650 1.35rem/1.3 system-ui,sans-serif; margin:2rem 0 .6rem}
h3{font:650 1.1rem/1.3 system-ui,sans-serif; margin:.2rem 0 .6rem}
.lead{color:var(--muted); font-size:1.05rem; margin-top:0}
p,li{max-width:75ch}
.card{background:var(--card); border:1px solid var(--line); border-radius:10px; padding:1rem 1.2rem; margin:1.2rem 0}
.ident{display:grid; grid-template-columns:max-content 1fr; gap:.3rem 1rem; margin:0}
.ident dt{font:600 .9rem system-ui,sans-serif; color:var(--muted)}
.ident dd{margin:0}
.table-wrap{overflow-x:auto; margin:1rem 0; border:1px solid var(--line); border-radius:8px}
table{border-collapse:collapse; width:100%; font:.92rem/1.45 system-ui,sans-serif; min-width:640px}
th,td{padding:.55rem .7rem; text-align:left; vertical-align:top; border-bottom:1px solid var(--line)}
th{background:var(--card); font-weight:650}
tr:last-child td{border-bottom:0}
pre.prompt{background:var(--pre-bg); border:1px solid var(--line); border-radius:8px; padding:1rem; overflow-x:auto;
  font:.82rem/1.5 ui-monospace,Menlo,Consolas,monospace; white-space:pre-wrap; word-break:break-word}
.badge{display:inline-block; font:600 .78rem system-ui,sans-serif; padding:.15rem .5rem; border-radius:999px; white-space:nowrap}
.badge.ok{background:var(--ok-bg); color:var(--ok-fg)}
.badge.warn{background:var(--warn-bg); color:var(--warn-fg)}
.badge.sup{background:var(--sup-bg); color:var(--sup-fg)}
.badge.orf{background:var(--orf-bg); color:var(--orf-fg)}
.legend{font:.9rem system-ui,sans-serif; color:var(--muted)}
.subnav{display:flex; flex-wrap:wrap; gap:.4rem; margin:.5rem 0 1rem}
ul.check{padding-left:1.2rem}
a{color:var(--accent); word-break:break-word}
.note{color:var(--muted); font-size:.95rem; margin-top:2rem}
.preencher{background:var(--ph-bg); color:var(--ph-fg); padding:.1rem .35rem; border-radius:4px; font-family:system-ui,sans-serif; font-size:.9em}
#rascunho{background:var(--ph-bg); color:var(--ph-fg); font:600 .9rem system-ui,sans-serif; text-align:center; padding:.5rem 1rem}
footer{border-top:1px solid var(--line); color:var(--muted); font:.85rem system-ui,sans-serif; text-align:center; padding:1rem}
@media (max-width:600px){ body{font-size:16px} h1{font-size:1.5rem} .ident{grid-template-columns:1fr} .ident dt{margin-top:.5rem} }
@media print{ header.top,#rascunho,.subnav{display:none} .view{display:block!important; page-break-after:always} }
</style>
</head>
<body>
<div id="rascunho" role="status">RASCUNHO: ainda há campos amarelos para preencher. Este aviso some quando não sobrar nenhum.</div>
<header class="top">
  <div class="inner">
    <span class="brand">Luís Felipe Azevedo Rodrigues · 221029089 · ADM0150 · 2026.2</span>
    <nav class="main" aria-label="Navegação principal">
      <a href="#abertura" data-view="abertura">Abertura</a>
      <a href="#prompt" data-view="prompt">Prompt-mestre</a>
      <a href="#protocolo" data-view="protocolo">Protocolo</a>
      <a href="#rastreabilidade" data-view="rastreabilidade">Síntese auditada</a>
      <a href="#conclusao" data-view="conclusao">Conclusão</a>
      <a href="#anexos" data-view="anexos">Anexos</a>
    </nav>
  </div>
</header>
<main>

<section id="abertura" class="view">
  <h1>Detecção de oportunidades com cadeia de IAs</h1>
  <p class="lead">Estudo de caso individual. Atividade 2 de Criação de Negócios.</p>

  <div class="card">
    <h2>Identificação</h2>
    <dl class="ident">
      <dt>Nome</dt><dd>Luís Felipe Azevedo Rodrigues</dd>
      <dt>Matrícula</dt><dd>221029089</dd>
      <dt>Disciplina</dt><dd>ADM0150 Criação de Negócios</dd>
      <dt>Semestre</dt><dd>2026.2</dd>
      <dt>Professora</dt><dd>Profa. Dra. Marina Figueiredo Moreira</dd>
      <dt>Instituição</dt><dd>Universidade de Brasília, Departamento de Administração</dd>
    </dl>
  </div>

  <h2>Objetivo do estudo</h2>
  <p>Construir um prompt que leve instrumentos das Aulas 4, 5 e 6 para a detecção de oportunidades de negócio no mercado brasileiro entre 2026 e 2030, aplicá-lo em três inteligências artificiais, entregar as três respostas a uma quarta IA para sintetizá-las e auditar essa síntese contra as respostas originais e contra fontes primárias.</p>

  <h2>A régua analítica</h2>
  <p>Estes são os instrumentos das aulas que carreguei para dentro do prompt e o lugar onde cada um entrou.</p>
  <div class="table-wrap"><table class=""><thead><tr><th scope='col'>Aula</th><th scope='col'>Instrumento</th><th scope='col'>Onde entrou no prompt</th><th scope='col'>Por que entrou</th></tr></thead><tbody><tr><td>Aula 4</td><td>Distinção entre tendência e oportunidade</td><td>Bloco DEFINIÇÕES e método de descer da tendência até um cliente nomeável</td><td><span>Usei essa distinção para impedir que a IA confundisse uma tendência ampla com uma oportunidade que tenha problema concreto, cliente identificável e alguém disposto a pagar.</span></td></tr><tr><td>Aula 4</td><td>Cinco perguntas de triagem</td><td>Filtro 1</td><td><span>Incluí as cinco perguntas para forçar a resposta a nomear quem sofre, quanto dói, quem paga, por que a oferta venceria e por que a janela se abre agora.</span></td></tr><tr><td>Aula 4</td><td>Leitura crítica de dado de mercado</td><td>Bloco PADRÃO DE EVIDÊNCIA</td><td><span>Incluí esse instrumento para não tratar número solto como prova de mercado e exigir fonte, ano, unidade e o que o dado mede, ou admitir que falta informação.</span></td></tr><tr><td>Aula 5</td><td>Checklist final de Dornelas</td><td>Filtro 2, usado como corte</td><td><span>Usei o checklist como último corte para rejeitar ideias sem problema, solução, cliente identificável, canal de venda viável ou janela aberta.</span></td></tr><tr><td>Aula 5</td><td>Os 3Ms</td><td>Filtro 3</td><td><span>Os 3Ms entraram para verificar demanda acessível e duradoura, estrutura do mercado e plausibilidade de margem e ponto de equilíbrio, em vez de confundir entusiasmo com viabilidade.</span></td></tr><tr><td>Aula 5</td><td>Critérios de alto e baixo potencial</td><td>Calibragem do Filtro 3: só critérios de lógica, sem os patamares numéricos de capital de risco</td><td><span>Usei esses critérios para calibrar a régua ao pequeno negócio: evitar ideias frágeis sem impor metas de crescimento típicas de empresas financiadas por capital de risco.</span></td></tr><tr><td>Aula 6</td><td>Definição de modelo de negócio</td><td>Bloco DEFINIÇÕES e Filtro 4: quem paga, por quê e a que custo</td><td><span>Incluí “quem paga, por quê e a que custo” para a ideia mostrar um comprador, o valor que recebe e as despesas necessárias para entregar a solução.</span></td></tr><tr><td>Aula 6</td><td>Os cinco modelos de negócio na web</td><td>Filtro 4: enquadramento ou "não se enquadra"</td><td><span>Incluí os cinco modelos para identificar a lógica real de captura de valor e evitar chamar qualquer negócio que tenha um site de marketplace ou plataforma.</span></td></tr><tr><td>Aula 6</td><td>Métrica de atenção versus métrica de negócio</td><td>Regra de métrica</td><td><span>Essa distinção entrou para barrar justificativas baseadas em audiência, seguidores ou downloads e exigir métricas ligadas a receita, custos, margem e clientes pagantes.</span></td></tr></tbody></table></div>
  <p><strong>Instrumentos deixados de fora:</strong> nenhum dos nove instrumentos da lista do enunciado foi deixado de fora.</p>
</section>


<section id="prompt" class="view">
  <h1>O prompt-mestre</h1>
  <p class="lead">O texto completo, usado sem alteração nas três IAs, e a justificativa de cada bloco.</p>

  <h2>As cinco decisões do prompt</h2>
  <div class="table-wrap"><table class=""><thead><tr><th scope='col'>Decisão pedida no enunciado</th><th scope='col'>Como ficou no prompt</th></tr></thead><tbody><tr><td>Recorte</td><td>Brasil, pequeno e médio porte, capital inicial até R$ 200 mil, horizonte 2026 a 2030.</td></tr><tr><td>Critério de corte</td><td>Os Filtros 1 a 4. Quem reprova em qualquer um vai para "Descartadas", com o filtro em que caiu.</td></tr><tr><td>Padrão de evidência</td><td>Fontes oficiais ou setoriais, com ano e o que o número mede. Sem dado confiável, a resposta deve dizer "sem dado verificável".</td></tr><tr><td>Formato de saída</td><td>Onze campos fixos por oportunidade, mais a seção "Descartadas".</td></tr><tr><td>Quantidade</td><td>Exatamente quatro oportunidades aprovadas.</td></tr></tbody></table></div>

  <h2>O prompt na íntegra</h2>
  <pre class="prompt" tabindex="0"># PAPEL
Você é um analista de oportunidades de negócio no Brasil. Seja cético e objetivo. Seu trabalho é rejeitar ideias fracas, não gerar entusiasmo.

# TAREFA
Detecte e qualifique oportunidades de negócio no Brasil para o horizonte 2026 a 2030. Entregue exatamente 4 oportunidades que passem por todos os filtros abaixo.

# RECORTE
1. Mercado: Brasil.
2. Porte: pequeno e médio negócio, capital inicial de até R$ 200 mil.
3. Horizonte: 2026 a 2030.
4. Fora do recorte: negócio que dependa de lei ainda não aprovada e negócio que só faz sentido com capital de risco.

# DEFINIÇÕES
Tendência: direção geral em que o mundo está indo. Não tem dono, preço nem cliente. Serve só como lugar para procurar.
Oportunidade: problema concreto de alguém identificável, que essa pessoa quer resolver a ponto de pagar, e que você resolve melhor do que quem já tenta.
Modelo de negócio: como a empresa gera receita e quais são os custos e investimentos necessários para isso.
Método: comece por uma tendência e desça até um problema específico com cliente que dê para nomear. &quot;As empresas&quot; ou &quot;os idosos&quot; ainda é tendência, não cliente.

# FILTROS
Aplique nesta ordem. Se a ideia reprovar em qualquer um, ela vai para &quot;Descartadas&quot;, com o filtro em que caiu.

Filtro 1, triagem. Responda as cinco perguntas, uma por uma:
1. Quem exatamente tem esse problema?
2. Quanto dói? O problema custa dinheiro, tempo ou risco?
3. Quem paga, e com que dinheiro? Em saúde, educação e setor público, quem usa raramente é quem paga.
4. Por que você, e não o concorrente já estabelecido? Se ele consegue copiar em três meses, não é oportunidade.
5. Por que agora, e não em 2020 ou 2035? Qual mudança recente abriu a janela?

Filtro 2, checklist final. Responda sim ou não, com uma frase:
a) Existe um problema a ser resolvido?
b) Existe um produto ou serviço que resolve esse problema?
c) Dá para identificar com clareza os potenciais clientes?
d) Dá para montar uma estratégia de marketing e vendas viável, em custo e retorno?
e) A janela da oportunidade está aberta?

Filtro 3, os 3Ms:
1. Demanda de mercado: qual a audiência-alvo, qual a durabilidade do produto, os clientes são acessíveis pelos canais, o custo de captação se recupera em menos de um ano?
2. Tamanho e estrutura do mercado: cresce, é emergente ou fragmentado? Há barreiras de entrada e custos de saída? Quantos concorrentes-chave existem? Em que estágio do ciclo de vida está o produto?
3. Análise de margem: quais as forças do negócio, qual a margem bruta possível, qual o ponto de equilíbrio e o retorno, como é a cadeia de valor até o cliente final?
Calibragem: use critérios de lógica (cliente identificado, concorrência não consolidada, barreira de entrada, equipe adequada). Não use patamares como &quot;crescer 30% a 50% ao ano&quot;, que vêm do capital de risco e reprovariam negócios pequenos e saudáveis.

Filtro 4, modelo de negócio. Diga quem paga, por quê e a que custo para o negócio. Depois enquadre em um destes cinco modelos: intermediação de negócios, comercialização de propaganda, mercado virtual, empresarial (empresa do mundo real que usa a web para vender mais ou gastar menos) ou redes sociais. Se não couber em nenhum, escreva &quot;não se enquadra&quot; e explique por quê, inclusive se o negócio não for digital.

Regra de métrica: é proibido justificar potencial por audiência, visitas, seguidores ou downloads. Só vale métrica de negócio: receita, custo, margem, lucro e cliente pagante.

# PADRÃO DE EVIDÊNCIA
1. Todo número precisa de fonte, ano e o que está sendo medido (volume ou faturamento, nominal ou real, e qual é o total usado como base).
2. Se houver estimativas diferentes para o mesmo mercado, mostre as duas e diga o que cada uma mede.
3. Fontes aceitas: IBGE, SEBRAE, Banco Central, ministérios, agências reguladoras, associações setoriais e legislação.
4. Marque cada dado com [BUSCA], se veio de busca na web, ou [TREINAMENTO], se veio do seu conhecimento anterior.
5. Sem dado confiável, escreva &quot;sem dado verificável&quot;. Não invente número, fonte ou link.

# FORMATO DE SAÍDA
Para cada oportunidade, use exatamente estes campos, nesta ordem:
Nome:
Tendência de origem:
Problema específico e cliente nomeável:
Cinco perguntas: (respostas de 1 a 5)
Checklist final: (a até e, sim ou não, com uma frase)
3Ms: demanda de mercado / tamanho e estrutura / margem
Modelo de negócio: quem paga / por quê / a que custo
Enquadramento nos cinco modelos:
Evidências: (número, fonte, ano, o que mede, [BUSCA] ou [TREINAMENTO])
Risco principal: (a condição que pode falhar)
Nível de certeza: alto, médio ou baixo, com o motivo.

No final, inclua a seção &quot;Descartadas&quot;: ideias que você considerou e rejeitou, com o filtro em que cada uma caiu.

# RESTRIÇÕES
1. Entregue exatamente 4 oportunidades aprovadas.
2. Não use ideias genéricas, como &quot;abrir um e-commerce&quot;, sem nicho.
3. Não escreva texto fora dessa estrutura.
4. Se faltar informação, diga o que falta em vez de supor.</pre>

  <h2>Justificativa bloco a bloco</h2>
  <div class="table-wrap"><table class=""><thead><tr><th scope='col'>Bloco do prompt</th><th scope='col'>Instrumento de aula</th><th scope='col'>Efeito esperado</th></tr></thead><tbody><tr><td>PAPEL</td><td>Postura do analista</td><td>Analista cético que rejeita em vez de gerar entusiasmo. Efeito esperado: lista curta, com descartes explícitos.</td></tr><tr><td>TAREFA e RECORTE</td><td>Decisão de recorte</td><td>Brasil, pequeno e médio porte, até R$ 200 mil, 2026 a 2030, sem depender de lei futura nem de capital de risco. Efeito esperado: evitar o lugar-comum e ideias fora da realidade de quem está começando.</td></tr><tr><td>DEFINIÇÕES</td><td>Aula 4 (tendência x oportunidade) e Aula 6 (modelo de negócio)</td><td>Obrigar a descer da tendência até um problema com cliente nomeável. Efeito esperado: ideias com cliente, não temas gerais.</td></tr><tr><td>FILTRO 1</td><td>Aula 4 (cinco perguntas de triagem)</td><td>Cada ideia vem respondida nas cinco perguntas, e não apenas descrita.</td></tr><tr><td>FILTRO 2</td><td>Aula 5 (checklist final de Dornelas)</td><td>Corte: quem reprova vai para Descartadas, com o filtro em que caiu.</td></tr><tr><td>FILTRO 3</td><td>Aula 5 (3Ms e critérios de alto e baixo potencial)</td><td>Estrutura a análise em demanda, tamanho e estrutura, e margem. A calibragem evita aplicar patamares de capital de risco a negócios pequenos.</td></tr><tr><td>FILTRO 4</td><td>Aula 6 (modelo de negócio e cinco modelos na web)</td><td>Forçar a resposta a dizer quem paga, por quê e a que custo, e a enquadrar a ideia ou justificar que não se enquadra.</td></tr><tr><td>REGRA DE MÉTRICA</td><td>Aula 6 (atenção x negócio)</td><td>Barrar justificativas por audiência, seguidores ou downloads.</td></tr><tr><td>PADRÃO DE EVIDÊNCIA</td><td>Aula 4 (leitura crítica de dado)</td><td>Todo número com fonte, ano e o que mede. Estimativas diferentes lado a lado. Marcação [BUSCA] ou [TREINAMENTO]. "Sem dado verificável" no lugar de dado inventado.</td></tr><tr><td>FORMATO DE SAÍDA</td><td>Decisão de formato</td><td>Campos fixos, para as respostas das três IAs ficarem comparáveis.</td></tr><tr><td>RESTRIÇÕES</td><td>Decisão de quantidade</td><td>Quatro oportunidades: poucas o bastante para forçar seleção. Sem ideias genéricas e sem texto fora da estrutura.</td></tr></tbody></table></div>
</section>


<section id="protocolo" class="view">
  <h1>Protocolo de teste</h1>
  <p class="lead">Ferramentas, versões, datas, condições de uso e o prompt de síntese.</p>

  <h2>Registro dos quatro usos</h2>
  <div class="table-wrap"><table class=""><thead><tr><th scope='col'>Etapa</th><th scope='col'>Ferramenta</th><th scope='col'>Versão</th><th scope='col'>Data e hora</th><th scope='col'>Busca na web</th><th scope='col'>Condição de uso</th></tr></thead><tbody><tr><td>Resposta A</td><td>Gemini</td><td>3.6 Flash</td><td>06/10/2026, 15:30</td><td>Ativada</td><td>Primeira resposta, sem refinamento</td></tr><tr><td>Resposta B</td><td>ChatGPT</td><td>GPT-5.6 Luna</td><td>06/10/2026, 15:37</td><td>Ativada</td><td>Primeira resposta, sem refinamento</td></tr><tr><td>Resposta C</td><td>Microsoft 365 Copilot</td><td>M365 Copilot</td><td>06/10/2026, 15:45</td><td>Ativada</td><td>Primeira resposta, sem refinamento</td></tr><tr><td>Síntese</td><td>Manus</td><td>2.0 Lite</td><td>06/10/2026, 15:53</td><td>Ativada</td><td>Primeira resposta ao prompt de síntese</td></tr></tbody></table></div>

  <h2>Observações do protocolo</h2>
  <ul>
    <li>O mesmo texto de prompt foi usado nas três IAs de geração (A, B e C), sem adaptação.</li>
    <li>Em cada ferramenta, a primeira resposta é a que entra na análise.</li>
    <li>No Gemini, mensagens enviadas depois no mesmo chat geraram textos fora do formato pedido. Foram descartadas e não fazem parte do estudo.</li>
    <li>A busca na web estava ativada nas quatro ferramentas, inclusive na síntese, embora o prompt de síntese mandasse não usar busca. Na auditoria não encontrei dado externo novo na síntese, então a obediência ao prompt é avaliada pelo conteúdo, e não por prova técnica.</li>
    <li>Nomes e versões seguem o que a tabela de uso informa. Não há verificação independente do identificador técnico dos modelos.</li>
  </ul>

  <h2>Prompt de síntese (quarta IA)</h2>
  <p>A quarta ferramenta é diferente das três primeiras, como exige o enunciado. O texto abaixo mostra o prompt com os campos das respostas resumidos. A versão completa, com as três respostas coladas, está no Anexo E.</p>
  <pre class="prompt" tabindex="0"># PAPEL
Você é um sintetizador. Sua única função é organizar o que três respostas de IA já disseram. Você não é analista e não dá opinião.

# ENTRADA
Abaixo estão três respostas à mesma pergunta.

&lt;resposta_A&gt;
[texto completo da resposta A, colado sem edição]
&lt;/resposta_A&gt;

&lt;resposta_B&gt;
[texto completo da resposta B, colado sem edição]
&lt;/resposta_B&gt;

&lt;resposta_C&gt;
[texto completo da resposta C, colado sem edição]
&lt;/resposta_C&gt;

# TAREFA
Produza um único panorama que reúna as três respostas.

# REGRAS
1. Use somente informação que esteja nas três respostas. Não acrescente fato, número, fonte ou oportunidade nova.
2. Atribua cada afirmação à origem, com [A], [B], [C] ou combinações como [A, C].
3. Copie números, anos e fontes exatamente como estão, inclusive as marcações [BUSCA] e [TREINAMENTO]. Não arredonde nem reescreva.
4. Preserve o grau de certeza original. Se a resposta disse &quot;sem dado verificável&quot; ou &quot;certeza baixa&quot;, mantenha.
5. Não busque consenso. Quando as respostas discordarem, mostre cada posição lado a lado, sem escolher uma.
6. Se algo relevante aparece em uma única resposta, mantenha e marque como &quot;só em [X]&quot;.
7. Se uma informação necessária não está nas respostas, escreva &quot;não consta nas respostas&quot;. Não complete.
8. Não use busca na web.

# FORMATO
1. Oportunidades em comum (2 ou 3 respostas): nome, quem citou e o que cada uma disse.
2. Oportunidades exclusivas, separadas por resposta.
3. Divergências: ponto e posição de cada resposta.
4. Dados citados: tabela com dado, fonte, ano, o que mede, marcação [BUSCA] ou [TREINAMENTO] e resposta de origem.
5. Descartadas: o que cada resposta rejeitou e em qual filtro.
6. Lacunas: o que as respostas não cobriram.
Máximo de 1500 palavras.</pre>
</section>


<section id="rastreabilidade" class="view">
  <h1>A síntese auditada</h1>
  <nav class="subnav" aria-label="Seções da síntese auditada">
    <a href="#rastreabilidade" class="sub-link">5.1 Rastreabilidade</a>
    <a href="#verificacao" class="sub-link">5.2 Verificação</a>
    <a href="#regua" class="sub-link">5.3 Aplicação da régua</a>
  </nav>
  <h2>5.1 Auditoria de rastreabilidade</h2>
  <p>A auditoria examina a primeira saída da síntese. O panorama foi revisado depois para corrigir a atribuição de “multimarcas” e completar a comparação da certeza solar; as linhas correspondentes registram o achado original e a correção. Os anexos A a C reproduzem o texto de cada resposta como foi copiado da ferramenta; não há arquivos exportados separadamente. Localização: A, linhas 8 a 33 para higienização, 37 a 62 para solar e 126 a 143 para descartes; B, linhas 189 a 229 para solar e 315 a 320 para descartes; C, linhas 382 a 417 para solar e 495 a 501 para descartes, além das seções de NR-1, NFS-e e eletroeletrônicos mencionadas no próprio anexo.</p>
  <p class="legend">Classificações: <span class="badge ok">Afirmação sustentada</span> <span class="badge warn">Afirmação distorcida</span> <span class="badge sup">Afirmação suprimida</span> <span class="badge orf">Afirmação órfã</span></p>
  <div class="table-wrap"><table class="rast"><thead><tr><th scope="col">Afirmação</th><th scope="col">Em quais das três IAs aparece</th><th scope="col">Foi reproduzida com fidelidade?</th><th scope="col">Comentário</th></tr></thead><tbody><tr><td>A base de MMGD passou de 36,2 GW para 45,0 GW entre 2024 e 2025 e chegou a 7,2 milhões de consumidores.</td><td>[B]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>A síntese preservou os dados da EPE. O cotejo adicional com o MME encontrou 44,8 GW em outra publicação oficial; a diferença de apresentação não é explicada pelas fontes consultadas (ver 5.2).</td></tr><tr><td>A síntese informa 25.429 pontos de recarga como evidência de expansão da infraestrutura.</td><td>[B]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>O número foi preservado. A ABVE especifica que são pontos públicos e semipúblicos até maio de 2026; isso não conta diretamente carregadores privados de condomínios e frotas, público-alvo da oportunidade. É contexto de expansão, não TAM direto.</td></tr><tr><td>A nova redação da NR-1 entrou em vigor em 26/05/2026.</td><td>[B]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>A data de vigência foi reproduzida corretamente e é confirmada pelo MTE; a Portaria 765/2025 explica a prorrogação (ver 5.2).</td></tr><tr><td>A Portaria MTE nº 1.419, de 27/08/2024, incluiu expressamente fatores psicossociais no GRO.</td><td>[C]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>A síntese preservou número, data e conteúdo. A fonte oficial confirma a alteração.</td></tr><tr><td>A tabela da síntese atribuiu conjuntamente a [B, C] a Portaria nº 1.419, sua data e a vigência em 2026.</td><td>[B] para a vigência; [C] para número/data da Portaria</td><td><span class="badge warn">Afirmação distorcida</span></td><td>B informa a data de vigência, mas não cita na resposta o número nem a data da Portaria. Esses dados aparecem em C. A combinação factual é compatível com a norma, mas a atribuição conjunta à origem exagera o que B disse.</td></tr><tr><td>B e C apresentam “marcos distintos” da NR-1, sem conciliação explícita.</td><td>[B, C]</td><td><span class="badge warn">Afirmação distorcida</span></td><td>Ao colocar o ponto na seção “Divergências”, a síntese sugere um desacordo. Na realidade, 27/08/2024 é a data da Portaria que aprovou a nova redação e 26/05/2026 é a vigência após a prorrogação da Portaria 765/2025; são etapas compatíveis.</td></tr><tr><td>A síntese atribui a B a proposta de O&amp;M solar “multimarcas”.</td><td>[C]</td><td><span class="badge warn">Afirmação distorcida</span></td><td>“Manutenção multimarcas” aparece explicitamente apenas em C. B propõe monitoramento, SLA, diagnóstico, inspeção e estoque mínimo de componentes comuns, mas não usa esse diferencial. Atribuição corrigida no panorama revisado.</td></tr><tr><td>A NFS-e para cobrança de taxas e demais valores condominiais começa em 01/12/2026.</td><td>[B]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>A data e o escopo foram reproduzidos; a Receita Federal/CGIBS os confirma.</td></tr><tr><td>A RAIS 2025 registrou 4,8 milhões de estabelecimentos, crescimento de 2,1%, e 59.970.945 vínculos ativos.</td><td>[B]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>Os números e o arredondamento foram preservados. O texto do PDF oficial confirma 59.970.945 vínculos em 31/12/2025 e a passagem de 4,7 para 4,8 milhões de estabelecimentos (+2,1%).</td></tr><tr><td>A resposta A estimou margem bruta acima de 60% para manutenção e lavagem solar.</td><td>[A]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>A síntese atribuiu a estimativa a A e preservou “estimada” e “acima de 60%”; não a apresentou como dado verificado.</td></tr><tr><td>A síntese diz que B e C propõem O&amp;M solar mais amplo e recorrente, enquanto A enfatiza limpeza.</td><td>[A, B, C]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>A fala em lavagem/manutenção; B em monitoramento, SLA, diagnóstico e manutenção; C em monitoramento, inspeção e coordenação. “Mais amplo” organiza os escopos sem criar número novo.</td></tr><tr><td>A síntese afirma que A não apresenta estatística verificável para suas quatro propostas.</td><td>[A]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>Em A, as quatro oportunidades trazem “Evidências: Sem dado verificável”.</td></tr><tr><td>A síntese afirma que não houve validação de piloto com clientes pagantes para coleta de eletroeletrônicos, Pix e NFS-e.</td><td>[B, C]</td><td><span class="badge ok">Afirmação sustentada</span></td><td>B e C descrevem hipóteses e pontos a validar, mas não relatam piloto pagante. É uma lacuna do material, não prova de que nenhum piloto exista.</td></tr><tr><td>C afirma que a responsabilidade legal pelos riscos psicossociais continua sendo da organização.</td><td>[C]</td><td><span class="badge sup">Afirmação suprimida</span></td><td>C explicita a ressalva ao falar de apoio à AEP/PGR; a síntese preserva a proposta de diagnóstico e acompanhamento, mas omite que terceirizar o serviço não transfere a responsabilidade legal do empregador.</td></tr><tr><td>A certeza solar de A é médio-alta, enquanto B avalia a base como alta e C dá certeza média.</td><td>[A, B, C]</td><td><span class="badge sup">Afirmação suprimida</span></td><td>A atribui certeza médio-alta à relação entre limpeza e geração (linha 62); B dá alta certeza à base instalada (linha 229); C dá média à viabilidade sem unit economics (linha 417). A síntese comparou B e C e deixou A de fora; a comparação foi completada no panorama revisado.</td></tr><tr><td>A afirma que uma interdição sanitária pode paralisar 100% do faturamento diário.</td><td>[A]</td><td><span class="badge sup">Afirmação suprimida</span></td><td>A quantifica essa dor na oportunidade de higienização de restaurantes. A síntese manteve a oportunidade exclusiva, mas omitiu a cifra.</td></tr><tr><td>A delimita restaurantes de pequeno porte com faturamento mensal de R$ 30 mil a R$ 150 mil.</td><td>[A]</td><td><span class="badge sup">Afirmação suprimida</span></td><td>A apresenta a faixa como definição do cliente na pergunta “Quem exatamente tem esse problema?”. O panorama não a registrou; ela não foi verificada como estatística de mercado.</td></tr><tr><td>A estima que o CAC da higienização pode ser recuperado em menos de 60 dias de contrato.</td><td>[A]</td><td><span class="badge sup">Afirmação suprimida</span></td><td>A menciona o prazo nos 3Ms da oportunidade. A síntese o omitiu; a própria resposta não oferece fonte setorial para essa estimativa.</td></tr><tr><td>A afirma que sujeira pode reduzir em até 30% a eficiência dos painéis solares.</td><td>[A]</td><td><span class="badge sup">Afirmação suprimida</span></td><td>A síntese omitiu a alegação. A checagem em 5.2 não conseguiu confirmá-la para o público descrito; A marca as evidências como “Sem dado verificável”.</td></tr><tr><td>As três respostas rejeitam uma proposta genérica de IA e algum marketplace.</td><td>[A, B, C]</td><td><span class="badge sup">Afirmação suprimida</span></td><td>A rejeita SaaS genérico de IA e marketplace de entulho; B, consultoria genérica de IA e marketplace de serviços para idosos; C, agência genérica de IA e marketplace nacional de resíduos eletrônicos. A síntese lista os itens por resposta, mas não destaca o padrão transversal.</td></tr><tr><td>A frase “as respostas não demonstram que sejam ofertas idênticas” aparece nas respostas originais.</td><td>Nenhuma</td><td><span class="badge orf">Afirmação órfã</span></td><td>É comentário editorial da síntese, não afirmação presente nas três respostas. Mesmo sendo plausível, deveria estar claramente separada de conteúdo atribuído às fontes.</td></tr></tbody></table></div>
  <div class="card">
    <h3>Balanço da auditoria</h3>
    <p>A tabela contém <strong>10 afirmações sustentadas, 3 distorções, 7 supressões e 1 afirmação órfã</strong>. As distorções são duas de enquadramento/atribuição na NR-1 e a atribuição a B do diferencial multimarcas, que está só em C. Entre as supressões estão a ressalva legal de C, a cifra da interdição em A, a certeza médio-alta de A e três números de A; também não foi destacada a recorrência de descartes de IA genérica e marketplaces. O panorama revisado já corrige a atribuição multimarcas e inclui a certeza de A. Não identifiquei número factual inventado, mas os números omitidos de A não devem ser tratados como verificados.</p>
  </div>
</section>

<section id="verificacao" class="view">
  <h1>A síntese auditada</h1>
  <nav class="subnav" aria-label="Seções da síntese auditada">
    <a href="#rastreabilidade" class="sub-link">5.1 Rastreabilidade</a>
    <a href="#verificacao" class="sub-link">5.2 Verificação</a>
    <a href="#regua" class="sub-link">5.3 Aplicação da régua</a>
  </nav>
  <h2>5.2 Verificação em fonte primária</h2>
  <p>Dos cinco pontos factuais originalmente confrontados, quatro foram confirmados e um permaneceu sem verificação. Reconfirmei o conteúdo diretamente nas fontes oficiais; acrescentei duas notas adicionais de contexto: o escopo do número da ABVE e a diferença entre as publicações sobre MMGD.</p>
  <div class="table-wrap"><table><thead><tr><th scope="col">Afirmação factual</th><th scope="col">Resultado</th><th scope="col">Referência primária completa</th></tr></thead><tbody>
    <tr><td>A nova redação do capítulo 1.5 da NR-1, que inclui fatores psicossociais no GRO, entrou em vigor em <b>26/05/2026</b>.</td><td><strong>Confirmou</strong></td><td>MINISTÉRIO DO TRABALHO E EMPREGO. <b>Norma Regulamentadora nº 1 (NR-1)</b>. A página vigente identifica a Portaria nº 1.419, de 27/08/2024, e registra que a vigência ocorreu após prorrogação pela Portaria nº 765, de 15/05/2025. A Portaria nº 765 prorrogou o prazo até 25/05/2026, explicando a vigência no dia seguinte. <a href="https://www.gov.br/trabalho-e-emprego/pt-br/acesso-a-informacao/participacao-social/conselhos-e-orgaos-colegiados/comissao-tripartite-partitaria-permanente/normas-regulamentadora/normas-regulamentadoras-vigentes/nr-1" rel="noopener">Página vigente da NR-1</a>; <a href="https://www.gov.br/trabalho-e-emprego/pt-br/assuntos/inspecao-do-trabalho/seguranca-e-saude-no-trabalho/sst-portarias/2025/portaria-mte-no-765-prorroga-inicio-de-vigencia-cap-1-5-da-nr-01.pdf" rel="noopener">Portaria MTE nº 765/2025, PDF</a>; <a href="https://www.gov.br/trabalho-e-emprego/pt-br/assuntos/inspecao-do-trabalho/seguranca-e-saude-no-trabalho/sst-portarias/2024/portaria-mte-no-1-419-nr-01-gro-nova-redacao.pdf" rel="noopener">Portaria MTE nº 1.419/2024, PDF</a>. Acesso em 6 out. 2026.</td></tr>
    <tr><td>A RAIS 2025 registrou <b>4,8 milhões</b> de estabelecimentos com empregados (+<b>2,1%</b>) e <b>59.970.945</b> vínculos ativos em 31/12/2025.</td><td><strong>Confirmou</strong></td><td>MINISTÉRIO DO TRABALHO E EMPREGO. <b>RAIS 2025: Sumário Executivo</b>, ano-base 2025. Baixei e extraí o texto do PDF oficial: ele informa 59.970.945 vínculos ativos e crescimento de 2,1%, de 4,7 para 4,8 milhões de estabelecimentos. <a href="https://www.gov.br/trabalho-e-emprego/pt-br/acesso-a-informacao/acoes-e-programas/programas-projetos-acoes-obras-e-atividades/estatisticas-trabalho/rais/rais-2025/sumario-executivo_rais-2025.pdf/@@download/file" rel="noopener">PDF oficial</a>. Acesso em 6 out. 2026.</td></tr>
    <tr><td>A EPE reportou <b>45,0 GW</b> de MMGD em 2025; o MME informou mais de <b>43 GW</b> no boletim e <b>44,8 GW</b> (18,1%) em dezembro de 2025 na ata do CMSE.</td><td><strong>Confirmou, com ressalva</strong></td><td>EMPRESA DE PESQUISA ENERGÉTICA. <b>EPE atualiza painel interativo sobre MMGD com dados de 2025</b>, 10 abr. 2026. <a href="https://www.epe.gov.br/pt/imprensa/noticias/epe-atualiza-painel-interativo-sobre-mmgd-com-dados-de-2025" rel="noopener">EPE</a>. MINISTÉRIO DE MINAS E ENERGIA. <b>Boletim Especial de Consolidação 2025</b>; informa mais de 43 GW, 16,8% da matriz e crescimento de 24% sobre 2024. <a href="https://www.gov.br/mme/pt-br/assuntos/secretarias/secretaria-nacional-energia-eletrica/publicacoes/boletim-anual-de-monitoramento-do-sistema-eletrico/consolidacao-2025/boletim-especial-consolidacao-2025_snee-ddos-defeso.pdf/@@download/file" rel="noopener">PDF do MME</a>. MME/CMSE. <b>Ata da 320ª Reunião Ordinária</b>, com 44,8 GW (18,1%) em dez. 2025. <a href="https://www.gov.br/mme/pt-br/assuntos/conselhos-e-comites/cmse/atas/2026-2/ata-da-320a-reuniao-do-cmse-ordinaria.pdf" rel="noopener">PDF da ata</a>. Os valores oficiais são próximos, mas a diferença entre 44,8 e 45,0 GW não é reconciliada nas publicações consultadas; não trato isso como contradição resolvida nem atribuo a diferença a arredondamento sem fonte. Acesso em 6 out. 2026.</td></tr>
    <tr><td>A NFS-e para cobrança de taxas e demais valores condominiais começa em <b>01/12/2026</b>.</td><td><strong>Confirmou</strong></td><td>RECEITA FEDERAL DO BRASIL; COMITÊ GESTOR DO IBS. <b>Cronograma de Implementação dos Documentos Fiscais Eletrônicos da Reforma Tributária do Consumo</b>, Ato Conjunto RFB/CGIBS nº 4, de 30 jul. 2026. O cronograma lista a NFS-e condominial com início em 01/12/2026. <a href="https://www.gov.br/receitafederal/pt-br/assuntos/noticias/2026/julho/receita-federal-e-comite-gestor-do-ibs-publicam-o-cronograma-de-implementacao-dos-documentos-fiscais-eletronicos-da-reforma-tributaria-do-consumo" rel="noopener">Página oficial</a>. Acesso em 6 out. 2026.</td></tr>
    <tr><td>A base tinha <b>25.429 pontos públicos e semipúblicos</b> de recarga, com dados até maio de 2026.</td><td><strong>Confirmou; escopo não equivale ao mercado-alvo</strong></td><td>ASSOCIAÇÃO BRASILEIRA DO VEÍCULO ELÉTRICO (ABVE) e TUPI MOBILIDADE. <b>Recarga rápida (DC) cresce 33% em três meses e puxa a expansão da rede</b>, 22 jun. 2026. A fonte define os 25.429 como pontos públicos e semipúblicos. Isso não mede diretamente carregadores privados internos de condomínios ou pequenas frotas; sustenta expansão de infraestrutura, mas não dimensiona por si só o mercado atendível pela tese de O&amp;M de B. <a href="https://abve.org.br/recarga-rapida-dc-cresce-33-em-tres-meses-e-puxa-a-expansao-da-rede/" rel="noopener">Página da ABVE</a>. Acesso em 6 out. 2026.</td></tr>
    <tr><td>A sujeira pode reduzir em “até <b>30%</b>” a eficiência de painéis no público descrito por A.</td><td><strong>Não foi possível verificar</strong></td><td>A não apresenta referência para a cifra. Não consegui confirmar em fonte primária acessível que esse percentual se aplique a residências e pequenos comércios brasileiros. A EPE confirma crescimento da base de MMGD, não perdas por sujeira; a própria A marca suas evidências como “Sem dado verificável”.</td></tr>
  </tbody></table></div>
</section>

<section id="regua" class="view">
  <h1>A síntese auditada</h1>
  <nav class="subnav" aria-label="Seções da síntese auditada">
    <a href="#rastreabilidade" class="sub-link">5.1 Rastreabilidade</a>
    <a href="#verificacao" class="sub-link">5.2 Verificação</a>
    <a href="#regua" class="sub-link">5.3 Aplicação da régua</a>
  </nav>

  <h2>5.3 Aplicação da régua</h2>
  <p>As três oportunidades escolhidas são: <strong>gestão de riscos psicossociais da NR-1</strong>, por combinar obrigação regulatória concreta e recorrência; <strong>O&amp;M solar</strong>, por haver base instalada mensurável nas três respostas; e <strong>transição fiscal e NFS-e para administradoras de condomínios</strong>, por ter prazo regulatório próximo e possibilidade de vender por carteira. A aplicação abaixo é uma avaliação própria, não uma reprodução automática das respostas.</p>
  
<div class="card">
  <h3>1. Gestão terceirizada de riscos psicossociais da NR-1</h3>
  <p><strong>Checklist de Dornelas.</strong></p>
  <ul class='check'><li>(a) <b>Sim</b>: há problema concreto de identificação, avaliação, prevenção, documentação e acompanhamento.</li><li>(b) <b>Sim</b>: diagnóstico, plano de ação, treinamento e acompanhamento atacam o problema.</li><li>(c) <b>Sim</b>: é possível recortar por setor, porte e número de empregados.</li><li>(d) <b>Sim, condicionalmente</b>: canais por contabilidades, clínicas de SST e prospecção direta são plausíveis, mas CAC e disposição a pagar exigem piloto.</li><li>(e) <b>Sim</b>: a vigência de 26/05/2026 cria janela comercial atual.</li></ul>
  <p><strong>Modelo de negócio.</strong> Quem paga é a PME empregadora, diretamente ou por parceiro de SST. Paga para cumprir e demonstrar a gestão contínua dos riscos. O custo é equipe técnica, psicólogo ou engenheiro parceiro, software, documentação, visitas, seguro e aquisição de clientes. A responsabilidade legal permanece da empresa contratante, mesmo com terceirização.</p>
  <p><strong>Pontos de falha.</strong> Virar uma consultoria genérica ou apenas um documento barato. Preço, margem, ticket e payback não estão comprovados. <strong>Sobrevive à régua, mas precisa de especialização setorial e entrega operacional mensurável.</strong></p>
</div>

  
<div class="card">
  <h3>2. O&amp;M independente de sistemas solares</h3>
  <p><strong>Checklist de Dornelas.</strong></p>
  <ul class='check'><li>(a) <b>Sim</b>: falhas, sujeira, baixa performance e indisponibilidade geram perda econômica.</li><li>(b) <b>Sim</b>: monitoramento, inspeção, limpeza quando necessária e manutenção corretiva e preventiva resolvem o problema.</li><li>(c) <b>Sim</b>: clientes podem ser delimitados por tipo de ativo, município, potência e perfil comercial.</li><li>(d) <b>Sim, condicionalmente</b>: instaladores, administradores de imóveis e prospecção geográfica são canais possíveis, mas a resposta do cliente e a frequência de contratação precisam ser testadas.</li><li>(e) <b>Sim</b>: há estoque instalado mensurável, não apenas venda futura de novos sistemas.</li></ul>
  <p><strong>Modelo de negócio.</strong> Quem paga é o titular ou gestor do imóvel ou da usina. Paga para reduzir indisponibilidade e proteger a economia do ativo. O custo é técnico qualificado, deslocamento, ferramentas, software, seguro e peças. A receita pode ser contrato recorrente mais serviços corretivos, mas os números de margem e equilíbrio permanecem não verificados.</p>
  <p><strong>Pontos de falha.</strong> Muitos instaladores já oferecem pós-venda (afirmação de B, não verificada). Limpeza simples é copiável. Rotas dispersas destroem a economia. <strong>Sobrevive à régua somente com densidade local, histórico de performance e diferenciação multimarcas.</strong></p>
</div>

  
<div class="card">
  <h3>3. Transição fiscal e NFS-e para administradoras de condomínios</h3>
  <p><strong>Checklist de Dornelas.</strong></p>
  <ul class='check'><li>(a) <b>Sim</b>: existe mudança operacional concreta com data publicada.</li><li>(b) <b>Sim</b>: diagnóstico, parametrização, integração, testes e suporte resolvem tarefas definidas.</li><li>(c) <b>Sim</b>: administradoras são compradoras nomeáveis e uma venda pode alcançar vários condomínios.</li><li>(d) <b>Sim, condicionalmente</b>: contadores, softwares condominiais e associações podem reduzir aquisição, mas concorrência, preço e ciclo de vendas não foram medidos.</li><li>(e) <b>Sim</b>: o marco de 01/12/2026 é próximo e oficial.</li></ul>
  <p><strong>Modelo de negócio.</strong> Quem paga é a administradora, ou o condomínio se o contrato permitir repasse. Paga para adaptar a emissão fiscal sem criar equipe interna. O custo é especialista tributário, integração e desenvolvimento, implantação, suporte e aquisição comercial. A oferta deve vender acompanhamento da transição, não apenas configuração pontual.</p>
  <p><strong>Pontos de falha.</strong> Leiautes e cronogramas podem mudar. Administradoras podem preferir que o fornecedor do sistema resolva. Não há preço setorial verificável. <strong>Sobrevive à régua, mas depende fortemente de integração e distribuição por carteira.</strong></p>
</div>


  <h2>Qual eu defenderia</h2>
  <p>Eu defenderia <strong>gestão especializada de riscos psicossociais da NR-1</strong>, com recorte inicial em um ou dois setores e venda por parceiros de SST e contabilidade. É a alternativa com melhor combinação entre dor obrigatória, comprador identificável, processo recorrente e possibilidade de documentação contínua. A preferência é <strong>condicional</strong>: não há base para afirmar margem ou demanda efetiva das PMEs antes de um piloto.</p>
  <p>O O&amp;M solar tem a melhor evidência de base física, mas enfrenta maior risco de substituição pelo instalador e exige logística de campo. A NFS-e condominial tem gatilho temporal forte e pode alcançar carteiras, mas é mais dependente de mudanças de leiaute, fornecedores de software e capacidade técnica de integração. Portanto, nenhuma das três está validada economicamente. A primeira apenas parece mais defensável na régua qualitativa.</p>
</section>


<section id="conclusao" class="view">
  <h1>Conclusão</h1>

  <h2>A oportunidade que defendo</h2>
  <p>Defendo a <strong>gestão especializada de riscos psicossociais da NR-1</strong>, para PMEs de um ou dois setores, vendida por parceiros de SST e contabilidade. A evidência que a sustenta é regulatória: a Portaria MTE nº 1.419/2024 incluiu os fatores psicossociais no gerenciamento de riscos ocupacionais e a nova redação entrou em vigor em 26/05/2026, conforme a página oficial do MTE (referência completa em 5.2). Ela passou no checklist de Dornelas e na pergunta do modelo de negócio, mas com condição: margem, ticket e disposição a pagar ainda precisam de um piloto.</p>

  <h2>O que aprendi sobre encadear IAs</h2>
  <p>Aprendi que encadear IAs ajuda a organizar respostas, mas a síntese não é automaticamente fiel. Na primeira versão, ela atribuiu a B o diferencial “multimarcas”, que só aparece em C; comparou a certeza de B e C e deixou de fora o médio-alto de A; e suprimiu três números de A: faturamento-alvo de R$ 30 mil a R$ 150 mil, CAC recuperado em menos de 60 dias e perda de até 30% por sujeira. Também não destacou que as três respostas rejeitaram IA genérica e algum marketplace. Percebi ainda as omissões da responsabilidade legal mantida pela organização em C e da cifra de 100% do faturamento diário em caso de interdição em A. A primeira síntese tratou os marcos da NR-1 como divergência e exagerou a atribuição da Portaria a B; o panorama revisado corrige a atribuição multimarcas e inclui a comparação de certeza que faltava.</p>
  <p>Ao conferir fontes primárias, confirmei a sequência normativa da NR-1: a Portaria 1.419/2024 aprovou a redação e a Portaria 765/2025 prorrogou sua vigência para 26/05/2026. Também conferi os números da RAIS no próprio PDF. Não consegui verificar a alegação de perda solar de até 30% para o público descrito. A ABVE confirma 25.429 pontos, mas o indicador cobre pontos públicos e semipúblicos, não o universo privado de condomínios e frotas. EPE e MME publicam 45,0 GW e 44,8 GW, respectivamente, para 2025; os valores são próximos, mas a diferença não está reconciliada nas fontes consultadas. Por fim, percebi que busca ligada não garante comportamento uniforme nem veracidade automática: a síntese tinha instrução para não pesquisar e não há prova técnica de uso; conferir as fontes continua indispensável.</p>

  <p class="note">Todas as afirmações factuais deste site têm fonte citada na seção 5.2. As quatro saídas de IA estão nos anexos e foram tratadas como objeto de estudo, não como texto do trabalho.</p>
</section>


<section id="anexos" class="view">
  <h1>Anexos</h1>
  <p class="lead">As quatro respostas completas, sem edição, estão em um PDF entregue à parte pelo Aprender.</p>
  <ul>
    <li><strong>Anexo A:</strong> resposta da IA A (Gemini 3.6 Flash).</li>
    <li><strong>Anexo B:</strong> resposta da IA B (ChatGPT GPT-5.6 Luna).</li>
    <li><strong>Anexo C:</strong> resposta da IA C (Microsoft 365 Copilot).</li>
    <li><strong>Anexo D:</strong> saída completa da síntese (Manus 2.0 Lite).</li>
    <li><strong>Anexo E:</strong> prompt de síntese completo, com linhas numeradas. As referências de linhas da seção 5.1 apontam para este anexo.</li>
  </ul>
</section>

</main>
<footer>Trabalho acadêmico individual. Universidade de Brasília, Departamento de Administração. Profa. Dra. Marina Figueiredo Moreira.</footer>
<script>
(function(){
  var views = Array.prototype.slice.call(document.querySelectorAll('.view'));
  var ids = views.map(function(v){return v.id});
  var subs = ['rastreabilidade','verificacao','regua'];
  function show(){
    var h = (location.hash||'#abertura').replace('#','');
    if(ids.indexOf(h) < 0) h = 'abertura';
    views.forEach(function(v){ v.classList.toggle('active', v.id===h); });
    var group = subs.indexOf(h) >= 0 ? 'rastreabilidade' : h;
    document.querySelectorAll('nav.main a').forEach(function(a){
      var t = a.getAttribute('data-view');
      if(t===group) a.setAttribute('aria-current','page'); else a.removeAttribute('aria-current');
    });
    document.querySelectorAll('.subnav a').forEach(function(a){
      if(a.getAttribute('href')==='#'+h) a.setAttribute('aria-current','page'); else a.removeAttribute('aria-current');
    });
    window.scrollTo(0,0);
  }
  window.addEventListener('hashchange', show);
  show();
  var banner = document.getElementById('rascunho');
  if(banner && document.querySelectorAll('.preencher').length === 0){ banner.style.display='none'; }
})();
</script>
</body>
</html>
