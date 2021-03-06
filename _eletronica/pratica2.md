---
layout: default
title: Eletrônica -- Prática 2
---

{{ page.title }}
================
{:.no_toc}

#### Leitura analógica, Potenciômetro, PWM e Servo
{:.no_toc}

Nesta prática, continuaremos abordamos as funcionalidades básicas do Arduino e
dispositivos comumente utilizados em projetos de eletrônica.

**Tópicos:**
* Procedimento x
* Procedimento y
{:toc}

Potenciômetro
-------------

Potenciômetros são resistores de três terminais com um terminal móvel.
Esses dispositivos são muito utilizados em eletrônica como uma entrada contínua
do usuário ou algum ajuste como tensão, volume ou brilho.

A construção de um potenciômetro está representada no diagrama da abaixo.
Há uma tira de material resistivo entre os terminais _A_ e _B_,
e sobre ela desliza um terminal móvel conectado ao pino _m_ do
potenciômetro.
Como a resistência de um material é proporcional ao seu comprimento,
a resistência entre os terminais fixos é fixa e a resistência entre os 
terminais fixos e o móvel depende da distância entre eles.

{%
   include figure.html
   file="pot.svg"
   caption="Diagrama da construção de um potenciômetro."
%}

Um potenciômetro sem a cobertura externa pode ser visto na figura abaixo.
Em primeiro plano temos o terminal móvel fazendo contato com a tira de material
resistivo. Ao fundo temos os três terminais do dispositivo.

{%
   include figure.html
   file="single-turn_potentiometer_with_internals_exposed_oblique_view.jpg"
   caption="Um potenciômetro sem a cobertura externa.
            Fonte: [Wikimedia Commons][pot-src], licença Creative Commons 
            [CC0 1.0] Universal -- Dedicação ao Domínio Público."
   img_style="width: 60%"
%}

### Modelo do potenciômetro

Abaixo temos o símbolo do potenciômetro em diagramas de circuito.

{%
   include figure.html
   file="pot-simb.svg"
   caption="Simbolo de um potenciômetro, mostrando os terminais fixos (A, B)
            e o móvel (m)."
   img_class="width-200px"
%}

Para analisar seu comportamento em um circuito, temos um modelo linear simples.
Representando a posição do terminal móvel por $$p\in[0,1]$$, onde $$p=0$$
corresponde ao terminal móvel sobre o terminal _A_ e $$p=1$$  correponde
ao terminal móvel sobre o terminal _B_, o potenciômetro equivale ao
circuito da figura abaixo, no qual $$R_{AB}$$ é a resistência 
fixa do material entre os terminais _A_ e _B_.

{%
   include figure.html
   file="pot-circuito.svg"
   caption="Circuito equivalente ao potenciômetro, em função da posição 
            do terminal móvel $$p\in[0,1]$$."
   img_class="width-400px"
%}

Na aplicação mais comum com o Arduino, os terminais fixos do potenciômetro são
ligados entre o terra e o 5V. Além disso, o terminal móvel é conectado a algum
circuito de medição com alta resistência de entrada, de forma que a corrente
por ele seja desprezível. Dessa forma, a tensão no terminal móvel é
proporcional à sua posição:

$$
  V_m = p \cdot 5V.
$$

Leitura analógica
-----------------

O Arduino Uno, utilizado nas práticas, possui 6 entradas analógicas.
Essas entradas funcionam como voltímetros digitais que medem níveis de tensão
entre 0V e 5V.
O conversor analógico/digital do Arduino possui resolução de 10 bits, o que
significa que os valores lidos variam de 0 a 1023 ($$2^{10}$$ valores
possíveis).
A função `analogRead(pino)` retorna a leitura analógica do canal especificado.
Confira a documentação oficial [dessa função][analogRead] para
mais informações.

### Procedimento: Arduino como voltímetro

O código abaixo mostra como transformar o Arduino em um voltímetro com fundo de
escala 5V. A tensão entre o terra e o pino analógico será impressa na porta
Serial.

```c++
void setup() {
  Serial.begin(230400);
}

void loop() {
  int medida = analogRead(A0);
  
  //medida será um inteiro de 0 a 1023
  //o fator de conversão para volts é 0.0048828125
  
  Serial.print(0.0048828125 * medida); 
  Serial.println(" V");
}
```

Teste esse código medindo a tensão do potencial móvel de um potenciômetro,
como mostrado na figura abaixo. A tensão medida pelo Arduino deverá
variar de 0 a 5V.

{%
   include figure.html
   file="potenciometro-a0.svg"
   caption="Montagem com potenciômetro para leitura analógica."   
%}

Modulação por largura de pulso
------------------------------

A modulação por largura de pulso é uma técnica para se variar a tensão média
fornecida para um componente utilizando uma saída de dois estados (digital).
É a mesma técnica utilizada para se variar a luminosidade do LED na prática 1.

Como as saídas digitais do Arduino possuem somente dois estados, 0V e 5V, 
seu estado é variado em alta frequência para que, na média, a carga perceba
uma tensão média entre os estados discretos possíveis.

Considere a imagem abaixo.
A razão cíclica (_duty cycle_) é o fração do período em que a onda permanece
no nível lógico alto.
Se uma onda de tensão com razão cíclica de 75% for aplicada a um componente,
a tensão média é 75% da tensão máxima.
Dessa forma, a tensão _média_ no componente pode ser variada entre 0
e a tensão do nível lógico alto.
A razão cíclica de 0% corresponde ao nível lógico baixo de de 100% corresponde
ao nível lógico alto.

{%
   include figure.html
   file="pwm_sparkfun.jpg"
   caption="Fonte: [Sparkfun][Tutorial PWM], licença [CC BY-SA 4.0]."
%}

O microcontrolador do Arduino possui um periférico para gerar onda com
modulação por largura de pulso (PWM, do inglês _pulse width modulation_)
em hardware, sem intervenção do usuário.
A função `analogWrite(pino, razao_ciclica)` é usada para gerar uma onda com
PWM.
O argumento `razao_ciclica` é um número de 0 a 255, correspondendo a uma razão
de 0% a 100%.
Antes de utilizá-la é necessário configurar o pino como saída através da função
`pinMode(pino, OUTPUT)`.
Confira a documentação oficial da função [analogWrite] para maiores informações.

### Procedimento: modulação por largura de pulso para alterar o brilho de um led

Este procedimento utiliza o circuito representado no diagrama esquemático
abaixo. O LED é alimentado pela saída digital número 9 do Arduino.
Quando a razão cíclica da onda PWM no pino 9 for 0% o LED irá apagar e quando
for 100% o LED estará no brilho máximo.

{%
   include figure.html
   file="pwm-led.svg"
   caption="Montagem para acionamento do LED por PWM."   
%}

O código altera a razão cíclica periodicamente de 0 a 100% e de volta a 0%.

```
int pino_led = 9; // Número do pino do LED

void setup() {
  pinMode(pino_led, OUTPUT); // Define o pino do LED como saída
}

void loop() {
  for (int rc=0; rc<255; rc++) {
    analogWrite(pino_led, rc);
    delay(10);
  }

  for (int rc=255; rc>0; rc--) {
    analogWrite(pino_led, rc);
    delay(10);
  }
}
```

> ### Atividade: Comando do brilho do LED com potenciômetro
>
> Comande a razão cíclica do LED com um potenciômetro. Para o terminal móvel do
> potenciômetro em cada um dos extremos o LED deverá estar com brilho máximo ou
> apagado. Nas posições intermediárias o brilho deverá ser proporcional à tensão
> medida.

Servomotor de radiocontrole
---------------------------

Servomotores são sistemas controlados com motores que permitem comandar a
posição do eixo de saída.
Servomotores de radiocontrole são utilizados para comando remoto de veículos
não tripulados como aeromodelos ou automodelos.
Eles geralmente são compostos de um pequeno motor de corrente contínua
conectado a um sistema de engrenagens cujo eixo de saída é um potenciômetro.
Dessa forma, a posição do eixo é medida e controlada por um sistema 
realimentado que atua no motor.

Esses dispositivos possuem 3 terminais: alimentação, terra e sinal.
A alimentação é o terminal central, de cor vermelha.
O terra é o terminal correspondente ao cabo preto ou marrom e o sinal
corresponde ao fio amarelo ou laranja.
A referência de posição é enviada pelo terminal de sinal como um pulso.
A duração do pulso codifica a posição desejada do eixo.
Um pulso de 1500 microssegundos corresponde à posição central do eixo, de
90 graus, enquanto
que as durações de 600 microssegundos e 2400 microssegundos correspondem, 
respectivamente, a deflexões de aproximadamente 0 graus e
180 graus.
Os pulsos devem estar espaçados de pelo menos 20 milissegundos entre si.

Usar um pulso de duração variável permite diferenciar o comando nulo de perda
de sinal de rádio.
Quando nenhum pulso é recebido o servo mantém a última referência recebida
ao invés de dar a deflexão máxima.
Esse comportamento é mais seguro para o comando de aeronaves remotamente 
pilotadas.
Um recurso adicional para mais informações sobre o funcionamento e uso de
servomotores é o [tutorial de servos da Sparkfun][tut-servo].

### Procedimento: Comando de um Servo

Abaixo temos um código de exemplo para comando de um servomotor de 
radiocontrole utilizando a biblioteca Servo do Arduino. Nesse exemplo,
o servo é comandado da posição 0 a 180 graus, e de volta de 179 a 0.
Seu movimento deverá ser contínuo.

```c++

// Inclui a biblioteca ao programa
#include <Servo.h> 

// Declara um objeto servo. Até 6 servos podem ser criados
Servo servo1; 

void setup() {
  servo1.attach(9);  // Define o pino 9 para o comando do servo
}

void loop() {
  // Varia a posição de 0 a 180 graus
  for (int pos=0; pos<=180; pos++) {
    servo1.write(pos); // Comanda o servo para a posição
    delay(15); // Aguarda 15 milissegundos entre incrementos
  }
  
  // Varia a posição de 179 a 0 graus
  for (int pos=179; pos>=0; pos--) {
    servo1.write(pos); // Comanda o servo para a posição
    delay(15); // Aguarda 15 milissegundos entre incrementos
  }
}
```

Para executar esse exemplo, o terminal vermelho do servo deve ser conectado
ao 5V, o terminal marrom ou preto ao terra e o terminal laranja ou branco
ao pino digital 9 do Arduino, como mostrado na figura abaixo.

{%
   include figure.html
   file="montagem_servo_protoboard.png"
   caption="Montagem para comando de um servo com o Arduino.
            Fonte: [arduino.cc][servo-sweep], 
            licença Creative Commons Attribution Share-Alike."
%}

> ### Atividade: Comando do servo com potenciômetro
>
> Comande a posição do servo com um potenciômetro. Para o terminal móvel do
> potenciômetro em cada um dos extremos a haste do servo deverá estar nos seus
> extremos. Nas posições intermediárias a haste estará em uma posição
> proporcional à do potenciômetro.
> 
> **Dica:** Utilize a função [map].

[analogRead]: https://www.arduino.cc/en/Reference/AnalogRead
[analogWrite]: https://www.arduino.cc/en/Reference/AnalogWrite
[CC0 1.0]: https://creativecommons.org/publicdomain/zero/1.0/deed.pt
[CC BY-SA 4.0]: https://creativecommons.org/licenses/by-sa/4.0/
[map]: https://www.arduino.cc/reference/pt/language/functions/math/map/
[pot-src]: https://commons.wikimedia.org/wiki/File:Single-turn_potentiometer_with_internals_exposed,_oblique_view.jpg
[servo-sweep]: https://www.arduino.cc/en/Tutorial/Sweep
[tut-servo]: https://learn.sparkfun.com/tutorials/hobby-servo-tutorial

[Tutorial PWM]: https://learn.sparkfun.com/tutorials/pulse-width-modulation
