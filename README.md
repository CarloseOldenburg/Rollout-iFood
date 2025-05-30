# Script de Rollout -iFood

Este script foi desenvolvido para realizar o **rollout completo** de atualização dos módulos do sistema VideoSoft em terminais de autoatendimento. Ele realiza a limpeza do cache e tokens antigos, instala a versão correta do launcher, remove versões anteriores dos módulos e instala os pacotes mais recentes com total automação.

---

## ⚙️ Funcionalidades

- 🧹 **Limpa cache e tokens** do Google Chrome e módulos antigos.
- 📥 **Baixa e instala** os seguintes pacotes atualizados:
  - `vs-os-interface` v
  - `pinpad-server`
  - `vsd-payment` 
- ⚙️ **Instala e configura** o `vsd-launcher` com ambiente `food`.
- 📄 Gera um **log de execução** no arquivo `install_log.txt`.
- 🔁 Reinicia o sistema automaticamente ao final da execução.

---

## 📥 Instalação

Para executar o script direto do terminal:

```bash
sudo wget --inet4-only -O- https://raw.githubusercontent.com/CarloseOldenburg/updater/refs/heads/main/rollout.sh | bash
````

---

## 🧱 Pré-requisitos

* Sistema baseado em **Debian** (Ubuntu, Raspbian, etc)
* Acesso root (`sudo`)
* Internet ativa com suporte a IPv4
* Ferramentas básicas instaladas:

  * `wget`
  * `dpkg`
  * `apt`

---

## 📂 O que o script faz

### 1. Limpa tokens e cache do navegador

```bash
rm -r .cache/google-chrome/*
rm -r .config/google-chrome/*
sudo rm -f /opt/videosoft/vs-os-interface/log/_database_token*
sudo rm -f /opt/videosoft/vs-os-interface/log/_database_recovery*
```

### 2. Instala o `vsd-launcher` atualizado

```bash
wget https://raw.githubusercontent.com/wilker-santos/VSDImplantUpdater/main/vsd-launcher.sh -O vsd-launcher
chmod 755 vsd-launcher
mv vsd-launcher /usr/bin/
vsd-launcher -s food
```

### 3. Remove versões antigas dos módulos

```bash
apt purge -y vsd-payment
apt purge -y pinpad-server
rm -rf /home/videosoft/.pinpad_server
rm -f /home/videosoft/DesktopPlugin.db
rm -rf /home/terminal/.pinpad_server
rm -f /home/terminal/DesktopPlugin.db
```

### 4. Instala os novos pacotes

```bash
dpkg -i vs-os-interface_2.24.0_amd64.deb
dpkg -i pinpad-server-installer_linux_3.10.0-beta.deb
dpkg -i vsd-payment_1.4.0_amd64.deb
```

---

## 📄 Log

Todo o processo é registrado no arquivo:

```
install_log.txt
```

Esse arquivo pode ser usado para auditoria ou depuração em caso de falhas.

---

## ⚠️ Observações Importantes

* O script reinicia automaticamente o sistema ao final, **sem solicitação de confirmação**.
* Verifique se nenhum processo crítico está em execução antes de executar este script.
* Certifique-se de que o terminal está conectado à internet com permissão de acesso às URLs de repositórios.

---

## 👤 Autor

Desenvolvido por [Carlos Eduardo Oldenburg](https://github.com/CarloseOldenburg)

---

## 📜 Licença

Distribuído sob licença livre para uso interno corporativo e manutenção de terminais com sistemas VideoSoft.
