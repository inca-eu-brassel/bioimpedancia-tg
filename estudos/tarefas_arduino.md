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

# Relatório da Tarefa 1: Arduino - Envio Sequencial de 1 a 1000

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
