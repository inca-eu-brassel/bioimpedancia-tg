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
- **Por que a leitura vai de `0` a `1023`:** O conversor analógico-digital (ADC) do Arduino Nano V3 possui resolução de **10 bits** (2^{10} = 1024 níveis, de `0` a `1023`) para a faixa de referência de `0 V` a `5 V`. Cada incremento de `1` unidade equivale a aproximadamente `4,88 mV` (`0,00488 V`).
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

<img src="../Potenciômetro BK10.jpg" alt="Sinal Mil" width="400" />
<img src="../Tarefa 1B.png" width="800" />

# Relatório da Tarefa 2: Aquisição Serial e Visualização Gráfica no Processing

## 1. Visão Geral da Tarefa
* **Objetivo:** Ler dados enviados pela porta serial (inicialmente via Arduino Nano com potenciômetro a `115200 bps`) e plotá-los em tempo real no **Processing**, construindo base visual e interativa para futuras etapas do TG2 (integração com o hardware de bioimpedância baseado em **STM32F103C8T6 / Blue Pill** e interface inspirada nas referências do projeto, como o trabalho do Emerson/Gustavo)[cite: 2, 3].
* **Contexto no TG2:** O uso do Arduino Nano e do potenciômetro nesta etapa atua como validação didática e de bancada para a comunicação serial e plotagem no Processing, antes da substituição pelo conjunto de hardware do projeto final com STM32 e eletrodos/circuito de bioimpedância[cite: 2, 3].

## 2. Dúvidas, Consultas e Aprendizados
* **Primeiros passos no Processing e visualização do console:** Ao iniciar no Processing 4.5.2 (sem experiência prévia na ferramenta), a dúvida inicial foi como localizar o índice da porta serial (`COM`) quando o console estava recolhido ou vazio antes da execução. O uso de `printArray(Serial.list())` e a expansão do painel **Console** permitem identificar o índice exato da porta ativa.
* **Como o Processing "reconheceu" o Arduino e o baud rate:** Não houve autodetecção inteligente oculta; o próprio código selecionou o primeiro item da lista (`Serial.list()[0]`) — que correspondia ao Arduino Nano ativo na porta `COM3` — e abriu a comunicação explicitamente a `115200 bps`, coincidindo com o `Serial.begin(115200)` já gravado na memória do microcontrolador[cite: 2, 3].
* **O que são dispositivos seriais e o que ocorreria em outros cenários:** Dispositivos seriais transmitem bits sequencialmente (muito comuns via portas `COM` / conversores USB-serial, como microcontroladores, sensores, módulos UART, balanças ou impressoras fiscais/térmicas seriais). Na prática, a numeração de portas no Windows começa em `COM1` (não existe `COM0`), e se houvesse outro dispositivo serial antes na lista, `Serial.list()[0]` conectaria nele.
* **Como diferenciar quando há mais de um Arduino conectado:** `Serial.list()` retorna apenas a lista de portas disponíveis (`COM3`, `COM4`, etc.), sem identificar qual placa é qual. Para diferenciar com segurança, pode-se verificar qual porta surge ao conectar cada placa, fixar/selecionar a porta adequada ou implementar uma identificação/handshake via texto enviado por cada placa.
* **Relação com o TG2 (Arduino + potenciômetro vs. STM32 + bioimpedância):** No TG2, o microcontrolador principal é o **STM32 (Blue Pill)**[cite: 2, 3], e o potenciômetro funciona apenas como simulador rápido de sinal de bancada, enquanto no sistema final haverá eletrodos e circuito de bioimpedância[cite: 2]. Além disso, o **ST-Link V2** é usado para gravação/debug (SWD), enquanto o envio de dados seriais (`UART`) para o computador/Processing passa pela interface serial/conversor USB-UART correspondente[cite: 2, 3].

## 3. Dificuldades Encontradas e Soluções
* **Erro de importação no início (`processing.net.*` vs. `processing.serial.*`):** A importação inicial com `processing.net.*` não carrega a classe `Serial`. A correção foi utilizar `import processing.serial.*;`.
* **Console sem saída antes da execução / painel minimizado:** O índice da porta só aparece após rodar o sketch que chama `printArray(Serial.list())` e observar o painel inferior **Console** expandido.
* **Conflito de porta COM com o Monitor Serial da Arduino IDE:** Manter o Monitor Serial aberto na mesma porta `COM` impede o Processing de abrir a conexão; fechar o Monitor Serial antes de executar o sketch resolve o bloqueio.
* **Diferença de escala entre potenciômetro (`0..1023`) e `map(..., 1, 1000, ...)`:** No sketch simples, o `map` estava configurado para `1` a `1000`, enquanto a leitura analógica do potenciômetro vai de `0` a `1023`[cite: 3], podendo gerar pequeno deslocamento nos extremos até ajustar a faixa para `0, 1023`.

## 4. Comandos Básicos e Recursos Úteis do Processing
* **Ciclo do programa:** `setup()` executa uma vez na inicialização; `draw()` executa continuamente em loop.
* **Desenho e texto:** `size(largura, altura)`, `background(...)`, `stroke(...)`, `strokeWeight(...)`, `fill(...)`, `point(...)`, `line(...)`, `rect(...)`, `text(...)`.
* **Conversão de faixa:** `map(valor, minEntrada, maxEntrada, minSaida, maxSaida)` converte leituras (ex.: `0..1023`) para coordenadas em pixels[cite: 3].
* **Interação e botões:** Verificação de `mouseX`, `mouseY` e `mousePressed` sobre a área (`x, y, largura, altura`) de um retângulo desenhado na tela (ou uso da biblioteca **ControlP5** para controles prontos como botões e sliders).

---

## 5. Código 1: Leitura Serial e Plotagem Simples (Potenciômetro)

```java
// Importa a biblioteca oficial do Processing para comunicação via porta serial (COM / USB-UART)
import processing.serial.*;

// Objeto que representa a conexão serial aberta com o microcontrolador
Serial minhaPorta;

// Armazena o último valor numérico recebido pela serial (float aceita casas decimais e inteiros)
float valorRecebido = 0;

// Posição horizontal (eixo X) onde o próximo ponto será desenhado na janela
int xPos = 1; // Posição horizontal do traço na tela

// Executada uma única vez quando o programa inicia
void setup() {
  // Cria uma janela gráfica de 800 pixels de largura por 400 pixels de altura
  size(800, 400);
  
  // Preenche todo o fundo da janela com um cinza bem escuro (valor 20 de 0 a 255)
  background(20);
  
  // Lista todas as portas seriais disponíveis no console (útil para conferir a COM)
  printArray(Serial.list());
  
  // Conecta na primeira porta da lista (altere o índice [0] se o Arduino estiver em outra COM)
  String nomePorta = Serial.list()[0];
  
  // Abre a conexão serial nesta janela ("this"), na porta escolhida e a 115200 bits por segundo
  minhaPorta = new Serial(this, nomePorta, 115200);
  
  // Só dispara o serialEvent() quando receber o caractere de quebra de linha ('\n')
  minhaPorta.bufferUntil('\n');
}

// Executada repetidamente em loop contínuo (por padrão a ~60 vezes por segundo)
void draw() {
  // Mapeia o valor recebido (1 a 1000) para a altura da janela (invertendo o eixo Y)
  float yPos = map(valorRecebido, 1, 1000, height - 20, 20);
  
  // Define a cor do traço em verde-água/neon (R=0, G=255, B=150)
  stroke(0, 255, 150);
  
  // Define a espessura do traço em 2 pixels
  strokeWeight(2);
  
  // Desenha um ponto na coordenada horizontal atual (xPos) e altura calculada (yPos)
  point(xPos, yPos);
  
  // Avança no eixo X; ao chegar no fim da tela, limpa e recomeça
  xPos++;
  if (xPos >= width) {
    // Volta o cursor horizontal para perto da margem esquerda
    xPos = 1;
    
    // Apaga o gráfico anterior redesenhando o fundo escuro para iniciar novo ciclo
    background(20);
  }
}

// Função de evento disparada automaticamente quando chegam dados seriais até o '\n'
void serialEvent(Serial porta) {
  // Lê todos os caracteres recebidos no buffer até encontrar '\n'
  String leitura = porta.readStringUntil('\n');
  
  // Garante que a leitura não está nula antes de tentar processar
  if (leitura != null) {
    // Remove espaços em branco, '\n' e '\r' (retorno de carro do Windows/Arduino)
    leitura = trim(leitura); // Remove espaços e '\r'
    
    // Converte o texto limpo (ex.: "512") para número decimal (float) e atualiza a variável
    valorRecebido = float(leitura);
  }
}
```

---
<img src="../Tarefa 2.png" width="800" />

## 6. Código 2: Visualização Aplicada com Grade, Unidades Fictícias, Simulação e Botões

```java
import processing.serial.*;

// --- CONFIGURAÇÃO DE SERIAL ---
boolean usarSerial = true;        // Define se tenta abrir a porta serial na inicialização (false = apenas simulado)
int indicePorta = 0;            // Índice da porta em Serial.list() (ajustar conforme exibido no console)
long baudRate = 115200;         // Taxa de transmissão em bits por segundo (deve coincidir com o microcontrolador)

Serial minhaPorta;              // Instância da conexão serial
boolean serialConectada = false;// Indica se a porta serial foi aberta com sucesso

// --- DADOS E BUFFER DE PLOTAGEM ---
int maxPontos = 250;            // Número máximo de amostras exibidas simultaneamente na janela
float[] bufferSinal = new float[maxPontos]; // Vetor circular/deslizante com as últimas amostras
int contadorAmostras = 0;       // Contador total de amostras registradas desde o início/limpeza

float valorAtual = 0;           // Último valor lido (da serial ou da simulação)
float faseSimulacao = 0;        // Ângulo/fase incremental usado para gerar a onda senoide simulada

// --- CONTROLES DE ESTADO ---
boolean pausado = false;        // Quando true, congela a atualização do buffer e do gráfico
boolean modoSimulado = false;     // Quando true, gera sinal senoidal interno em vez de usar a porta serial

// --- ÁREA DO GRÁFICO (margens reservadas para cabeçalho, eixos e rótulos) ---
float gx, gy, gw, gh;

// --- DEFINIÇÃO DOS BOTÕES: {x, y, largura, altura} ---
float[] btnPausar  = {20,  14, 115, 32}; // Alterna entre Pausar e Retomar
float[] btnLimpar  = {145, 14, 100, 32}; // Limpa o buffer e zera a contagem visível
float[] btnModo    = {255, 14, 155, 32}; // Alterna entre fonte Serial e fonte Simulada

// Executado uma única vez ao iniciar o sketch
void setup() {
  size(900, 520);               // Tamanho da janela gráfica
  smooth();                     // Suavização de linhas e bordas

  // Calcula posição e dimensões da área de plotagem
  gx = 75;
  gy = 65;
  gw = width - 105;
  gh = height - 115;

  // Inicializa todas as posições do buffer com zero
  for (int i = 0; i < maxPontos; i++) {
    bufferSinal[i] = 0;
  }

  // Tenta iniciar a serial se habilitado; caso contrário, entra em modo simulado
  if (usarSerial) {
    inicializarSerial();
  } else {
    modoSimulado = true;
  }
}

// Lista as portas disponíveis e tenta abrir a porta configurada
void inicializarSerial() {
  printArray(Serial.list());
  try {
    if (Serial.list().length > indicePorta) {
      String nomePorta = Serial.list()[indicePorta];
      minhaPorta = new Serial(this, nomePorta, int(baudRate));
      minhaPorta.bufferUntil('\n'); // Dispara serialEvent() a cada quebra de linha '\n'
      serialConectada = true;
      modoSimulado = false;
      println("Conectado em: " + nomePorta);
    } else {
      println("Porta não encontrada. Entrando em modo simulado.");
      modoSimulado = true;
    }
  } catch (Exception e) {
    println("Não foi possível abrir a porta serial. Entrando em modo simulado.");
    modoSimulado = true;
  }
}

// Loop principal de atualização e renderização da interface
void draw() {
  background(18, 22, 28);       // Fundo escuro da janela

  // 1. Atualiza o sinal apenas quando não está pausado
  if (!pausado) {
    if (modoSimulado) {
      // Gera uma senoide contínua normalizada (0.0 a 1.0) e escala para 0..1023
      faseSimulacao += 0.08;
      float senoNormalizado = (sin(faseSimulacao) + 1.0) * 0.5;
      valorAtual = senoNormalizado * 1023.0;
    }
    registrarAmostra(valorAtual);
  }

  // 2. Desenha botões superiores e status
  desenharBarraSuperior();

  // 3. Desenha grade, eixos e unidades fictícias (mV e ms)
  desenharGradeEUnidades();

  // 4. Desenha a curva contínua do sinal armazenado no buffer
  desenharCurva();

  // 5. Exibe valores instantâneos e conversão ilustrativa acima do gráfico
  desenharRodape();
}

// Desloca os dados antigos para a esquerda e insere a nova amostra na última posição
void registrarAmostra(float novoValor) {
  for (int i = 0; i < maxPontos - 1; i++) {
    bufferSinal[i] = bufferSinal[i + 1];
  }
  bufferSinal[maxPontos - 1] = novoValor;
  contadorAmostras++;
}

// Zera o buffer de plotagem e o contador de amostras
void limparBuffer() {
  for (int i = 0; i < maxPontos; i++) {
    bufferSinal[i] = 0;
  }
  contadorAmostras = 0;
}

// Desenha os três botões interativos e o indicador de estado atual
void desenharBarraSuperior() {
  desenharBotao(btnPausar, pausado ? "Retomar" : "Pausar", pausado ? color(210, 140, 40) : color(45, 110, 180));
  desenharBotao(btnLimpar, "Limpar", color(65, 75, 90));

  String rotuloModo = modoSimulado ? "Fonte: Simulado" : "Fonte: Serial";
  desenharBotao(btnModo, rotuloModo, modoSimulado ? color(120, 85, 175) : color(35, 145, 105));

  fill(180, 190, 205);
  textAlign(RIGHT, CENTER);
  textSize(12);
  String statusTexto = pausado ? "ESTADO: PAUSADO" : "ESTADO: COLETANDO";
  text(statusTexto, width - 25, 30);
}

// Renderiza um botão retangular com efeito hover ao passar o cursor
void desenharBotao(float[] b, String rotulo, color corBase) {
  boolean hover = mouseSobre(b);
  fill(hover ? lerpColor(corBase, color(255), 0.2) : corBase);
  stroke(220, 230, 240, 90);
  strokeWeight(1);
  rect(b[0], b[1], b[2], b[3], 6);

  fill(255);
  textAlign(CENTER, CENTER);
  textSize(12);
  text(rotulo, b[0] + b[2] / 2.0, b[1] + b[3] / 2.0);
}

// Desenha a caixa do gráfico, linhas de grade e rótulos com unidades fictícias
void desenharGradeEUnidades() {
  fill(25, 31, 39);
  stroke(70, 82, 98);
  strokeWeight(1);
  rect(gx, gy, gw, gh);

  int divisoesY = 5;            // Quantidade de divisões horizontais da grade
  int divisoesX = 10;           // Quantidade de divisões verticais da grade

  textSize(11);

  // Linhas horizontais e escala vertical fictícia em mV (0 a 3300 mV)
  for (int i = 0; i <= divisoesY; i++) {
    float y = map(i, 0, divisoesY, gy + gh, gy);
    stroke(45, 54, 66);
    line(gx, y, gx + gw, y);

    float mvFicticio = map(i, 0, divisoesY, 0, 3300);
    fill(165, 175, 190);
    textAlign(RIGHT, CENTER);
    text(nf(mvFicticio, 0, 0), gx - 8, y);
  }

  // Linhas verticais e escala horizontal fictícia em ms
  for (int j = 0; j <= divisoesX; j++) {
    float x = map(j, 0, divisoesX, gx, gx + gw);
    stroke(45, 54, 66);
    line(x, gy, x, gy + gh);

    float msFicticio = map(j, 0, divisoesX, 0, maxPontos * 10); // Exemplo ilustrativo: 10 ms por amostra
    fill(165, 175, 190);
    textAlign(CENTER, TOP);
    text(nf(msFicticio, 0, 0), x, gy + gh + 6);
  }

  // Rótulo rotacionado do eixo Y
  pushMatrix();
  translate(22, gy + gh / 2.0);
  rotate(-HALF_PI);
  fill(120, 210, 255);
  textAlign(CENTER, CENTER);
  textSize(12);
  text("Amplitude (mV fictício)", 0, 0);
  popMatrix();

  // Rótulo do eixo X
  fill(120, 210, 255);
  textAlign(CENTER, CENTER);
  textSize(12);
  text("Tempo de janela (ms fictício)", gx + gw / 2.0, height - 16);
}

// Liga os pontos do buffer formando uma curva contínua dentro da área do gráfico
void desenharCurva() {
  stroke(0, 230, 155);
  strokeWeight(2.2);
  noFill();

  beginShape();
  for (int i = 0; i < maxPontos; i++) {
    float x = map(i, 0, maxPontos - 1, gx, gx + gw);
    float y = map(bufferSinal[i], 0, 1023, gy + gh, gy);
    vertex(x, y);
  }
  endShape();
}

// Mostra valor bruto, tensão fictícia convertida e contagem de amostras
void desenharRodape() {
  float tensaoSimuladaMv = map(valorAtual, 0, 1023, 0, 3300);
  fill(210, 220, 235);
  textAlign(LEFT, CENTER);
  textSize(12);
  text("Valor bruto: " + nf(valorAtual, 0, 1) +
       "   |   Leitura convertida: " + nf(tensaoSimuladaMv, 0, 1) + " mV" +
       "   |   Amostras: " + contadorAmostras, gx, gy - 12);
}

// Trata cliques do mouse sobre os botões
void mousePressed() {
  if (mouseSobre(btnPausar)) {
    pausado = !pausado;
  } else if (mouseSobre(btnLimpar)) {
    limparBuffer();
  } else if (mouseSobre(btnModo)) {
    if (modoSimulado && !serialConectada && usarSerial) {
      inicializarSerial();
    } else {
      modoSimulado = !modoSimulado;
    }
  }
}

// Verifica se o cursor do mouse está dentro de um botão {x, y, largura, altura}
boolean mouseSobre(float[] b) {
  return (mouseX >= b[0] && mouseX <= b[0] + b[2] &&
          mouseY >= b[1] && mouseY <= b[1] + b[3]);
}

// Recebe os dados enviados pela porta serial até '\n'
void serialEvent(Serial porta) {
  String linha = porta.readStringUntil('\n');
  if (linha != null) {
    linha = trim(linha);
    if (linha.length() > 0 && !modoSimulado) {
      float convertido = float(linha);
      if (!Float.isNaN(convertido)) {
        valorAtual = convertido;
      }
    }
  }
}
```
<img src="../Tarefa 2 mais.png" width="800" />
<img src="../Tarefa 2 mais fonte simulado.png" width="800" />
