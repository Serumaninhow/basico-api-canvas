## API Canvas

Começa-se definindo o frame no HTML, capta-se essa referencia no JavaScript, e depois se defini o elemento que poderá colorir a tela. Um exemplo abaixo de como fazer esse modelo em um arquivo `.html`:

```html
<canvas id="quadro" width="600" height="400">

<script>
    let frame = document.getElementById("quadro");
    let pincel = frame.getContext("2d");
</script>
```

> A variável `pincel` vai realizar todas as intereções nesse ecossistema.

Para colorir uma forma retangular, pode-se fazer:

```html
<canvas id="quadro" width="600" height="400">

<script>
    let frame = document.getElementById("quadro");
    let pincel = frame.getContext("2d");

    pincel.fillStyle = "green";
    pincel.fillRect(0, 0, 200, 400);
</script>
```

> Um retângulo verde que começa em (0,0) e adianta-se em `200` e `400` pixeis, respectivamente na horizontal e vertical (eixo X e eixo Y).

Para colorir um triângulo, pode-se executar o seguinte passo a passo:

```html
<canvas id="quadro" width="600" height="400">

<script>

    let frame = document.getElementById("quadro");
    let pincel = frame.getContext("2d");

    pincel.fillStyle = "yellow";
    pincel.beginPath();
    pincel.moveTo(300, 200);
    pincel.lineTo(200, 400);
    pincel.lineTo(400, 400);
    pincel.fill();

</script>
```

> Define-se a cor `"yellow"`, começa-se um caminho a ser percorrido pelo pincel em `beginPath()`, posiciona-se o pincel em `moveTo`, traça-se linhas considerando a origem com `lineTo`, e termina-se preenchendo a figura com `fill`.

Para colorir um círculo, pode-se realizar o seguinte esquema:

```html
<canvas id="quadro" width="600" height="400">

<script>
    let frame = document.getElementById("quadro");
    let pincel = frame.getContext("2d");

    pincel.fillStyle = "blue";
    pincel.beginPath();
    pincel.arc(300, 200, 100, 0, 2 * 3.14);
    pincel.fill();
</script>
```

> Define-se a cor `"blue"`, começa-se um caminho a ser percorrido pelo pincel em `beginPath()`, define-se o caminho do arco em `pincel.arc(x_centro, y_centro, raio, ini_arco, final_arco)`, e termina-se preenchendo a figura com `fill`.

## Interação pelo mouse

Abaixo estão eventos de mouse, que são propriedades/atributos que podem influenciar elementos do HTML (ou via ``addEventListener``) para capturar interações do usuário.

#### Clique do mouse

- ``onclick`` → Dispara quando o usuário clica (pressiona e solta) um botão do mouse sobre o elemento.

- ``ondblclick`` dispara quando ocorre um duplo clique.

- ``onmousedown`` dispara quando o botão do mouse é pressionado.

- ``onmouseup`` dispara quando o botão do mouse é solto.

#### Movimento

- ``onmousemove`` dispara sempre que o mouse é movido dentro da área do elemento.

- ``onmouseover`` dispara quando o ponteiro entra na área do elemento (sem clicar).

- ``onmouseout`` dispara quando o ponteiro sai da área do elemento.

- ``onmouseenter`` parecido com onmouseover, mas não se propaga para elementos filhos.

- ``onmouseleave`` parecido com onmouseout, mas não se propaga para elementos filhos.

#### Scroll e botão direito

- ``onwheel`` dispara quando a rodinha do mouse (scroll) é usada.

- ``oncontextmenu`` dispara ao clicar com o botão direito (abre o menu de contexto, se não for prevenido).

> Previnir com `event.preventDefault()`.

#### Diferenças importantes

- ``onmousedown + onmouseup`` formam um ``onclick`` (mas só contam se forem no mesmo elemento).

- ``mouseover / mouseout`` disparam várias vezes ao passar por elementos filhos.

- ``mouseenter / mouseleave`` só disparam uma vez quando entra/sai do elemento.

#### Exemplo prático

```html
<button id="botao">clique-me</button>

<script>
  let botao = document.getElementById("botao");

  botao.onclick = () => alert("Clique simples!");
  botao.onmousedown = () => console.log("Mouse pressionado");
  botao.onmouseup = () => console.log("Mouse solto");
  botao.onmouseover = () => console.log("Entrou no botão");
  botao.onmouseout = () => console.log("Saiu do botão");

  botao.oncontextmenu = function(event){
    event.preventDefault(); // impede o menu do navegador
    alert("Clique direito!");
  }
</script>
```

> O referente trecho de código manifesta as ações de clicar, passar pelo botão, segurar e soltar o botão.

## Interação com teclado

Para utilizar interações com o teclado, pode-se descobrir a tecla usada pelo seguinte trecho.

```html
<input type="text" id="meuInput" placeholder="Digite algo...">
<p>Tecla pressionada: <span id="codigoTecla"></span></p>

<script>
    const meuInput = document.getElementById('meuInput');
    const spanCodigoTecla = document.getElementById('codigoTecla');

    meuInput.addEventListener('keydown', function(event) {
        const nomeDaTecla = event.key;
        const codigoDaTecla = event.keyCode;

        spanCodigoTecla.textContent = `Nome: "${nomeDaTecla}" (Código: ${codigoDaTecla})`;
        console.log('Evento de tecla:', event);
    });
</script>
```

> Esse trecho vai exibir pop-ups e mensagens no terminal do navegador.

## Quadro de desenho

Para gerar o quadro consideramos os seguintes x passos:

1) Criar um quadro de 1200x800, um botão para mudar a cor do pincel, definir a borda do quadro e referenciar todos os elementos.

```html
<canvas id ="quadro" width="1200"  height="800"></canvas>

Escolha uma cor <input id="paleta" type="color">

<script>
    var tela = document.getElementById("quadro");
    var pincel = tela.getContext('2d');
    var paleta = document.getElementById("paleta");

    pincel.fillStyle = "lightgrey";
    pincel.fillRect(0, 0, 1200, 800);
    pincel.strokeStyle = 'black';    
    pincel.strokeRect(0, 0, 1200, 800);
</script>
```

2) Função para desenhar na posição do ponteiro do mouse e para apagar o quadro.

```js
    function desenha_circulo(evento){
        var x = evento.pageX - tela.offsetLeft;
        var y = evento.pageY - tela.offsetTop;

        pincel.fillStyle=paleta.value;
        pincel.beginPath();
        
        pincel.arc(x, y, 5, 0, 2*3.14);
        pincel.fill();
    }

    function apaga_tela(){
        aux = pincel.fillStyle;

        pincel.fillStyle = "lightgrey";
        pincel.fillRect(0, 0, 1200, 800);

        pincel.fillStyle = aux;
        return false;
    }
```

3) Relacionar escrever na tela enquanto se movimenta o mouse e mantendo o botão esquerdo precionado, e parar quando soltar; também relacionar o gatilho de apagar a tela no botão direito.

```js
    tela.onmousedown = function(){
        tela.onmousemove = desenha_circulo;
    };

    tela.onmouseup = function(){
        tela.onmousemove = false;
    };

    tela.oncontextmenu = apaga_tela;
```

O conjunto desses passos constroem o código abaixo do quadro para desenho.

```html
<canvas id ="quadro" width="1200"  height="800"></canvas>

Escolha uma cor <input id="paleta" type="color">

<script>
    var tela = document.getElementById("quadro");
    var pincel = tela.getContext('2d');
    var paleta = document.getElementById("paleta");

    pincel.fillStyle = "lightgrey";
    pincel.fillRect(0, 0, 1200, 800);
    pincel.strokeStyle = 'black';    
    pincel.strokeRect(0, 0, 1200, 800);

    function desenha_circulo(evento){
        var x = evento.pageX - tela.offsetLeft;
        var y = evento.pageY - tela.offsetTop;

        pincel.fillStyle=paleta.value;
        pincel.beginPath();
        
        pincel.arc(x, y, 5, 0, 2*3.14);
        pincel.fill();
    }

    function apaga_tela(){
        aux = pincel.fillStyle;

        pincel.fillStyle = "lightgrey";
        pincel.fillRect(0, 0, 1200, 800);

        pincel.fillStyle = aux;
        return false;
    }

    tela.onmousedown = function(){
        tela.onmousemove = desenha_circulo;
    };

    tela.onmouseup = function(){
        tela.onmousemove = false;
    };

    tela.oncontextmenu = apaga_tela;
</script>
```

> Esta é uma prática de tudo visto anteriormente.

## Animar elementos

Uma primeira maneira de animar elementos no canvas é simular o movimento, apagando a tela e reapresentando o mesmo elemento mas em uma posição diferente, para manipular o tempo de ação, utiliza-se o comando `setInterval(funcao, tempo_em_milissegundos);`

Segue o exemplo abaixo de uma bola pulsante.

```html
<canvas id="quadro" width="400" height="400"></canvas>

<script>
    var raio = 20;
    var sentido = 1;
    function restauraTela(){
        var tela = document.getElementById("quadro");
        var pincel = tela.getContext("2d");
        pincel.fillStyle = 'lightgrey';
        pincel.fillRect(0, 0, tela.clientWidth, tela.clientHeight);
    }

    function desenhaBola(x, y, r){
        var tela = document.getElementById("quadro");
        var pincel = tela.getContext("2d");
        pincel.fillStyle = "red";
        pincel.beginPath();
        pincel.arc(x, y, r, 0, 2 * Math.PI);
        pincel.fill();
    }

    function atualizaTela(){
        restauraTela();
        if(raio > 40)
        {
            sentido = -1;
        }
        else if(raio < 20)
        {
            sentido = 1;
        }
        desenhaBola(200, 200, raio);
        raio += sentido;
    }
    setInterval(atualizaTela, 10);
</script>
```

> Esse conceito foi aplicado no diretório sobre praticas/animacoes.

## Referência

> https://developer.mozilla.org/pt-BR/docs/Web/API/Canvas_API

## Extra

Exercício aprimorado do jogo da adivinhação, clique [aqui](https://jogo-da-adivinhacao-topaz.vercel.app/).
