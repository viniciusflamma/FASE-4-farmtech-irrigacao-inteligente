# 🌱 Sistema de Irrigação Inteligente - FarmTech Solutions

## 📌 Visão Geral
Sistema IoT para automação agrícola que monitora condições do solo e controla irrigação automaticamente, desenvolvido como projeto acadêmico na FIAP.

## 🎯 Funcionalidades Principais
- **Monitoramento em tempo real** de umidade, pH e nutrientes
- **Controle automatizado** da bomba de irrigação
- **Armazenamento de dados** em banco SQL
- **Simulação completa** na plataforma Wokwi

## 🛠️ Componentes e Tecnologias

### 💻 Hardware
| Componente          | Função                          | Pino ESP32 |
|---------------------|---------------------------------|------------|
| DHT22               | Sensor de umidade/temperatura   | GPIO16     |
| LDR                 | Simulador de pH (0-14)          | GPIO34     |
| Botões (2x)         | Sensores de Fósforo e Potássio  | GPIO4/5    |
| Relé 5V             | Acionamento da bomba            | GPIO17     |
| Resistores 10kΩ (4x)| Circuitos de pull-down/up       | -          |

### 📚 Software
- **Microcontrolador**: ESP32 (Programado em C++)
- **Backend**: Python 3.x com SQLite3
- **Ferramentas**: Wokwi, PlatformIO, VS Code

## 📁 Estrutura do Projeto
```
farmtech-irrigacao/
├── firmware/               # Código do ESP32
│   ├── src/                # Códigos-fonte
│   │   ├── prog1.cpp        # Lógica principal
│   │   
│   ├── platformio.ini      # Configuração do PlatformIO
│   └── diagram.json        # Esquema do circuito Wokwi
│
├── backend/                # Sistema de dados
│   ├── database.py/         # Operações CRUD
│   |--CRUD.PY
│   |--irrigation.dp
└── README/                   # Documentação
    ├── images/             # Diagramas e prints
    └── manual_operacao.md  # Guia de uso
```

## 🔧 Instalação e Execução

### 1. Simulação Online
[![Open in Wokwi](https://img.shields.io/badge/Simular-Wokwi-blue)](https://wokwi.com/projects/SEU_PROJETO)

### 2. Execução Local
**Pré-requisitos**:
- PlatformIO (extensão VS Code)
- Python 3.8+

**Passos**:
```bash
# Clonar repositório
git clone https://github.com/seu-usuario/farmtech-irrigacao.git
cd farmtech-irrigacao

# Upload para ESP32
cd firmware
platformio run --target upload

# Iniciar banco de dados
cd ../backend
python database.py
```

## 📊 Lógica de Controle
```cpp
void loop() {
  // Leitura dos sensores
  float umidade = dht.readHumidity();
  float pH = map(analogRead(ldrPin), 0, 4095, 0, 14);
  bool temFosforo = digitalRead(botaoP);
  bool temPotassio = digitalRead(botaoK);

  // Controle da bomba
  if (umidade < 30 && pH >=5 && pH <=7 && temFosforo && temPotassio) {
    digitalWrite(rele, HIGH);
    Serial.println("Bomba LIGADA");
  } else {
    digitalWrite(rele, LOW);
  }
}
```

## 📝 Documentação Técnica

### Fluxo de Dados
1. Sensores → ESP32 → Serial Monitor
2. Serial Monitor → Script Python → Banco SQL
3. Banco SQL → Análises/Visualização

## 🚨 Solução de Problemas
| Sintoma                | Possível Causa               | Solução                      |
|------------------------|------------------------------|------------------------------|
| Erro no DHT22          | Falta de resistor pull-up    | Adicionar 10kΩ entre DATA e 3.3V |
| Botão não responde     | Resistor pull-down incorreto | Verificar conexão com GND    |
| Valores de pH erraticos| LDR mal calibrado            | Ajustar mapeamento no código |

## 📄 Licença
MIT License - Consulte o arquivo [LICENSE](LICENSE) para detalhes.
