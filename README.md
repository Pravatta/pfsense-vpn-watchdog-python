# 🛡️ VPN Watchdog em Python para pfSense com alerta WhatsApp

Este é um script de watchdog desenvolvido em **Python** para servidores **pfSense**//**SSH** com OpenVPN. Ele realiza o monitoramento em tempo real da conexão VPN via Python e reinicia automaticamente a VPN em caso de falha, além de enviar alertas pelo **WhatsApp** utilizando a API do [CallMeBot](https://www.callmebot.com/).

---

## ✅ Funcionalidades

- Monitoramento em paralelo de múltiplos pfSense via `sockstat | grep openvpn`
- Reinício automático da VPN com `pfSsh.php playback svc stop/start`
- Alertas via WhatsApp (CallMeBot API)
- Logs individuais para cada servidor, salvos no mesmo diretório do script
- Verificação a cada 5 segundos sem flood

---

## 🔧 Requisitos

- Python 3.8 ou superior
- Bibliotecas:

```bash
pip install paramiko requests
```

---

## ⚙️ Configuração

### 1. Servidores
Edite a lista de servidores no início do script:

```python
SERVIDORES = [
    {"nome": "VPN SERVIDOR 1", "host": "exemplo1.ddns.net"},
    ...
]
```

### 2. SSH
Preencha seu usuário e senha SSH:

```python
USERNAME = "seu_usuario"
PASSWORD = "sua_senha"
```

### 3. WhatsApp (CallMeBot)

**Passo a passo:**

1. Adicione este número no WhatsApp: `+34 603 21 25 65`
2. Envie a mensagem: `I allow callmebot to send me messages`
3. Você receberá sua **API key**
4. Preencha no script:

```python
WHATSAPP_PHONE = "55SEUNUMERO"  # Ex: 5511999999999
WHATSAPP_APIKEY = "SUA_APIKEY"
```

---

## 🚀 Como executar

```bash
python vpn_watchdog.py
```

- O script roda todos os servidores em paralelo
- Logs são gerados no mesmo diretório:

```
vpn_monitor_vpn_servidor_1.log
vpn_monitor_vpn_servidor_2.log
...
```

---

## 📲 Exemplo de alerta no WhatsApp

```
⚠️ Falha na VPN de VPN SERVIDOR 1, reiniciando...
✅ VPN de VPN SERVIDOR 1 restaurada com sucesso.
```

---

## 🔐 Observação

Evite versionar este script com usuário/senha reais ou chave de API. Use variáveis de ambiente em ambientes produtivos.
