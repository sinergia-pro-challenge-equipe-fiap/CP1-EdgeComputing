# 🖥️ Sistema de Monitoramento Vinheria Agnello

Grupo: **Sinergia Pro**  
Disciplina: **Edge Computing & Computer Systems**  
Checkpoint 1  

---

## 📝 Visão Geral

Este projeto faz parte da primeira etapa do sistema de monitoramento ambiental para a **Vinheria Agnello**. O objetivo é garantir as condições ideais para o armazenamento dos vinhos, que são diretamente influenciadas pelas variáveis de **luminosidade**, **temperatura** e **umidade**. Na fase inicial, desenvolvemos um sistema baseado em **Arduino** para monitorar a luminosidade do ambiente.

Com a implementação de sensores, LEDs e buzzer, o sistema é capaz de monitorar a luminosidade, exibir informações em um display LCD, e gerar alertas visuais e sonoros caso os níveis de luminosidade não atendam aos parâmetros estabelecidos.

---

## 🔍 Funcionalidades

- **Leitura da luminosidade** por meio de um sensor LDR.
- **Conversão dos dados** com o ADC (Conversor Analógico para Digital) do Arduino.
- **Normalização dos valores** utilizando a função `map()` (0 a 100%).
- **Sinalização com LEDs**:
  - 🟢 **Verde**: Luminosidade adequada.
  - 🟡 **Amarelo**: Alerta – Buzzer soa por 3 segundos.
  - 🔴 **Vermelho**: Luminosidade fora dos padrões aceitáveis.
- **Exibição no display LCD** de mensagens de boas-vindas e status do sistema.

---

## 🧰 Componentes Utilizados

### Hardware

- 1x **Arduino UNO**
- 1x **Sensor LDR**
- 1x **Resistor 10kΩ** (para o divisor de tensão)
- 3x **LEDs** (verde, amarelo e vermelho)
- 3x **Resistores 220Ω** (para os LEDs)
- 1x **Buzzer piezoelétrico**
- **Jumpers e protoboard**

### Software

- **Arduino IDE** – versão 1.8 ou superior

---

## ⚙️ Como Reproduzir

1. **Monte o circuito** com os seguintes pinos sugeridos:
   - LDR: A0
   - LEDs: 8, 9, 10
   - Buzzer: 7
2. **Faça upload do código** usando a **Arduino IDE**.
3. **Ajuste os limites de luminosidade** no código, se necessário.
4. **Alimente o Arduino** via USB ou fonte externa.
5. **Teste o sistema** em diferentes condições de luminosidade e veja os LEDs e o display LCD respondendo.

---

## 🖥️ Código

Aqui está a parte relevante do código que exibe no monitor serial o tempo de funcionamento do sistema e realiza a leitura da luminosidade, acionando os LEDs e o buzzer conforme os valores de luminosidade:

```cpp
// Exibe no monitor serial o tempo desde que o sistema foi ligado
Serial.println("Há quanto tempo o sistema foi ligado: ");  // Mensagem indicando que o próximo valor será o tempo de funcionamento do sistema.

Serial.print("Dias: ");    // Exibe a palavra "Dias:" no monitor serial
Serial.println(d);         // Exibe o valor da variável 'd' (dias) no monitor serial

Serial.print("Horas: ");   // Exibe a palavra "Horas:" no monitor serial
Serial.println(h);         // Exibe o valor da variável 'h' (horas) no monitor serial

Serial.print("Minutos: "); // Exibe a palavra "Minutos:" no monitor serial
Serial.println(m);         // Exibe o valor da variável 'm' (minutos) no monitor serial

Serial.print("Segundos: "); // Exibe a palavra "Segundos:" no monitor serial
Serial.println(s);          // Exibe o valor da variável 's' (segundos) no monitor serial

delay(3000); // Aguarda 3 segundos (3000 milissegundos) antes de continuar com a execução do programa

analogValue = analogRead(sensorLed);                    // Lê o valor analógico do sensor de luminosidade
percentage = map(analogValue,0,1000,0,100);             // Mapeia o valor para uma escala de 0 a 100
Serial.println(percentage);                             // Exibe o valor de luminosidade em percentual no monitor serial

if (percentage < 40){                                   // Se a luminosidade for menor que 40%
  digitalWrite(ledGreen,HIGH);                          // Acende o LED verde
  Serial.println("Tudo OK!!!");                         // Exibe mensagem "Tudo OK!!!"
  lcd.setCursor(0,0);                                   // Define a posição do cursor no LCD
  lcd.print("Tudo OK!!!");                              // Exibe "Tudo OK!!!" no LCD
  delay(3000);                                          // Aguarda 3 segundos
  digitalWrite(ledGreen,LOW);                           // Desliga o LED verde
} else if(percentage < 60){                              // Se a luminosidade for entre 40% e 60%
  digitalWrite(ledYellow,HIGH);                         // Acende o LED amarelo
  digitalWrite(Buzzer,HIGH);                            // Aciona o buzzer
  Serial.println("Cuidado, a luminosidade está em nível de alerta!!!!!!!");  // Exibe alerta no monitor serial
  lcd.setCursor(0,0);                                   // Define a posição do cursor no LCD
  lcd.print("ALERTA!!!!");                              // Exibe "ALERTA!!!!" no LCD
  delay(3000);                                          // Aguarda 3 segundos
  digitalWrite(ledYellow,LOW);                          // Desliga o LED amarelo
  digitalWrite(Buzzer,LOW);                             // Desliga o buzzer
} else{                                                 // Se a luminosidade for maior que 60%
  digitalWrite(ledRed,HIGH);                            // Acende o LED vermelho
  Serial.println("Luminosidade alta demais, risco de problemas!!!!!!"); // Exibe alerta no monitor serial
  lcd.setCursor(0,0);                                   // Define a posição do cursor no LCD
  lcd.print("RISCO ALTO!!!!");                          // Exibe "RISCO ALTO!!!!" no LCD
  delay(3000);                                          // Aguarda 3 segundos
  digitalWrite(ledRed,LOW);                             // Desliga o LED vermelho
};
