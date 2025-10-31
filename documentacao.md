# Documentação do Projeto: Semáforo com Arduino

&emsp;Este documento detalha a montagem do circuito e o funcionamento do código para um projeto de semáforo simples, utilizando um Arduino UNO e três LEDs.

## 1. Componentes Utilizados e Montagem do Circuito

&emsp;A tabela a seguir lista todos os componentes necessários para a montagem do circuito:

| Componente | Quantidade | Especificação / Valor |
| :--- | :--- | :--- |
| Arduino UNO R3 | 1 | |
| Protoboard | 1 |  |
| LED Verde | 1 |  |
| LED Amarelo | 1 |  |
| LED Vermelho | 1 |  |
| Resistor | 3 | 220 $\Omega$ (Vermelho, Vermelho, Marrom) |
| Jumpers (Fios de Conexão) | 13 | Macho-Macho (7) e Macho-Fêmea (6) |
| Cabo USB | 1 | Tipo A/B (para conexão Arduino-PC) |

**Passo a passo da montagem:**

1.  **Terra Comum (GND):** Um fio jumper (preto, na imagem) conecta um dos pinos `GND` (Terra) do Arduino ao trilho negativo (linha azul, `-`) da protoboard. Este será o terra comum para todos os componentes.
2.  **Posicionamento dos LEDs:** Os três LEDs (verde, amarelo e vermelho) são inseridos na protoboard, com cada perna em uma fileira diferente.
3.  **Conexão dos Cátodos (Lado Negativo):**
    * A perna curta (cátodo) de cada LED é conectada a uma das pernas de um resistor de 220 $\Omega$.
    * A outra perna de cada um dos três resistores é, então, conectada ao trilho `GND` (negativo) da protoboard.
4.  **Conexão dos Ânodos (Lado Positivo/Sinal):**
    * A perna longa (ânodo) do **LED verde** é conectada, através de um jumper (laranja), ao pino digital `8` do Arduino.
    * A perna longa (ânodo) do **LED amarelo** é conectada, através de um jumper (amarelo), ao pino digital `10` do Arduino.
    * A perna longa (ânodo) do **LED vermelho** é conectada, através de um jumper (roxo), ao pino digital `12` do Arduino.
5.  **Alimentação:** O Arduino é alimentado via cabo USB conectado ao computador.

<div align="center">

<sub>Figura 1 - Foto do semáforo </sub>

   <img src="/semaforof.jpg">

<sup>Fonte: Material produzido pelo autor (2025)</sup>

</div>

&emsp;Link do vídeo explicando brevemente o funcionamento: https://youtube.com/shorts/fTmejCvoXOU?feature=share


## 2. Código

&emsp;O código que faz o semáforo funcionar é este:

```c++
// pinos utilizados no arduino
#define LEDverde 8
#define LEDamarelo 10
#define LEDvermelho 12

// declara variáveis que serão atualizadas no switch case
unsigned long tempoAnterior = 0;
int8_t estado = 0;

// tempo de cada led em milisegundos
const unsigned long tempoVerde = 4000;
const unsigned long tempoAmarelo = 2000;
const unsigned long tempoVermelho = 6000;

void setup() {
  // definindo leds como saídas
  pinMode (LEDverde, OUTPUT);
  pinMode (LEDamarelo, OUTPUT);
  pinMode (LEDvermelho, OUTPUT);

}

void loop() {
  // tempoAtual usa millis pra contar os segundos que o código está rodando
  unsigned long tempoAtual = millis();
  
  switch (estado) {
    case 0: //led verde
  		digitalWrite(LEDverde, 1);
        digitalWrite(LEDamarelo, 0);
        digitalWrite(LEDvermelho, 0);
    
    	if(tempoAtual - tempoAnterior >= tempoVerde){ // diferença que calcula por quanto tempo o led ficou aceso
          estado = 1; // muda de estado quando o tempo definido no topo do código for atingido
          tempoAnterior = tempoAtual;
    	}
    	break;
    
    case 1: //led amarelo
    	digitalWrite(LEDverde, 0);
    	digitalWrite(LEDamarelo, 1);
    	digitalWrite(LEDvermelho, 0);
    
    	if(tempoAtual - tempoAnterior >= tempoAmarelo){
        estado = 2;
        tempoAnterior = tempoAtual;
        }
    	break;
    
    case 2: //led vermelho
    	digitalWrite(LEDverde, 0);
    	digitalWrite(LEDamarelo, 0);
    	digitalWrite(LEDvermelho, 1);
    
    	if(tempoAtual - tempoAnterior >= tempoVermelho){
        estado = 0;
        tempoAnterior = tempoAtual;
        }
    	break;
  }

}
```

### 2.1. Explicação do Código

&emsp;O código implementa um *switch case* para controlar o semáforo através de uma variável de estado que muda de acordo com determinadas condições. Ele não utiliza a função `delay()`, o que permite que o Arduino possa, em tese, realizar outras tarefas enquanto espera o tempo de cada LED passar.

#### 2.1.1 Definições Globais

&emsp;No início do código, definimos as variáveis e constantes que serão usadas em todo o programa.

* `#define LEDverde 8`, `#define LEDamarelo 10`, `#define LEDvermelho 12`: Estas diretivas associam os nomes "LEDverde", "LEDamarelo" e "LEDvermelho" aos números dos pinos digitais do Arduino onde eles estão fisicamente conectados.
* `unsigned long tempoAnterior = 0;`: Esta variável armazena o "momento" (em milissegundos) em que o último estado do semáforo mudou. É usada como referência para calcular o tempo decorrido.
* `int8_t estado = 0;`: Esta é a variável principal da máquina de estados. Ela armazena qual estado (qual cor) está ativo no momento (`0` = Verde, `1` = Amarelo, `2` = Vermelho).
* `const unsigned long tempo...`: Define os tempos de duração de cada fase do semáforo em milissegundos (Verde = 4s, Amarelo = 2s, Vermelho = 6s).

#### 2.1.2 Função `setup()`

&emsp;Esta função é executada apenas uma vez, quando o Arduino é ligado ou resetado.

* `pinMode(LED..., OUTPUT);`: Configura os três pinos dos LEDs (8, 10 e 12) como saídas (`OUTPUT`), pois o Arduino irá enviar um sinal elétrico para eles.

#### 2.1.3 Função `loop()`

&emsp;Esta função é executada continuamente, em um ciclo infinito.

1.  **Controle de Tempo (Sem `delay()`):**
    * `unsigned long tempoAtual = millis();`: A cada ciclo do `loop`, o código captura o tempo atual (em milissegundos) desde que o Arduino foi ligado. A função `millis()` é o "relógio" do nosso sistema.

2.  **Máquina de Estados (`switch (estado)`):**
    * O `switch` verifica o valor da variável `estado` para decidir qual bloco de código deve ser executado.

3.  **`case 0:` (Estado Verde)**
    * `digitalWrite(...)`: Acende o LED verde (`1` ou `HIGH`) e apaga os outros dois (`0` ou `LOW`).
    * `if(tempoAtual - tempoAnterior >= tempoVerde)`: Esta é a lógica principal. O código calcula o tempo decorrido (`tempoAtual - tempoAnterior`).
    * Se o tempo decorrido for maior ou igual ao `tempoVerde` (4000ms), significa que a fase verde terminou.
    * `estado = 1;`: O programa muda a variável `estado` para `1`, para que na próxima vez que o `loop` rodar, ele execute o `case 1` (Amarelo).
    * `tempoAnterior = tempoAtual;`: O tempo "anterior" é atualizado para o tempo "atual". Isso "reseta o cronômetro" para a próxima fase.

4.  **`case 1:` (Estado Amarelo)**
    * `digitalWrite(...)`: Acende o LED amarelo e apaga os outros.
    * `if(...)`: Verifica se o tempo decorrido (`tempoAtual - tempoAnterior`) atingiu o `tempoAmarelo` (2000ms).
    * `estado = 2;`: Se o tempo passou, muda o estado para `2` (Vermelho).
    * `tempoAnterior = tempoAtual;`: Salva o tempo da mudança para "resetar o cronômetro".

5.  **`case 2:` (Estado Vermelho)**
    * `digitalWrite(...)`: Acende o LED vermelho e apaga os outros.
    * `if(...)`: Verifica se o tempo decorrido atingiu o `tempoVermelho` (6000ms).
    * `estado = 0;`: Se o tempo passou, muda o estado de volta para `0` (Verde), reiniciando o ciclo do semáforo.
    * `tempoAnterior = tempoAtual;`: Salva o tempo da mudança.


## 3. Avaliação dos Pares


Paulo Vitor Barros de Almeida
|Critério|	Contempla (Pontos)|	Contempla Parcialmente (Pontos)	|Não Contempla (Pontos)	|Observações do Avaliador|
|-|-|-|-|-|
|Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores	|Até 3	|Até 1,5	|0 | 3 - Organização exemplar|	
|Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo	|Até 3	|Até 1,5	|0 |3 - Temporização está adequada |	
|Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |	Até 3|	Até 1,5 |	0 |3 - Sem comentários |	
|Ir além: Implementou um componente de extra, fez com millis() ao invés do delay() e/ou usou ponteiros no código |	Até 1 |	Até 0,5 |	0 | 1 - muito bom, economizou bits definindo a variável de estado|	
| | | | |Pontuação Total = 10|


Sarah Araujo Duarte
|Critério|	Contempla (Pontos)|	Contempla Parcialmente (Pontos)	|Não Contempla (Pontos)	|Observações do Avaliador|
|-|-|-|-|-|
|Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores	|Até 3	|Até 1,5	|0 |3 - Montagem bem organizada |	
|Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo	|Até 3	|Até 1,5	|0 |3 - Temporização adequada |	
|Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |	Até 3|	Até 1,5 |	0 |3 - Código bem estruturado e comentários adequados |	
|Ir além: Implementou um componente de extra, fez com millis() ao invés do delay() e/ou usou ponteiros no código |	Até 1 |	Até 0,5 |	0 | 1 - Usou millis() |	
| | | | |Pontuação Total = 10|