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

## 📚 Conteúdos e Estudo
*(Esta seção reúne todo o embasamento teórico, análises e materiais de estudo)*
- **Firmware (STM32):** Estudos de arquitetura, configuração de portas seriais (UART), amostragem e conversão A/D.
- **Software (Processing):** Manipulação de dados na porta COM, tratamento de pacotes e plotagem em tempo real (`beginShape`, `vertex`).
- **Fundamentação Teórica:** Princípios de bioimpedância elétrica e instrumentação biomédica.

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
