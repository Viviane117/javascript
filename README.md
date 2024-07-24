# javascript
O projeto "Lista de Tarefas" é uma aplicação web simples que permite a manipulação de tarefas, incluindo operações de exibição, adição, edição e exclusão de tarefas. A aplicação usa a API 


### index.html
Este arquivo HTML define a estrutura básica e o conteúdo da página web.

#### Cabeçalho e Navegação
```html
<header>
    <div class="cabeçalho">
        <h1>Lista de Tarefas</h1>
        <nav>
            <ul>
                <li><a href="#">Início</a></li>
                <li><a href="#">Instruções</a></li>
                <li><a href="#">Grupo</a></li>
            </ul>
        </nav>
    </div>
</header>
```
- Um cabeçalho que inclui o título "Lista de Tarefas" e uma barra de navegação com links para "Início", "Instruções" e "Grupo".

#### Funcionalidades e Botões
```html
<div class="funcionalidades">
    <nav>
        <ul>
            <li><button id="B1">Obter Todas as Tarefas</button></li>
            <li><button id="B2">Obter Todas as Tarefas de um Usuário</button></li>
            <li><button id="B3">Adicionar uma Tarefa</button></li>
            <li><button id="B4">Editar Tarefa</button></li>
            <li><button id="B5">Deletar Tarefa</button></li>
        </ul>
    </nav>
</div>
```
- Uma seção de navegação com botões para executar diferentes ações: obter todas as tarefas, obter tarefas de um usuário específico, adicionar, editar e deletar tarefas.

#### Formulários para Obter e Deletar Tarefas
```html
<div id="obter-tarefas-usuario" style="display: none;">
    <label for="idUsuario">Digite o Id do Usuário</label>
    <input type="text" id="idUsuario" name="idUsuario">
    <button id="btnBuscarTarefasUsuario">Buscar Tarefas</button>
</div>

<div id="deletar-tarefas-usuario" style="display: none;">
    <label for="tarefaId">Digite o Id da Tarefa</label>
    <input type="text" id="tarefaId" name="tarefaId">
    <button id="btnDeletar">Deletar Tarefa</button>
</div>
```
- Formulários que permitem ao usuário digitar o ID de um usuário para obter suas tarefas e o ID de uma tarefa para deletá-la. Estes formulários são inicialmente ocultos.

#### Container para Exibir Tarefas
```html
<div id="lista-tarefas"></div>
```
- Um contêiner onde as tarefas serão exibidas.

#### Inclusão do Arquivo de Script
```html
<script src="script.js"></script>
```
- Inclusão do arquivo `script.js`, que contém a lógica de manipulação das tarefas.

### script.js
Este arquivo contém a lógica para as operações de CRUD nas tarefas.

#### Função para Obter Todas as Tarefas
```javascript
function obterTodasTarefas() {
    fetch('https://jsonplaceholder.typicode.com/todos')
        .then((response) => response.json())
        .then((json) => {
            exibirTarefas(json);
        });
}
```
- Busca todas as tarefas da API e chama a função `exibirTarefas` para exibi-las.

#### Função para Exibir Tarefas
```javascript
function exibirTarefas(tarefas) {
    const listaTarefas = document.getElementById('lista-tarefas');
    listaTarefas.innerHTML = '';
    tarefas.forEach(tarefa => {
        const tarefaElemento = document.createElement('div');
        tarefaElemento.classList.add('tarefa');
        tarefaElemento.innerHTML = `
            <h3>${tarefa.title}</h3>
            <p>Usuário: ${tarefa.userId}</p>
            <p>ID: ${tarefa.id}</p>
        `;
        listaTarefas.appendChild(tarefaElemento);
    });
}
```
- Recebe uma lista de tarefas e as exibe no contêiner `lista-tarefas`.

#### Função para Obter Tarefas de um Usuário
```javascript
function obterTarefasUsuario(idUsuario) {
    const url = `https://jsonplaceholder.typicode.com/users/${idUsuario}/todos`;
    fetch(url)
        .then((response) => {
            if (!response.ok) {
                throw new Error(`Erro na requisição HTTP! Status: ${response.status}`);
            }
            return response.json();
        })
        .then((json) => {
            exibirTarefas(json);
        })
        .catch((error) => {
            console.error('Erro ao obter tarefas do usuário:', error);
        });
}
```
- Busca tarefas de um usuário específico com base no ID fornecido.

#### Função para Adicionar uma Tarefa
```javascript
function adicionarTarefa(tituloTarefa, idUsuario) {
    fetch('https://jsonplaceholder.typicode.com/todos', {
        method: 'POST',
        body: JSON.stringify({
            title: tituloTarefa,
            userId: idUsuario,
            completed: false
        }),
        headers: {
            'Content-type': 'application/json; charset=UTF-8',
        },
    })
        .then(response => response.json())
        .then(json => {
            alert('Tarefa adicionada com sucesso!');
        })
        .catch(error => console.error(error));
}
```
- Adiciona uma nova tarefa enviando uma requisição POST para a API.

#### Função para Editar uma Tarefa
```javascript
function editarTarefa(idTarefa, novoTitulo) {
    fetch(`https://jsonplaceholder.typicode.com/todos/${idTarefa}`, {
        method: 'PUT',
        body: JSON.stringify({
            title: novoTitulo,
            completed: false
        }),
        headers: {
            'Content-type': 'application/json; charset=UTF-8',
        },
    })
        .then(response => response.json())
        .then(json => {
            alert('Tarefa editada com sucesso!');
        })
        .catch(error => console.error('Erro ao editar tarefa:', error));
}
```
- Edita uma tarefa existente enviando uma requisição PUT para a API.

#### Função para Deletar uma Tarefa
```javascript
function deletarTarefa(idTarefa) {
    fetch(`https://jsonplaceholder.typicode.com/todos/${idTarefa}`, {
        method: 'DELETE',
    })
        .then(response => {
            if (response.ok) {
                alert('Tarefa deletada com sucesso!');
            } else {
                alert('Erro ao deletar a tarefa.');
            }
        })
        .catch(error => console.error('Erro ao deletar tarefa:', error));
}
```
- Deleta uma tarefa enviando uma requisição DELETE para a API.

#### Manipulação de Eventos
```javascript
document.getElementById("B1").addEventListener("click", obterTodasTarefas);
document.getElementById("B2").addEventListener("click", function () {
    document.getElementById('obter-tarefas-usuario').style.display = 'block';
});
document.getElementById("B3").addEventListener("click", function () {
    const tituloTarefa = prompt("Digite o título da tarefa:");
    const idUsuario = prompt("Digite o ID do usuário:");
    adicionarTarefa(tituloTarefa, idUsuario);
});
document.getElementById("B4").addEventListener("click", function () {
    const idTarefa = prompt("Digite o ID da tarefa que deseja editar:");
    const novoTitulo = prompt("Digite o novo título da tarefa:");
    editarTarefa(idTarefa, novoTitulo);
});
document.getElementById("B5").addEventListener("click", function () {
    document.getElementById('deletar-tarefas-usuario').style.display = 'block';
});
document.getElementById("btnBuscarTarefasUsuario").addEventListener("click", function () {
    const idUsuario = document.getElementById('idUsuario').value;
    obterTarefasUsuario(idUsuario);
});
document.getElementById("btnDeletar").addEventListener("click", function () {
    const idTarefa = document.getElementById('tarefaId').value;
    deletarTarefa(idTarefa);
});
```
- Adiciona ouvintes de eventos aos botões para chamar as funções apropriadas quando clicados.

Para mais informações, visite [GPT Online](https://chatgptonline.tech/pt/) e confira nossos produtos, como o [Idéias para tatuagem GPT](https://chatgpt.com/g/g-t11nuTlg3-tattoo-ideas-gpt).
