<!--
Front-end implementation for "Projeto Integrador Transdisciplinar em Engenharia de Software II"
Single-file demo (HTML/CSS/JS). No back-end required.
Features:
 - Responsive layout (header, nav, main)
 - Project overview and PDF viewer link placeholder
 - Interface design checklist and UX guidance
 - Forms: Projeto (meta), Dicionario de Dados (simple editor), Testes por Pares (feedback form)
 - Simulated save using localStorage (so it "persists" on the client only)
 - Validation, accessible labels, modals, and helpful UI affordances
 - Exports: generate a JSON file of the current project data

How to use: save this file as index.html and open it in your browser.
-->

<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Projeto Integrador — Front-end</title>
  <style>
    :root{ --accent:#2563eb; --muted:#6b7280; --bg:#f8fafc; --card:#ffffff; }
    *{box-sizing:border-box}
    body{font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial; margin:0; background:var(--bg); color:#111827}
    header{background:linear-gradient(90deg,#083e8a, #2563eb); color:white; padding:20px 24px}
    .container{max-width:1100px;margin:20px auto;padding:0 16px}
    .top{display:flex;gap:16px;align-items:center}
    h1{margin:0;font-size:20px}
    nav{margin-left:auto}
    nav a{color:rgba(255,255,255,0.9);margin-left:12px;text-decoration:none;font-weight:600}
    main{display:grid;grid-template-columns:320px 1fr;gap:20px;margin-top:18px}

    /* Sidebar */
    .card{background:var(--card);border-radius:12px;padding:16px;box-shadow:0 6px 18px rgba(15,23,42,0.06)}
    .sidebar .section + .section{margin-top:12px}
    .nav-item{display:block;padding:8px;border-radius:8px;color:#0f172a;text-decoration:none}
    .nav-item:hover{background:#f1f5f9}

    /* Content */
    .toolbar{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
    .btn{background:var(--accent);color:#fff;padding:8px 12px;border-radius:8px;border:none;cursor:pointer;font-weight:600}
    .btn.ghost{background:transparent;color:var(--accent);border:1px solid rgba(37,99,235,0.15)}
    h2{margin:0 0 12px 0}
    .grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
    label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
    input[type=text], textarea, select{width:100%;padding:10px;border-radius:8px;border:1px solid #e6edf3;background:#fff}
    textarea{min-height:120px;resize:vertical}

    /* list & feedback */
    .list{display:flex;flex-direction:column;gap:8px}
    .item{padding:10px;border-radius:8px;border:1px solid #eef2ff;background:#fbfdff}
    .muted{color:var(--muted);font-size:13px}

    /* small screens */
    @media (max-width:900px){main{grid-template-columns:1fr;}.grid{grid-template-columns:1fr}}

    /* footer */
    footer{margin-top:24px;text-align:center;color:var(--muted);font-size:13px}

    /* modal */
    .modal-backdrop{position:fixed;inset:0;background:rgba(2,6,23,0.5);display:none;align-items:center;justify-content:center}
    .modal{background:white;padding:18px;border-radius:10px;max-width:720px;width:92%;box-shadow:0 18px 40px rgba(2,6,23,0.35)}

    .kbd{display:inline-block;background:#111827;color:white;padding:2px 6px;border-radius:6px;font-size:12px}
    .small{font-size:13px}
  </style>
</head>
<body>
  <header>
    <div class="container top">
      <div>
        <h1>Projeto Integrador — Front-end</h1>
        <div class="small">Implementação front-end baseada no material do PDF enviado</div>
      </div>
      <nav>
        <a href="#overview">Visão</a>
        <a href="#projeto">Projeto</a>
        <a href="#dicionario">Dicionário</a>
        <a href="#feedback">Testes</a>
      </nav>
    </div>
  </header>

  <div class="container">
    <main>
      <aside class="sidebar">
        <div class="card">
          <div style="display:flex;align-items:center;gap:12px">
            <div style="width:52px;height:52px;border-radius:10px;background:linear-gradient(180deg,#e0f2fe,#bfdbfe);display:flex;align-items:center;justify-content:center;font-weight:700;color:#084c8a">PI</div>
            <div>
              <div style="font-weight:700">Projeto Integrador</div>
              <div class="muted">Front-end demo • local</div>
            </div>
          </div>

          <div class="section" style="margin-top:12px">
            <strong>Atalhos</strong>
            <div class="muted" style="margin-top:8px">Salvar local: <span class="kbd">Ctrl/Cmd + S</span></div>
            <div class="muted">Exportar JSON: clique em "Exportar"</div>
          </div>

          <div class="section" style="margin-top:12px">
            <strong>Conteúdos</strong>
            <a class="nav-item" href="#overview">Visão geral</a>
            <a class="nav-item" href="#projeto">Formulário do Projeto</a>
            <a class="nav-item" href="#dicionario">Dicionário de Dados</a>
            <a class="nav-item" href="#feedback">Testes por Pares</a>
          </div>
        </div>

        <div class="card" style="margin-top:12px">
          <strong>Checklist UX</strong>
          <ul style="margin-top:8px;padding-left:18px;color:var(--muted)">
            <li>Clareza</li>
            <li>Consistência</li>
            <li>Simplicidade</li>
            <li>Feedback</li>
            <li>Testes com usuários</li>
          </ul>
        </div>

        <div class="card" style="margin-top:12px">
          <strong>Links úteis</strong>
          <div class="muted" style="margin-top:8px">Repositório sugerido: <a href="https://github.com/" target="_blank">github.com</a></div>
          <div class="muted" style="margin-top:6px">PDF fonte: (upload original)</div>
        </div>
      </aside>

      <section>
        <div id="overview" class="card">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <div>
              <h2>Visão geral do projeto</h2>
              <div class="muted">Repositório front-end para evidenciar entregas solicitadas no PDF.</div>
            </div>
            <div class="toolbar">
              <button class="btn" id="exportBtn">Exportar JSON</button>
              <button class="btn ghost" id="openPdf">Abrir PDF (local)</button>
            </div>
          </div>

          <hr style="margin:12px 0;border:none;border-top:1px solid #eef2ff" />

          <div class="grid">
            <div class="card">
              <strong>Objetivos</strong>
              <p class="muted">Revisar planejamento, codificar front-end, coletar feedbacks de 5 colegas e documentar os testes.</p>

              <strong style="margin-top:8px;display:block">Entregáveis</strong>
              <ul style="margin-top:8px;padding-left:18px;color:var(--muted)">
                <li>Código front-end (este HTML)</li>
                <li>Dicionário de dados (formulário)</li>
                <li>Formulário de testes por pares</li>
                <li>Exportação das opiniões (JSON)</li>
              </ul>
            </div>

            <div class="card">
              <strong>Como usar</strong>
              <ol style="margin-top:8px;padding-left:18px;color:var(--muted)">
                <li>Preencha o formulário do projeto.</li>
                <li>Adicione itens ao dicionário de dados.</li>
                <li>Compartilhe a página com colegas para coletar testes.</li>
                <li>Exporte JSON e suba para o seu repositório.</li>
              </ol>
            </div>
          </div>
        </div>

        <div id="projeto" class="card" style="margin-top:16px">
          <h2>Formulário do Projeto</h2>
          <div class="muted">Preencha as informações do seu Projeto Integrador.</div>
          <form id="projectForm" style="margin-top:12px">
            <label for="title">Nome do Projeto</label>
            <input id="title" name="title" type="text" placeholder="Ex: Aplicativo de Agendamento Social" required />

            <label for="summary" style="margin-top:8px">Resumo (breve)</label>
            <textarea id="summary" name="summary" placeholder="Descreva em 2-3 linhas..."></textarea>

            <div style="display:flex;gap:8px;margin-top:10px;align-items:center">
              <button class="btn" type="button" id="saveProject">Salvar local</button>
              <button class="btn ghost" type="button" id="resetProject">Limpar</button>
            </div>
          </form>
        </div>

        <div id="dicionario" class="card" style="margin-top:16px">
          <h2>Dicionário de Dados (simples)</h2>
          <p class="muted">Adicione atributos que serão usados no modelo relacional.</p>

          <form id="fieldForm" style="margin-top:8px;display:grid;gap:8px">
            <label for="entity">Entidade</label>
            <input id="entity" placeholder="Ex: Usuario" />

            <label for="attribute">Atributo</label>
            <input id="attribute" placeholder="Ex: nome" />

            <label for="type">Tipo de Dado</label>
            <select id="type">
              <option value="VARCHAR">VARCHAR</option>
              <option value="INT">INT</option>
              <option value="DATE">DATE</option>
              <option value="BOOLEAN">BOOLEAN</option>
            </select>

            <div style="display:flex;gap:8px">
              <button class="btn" type="button" id="addField">Adicionar</button>
              <button class="btn ghost" type="button" id="clearFields">Limpar lista</button>
            </div>
          </form>

          <div style="margin-top:12px">
            <strong>Campos adicionados</strong>
            <div id="fieldsList" class="list" style="margin-top:8px"></div>
          </div>
        </div>

        <div id="feedback" class="card" style="margin-top:16px">
          <h2>Formulário de Testes por Pares</h2>
          <p class="muted">Colete 5 opiniões conforme solicitado pelo PDF.</p>

          <form id="feedbackForm" style="display:grid;gap:8px">
            <label for="tester">Nome de quem testou</label>
            <input id="tester" />

            <label for="dateTest">Data do teste</label>
            <input id="dateTest" type="date" />

            <label for="testedOk">O que testou e funcionou</label>
            <textarea id="testedOk"></textarea>

            <label for="testedFail">O que testou e NÃO funcionou</label>
            <textarea id="testedFail"></textarea>

            <label for="notTested">Funcionalidade não testada</label>
            <textarea id="notTested"></textarea>

            <div style="display:flex;gap:8px;margin-top:6px">
              <button class="btn" type="button" id="addFeedback">Adicionar opinião</button>
              <button class="btn ghost" type="button" id="exportFeedback">Exportar opiniões</button>
            </div>
          </form>

          <div style="margin-top:12px">
            <strong>Opiniões coletadas</strong>
            <div id="feedbackList" class="list" style="margin-top:8px"></div>
          </div>
        </div>

        <footer>
          <div>Feito por você • Abra este arquivo localmente e personalize conforme o seu projeto.</div>
        </footer>

      </section>
    </main>
  </div>

  <!-- modal for PDF viewer (placeholder) -->
  <div id="modal" class="modal-backdrop">
    <div class="modal">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <strong>Visualizador de PDF (local)</strong>
        <button id="closeModal" class="btn ghost">Fechar</button>
      </div>
      <div style="margin-top:12px;color:var(--muted)">Este front-end é estático: para abrir o PDF carregue o arquivo no mesmo diretório e clique em "Abrir PDF". Se preferir, faça upload no repositório Git.</div>
    </div>
  </div>

  <script>
    // Basic client-side "storage" for demo purposes
    const state = {
      project: JSON.parse(localStorage.getItem('pi_project')||'null') || {title:'',summary:''},
      fields: JSON.parse(localStorage.getItem('pi_fields')||'[]'),
      feedbacks: JSON.parse(localStorage.getItem('pi_feedbacks')||'[]')
    }

    // Populate forms from state
    function renderState(){
      document.getElementById('title').value = state.project.title || ''
      document.getElementById('summary').value = state.project.summary || ''

      const fl = document.getElementById('fieldsList'); fl.innerHTML = '';
      state.fields.forEach((f,i)=>{
        const div = document.createElement('div'); div.className='item';
        div.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><div><strong>${f.entity}</strong>.<span style='color:var(--muted)'>${f.attribute}</span> <div class='muted'>${f.type}</div></div><div><button class='btn ghost' data-index='${i}'>Remover</button></div></div>`;
        fl.appendChild(div)
      })

      const fb = document.getElementById('feedbackList'); fb.innerHTML = '';
      state.feedbacks.forEach((f,i)=>{
        const item = document.createElement('div'); item.className='item';
        item.innerHTML = `<div style='display:flex;justify-content:space-between;'><div><strong>${f.tester}</strong> <div class='muted'>${f.date}</div></div><div><button class='btn ghost' data-fb='${i}'>Remover</button></div></div><div style='margin-top:6px'><strong>O que funcionou</strong><div class='muted'>${escapeHtml(f.ok)}</div><strong style='margin-top:6px;display:block'>O que não funcionou</strong><div class='muted'>${escapeHtml(f.fail)}</div></div>`;
        fb.appendChild(item)
      })
    }

    function escapeHtml(s){ return (s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;') }

    // Save project
    document.getElementById('saveProject').addEventListener('click',()=>{
      state.project.title = document.getElementById('title').value.trim()
      state.project.summary = document.getElementById('summary').value.trim()
      localStorage.setItem('pi_project', JSON.stringify(state.project))
      alert('Projeto salvo localmente (localStorage).')
      renderState()
    })

    document.getElementById('resetProject').addEventListener('click',()=>{
      if(!confirm('Limpar formulário do projeto?')) return;
      state.project = {title:'',summary:''}
      localStorage.setItem('pi_project', JSON.stringify(state.project))
      renderState()
    })

    // Fields
    document.getElementById('addField').addEventListener('click',()=>{
      const entity = document.getElementById('entity').value.trim();
      const attribute = document.getElementById('attribute').value.trim();
      const type = document.getElementById('type').value;
      if(!entity || !attribute){ alert('Preencha entidade e atributo.'); return }
      state.fields.push({entity,attribute,type})
      localStorage.setItem('pi_fields', JSON.stringify(state.fields))
      document.getElementById('entity').value=''; document.getElementById('attribute').value='';
      renderState()
    })

    document.getElementById('clearFields').addEventListener('click',()=>{
      if(!confirm('Excluir todos os campos adicionados?')) return;
      state.fields = []
      localStorage.setItem('pi_fields', JSON.stringify(state.fields))
      renderState()
    })

    // remove field (event delegation)
    document.getElementById('fieldsList').addEventListener('click', (e)=>{
      if(e.target && e.target.matches('button')){
        const idx = Number(e.target.dataset.index)
        state.fields.splice(idx,1)
        localStorage.setItem('pi_fields', JSON.stringify(state.fields))
        renderState()
      }
    })

    // feedback
    document.getElementById('addFeedback').addEventListener('click',()=>{
      const tester = document.getElementById('tester').value.trim();
      const date = document.getElementById('dateTest').value || new Date().toISOString().slice(0,10);
      const ok = document.getElementById('testedOk').value.trim();
      const fail = document.getElementById('testedFail').value.trim();
      const notTested = document.getElementById('notTested').value.trim();
      if(!tester){ alert('Informe o nome de quem testou.'); return }
      state.feedbacks.push({tester,date,ok,fail,notTested})
      localStorage.setItem('pi_feedbacks', JSON.stringify(state.feedbacks))
      // clear
      document.getElementById('tester').value=''; document.getElementById('dateTest').value=''; document.getElementById('testedOk').value=''; document.getElementById('testedFail').value=''; document.getElementById('notTested').value=''
      renderState()
    })

    // remove feedback
    document.getElementById('feedbackList').addEventListener('click', (e)=>{
      if(e.target && e.target.matches('button')){
        const idx = Number(e.target.dataset.fb)
        state.feedbacks.splice(idx,1)
        localStorage.setItem('pi_feedbacks', JSON.stringify(state.feedbacks))
        
