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
