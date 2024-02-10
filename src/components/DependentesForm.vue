<template>
    <div class="hello">
        <h1>Cadastro</h1>
        <h2>Cadastro de Dependentes</h2>
        <hr>

        <div class="componente-form-table">
            <form action="">
                <label for="">Funcionário.:</label>
                <select 
                    name="" 
                    id="" 
                    v-model="funcionario"
                >
                    <option value=""></option>
                    <option 
                        v-for="funcionario in funcionarios_disponiveis"
                        :key="funcionario.id"
                        :value="funcionario.id"
                    >
                        {{ funcionario.nome }}
                    </option>
                </select>
                <br><br>
                <label for="">Nome.:</label>
                <input 
                    type="text" 
                    name="" 
                    id=""
                    v-model="nome"
                />
                <br><br>
                <input 
                    type="submit" 
                    value="Cadastrar"
                    @click="cadastrarNovoDependete($event)"
                />
            </form>  

            <table>
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Funcionário</th>
                        <th>Nome</th>
                        <th>Ações</th>
                    </tr>
                </thead>
                <tbody>
                    <tr
                        v-for="dependente in dependentes"
                        :key="dependente.id"
                    >
                        <td>{{ dependente.id }}</td>
                        <td>{{ dependente.funcionario }}</td>
                        <td>{{ dependente.dependente }}</td>
                        <td>
                            <button @click="atualizarDependente(dependente.id)">Alterar</button>
                            <button @click="excluirDependente(dependente.id)">Excluir</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>        
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'DependentesForm',
    data() {
        return {
            funcionario: '',
            nome: '',
            funcionarios_disponiveis: [],
            dependentes: [],
        }
    },
    mounted() {
        this.preencherSelect();
        this.listar();
    },
    methods: {
        cadastrarNovoDependete(e) {

            e.preventDefault();

            axios
                .post('http://localhost:3000/routes/dependentes/cadastrar', {
                    "id_funcionario": this.funcionario,
                    "nome": this.nome,
                })
                .then(response => {
                    console.log(response);
                    this.listar()
                })
                .catch(error => console.log(error))
        },
        preencherSelect() {
            axios
                .get('http://localhost:3000/routes/funcionarios/listar')
                .then(response => {

                    this.funcionarios_disponiveis = response.data.data
                })
                .catch(error => console.log(error))
        },
        listar() {
            axios
                .get('http://localhost:3000/routes/dependentes/listar')
                .then(response => {

                this.dependentes = response.data.data
                })
                .catch(error => console.log(error))
        },
        atualizarDependente(id) {

            axios
                .get(`http://localhost:3000/routes/dependentes/${id}`)
                .then(response => {

                    // Exibe uma caixa de diálogo com uma pergunta e um campo de entrada
                    var id_funcionario = window.prompt("ID do funcionário:", response.data.data.id_funcionario);

                    
                    // Verifica se o usuário inseriu algum valor
                    if (id_funcionario !== null) {

                        var nome_dependente = window.prompt("Nome do dependente:", response.data.data.dependente);

                        if (nome_dependente !== null) {

                            axios
                                .patch(`http://localhost:3000/routes/dependentes/alterar/${id}`, {
                                    id_funcionario: id_funcionario,
                                    nome: nome_dependente,
                                })
                                .then(response => {
                                    console.log(response);
                                    this.listar();
                                })
                                .catch(error => console.log(error))

                        } else {
                            // Se o usuário clicou em "Cancelar" ou fechou a caixa de diálogo
                            console.log("Você cancelou a operação.");
                        }

                    } else {
                        // Se o usuário clicou em "Cancelar" ou fechou a caixa de diálogo
                        console.log("Você cancelou a operação.");
                    }


                })
                .catch(error => console.log(error))

        },
        excluirDependente(id) {

            console.log(id)

            // Exibe uma caixa de diálogo com uma pergunta
            var result = window.confirm("Você tem certeza que deseja excluir este Dependente?");

            // Verifica o resultado da ação do usuário
            if (result) {
                axios
                    .delete(`http://localhost:3000/routes/dependentes/excluir/${id}`)
                    .then(response => {
                        console.log(response);
                        this.listar()
                    })
                    .catch(error => console.log(error))
            } else {
                // Se o usuário clicar em "Cancelar" ou fechar a caixa de diálogo, executa o código aqui
                window.alert("O usuário cancelou a exclusão.");
            }
        }
    },
}

</script>

<style scoped>
hr {
  width: 40%;
  margin-bottom: 1rem;
}
input, select {
  margin-left: .5rem;
}
</style>