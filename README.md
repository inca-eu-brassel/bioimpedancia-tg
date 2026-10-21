# Trabalho de Graduação: Integração de Sistema de Bioimpedância Elétrica

Repositório oficial do meu Trabalho de Graduação em **Engenharia Biomédica** pela Universidade Federal do ABC (UFABC).

---

## 🎯 Objetivo do Projeto
Realizar a integração completa entre o hardware de leitura de bioimpedância elétrica (desenvolvido no Mestrado por Gustavo Pinheiro) e a interface gráfica em Processing (desenvolvida na Graduação por Emerson Graciano).

---

## 👥 Agradecimentos e Referências
* **Gustavo Pinheiro:** Hardware e aquisição de bioimpedância (Mestrado).
* **Emerson Graciano:** Interface gráfica em Processing (Graduação).

---
## Como usar o conteúdo desta página:
1. Decidi começar com o curso da Udemy em alternância com a fundamentação teórica. Vou deixar um cronograma do que está sendo feito e no final vou modificá-lo para o que achar o fluxo mais "coerente".

| Coluna 1: Tópico | Coluna 2: Descrição |
| :--- | :--- |
| 16/10/2026| Fundamentos de GitHub para registro de atividades feitas |
| 17/10/2026 | Planejamento para realização do curso da Udemy (mais detalhes em "Cursos e Práticas (Udemy / Complementares)" + fundamentação teórica |
| 18/10/2026 | Curso Udemy "Seção 1 - Getting Started - Constructing Peripheral Registers from Memory Address" - "1 Downloading our Integrated Development Environment (IDE)" |
| 20/10/2026 | Estudo da Blue-Pill: Hardware |

## 📚 Conteúdos e Estudo
*(Esta seção reúne todo o embasamento teórico, análises e materiais de estudo)*
- [**Firmware (STM32):** Estudos de arquitetura, configuração de portas seriais (UART), amostragem e conversão A/D](https://inca-eu-brassel.github.io/bioimpedancia-tg/estudos/Blue-Pill)
- **Software (Processing):** Manipulação de dados na porta COM, tratamento de pacotes e plotagem em tempo real (`beginShape`, `vertex`).


### 📖 Leituras e Fundamentação Teórica
- [ ] Bioeletricidade Aplicada e Propriedades de Tecidos Biológicos.
- [ ] Processamento de Sinais Bioelétricos (Aplicações Cardíacas e Neurológicas).
- [ ] Acoplamento Neurovascular e Modulação do Fluxo Sanguíneo pelo SNC.

### 💻 Cursos e Práticas (Udemy / Complementares)

* 💻 [Anotações do Curso da Udemy: "Embedded Systems Bare-Metal Programming Ground Up (STM32)"](https://inca-eu-brassel.github.io/bioimpedancia-tg/estudos/udemy-processamento)
* 🧠 [Leituras em Bioeletricidade e Neurociência](./estudos/bioeletricidade.md)

---

## 🛠️ Desenvolvimento e Histórico
*(Este é o diário de bordo e a evolução prática da integração)*
- **Etapa 1:** Configuração do ambiente e testes isolados no STM32.
- **Etapa 2:** Validação e testes isolados da interface em Processing.
- **Etapa 3:** Desenvolvimento do protocolo de comunicação serial (Handshake).
- **Etapa 4:** Testes integrados e validação experimental de sinais.

---

## 📁 Estrutura de Pastas
- `/firmware`: Códigos-fonte e projetos do STM32.
- `/software`: Scripts da interface em Processing.
