---
layout: default
title: Estudo da Blue-Pill
---

[🏠 Início do TG](https://inca-eu-brassel.github.io/bioimpedancia-tg/) | [📁 Voltar para Conteúdos e Estudo](https://inca-eu-brassel.github.io/bioimpedancia-tg/#conteúdos-e-estudo) | [🌐 Meu Portfólio Principal](https://inca-eu-brassel.github.io/)

---

# Estudo da Blue-Pill: Gravação com ST-LINK V2 para teste pisca led

<img src="../Conexão ST-LINK V2.png" alt="Detalhes da Blue-Pill" width="800" />

# Relatório de Progresso: Configuração Inicial e Teste de Bancada (Blink STM32F103)

## 1. O que foi feito:
* **Definição do Ambiente de Desenvolvimento:** Configuração do STM32CubeIDE para a placa **STM32F103C8T6 (Blue Pill)**[cite: 6], integrando o escopo do projeto voltado à plataforma de hardware e integração sistêmica[cite: 6].
* **Implementação Bare-Metal (Registradores Diretos):** Desenvolvemos uma rotina em C puro no arquivo `main.c` contornando a ausência do assistente gráfico de `.ioc` na versão atual da IDE. O código manipula diretamente os registradores de clock (`RCC_APB2ENR`) e de E/S (`GPIOC_CRH`, `GPIOC_ODR`) para alternar o estado lógico do pino **PC13** (onde o LED integrado da Blue Pill está conectado).
* **Comissionamento e Gravação:** Estabelecemos a conexão física via gravador **ST-Link V2** utilizando a interface SWD (pinos SWCLK e SWDIO), realizando a compilação sem erros e o upload bem-sucedido do firmware para a memória flash do microcontrolador (*Download verified successfully*).

## 2. Dificuldades Encontradas:
* **Ausência do Gerador `.ioc` e Estrutura Padrão:** O assistente de projetos da versão utilizada da IDE gerou uma estrutura simplificada sem as pastas de HAL/CubeMX (`Core`), exigindo uma abordagem de programação de baixo nível (acesso direto a registradores).
* **Falta de Dependências de Cabeçalho:** Tentativas iniciais de usar funções HAL geraram erros de compilação por falta do arquivo `main.h`. Isso foi resolvido criando um código autossuficiente baseado no mapeamento de memória do datasheet do chip STM32F103.
* **Validação de Firmware do ST-Link:** O utilitário exigiu uma atualização de firmware do gravador que requisitava o modo DFU. Como se tratava de um clone sem botão físico de boot dedicado, a solução passou pela reinicialização da porta USB e pela utilização do perfil de comunicação padrão via SWD.

---

## 3. Código Utilizado (`main.c`)

```c
/**
 ******************************************************************************
 * @file           : main.c
 * @author         : Auto-generated / User Commissioning
 * @brief          : Main program body for Blue Pill LED Blink (Bare-Metal Registers)
 ******************************************************************************
 */

#include <stdint.h>

/* Definições diretas dos registradores da Blue Pill (STM32F103) */
#define RCC_BASE      (0x40021000UL)
#define GPIOC_BASE    (0x40011000UL)

#define RCC_APB2ENR   (*(volatile uint32_t *)(RCC_BASE + 0x18))
#define GPIOC_CRH     (*(volatile uint32_t *)(GPIOC_BASE + 0x04))
#define GPIOC_ODR     (*(volatile uint32_t *)(GPIOC_BASE + 0x0C))

void delay(volatile uint32_t count) {
    while(count--) {
        __asm__("nop");
    }
}

int main(void)
{
    /* 1. Habilita o clock para a porta GPIOC (Bit 4 do RCC_APB2ENR) */
    RCC_APB2ENR |= (1 << 4);

    /* 2. Configura o pino PC13 como saída Push-Pull de 2MHz 
       No registrador CRH, o pino 13 ocupa os bits 20 a 23.
       Colocamos 0011 nos bits 20-23 para configurar como Output max speed 2MHz. */
    GPIOC_CRH &= ~(0xF << 20);
    GPIOC_CRH |=  (0x3 << 20);

    /* Loop infinito */
    while (1)
    {
        /* Liga o LED no PC13 (Na Blue Pill, nível LOW (0) acende o LED integrado) */
        GPIOC_ODR &= ~(1 << 13);
        delay(500000); // Atraso aproximado

        /* Desliga o LED (nível HIGH (1) apaga o LED) */
        GPIOC_ODR |= (1 << 13);
        delay(500000); // Atraso aproximado
    }
}
