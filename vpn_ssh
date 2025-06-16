import paramiko
import time
import requests
import threading
import os
from datetime import datetime

# Lista de servidores para monitorar
SERVIDORES = [
    {"nome": "VPN SERVIDOR 1", "host": "exemplo1.ddns.net"},
    {"nome": "VPN SERVIDOR 2", "host": "exemplo2.ddns.net"},
    {"nome": "VPN SERVIDOR 3", "host": "exemplo3.ddns.net"},
    {"nome": "VPN SERVIDOR 4", "host": "exemplo4.ddns.net"}
]

PORT = 22
USERNAME = "seu_usuario"
PASSWORD = "sua_senha"

# API WhatsApp (CallMeBot)
WHATSAPP_PHONE = "SEU_NUMERO"
WHATSAPP_APIKEY = "SUA_APIKEY"

# Diretório base onde o script está localizado
BASE_DIR = os.path.dirname(os.path.abspath(__file__))

# Funções auxiliares
def log_evento(nome, msg):
    log_file = os.path.join(BASE_DIR, f"vpn_monitor_{nome.replace(' ', '_').lower()}.log")
    with open(log_file, "a") as log:
        log.write(f"[{datetime.now().strftime('%Y-%m-%d %H:%M:%S')}] {msg}\n")

def enviar_whatsapp(msg):
    url = f"https://api.callmebot.com/whatsapp.php?phone={WHATSAPP_PHONE}&text={msg}&apikey={WHATSAPP_APIKEY}"
    try:
        requests.get(url, timeout=10)
    except Exception as e:
        print(f"Erro ao enviar WhatsApp: {e}")

# Monitora um servidor individualmente
def monitorar_vpn(nome, host):
    vpn_ativa = True
    aguardando_retorno = False
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

    try:
        client.connect(host, port=PORT, username=USERNAME, password=PASSWORD)
        print(f"Monitorando VPN de {nome}... (Ctrl+C para sair)")

        while True:
            stdin, stdout, stderr = client.exec_command("sockstat | grep openvpn")
            saida = stdout.read().decode().strip()

            if "udp4" not in saida:
                if vpn_ativa:
                    msg_queda = f"⚠️ Falha na VPN de {nome}, reiniciando..."
                    print(f"[{datetime.now().strftime('%H:%M:%S')}] {msg_queda}")
                    log_evento(nome, "VPN caiu. Reiniciando...")
                    enviar_whatsapp(msg_queda)
                    client.exec_command("pfSsh.php playback svc stop openvpn client 1")
                    time.sleep(3)
                    client.exec_command("pfSsh.php playback svc start openvpn client 1")
                    aguardando_retorno = True
                    vpn_ativa = False
            else:
                if aguardando_retorno:
                    msg_restaurada = f"✅ VPN de {nome} restaurada com sucesso."
                    print(f"[{datetime.now().strftime('%H:%M:%S')}] {msg_restaurada}")
                    log_evento(nome, "VPN restaurada com sucesso.")
                    enviar_whatsapp(msg_restaurada)
                    aguardando_retorno = False
                    vpn_ativa = True

            time.sleep(5)

    except Exception as e:
        log_evento(nome, f"Erro: {e}")
        print(f"Erro em {nome}: {e}")
    finally:
        client.close()

# Início do monitoramento em paralelo
if __name__ == "__main__":
    threads = []
    for servidor in SERVIDORES:
        t = threading.Thread(target=monitorar_vpn, args=(servidor["nome"], servidor["host"]))
        t.start()
        threads.append(t)

    for t in threads:
        t.join()
