<template>
    <div>
        <h2>Calculate</h2>
        <div class="container">
            <div class="item input">
            <form v-module="form"><input type="text" name='textview' readonly></form>
            </div>
            <div class="item clean" onclick="clean()">c</div>
            <div class="item back" onclick="back()">&larr;</div>
            <div class="item operation plus" onclick="insert('+')">+</div>
            <div class="item operation minus" onclick="insert('-')">-</div>
            <div class="item operation multiplication" onclick="insert('*')">&times;</div>
            <div class="item operation division" onclick="insert('/')">&divide;</div>
            <div class="item" onclick="insert('7')">7</div>
            <div class="item" onclick="insert('8')">8</div>
            <div class="item" onclick="insert('9')">9</div>
            <div class="item open" onclick="insert('(')">(</div>
            <div class="item" onclick="insert('4')">4</div>
            <div class="item" onclick="insert('5')">5</div>
            <div class="item" onclick="insert('6')">6</div>
            <div class="item close" onclick="insert(')')">)</div>
            <div class="item" onclick="insert('1')">1</div>
            <div class="item" onclick="insert('2')">2</div>
            <div class="item" onclick="insert('3')">3</div>
            <div class="item equal" onclick="equal()">=</div>
            <div class="item zero" onclick="insert('0')">0</div>
            <div class="item dot" onclick="insert('.')">.</div>
        </div>
            <router-link to="/calculate">Todos</router-link>
    </div>
</template>
<script>
        let result = false; //Для удаления результата из input
        let dotPos = false; //Для блокировки повторной точки
        let itemMass = document.querySelectorAll('.item');
        let dot = document.querySelector('.dot')
        let close = document.querySelector('.close')
        let open = document.querySelector('.open')

        function insert(num) {
            document.form.textview.value = document.form.textview.value + num;
        }

        function clean() {
            document.form.textview.value = ''
        }

        function back() {
            let exp = document.form.textview.value;
            if (exp[exp.length - 1]) {
                dotPos = false
            };
            form.textview.value = exp.substring(0, exp.length - 1)
        }

        function equal() {
            let exp = document.form.textview.value;
            if (exp) {
                document.form.textview.value = eval(exp)
            }
            setTimeout(function() {
                result = true;
            }, 20)
        }

        //Блокировка повторной точки
        dot.addEventListener('click', function() {
            let exp = document.form.textview.value
            if ((exp[exp.length - 2]) == '.' || dotPos == true) {
                form.textview.value = exp.substring(0, exp.length - 1);
            }
            dotPos = true
        })

        itemMass.forEach(function(item) {
                    item.addEventListener('click',
                        function() {
                            let exp = document.form.textview.value;

                            //Очистка после вычислений
                            if (result == true) {
                                document.form.textview.value = '';
                                result = false;
                            }

                            //Замена математического символа на нажатый
                            if (this.classList.contains('operation') && (exp[exp.length - 2] == '*' || exp[exp.length - 2] == '/' || exp[exp.length - 2] == '+' || exp[exp.length - 2] == '-')) {
                                let lengthExp = exp.length - 1
                                document.form.textview.value = exp.substring(0, (lengthExp - 1)) + exp.substring(lengthExp, (lengthExp + 1));
                            }

                            //Некорректные для старта операции 
                            if (exp.slice(0) == '/' || exp.slice(0) == '*' || exp.slice(0) == ')') {
                                form.textview.value = exp.substring(0, exp.length - 1);
                            }

                            //операции деления и умножения после точки
                            if ((this.classList.contains('operation') || this.classList.contains('open') || this.classList.contains('close')) && exp[exp.length - 2] == '.') {
                                form.textview.value = exp.substring(0, exp.length - 1);
                            }

                            //Сброс блокировки точки при вводе математического символа
                            if (this.classList.contains('operation') || this.classList.contains('open') || this.classList.contains('close') || this.classList.contains('clean')) {
                                dotPos = false
                            }

                            //Обязательный математический символ перед скобкой
                            if (this.classList.contains('open') && (exp[exp.length - 2] != '*' && exp[exp.length - 2] != '/' && exp[exp.length - 2] != '+' && exp[exp.length - 2] != '-' && exp[exp.length - 2] != '(')) {
                                form.textview.value = exp.substring(0, exp.length - 1);
                            }

                            //Блокирока недопустимых символов после скобки
                            if ((exp[exp.length - 2] == '(') && (exp[exp.length - 1] == '*' || exp[exp.length - 1] == '/')) {
                                form.textview.value = exp.substring(0, exp.length - 1)
                            }

                            //Обязательный математический символ после скобки
                            if ((exp[exp.length - 2] == ')') && (exp[exp.length - 1] != '*' && exp[exp.length - 1] != '/' && exp[exp.length - 1] != '+' && exp[exp.length - 1] != '-' && exp[exp.length - 1] != ')')) {
                                form.textview.value = exp.substring(0, exp.length - 1);
                            }
                            
                        //Отсутствие знаков перед закрывающей скобкой
                            if ((exp[exp.length - 1] == ')') && (exp[exp.length - 2] == '*' || exp[exp.length - 2] == '/' || exp[exp.length - 2] == '+' || exp[exp.length - 2] == '-' || exp[exp.length - 2] == '.')){
                                form.textview.value = exp.substring(0, exp.length - 1);
                            }

                            })
                    });

                //Контроль количества скобок
                close.addEventListener('click', function() {
                    let exp = document.form.textview.value
                    let collectionOpen = 0;
                    let collectionClose = 0;
                    for (let i = 0; i < exp.length; i++) {
                        if (exp[i] == '(') {
                            collectionOpen++
                        }
                        if (exp[i] == ')') {
                            collectionClose++
                        }
                    }
                    if (collectionClose > collectionOpen || exp[exp.length - 2] == '(' || exp[exp.length - 2] == '.' || exp[exp.length - 2] == '*' || exp[exp.length - 2] == '/') {
                        form.textview.value = exp.substring(0, exp.length - 1);
                    }
                })
</script>


<style scroped>
body {
    width:100vw;
    height: 100vh;
    background: white;
    user-select: none;
}

.container {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    grid-gap: 4px;
    margin:auto;
    background-color: gray;
    padding: 4px;
    border-radius: 8px;
    max-width: 360px;
}

.item {
    background: linear-gradient(to bottom, #fff 0, #e6e6e6 100%);;
    color: black;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 22px;
    cursor: pointer;
    font-weight: bold;
    border-radius: 5px;
}

form {
    max-width: 100%;
}

input {
    width: 100%;
    height: 60px;
    border: none;
    font-size: 30px;
    outline: none;
    text-align: right;
    padding: 0;
}

.input {
    grid-column: 1/-1;
    padding: 0;
}

.clean {
    grid-column: 1/3;
    background-color: blue;
}

.back {
    grid-column: 3/5;
}

.equal {
    grid-column: 4;
    grid-row: 6/8
}

.zero {
    grid-column: 1/3
}

</style>