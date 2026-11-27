---
layout: default
title: Estudo da Blue-Pill
---

[🏠 Início do TG](https://inca-eu-brassel.github.io/bioimpedancia-tg/) | [📁 Voltar para Conteúdos e Estudo](https://inca-eu-brassel.github.io/bioimpedancia-tg/#conteúdos-e-estudo) | [🌐 Meu Portfólio Principal](https://inca-eu-brassel.github.io/)

---

# Glossário Técnico - TG2: Plataforma de Bioimpedância e Integração STM32 / Processing

Este glossário reúne os principais termos, conceitos e ferramentas abordados durante o desenvolvimento e a configuração inicial do microcontrolador e do ambiente de integração sistêmica.

## Termos e Conceitos

* **Assistente Gráfico de `.ioc`**  
  Ferramenta visual integrada ao STM32CubeIDE (proveniente do ecossistema CubeMX) que permite configurar pinos, clocks e periféricos de maneira gráfica através de uma interface de cliques, gerando automaticamente os arquivos de inicialização do projeto.

* **UART (Universal Asynchronous Receiver-Transmitter)**  
  Protocolo de comunicação serial assíncrona utilizado para a troca de dados entre o microcontrolador e outros dispositivos (como um computador via cabo serial ou conversor USB), utilizando normalmente dois pinos de sinal: **TX** (Transmissão) e **RX** (Recepção).

* **UART em C puro (Bare-Metal)**  
  Abordagem de programação de baixo nível na qual a comunicação serial e os periféricos são configurados e controlados manipulando diretamente os registradores de hardware do chip (endereços de memória específicos), sem o auxílio de bibliotecas de abstração prontas.

* **UART via HAL (Hardware Abstraction Layer)**  
  Abordagem baseada na camada de abstração de hardware oficial da STMicroelectronics. Ela fornece funções padronizadas e de alto nível (como `HAL_UART_Transmit`) que simplificam o uso do periférico UART, tornando o código mais portátil e legível sem exigir o gerenciamento manual direto dos registradores internos.

# Relatório da Tarefa 1.A: Arduino - Envio Sequencial de 1 a 1000

## 1. Visão Geral da Tarefa
* **Plataforma Utilizada:** Arduino Nano V3
* **Objetivo:** Estabelecer a comunicação serial (UART) e enviar uma contagem numérica sequencial de 1 a 1000 de forma contínua, utilizando um intervalo de controle de 1 milissegundo por iteração.
* **Taxa de Transmissão (Baud Rate):** 115200 bps para assegurar alta velocidade de transmissão sem gargalos de comunicação.

## 2. Passo a Passo do que Foi Feito
* **Configuração do Ambiente:** Seleção do modelo da placa (Arduino Nano) e da porta de comunicação ativa no ambiente de desenvolvimento.
* **Desenvolvimento da Rotina:** Implementação da estrutura padrão com `setup()` para inicializar a serial e `loop()` contendo um laço `for` de 1 a 1000 com comandos de escrita serial e espaçamento temporal.
* **Validação no Monitor Serial:** Configuração do Baud Rate para 115200 na interface de monitoramento para sincronizar a taxa de transmissão com o código-fonte.

## 3. Dificuldades Encontradas e Soluções
* **Conflito de Porta COM (`Exit Status 1` / Tentativa na COM9):**
  * *Dificuldade:* A ferramenta de upload tentava realizar a gravação em uma porta incorreta devido a configurações anteriores salvas no perfil da IDE.
  * *Solução:* Verificação e seleção explícita da porta correta (`COM3`), seguida da reconexão física do cabo USB para reativar o barramento.
* **Caracteres Corrompidos no Monitor Serial (``):**
  * *Dificuldade:* Exibição de símbolos ilegíveis na tela de monitoramento logo após o upload do código.
  * *Solução:* Ajuste da taxa de transmissão (Baud Rate) na janela do Monitor Serial da IDE, alterando o padrão para 115200 baud para corresponder ao parâmetro definido em `Serial.begin()`.

---

## 4. Código Comentado (`MASTER_EN.ino`)

```cpp
/**
 * @file           : MASTER_EN.ino
 * @author         : Projeto TG2 - Bioimpedância e Integração
 * @brief          : Envio de sequência numérica de 1 a 1000 via UART (115200 bps)
 */

void setup() {
  // Inicializa a comunicação serial (UART) a 115200 bps (taxa rápida para comunicação eficiente)
  Serial.begin(115200);
}

void loop() {
  // Executa uma contagem sequencial de 1 até 1000
  for (int i = 1; i <= 1000; i++) {
    Serial.println(i); // Envia o número atual seguido de quebra de linha
    delay(1);          // Aguarda exatamente 1 milissegundo por iteração
  }
}

<img src="../sinal mil.png" alt="Sinal Mil" width="800" />

# Relatório da Tarefa 1.B. Usar um potenciômetro para mandar sinal AD (Analógico-Digital) e Simular Buffer

## 1. Visão Geral da Tarefa
- **Plataforma Utilizada:** Arduino Nano V3 (comunicação USB/UART na porta `COM3`)
- **Componente de Entrada:** Potenciômetro ligado à entrada analógica `A0`
- **Objetivo:** Realizar a leitura analógica contínua (`analogRead`) de um sinal variável de `0 V` a `5 V` gerado pelo potenciômetro, convertê-lo para a escala digital de 10 bits (`0` a `1023`) e transmiti-lo pela porta serial a `115200 bps` para acompanhamento no Serial Monitor e no Serial Plotter.
- **Taxa de Transmissão (Baud Rate):** `115200 bps`

## 2. Ligações e Passo a Passo do Desenvolvimento
- **Conexão do Potenciômetro na Protoboard:**
  - **Cabo Preto (`GND`):** conectado ao pino `GND` do Arduino Nano V3 (referência `0 V`).
  - **Cabo Amarelo (`OP` — terminal central/cursor):** conectado ao pino analógico `A0` (sinal variável de `0 V` a `5 V`).
  - **Cabo Vermelho (`IP` — alimentação):** conectado ao pino `5V` do Arduino Nano V3 (`+5 V`).
- **Configuração e Upload:** Carregamento do sketch na IDE do Arduino com taxa serial de `115200 bps`, utilizando o Serial Monitor para inspeção numérica e o Serial Plotter para visualização gráfica contínua da variação do eixo (*knob*).

## 3. O Que Foi Aprendido / Consultas Técnicas
- **Por que a leitura vai de `0` a `1023`:** O conversor analógico-digital (ADC) do Arduino Nano V3 possui resolução de **10 bits** ($2^{10} = 1024$ níveis, de `0` a `1023`) para a faixa de referência de `0 V` a `5 V`. Cada incremento de `1` unidade equivale a aproximadamente `4,88 mV` (`0,00488 V`).
- **Comportamento do `delay` vs. Serial Plotter:** Alterar o `delay(...)` no código controla o intervalo entre leituras/envios no loop, exigindo a confirmação de que o novo upload foi concluído na placa. O **Serial Plotter** mostrou-se especialmente útil para enxergar visualmente a transição contínua de `0` a `1023` ao girar o potenciômetro.
- **Oscilação em valores intermediários:** Ao tentar manter o potenciômetro parado em posições intermediárias, pequenas oscilações na leitura ocorrem devido a ruído elétrico na alimentação USB/referência, pequenas variações de contato mecânico no cursor/protoboard e à alta sensibilidade do passo de ~`4,88 mV` do ADC de 10 bits. Como melhorias práticas discutidas: filtro cerâmico de `100 nF` entre `A0` e `GND` e média de leituras no código.
- **Conceito de Buffer:** Um *buffer* é uma região temporária de memória que atua como fila/memória intermediária entre processos ou dispositivos com ritmos diferentes (como buffers de transmissão/recepção `TX`/`RX` na comunicação serial ou janelas/arrays de amostras para suavização de sinais), evitando perda de dados e absorvendo variações de tempo, respeitando limites de capacidade e latência.
- **Limpeza do Serial Monitor:** A saída do Serial Monitor pode ser limpa rapidamente pelo atalho `Ctrl + L` (ou `Cmd + L`) ou pelo botão de limpar saída da IDE, apagando apenas o texto exibido sem interromper a execução do Arduino.

## 4. Dificuldades Encontradas e Soluções
- **Porta Serial Ocupada (`Port monitor error: command 'open' failed: Serial port busy. Could not connect to COM3 serial port.`):**
  - *Dificuldade:* Tentativa de abrir o Serial Monitor enquanto a porta `COM3` já estava em uso por outro processo/janela ou logo após operação serial.
  - *Solução:* Fechar outros programas/janelas que acessam a `COM3` (como Serial Plotter, Processing ou outra instância/aba), aguardar ou reconectar o cabo USB quando necessário e confirmar a porta ativa no sistema.
- **Oscilação de leitura em posições intermediárias:**
  - *Dificuldade:* Pequena trepidação dos valores quando o eixo do potenciômetro é posicionado em pontos do meio da escala.
  - *Solução:* Compreensão da origem física/elétrica do fenômeno (ruído, contato e resolução de 10 bits) e identificação de estratégias de mitigação via hardware (capacitor de `100 nF` entre `A0` e `GND`) e software (média de leituras).

---

## 5. Código Comentado (`teste_1b_potenciometro.ino`)

```cpp
/**
 * @file   : teste_1b_potenciometro.ino
 * @author : Projeto TG2 - Bioimpedância e Integração
 * @brief  : Leitura analógica de potenciômetro em A0 (0 a 1023) enviada via UART a 115200 bps
 */

const int potPin = A0; // Pino analógico conectado ao cursor (fio amarelo / OP) do potenciômetro

void setup() {
  // Inicializa a comunicação serial a 115200 bps para compatibilidade com Serial Monitor/Plotter
  Serial.begin(115200);
}

void loop() {
  // Lê a tensão em A0 (0 V -> 0; 5 V -> 1023 no ADC de 10 bits)
  int valorAD = analogRead(potPin);

  // Envia o valor convertido seguido de quebra de linha (\n)
  Serial.println(valorAD);

  // Intervalo entre leituras (ajuste este valor conforme a taxa de amostragem desejada)
  delay(10);
}

<img src="../Potenciômetro BK10.jpg" alt="Sinal Mil" width="800" />
