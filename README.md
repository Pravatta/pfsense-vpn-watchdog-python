# 🔒 VPN Watchdog para pfSense (Python)

Script em Python para monitoramento de conexões VPN OpenVPN em servidores pfSense, com reinício automático via SSH e envio de alertas por WhatsApp utilizando o CallMeBot.

---

## 📌 O que faz este script

- Monitora continuamente o status da VPN com `sockstat | grep openvpn`;
- Reinicia o cliente OpenVPN caso a conexão caia;
- Envia alertas personalizados por WhatsApp usando emojis ⚠️ e ✅;
- Gera arquivos de log separados por servidor no mesmo diretório do script;
- Executa o monitoramento em paralelo para múltiplos servidores pfSense.

---

## 📦 Bibliotecas utilizadas

### `paramiko` (SSH)
Permite que o script se conecte ao pfSense via SSH e execute comandos como `pfSsh.php playback svc start/stop`.

### `requests` (HTTP)
Usado para enviar alertas via API HTTP do CallMeBot para o WhatsApp.

### `threading`
Utilizado para monitorar todos os servidores simultaneamente.

---

## 🧪 Requisitos

- Python 3.8 ou superior

Instale as dependências com:

```bash
pip install paramiko requests
```

---

## ⚙️ Configuração

### 1. Edite os dados dos servidores:

```python
SERVIDORES = [
    {"nome": "VPN SERVIDOR 1", "host": "exemplo1.ddns.net"},
    {"nome": "VPN SERVIDOR 2", "host": "exemplo2.ddns.net"}
]
```

### 2. Preencha os dados de acesso SSH:

```python
USERNAME = "seu_usuario"
PASSWORD = "sua_senha"
```

### 3. Configure o CallMeBot (WhatsApp)

#### 📲 Como ativar o CallMeBot:

1. Adicione este número no WhatsApp: **+34 603 21 25 65**
2. Envie a mensagem:  
   ```
   I allow callmebot to send me messages
   ```
3. Você receberá uma resposta com sua API Key.
4. Atualize o script com seu número e chave:

```python
WHATSAPP_PHONE = "55SEUNUMERO"  # Ex: 5511999999999
WHATSAPP_APIKEY = "CHAVE_RECEBIDA"
```

---

## 🚀 Como usar

1. Salve o script como `vpn_watchdog.py`.
2. Execute com:

```bash
python vpn_watchdog.py
```

> O monitoramento será iniciado em paralelo para todos os servidores configurados.

---

## 🗂️ Logs

O script cria logs individuais por servidor, salvos no mesmo diretório onde o `.py` é executado:

```
vpn_monitor_vpn_servidor_1.log
vpn_monitor_vpn_servidor_2.log
```

---

## 📌 Exemplo de alerta no WhatsApp

```
⚠️ Falha na VPN de VPN SERVIDOR 1, reiniciando...
✅ VPN de VPN SERVIDOR 1 restaurada com sucesso.
```
