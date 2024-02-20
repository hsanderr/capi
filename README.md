# Comedouro Acionado por Proximidade e Identificação (CAPI)

Este repositório contém código e informações gerais do projeto Comedouro Acionado por Proximidade e Identificação (CAPI).

## Descrição geral do produto

O CAPI consiste em um kit de comedouro e coleira para pets em que a tampa do comedouro é aberta apenas quando o pet autorizado se aproxima usando sua coleira.

## Objetivos do produto

- Permitir que apenas o pet ao qual o alimento dentro do comedouro se destina coma a mesma.
- Conservar melhor o alimento dentro do comedouro.

## Premissas

- O usuário tem acesso a um celular ou computador.
- O pet do usuário deve usar a coleira do produto para comer.
- O usuário tem acesso a uma tomada para alimentar e recarregar o produto.

## Funcionamento básico

O pet usa uma coleira que contém um microcontrolador (nRF52810) de baixo consumo de energia que envia advertisings Bluetooth Low Energy (BLE) constantemente. O advertising BLE é um conjunto de dados transmitido por radiofrequência que pode conter diversos tipos de informação, incluindo o endereço MAC do dispositivo que está transmitindo os dados, através do qual é possível identificar a origem do advertising, e portanto, o pet. O comedouro é equipado com um microcontrolador (ESP32) que realiza constantemente escaneamento por advertisings BLE. Quando um advertising BLE referente da coleira do pet é encontrado nas proximidades (checada pelo indicador de potência do sinal recebido, ou RSSI) pelo comedouro, o microcontrolador envia um sinal para um servo motor abrir a tampa do comedouro através de modulação por largura de pulso (PWM). Para que o usuário (dono do pet) interaja com o comedouro para trocar a coleira autorizada, por exemplo, o microcontrolador do comedouro age como um servidor web ao qual o usuário pode se conectar através do seu celular ou computador e interagir com o microcontrolador através de uma página web hospedada no mesmo. A Figura 1 mostra um diagrama do funcionamento do sistema.

![Figura 1: Diagrama de funcionamento básico do sistema](/imgs/funcionamento-basico.png)
Figura 1: Diagrama de funcionamento básico do sistema

## Design inicial

A Figura 2 mostra o design inicial do comedouro, sujeito a modificações.

![Figura 2: Design inicial do comedouro](/imgs/design-comedouro.jpg)
Figura 2: Design inicial do comedouro

## Requisitos funcionais do produto

- RF01: O comedouro deve realizar o escaneamento BLE com filtro de RSSI continuamente.
- RF02: Quando o usuário segurar um botão por 3 segundos, o comedouro deve iniciar um servidor web com uma página web em que o dono do pet possa trocar a coleira autorizada.
- RF03: Quando o servidor web for iniciado, um LED deve piscar para indicar ao usuário sucesso da operação.
- RF04: Após o envio do formulário da página de RF02 ou após 1 minuto a partir do momento em que o servidor web for iniciado, o mesmo deve ser parado.
- RF05: Quando o servidor web for parado, um LED deve piscar para indicar ao usuário que o servidor foi parado.
- RF05: Quando o escaneamento BLE identificar que coleira autorizada está próxima por duas janelas de escaneamento seguidas e a tampa estiver fechada, a tampa do comedouro deve abrir.
- RF06: Quando a coleira autorizada não for vista por duas janelas de escaneamento seguidas e a tampa estiver aberta, a tampa do comedouro deve fechar. 
- RF07: O comedouro deve inicializar no mesmo modo de operação em relação ao estado da tampa em que se encontrava quando foi desligado.
- RF09: O comedouro deve piscar um LED diferente dos LEDS de RF03 e RF05 periodicamente quando a bateria da coleira autorizada estiver acabando.
- RF08: O comedouro deve operar em dois modos de alimentação, alternados por meio de uma chave: bateria e tomada.

## Requisitos não funcionais do produto

- RNF01: O produto deve permanecer ligado por pelo menos 12 horas quando alimentado por bateria.