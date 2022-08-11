<script setup>
import { ref, computed } from 'vue';

    let id = 0;

    let items = ref([]);
    let newItem = ref("");
    let priorityItem = ref(false);

    function addItem () {
        items.value.push({key: id++, nome: newItem.value, priority: priorityItem.value});
        newItem.value = "";
        priorityItem.value = false;
    }

    function deleteItem (index) {
        items.value.splice(index, 1);
    }

    function changePriority (index) {
    items.value[index].priority = !items.value[index].priority;
    }

    function completed (index) {
        items.value[index].completed = !items.value[index].completed;

    }

    let counter = computed(() => {
        return items.value.length;
    })

    let counterPriority = computed(() => {
        var a = 0;

        for(let i = 0; i < items.value.length; i++) {
            if (items.value[i].priority === true) {
                a++;
            }
        }
        return a;
    })

    let counterCompleted = computed(() => {
        var a = 0;

        for(let i = 0; i < items.value.length; i++) {
            if (items.value[i].completed === true) {
                a++;
            }
        }
        return a;
    })

    let computedCaract = computed(() => {
        return newItem.value.length;
    })

</script>

<!-- ----------------------------------------------------------------------- -->

<template>

    <header>
        <h1>To Do List</h1>
        <p>Anote seus afazeres. Marque suas prioridades. Realize seus objetivos!</p>
    </header>

    <main class="content">
        <div class="sup-nav">
            <span class="countCaract">{{computedCaract}}/30</span>
            <input class="input-txt" v-model="newItem" placeholder="Adicione um item..." maxlength="30">
            <span>Prioridade?</span>
            <input type="checkbox" v-model="priorityItem" class="checkbox-input">
            <button :disabled="newItem.length < 1" class="btn-addItem" @click="addItem()">Adicionar</button>
        </div>    

        <div class="resumo">
            <p v-if="!counter">Nenhum item na lista</p>
            <p v-else-if="counter === 1">{{counter}} item na lista.</p>
            <p v-else>{{counter}} itens na lista.</p>

            <p v-if="!counterPriority">Nenhuma prioridade.</p>
            <p v-else-if="counterPriority === 1">{{counterPriority}} item de alta prioridade.</p>
            <p v-else>{{counterPriority}} itens de alta prioridade.</p>

            <p v-if="!counterCompleted">Nada completado.</p>
            <p v-else-if="counterCompleted === 1">{{counterCompleted}} item completado.</p>
            <p v-else>{{counterCompleted}} itens completados.</p>
        </div>

        <table>
            <thead align="center">
                <tr>
                    <td class="items-td">ITEMS</td>
                    <td>AÇÕES</td>
                </tr>           
            </thead>
            <tr v-for="(item, index) in items" :key="item.key" :class="{marked: item.priority, completed: item.completed}"> 
                <td>
                {{item.nome}}
                </td>
                <td class="td-action">
                    <button id="btn-delete" @click="deleteItem(index)"><img src="/images/delete.png" alt="delete item" title="delete item"></button>
                    <button id="btn-priority" @click="changePriority(index)"><img src="/images/priority.png" alt="chenge priority" title="change priority"></button>
                    <button id="btn-check" @click="completed(index)"><img  id="btn-check" src="/images/check.png" alt="check" title="check"></button> 
                </td>
            </tr>
        </table>
    </main>

</template>
<!-- ----------------------------------------------------------------------- -->
<style scoped>

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');

header {
    font-family: 'Montserrat', sans-serif;    
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    border-bottom: 1px solid white;
    margin-bottom: 26px;
}

header h1 {
    font-size: 42px;
    margin-right: 36px;
    border-bottom: 1px solid black;
}

header p {
    font-size: 22px;
}


/* MAIN, INPUT DE TEXTO e PRIORIDADE */
.content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border: 1px solid rgb(252, 246, 246);
    border-radius: 6px;

    max-width: 85vw;
    position: relative;
    left: 50%;
    transform: translate(-50%);
    padding: 40px 0;
    background-image: linear-gradient(to right, rgb(219, 192, 192), rgb(203, 203, 231));
}

.sup-nav {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
}

.sup-nav span {
    font-size: 20px;
    margin-left: 20px;
}

.sup-nav label {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 22px;
}

.countCaract{
    font-size: 18px;
    margin: 0 10px;
}

.input-txt {
    height: 36px;
    font-size: 20px;
    border: 2px solid rgb(129, 91, 129);
    border-radius: 8px;
}

.input-txt::placeholder {
    font-size: 14px;
}

.checkbox-input {
    width: 20px;
    height: 20px;
    margin: 6px;
}

/* BOTÃO DE ADICIONAR */
.btn-addItem {
    font-family: 'Montserrat', sans-serif;
    height: 38px;
    width: 92px;
    margin: 0 16px;
    border: 2px solid rgb(94, 22, 94);
    border-radius: 16px;
    background-color: rgb(225, 181, 250);

    color: rgb(94, 22, 94);
    font-size: 14px;
    cursor: pointer;
    transition: 0.5s;
}

.btn-addItem:hover {
    border: 2px solid rgb(196, 119, 196);
    border-radius: 16px;
    background-color: rgb(157, 94, 194);
    color: rgb(212, 149, 212);
}

/* RESUMO DE INFORMAÇÕES */

.resumo {
    display: flex;
    flex-wrap: wrap;
    justify-content:space-between;
    align-items: center;
    margin: 0 10px;
}

.resumo p {
    font-size: 20px;
    margin: 30px 30px;
}

/* TABLE */

table {
    border-collapse: collapse;
}

table thead tr td {
    font-weight: 800;
}

.items-td {
    min-width: 360px;
}

table tr td {
    font-size: 18px;
    padding: 8px; 
    border-bottom: 1px solid white;
}
.marked {
    color: red;
}

.completed {
    text-decoration: line-through;
}

/* ITEMS DA TABELA */

table button img {
    width: 22px;
    height: 22px;
    border: none;
    cursor: pointer;
}

.td-action button {
    margin: 4px;
    border: 1px solid rgb(219, 212, 212);
    border-radius: 6px;
    cursor: pointer;
}


@media (max-width: 500px) {

    .content {
        max-width: 95vw;
    }

    header h1 {
      padding: 0; 
      margin: 6px 0;
      border: none;
    }

    header p {
        display: none;
    }

    .btn-addItem {
        margin: 12px;
    }
    .resumo {
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
    }

    .resumo p {
    font-size: 20px;
    margin: 4px;
    }

    table {
        margin-top: 40px;
    }
    .items-td {
    min-width: 180px;
    }

    table tr td {
        max-width: 50vw;
        overflow: scroll;
    }
}

</style>