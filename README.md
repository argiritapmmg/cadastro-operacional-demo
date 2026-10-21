
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Cadastro Operacional - Protótipo</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#f1f4f8;color:#182230;font-family:Arial,sans-serif}
header{background:#183b32;color:white;padding:20px}
header h1{font-size:21px;margin:0 0 7px}
header p{font-size:13px;margin:0;color:#d8e7df}
main{max-width:700px;margin:auto;padding:15px}
.notice{background:#fff4d6;border:1px solid #e8d39a;padding:12px;border-radius:9px;font-size:13px;line-height:1.5;margin-bottom:15px}
.card{background:white;border:1px solid #dce2e9;border-radius:12px;padding:16px;margin-bottom:15px}
h2{font-size:17px;margin:0 0 14px}
label{display:block;font-size:13px;font-weight:bold;margin:12px 0 6px}
input,textarea,select{width:100%;padding:12px;border:1px solid #c7d0da;border-radius:8px;background:white;font:inherit}
textarea{min-height:85px;resize:vertical}
button{border:0;border-radius:8px;padding:11px 14px;font-weight:bold;cursor:pointer;margin-top:10px}
.primary{background:#1d5545;color:white;width:100%}
.secondary{background:#e7edf2;color:#243746}
.danger{background:#fbe7e7;color:#8b2020}
.record{border:1px solid #dce2e9;border-radius:9px;padding:13px;margin-top:10px;overflow-wrap:anywhere}
.record h3{font-size:16px;margin:0 0 8px}
.record-photo{display:block;width:120px;height:120px;object-fit:cover;border-radius:10px;margin:0 auto 12px;background:#e7edf2;border:1px solid #dce2e9}
.meta{font-size:12px;color:#596879;line-height:1.7}
.detail{font-size:14px;line-height:1.6;overflow-wrap:anywhere}
.actions{display:flex;gap:8px;flex-wrap:wrap}
.actions button{margin-top:8px}
.status{font-size:12px;color:#315c4d}
#photoPreview{display:none;max-width:100%;max-height:240px;object-fit:contain;margin-top:12px;border-radius:8px}
footer{text-align:center;color:#687687;font-size:12px;padding:15px}
</style>
</head>
<body>
<header>
  <h1>Cadastro Operacional</h1>
  <p>Protótipo demonstrativo • Dados exclusivamente fictícios</p>
</header>

<main>
  <div class="notice">
    <strong>AMBIENTE DE TESTE — NÃO UTILIZE DADOS REAIS</strong><br>
    Esta página não possui login, servidor ou criptografia específica
    dos registros. Os dados e as fotos ficam no armazenamento local
    deste navegador. Use somente informações e imagens fictícias.
  </div>

  <section class="card">
    <h2 id="formTitle">Novo registro fictício</h2>
    <form id="recordForm">
      <input type="hidden" id="editId">

      <label for="name">Nome *</label>
      <input id="name" maxlength="100" required placeholder="Nome fictício">

      <label for="mother">Nome da mãe</label>
      <input id="mother" maxlength="100" placeholder="Nome fictício da mãe">

      <label for="nickname">Apelido</label>
      <input id="nickname" maxlength="80" placeholder="Apelido inventado">

      <label for="cpf">CPF fictício</label>
      <input id="cpf" maxlength="20" autocomplete="off" placeholder="Identificador de teste">

      <label for="rg">RG fictício</label>
      <input id="rg" maxlength="30" autocomplete="off" placeholder="Identificação inventada">

      <label for="address">Endereço fictício</label>
      <textarea id="address" maxlength="300" placeholder="Endereço inventado para demonstração"></textarea>

      <label for="involved">Envolvido em</label>
      <textarea id="involved" maxlength="500" placeholder="Descrição inteiramente fictícia"></textarea>

      <label for="photo">Anexar foto de demonstração</label>
      <input id="photo" type="file" accept="image/*">
      <p class="meta">Use somente uma imagem fictícia. A foto será guardada localmente neste protótipo.</p>
      <img id="photoPreview" alt="Prévia da imagem selecionada">

      <label for="status">Situação do registro</label>
      <select id="status">
        <option value="Em revisão">Em revisão</option>
        <option value="Ativo">Ativo</option>
        <option value="Arquivado">Arquivado</option>
      </select>

      <button class="primary" type="submit" id="saveBtn">Salvar registro fictício</button>
      <button class="secondary" type="button" id="cancelBtn" hidden>Cancelar edição</button>
    </form>
  </section>

  <section class="card">
    <h2>Pesquisar registros</h2>
    <label for="search">Código, nome ou apelido</label>
    <input id="search" placeholder="Digite para pesquisar">
    <p id="count" class="meta"></p>
    <div id="records"></div>
  </section>

  <section class="card">
    <h2>Dados de demonstração</h2>
    <p class="meta">Os registros são locais e não são compartilhados com outros usuários ou dispositivos.</p>
    <button class="secondary" type="button" id="resetBtn">Restaurar exemplos fictícios</button>
    <p id="message" class="status" aria-live="polite"></p>
  </section>
</main>

<footer>Protótipo local • Não utilizar para dados operacionais reais</footer>

<script>
const KEY = "cadastro_demo_v3";
const $ = id => document.getElementById(id);
let records = [];
let photoUrl = null;

function examples(){
  return [
    {
      id:"DEMO-0001",
      name:"Pessoa Exemplo A",
      mother:"Mãe Exemplo A",
      nickname:"Exemplo Azul",
      cpf:"CPF-TESTE-001",
      rg:"RG-TESTE-001",
      address:"Rua Inventada, 100 — Cidade de Teste",
      involved:"Cenário inteiramente fictício para teste.",
      photo:"",
      status:"Em revisão",
      updated:new Date().toISOString()
    },
    {
      id:"DEMO-0002",
      name:"Pessoa Exemplo B",
      mother:"Mãe Exemplo B",
      nickname:"Exemplo Verde",
      cpf:"CPF-TESTE-002",
      rg:"RG-TESTE-002",
      address:"Avenida Imaginária, 200 — Cidade de Teste",
      involved:"Descrição inventada, sem referência a fatos reais.",
      photo:"",
      status:"Ativo",
      updated:new Date().toISOString()
    }
  ];
}

function save(){
  try{
    localStorage.setItem(KEY,JSON.stringify(records));
    return true;
  }catch(e){
    $("message").textContent =
      "Não foi possível salvar. A imagem ou os dados podem exceder o armazenamento disponível.";
    return false;
  }
}

function load(){
  try{
    const raw=localStorage.getItem(KEY);
    records=raw===null?examples():JSON.parse(raw);
    if(!Array.isArray(records))throw Error("Formato inválido");
    if(raw===null)save();
  }catch(e){
    records=examples();
    save();
    $("message").textContent="Os exemplos fictícios foram recriados.";
  }
  render();
}

function esc(value){
  return String(value??"").replace(/[&<>"']/g,c=>({
    "&":"&amp;","<":"&lt;",">":"&gt;",
    '"':"&quot;","'":"&#39;"
  })[c]);
}

function readPhoto(file){
  return new Promise((resolve,reject)=>{
    const reader=new FileReader();
    reader.onload=()=>resolve(reader.result);
    reader.onerror=()=>reject(new Error("Falha ao ler a imagem"));
    reader.readAsDataURL(file);
  });
}

function render(){
  const q=$("search").value.trim().toLowerCase();
  const filtered=records.filter(r=>
    [r.id,r.name,r.nickname].some(v=>
      String(v||"").toLowerCase().includes(q)
    )
  );

  $("count").textContent=filtered.length+" registro(s) encontrado(s)";
  $("records").innerHTML="";

  filtered.forEach(r=>{
    const article=document.createElement("article");
    article.className="record";

    article.innerHTML=`
      ${r.photo
        ? `<img class="record-photo" src="${esc(r.photo)}" alt="Foto fictícia de demonstração">`
        : ""}
      <h3 style="text-align:center">${esc(r.name)}</h3>
      <div class="meta">
        Código: ${esc(r.id)}<br>
        Situação: ${esc(r.status)}<br>
        Última alteração: ${esc(new Date(r.updated).toLocaleString("pt-BR"))}
      </div>
      <div class="detail">
        <p><b>Nome da mãe:</b> ${esc(r.mother||"Não informado")}</p>
        <p><b>Apelido:</b> ${esc(r.nickname||"Não informado")}</p>
        <p><b>CPF fictício:</b> ${esc(r.cpf||"Não informado")}</p>
        <p><b>RG fictício:</b> ${esc(r.rg||"Não informado")}</p>
        <p><b>Endereço:</b> ${esc(r.address||"Não informado")}</p>
        <p><b>Envolvido em:</b><br>${esc(r.involved||"Não informado")}</p>
      </div>
      <div class="actions">
        <button class="secondary" data-edit="${esc(r.id)}">Editar</button>
        <button class="danger" data-delete="${esc(r.id)}">Excluir</button>
      </div>`;

    $("records").appendChild(article);
  });
}

function resetForm(){
  $("recordForm").reset();
  $("editId").value="";
  $("formTitle").textContent="Novo registro fictício";
  $("saveBtn").textContent="Salvar registro fictício";
  $("cancelBtn").hidden=true;

  if(photoUrl){
    URL.revokeObjectURL(photoUrl);
    photoUrl=null;
  }

  $("photoPreview").removeAttribute("src");
  $("photoPreview").style.display="none";
}

$("photo").addEventListener("change",()=>{
  if(photoUrl){
    URL.revokeObjectURL(photoUrl);
    photoUrl=null;
  }

  const file=$("photo").files[0];
  const preview=$("photoPreview");
  preview.style.display="none";
  preview.removeAttribute("src");

  if(!file)return;

  if(!file.type.startsWith("image/")){
    $("message").textContent="Selecione um arquivo de imagem válido.";
    $("photo").value="";
    return;
  }

  if(file.size>2*1024*1024){
    $("message").textContent="Escolha uma imagem de até 2 MB para este teste.";
    $("photo").value="";
    return;
  }

  photoUrl=URL.createObjectURL(file);
  preview.src=photoUrl;
  preview.style.display="block";
  $("message").textContent="Prévia da foto carregada.";
});

$("recordForm").addEventListener("submit",async e=>{
  e.preventDefault();

  const id=$("editId").value;
  const existing=records.find(r=>r.id===id);
  let photo=existing?existing.photo||"":"";

  const file=$("photo").files[0];

  try{
    if(file){
      if(!file.type.startsWith("image/") || file.size>2*1024*1024){
        $("message").textContent="Selecione uma imagem válida de até 2 MB.";
        return;
      }
      photo=await readPhoto(file);
    }
  }catch(err){
    $("message").textContent="Não foi possível carregar a imagem.";
    return;
  }

  const record={
    id:existing?existing.id:"DEMO-"+Date.now().toString().slice(-10),
    name:$("name").value.trim(),
    mother:$("mother").value.trim(),
    nickname:$("nickname").value.trim(),
    cpf:$("cpf").value.trim(),
    rg:$("rg").value.trim(),
    address:$("address").value.trim(),
    involved:$("involved").value.trim(),
    photo:photo,
    status:$("status").value,
    updated:new Date().toISOString()
  };

  if(!record.name)return;

  if(existing){
    records=records.map(r=>r.id===id?record:r);
  }else{
    records.unshift(record);
  }

  if(save()){
    resetForm();
    render();
    $("message").textContent="Registro fictício salvo neste navegador.";
  }
});

$("records").addEventListener("click",e=>{
  const edit=e.target.dataset.edit;
  const del=e.target.dataset.delete;

  if(edit){
    const r=records.find(item=>item.id===edit);
    if(!r)return;

    $("editId").value=r.id;
    $("name").value=r.name||"";
    $("mother").value=r.mother||"";
    $("nickname").value=r.nickname||"";
    $("cpf").value=r.cpf||"";
    $("rg").value=r.rg||"";
    $("address").value=r.address||"";
    $("involved").value=r.involved||"";
    $("status").value=r.status||"Em revisão";

    $("formTitle").textContent="Editar registro fictício";
    $("saveBtn").textContent="Salvar alterações";
    $("cancelBtn").hidden=false;

    if(photoUrl){
      URL.revokeObjectURL(photoUrl);
      photoUrl=null;
    }

    $("photo").value="";
    const preview=$("photoPreview");
    preview.removeAttribute("src");
    preview.style.display="none";

    if(r.photo){
      preview.src=r.photo;
      preview.style.display="block";
    }

    window.scrollTo({top:0,behavior:"smooth"});
  }

  if(del){
    if(!confirm("Excluir este registro fictício?"))return;

    records=records.filter(r=>r.id!==del);

    if(save()){
      render();
      $("message").textContent="Registro excluído.";
    }
  }
});

$("cancelBtn").addEventListener("click",resetForm);
$("search").addEventListener("input",render);

$("resetBtn").addEventListener("click",()=>{
  if(!confirm("Substituir os registros atuais pelos exemplos fictícios?"))return;

  records=examples();

  if(save()){
    resetForm();
    render();
    $("message").textContent="Exemplos fictícios restaurados.";
  }
});

load();
</script>
</body>
</html>
