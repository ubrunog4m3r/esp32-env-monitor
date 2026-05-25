# 🌡️ esp32-env-monitor

Projeto de estudo em IoT com **ESP32** para monitoramento ambiental em tempo real. O sistema coleta temperatura, umidade e distância via sensores e envia os dados periodicamente para o **Supabase** por requisições HTTP POST.

---

## 📋 Funcionalidades

- Leitura de **temperatura** e **umidade** com o sensor DHT22
- Leitura de **distância** com o ultrassônico HC-SR04
- Envio automático dos dados para o **Supabase** a cada 5 segundos
- Controle do sistema por **botão físico** com debounce
- Indicação visual do estado com **LED verde** (ativo) e **LED vermelho** (inativo)
- Reconexão automática ao Wi-Fi
- Monitor serial para acompanhamento em tempo real

---

## 🛠️ Hardware utilizado

| Componente | Descrição |
|---|---|
| ESP32 | Microcontrolador principal com Wi-Fi integrado |
| DHT22 | Sensor de temperatura e umidade |
| HC-SR04 | Sensor ultrassônico de distância |
| LED verde | Indica sistema ativo |
| LED vermelho | Indica sistema inativo |
| Botão | Liga/desliga o monitoramento |

### Pinagem

| Componente | Pino ESP32 |
|---|---|
| Botão | GPIO 13 |
| HC-SR04 TRIG | GPIO 26 |
| HC-SR04 ECHO | GPIO 27 |
| LED Verde | GPIO 21 |
| LED Vermelho | GPIO 19 |
| DHT22 | GPIO 23 |

---

## 📁 Estrutura do projeto

```
esp32-env-monitor/
├── Estudo_DHT.ino   # Arquivo principal (setup, loop, definições)
├── rede.ino         # Conexão e reconexão Wi-Fi
├── sensores.ino     # Leitura do DHT22 e HC-SR04
├── http.ino         # Envio dos dados para o Supabase
├── outros.ino       # Controle de botão, LEDs e monitor serial
└── secrets.h        # Credenciais (não versionar!)
```

---

## ⚙️ Configuração

### 1. Pré-requisitos

- [Arduino IDE](https://www.arduino.cc/en/software) com suporte ao ESP32
- Bibliotecas necessárias:
  - `DHT sensor library` (Adafruit)
  - `WiFi.h` (incluída no ESP32 core)
  - `HTTPClient.h` (incluída no ESP32 core)

### 2. Credenciais

Crie o arquivo `secrets.h` na raiz do projeto com base no modelo abaixo. **Nunca faça commit desse arquivo.**

```cpp
#ifndef SECRETS_H
#define SECRETS_H

#define WIFI_SSID     "NOME_DA_REDE"
#define WIFI_PASSWORD "SENHA_DA_REDE"
#define SUPABASE_URL  "URL_DO_SUPABASE"
#define API_KEY       "CHAVE_DA_API"

#endif
```

> ⚠️ O arquivo `secrets.h` já está listado no `.gitignore` para evitar exposição de credenciais.

### 3. Supabase

Crie uma tabela no seu projeto Supabase com ao menos as colunas:

```sql
create table leituras (
  id bigint generated always as identity primary key,
  temperatura float,
  umidade float,
  created_at timestamp with time zone default now()
);
```

Copie a URL da tabela e a chave de API (`anon key`) para o `secrets.h`.

---

## 🚀 Como usar 

1. Clone este repositório
2. Abra o arquivo `Estudo_DHT.ino` na Arduino IDE
3. Configure o `secrets.h` com suas credenciais
4. Selecione a placa **ESP32** e a porta correta
5. Faça o upload do código
6. Pressione o botão para ativar o monitoramento — o LED verde acenderá
7. Acompanhe as leituras pelo monitor serial (115200 baud)

---

## 📡 Fluxo de dados

```
Sensores (DHT22 + HC-SR04)
        ↓
      ESP32
        ↓ (HTTP POST a cada 5s)
    Supabase
```

---

## 📝 Licença

Este projeto é de uso educacional e livre para estudo e modificação.
