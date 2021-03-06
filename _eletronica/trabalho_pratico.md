---
layout: default
title: Eletrônica -- Trabalho Prático
---

{{page.title}}
==============

O trabalho final de eletrônica deverá ser um projeto prático utilizando os 
conhecimentos explorados ao longo da disciplina. Alguns exemplos de projetos
realizados em semestres anteriores estão listados abaixo.

* Jogo _TRON_ com uma matriz de LEDs
  [(link)](https://www.hackster.io/carolina-coimbra/jogo-eletronica-80ff11).
* Robô seguidor de linha
  [(link)](https://www.hackster.io/azevedoarthur/line-follower-robot-arthur-azevedo-ufmg-ba01ac).
* Sistema de controle de posição de um pêndulo acionado com uma hélice
  [(link)](http://www.instructables.com/id/Sistema-Pêndulo-Hélice-Controle-De-Posição).
* Jogo com uma matriz LCD 
  [(link)](https://www.hackster.io/fabricio-franca/western-dodge-f7c633).
* Robô com desvio de obstáculos com sensor ultrassônico
  [(link)](https://www.hackster.io/samuel-costa/carro-desviador-de-objetos-com-arduino-207c76).
* Instrumento musical de arco elétrico gerado com um transformador _flyback_
  de televisão de tubo.
* Um carrinho de controle remoto acionado por Arduino e controlado por 
  bluetooth de um celular.
* Jogo de Tetris com um display de matriz de LEDs acionado pelo CI 
  MAX7219 pela interface SPI de um Arduino.
* Protótipo de controle automático longitudinal de um VANT com Arduino que
  acionava um servo com um regulador PD baseado na informação lida de uma IMU
  MPU6050 através da interface I²C.

Sites como [arduino.cc], [hackster.io], [instructables.com] ou [maker.pro]
tem várias idéias de projetos interessantes. Não é necessário usar o Arduino,
mas ele é recomendado por ser uma plataforma relativamente simples, barata,
difícil de queimar acidentalmente e com vários exemplos disponíveis. Algumas
alternativas ao Arduino são o [NodeMCU] ou [ESP8266], microcontroladores WiFi
muito utilizados para projetos IoT.

Os trabalhos deverão ser feitos
individualmente ou em duplas ou trios de alunos da mesma turma prática. 
A avaliação consistirá de:

* uma apresentação inicial em sala, descrevendo o projeto, seus objetivos e
  como se planeja implementá-lo;
* a realização do projeto nas aulas de laboratório;
* uma apresentação final do projeto em sala, mostrando o resultado final, o
  que funcionou e o que não;
* um guia de como reproduzir o projeto com um vídeo mostrando os resultados
  finais.

Apresentação inicial
--------------------

A apresentação inicial deverá ser feita em até 10 minutos. 
O projetor e o quadro da sala poderão ser utilizados.
Os temas abaixo devem ser abordados:

* os objetivos do projeto,
* os principais componentes necessários (eletrônicos e mecânicos),
* uma estimativa de gastos,
* projetos semelhantes,
* as referências consultadas (sites, guias, livros, datasheets, etc).

Apresentação final
------------------

A apresentação final deverá ser feita em até 10 minutos.
O projetor e o quadro da sala poderão ser utilizados.
O projeto deverá ser apresentado novamente e, se possível, demonstrado em
funcionamento. 
Caso isso não seja possível, um vídeo deverá ser exibido.
Além disso, deverão ser apresentados

* o que foi revisado do projeto inicial e por quê,
* os principais componentes utilizados (eletrônicos e mecânicos),
* as maiores dificuldades e as soluções encontradas.

Documentação do projeto 
-----------------------

Ao final do projeto deverá ser feita uma documentação do projeto como um guia
dos passos para se reproduzir o projeto. Esse guia deverá incluir:

* um vídeo do projeto em funcionamento,
* uma lista de materiais com orçamento aproximado,
* um diagrama esquemático do montagem elétrica e eletrônica,
* os códigos-fonte do microcontrolador,
* o princípio de funcionamento do projeto,
* referências utilizadas.

Um exemplo legal de projeto bem documentado é o robô [equilibrista] do 
instructables. O guia e o vídeo devem ser colocados online em sites como 
[hackster.io], [instructables.com], [maker.pro] ou [github.io].


[arduino.cc]: https://create.arduino.cc/projecthub
[equilibrista]: http://www.instructables.com/id/Arduino-Self-Balancing-Robot-1
[ESP8266]: https://www.espressif.com/en/products/hardware/esp8266ex/overview
[github.io]: http://github.io
[hackster.io]: http://hackster.io
[instructables.com]: http://www.instructables.com/technology/arduino/
[maker.pro]: https://maker.pro/arduino/projects
[NodeMCU]: http://www.nodemcu.com/index_en.html
