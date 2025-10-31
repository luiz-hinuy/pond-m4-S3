# Documentação do Projeto: Semáforo com Arduino

Este documento detalha a montagem do circuito e o funcionamento do código para um projeto de semáforo simples, utilizando um Arduino UNO e três LEDs.

## 1. Componentes Utilizados

A tabela a seguir lista todos os componentes necessários para a montagem do circuito:

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

Link do vídeo explicando brevemente o funcionamento: https://youtube.com/shorts/fTmejCvoXOU?feature=share








Paulo Vitor
|Critério|	Contempla (Pontos)|	Contempla Parcialmente (Pontos)	|Não Contempla (Pontos)	|Observações do Avaliador|
|-|-|-|-|-|
|Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores	|Até 3	|Até 1,5	|0 | 3 - Organização exemplar|	
|Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo	|Até 3	|Até 1,5	|0 |3 - Temporização está adequada |	
|Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |	Até 3|	Até 1,5 |	0 |3 - Sem comentários |	
|Ir além: Implementou um componente de extra, fez com millis() ao invés do delay() e/ou usou ponteiros no código |	Até 1 |	Até 0,5 |	0 | 1 - muito bom, economizou bits definindo a variável de estado|	
| | | | |Pontuação Total = 10|





Sarah Araújo
|Critério|	Contempla (Pontos)|	Contempla Parcialmente (Pontos)	|Não Contempla (Pontos)	|Observações do Avaliador|
|-|-|-|-|-|
|Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores	|Até 3	|Até 1,5	|0 |3 - Montagem bem organizada |	
|Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo	|Até 3	|Até 1,5	|0 |3 - Temporização adequada |	
|Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |	Até 3|	Até 1,5 |	0 |3 - Código bem estruturado e comentários adequados |	
|Ir além: Implementou um componente de extra, fez com millis() ao invés do delay() e/ou usou ponteiros no código |	Até 1 |	Até 0,5 |	0 | 1 - Usou millis() |	
| | | | |Pontuação Total = 10|