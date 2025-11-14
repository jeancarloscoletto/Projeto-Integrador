<!doctype html>
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
renderState()
}
})


// export JSON of entire project
document.getElementById('exportBtn').addEventListener('click', ()=>{
const payload = {project:state.project,fields:state.fields,feedbacks:state.feedbacks,exportedAt:new Date().toISOString()}
downloadJson(payload, 'projeto_integrador_export.json')
})


document.getElementById('exportFeedback').addEventListener('click', ()=>{
const payload = {feedbacks:state.feedbacks, exportedAt:new Date().toISOString()}
downloadJson(payload, 'feedbacks_projeto.json')
})


function downloadJson(obj, filename){
const blob = new Blob([JSON.stringify(obj,null,2)],{type:'application/json'})
const url = URL.createObjectURL(blob)
const a = document.createElement('a'); a.href=url; a.download=filename; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url)
}


// modal actions
document.getElementById('openPdf').addEventListener('click', ()=>{
document.getElementById('modal').style.display='flex'
})
document.getElementById('closeModal').addEventListener('click', ()=>{
document.getElementById('modal').style.display='none'
})


// keyboard shortcut save
window.addEventListener('keydown', (e)=>{
if((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === 's'){
e.preventDefault(); document.getElementById('saveProject').click();
}
})


// initial render
renderState()
</script>
</body>
</html>
