# 🚀 Projeto: Servidor Web Intranet com Linux e Nginx

Este projeto documenta a criação de um servidor web doméstico rodando em uma VM Linux, configurado para ser acessível por qualquer dispositivo na rede local (Intranet).

## 🛠️ Tecnologias e Ferramentas
* **Hypervisor:** VMware Workstation (Modo Bridge)
* **SO:** Ubuntu Server 24.04 LTS
* **Serviços (Softwares):** Nginx (Web Server), OpenSSH (Acesso Remoto)
* **Protocolos:** HTTP (Porta 80), SSH (Porta 22), TCP/IP, ARP, IPv4
* **Rede:** Endereçamento IPv4 Estático Manual

## 📸 Topologia e Resultado Final

<img width="783" alt="Topologia e Resultado" src="https://github.com/user-attachments/assets/7c3f4293-ecd7-4aa0-a8d6-6d7a36b71098" /><br>

![Acesso Mobile](https://github.com/user-attachments/assets/9f6dc6b7-09f0-4c5d-b356-2582b9e7dd76)<br>

---

## 🏃‍♂️ 1. Criando a Máquina Virtual (Ubuntu Server)

As configurações mínimas já serão suficientes. Nossa configuração mais importante é a de rede: a VM tem que estar em **modo BRIDGE**.

### O que é o modo BRIDGE?
O modo Bridge faz sua máquina virtual (VM) agir como se fosse um dispositivo físico independente conectado diretamente ao seu roteador, "ignorando" o PC hospedeiro.
* **Como funciona:** A VM ganha acesso direto à rede física e solicita um IP próprio ao seu roteador (DHCP), ficando na mesma faixa de IP do seu computador (ex: PC 192.168.1.10, VM 192.168.1.11).
* **Para que serve:** Permite que a VM seja visível e acessível por outros dispositivos na rede (como seu celular ou outros hosts conectados à intranet).

---

## ⚙️ Configuração de Rede e Troubleshooting

### 🛠️ Primeiro Problema e Troubleshooting (IP Manual)
Nesta etapa, nosso virtualizador (VMware) estava em modo automático e não encontrava o range de IP da nossa rede. Foi necessário configurar o IP e o Gateway (Roteador doméstico) manualmente.
> **OBS:** Caso não saiba o IP e o gateway da sua rede, use o CMD no Windows e digite `ipconfig` para descobrir.

<img width="567" alt="Configuração IP Manual VMware" src="https://github.com/user-attachments/assets/74ed8abd-3604-44b6-bb85-0d376ba80b7f" /><br>

### 🛠️ Segundo Problema e Troubleshooting (Wi-Fi vs Cabo)
Caso você esteja usando Wi-Fi, é necessária uma configuração extra para o VMware encontrar sua placa de rede correta.

1. Vá até a aba **“Edit”** > **“Virtual Network Editor”**.

<img width="344" alt="Virtual Network Editor" src="https://github.com/user-attachments/assets/4adb980b-94b1-4615-a2ad-eaa078e527fa" /><br>

2. Clique em **“Change Settings”** para habilitar privilégios de administrador.

<img width="567" alt="Change Settings" src="https://github.com/user-attachments/assets/33fbf573-e7b8-4694-946d-096f04198a2a" /><br>

3. Garanta que a VM esteja configurada para pegar a rede bridge da sua placa Wi-Fi (se usar cabo, selecione a placa Ethernet).

<img width="546" alt="Seleção de Placa Bridge" src="https://github.com/user-attachments/assets/b1ecae32-6821-495e-aa91-dc06d50ec8de" /><br>

### Configuração no Ubuntu
Configurando e adicionando nossa VM à rede manualmente:

<img width="567" alt="Configuração Netplan" src="https://github.com/user-attachments/assets/acdaac5b-8b16-4a91-be5d-662bed6de4f9" /><br>

*Como é uma rede doméstica, não necessitamos de proxy.*

---

## 💻 2. Instalação e Configuração do Nginx

### Instalação do Servidor (No Terminal Linux)
 
Vamos baixar todas as atualizações via Bash: `sudo apt update'`

Vamos baixar o pacote nginx: `sudo apt install nginx'`

Vamos confirma se o pacote esta rodando: `sudo systemctl status nginx '`

<img width="1013" alt="Status Nginx" src="https://github.com/user-attachments/assets/d7397abe-6a1e-44b7-897e-bbbfa4ee4a0d" /><br>

<strong> 2. Criação da Página (O Painel) </strong>.

Vamos criar a página HTML personalizada.

1. Entre na pasta do site:Bash
    `cd /var/www/html`
    
2. Faça backup do arquivo original:Bash
    `sudo mv index.nginx-debian.html index.old`
    
3. Crie o novo arquivo:Bash
    `sudo nano index.html`
    
4. Cole o código abaixo dentro do editor:

*(Para salvar no Nano: `Ctrl + O` -> `Enter` -> `Ctrl + X`)*

🛠️ Dica de Troubleshooting: Caso esteja usando o Ubuntu Server (sem interface gráfica), sugerimos instalar o pacote OpenSSH. Assim, você pode acessar a VM pelo terminal do Windows (CMD/PowerShell) e usar Ctrl+C / Ctrl+V para colar seu codigo HTMl.<br>

<img width="1066" alt="Código HTML" src="https://github.com/user-attachments/assets/584205b1-4a1f-4862-ad75-1cfa5bdcf163" /><br>


🔒 <strong> 3. Configuração de Rede e Firewall </strong>.

Para que o celular consiga acessar, libere a porta e descubra seu IP.

1. Liberar porta 80: `sudo ufw allow 'Nginx HTTP'`
    
2. Descobrir seu IP: `ip addr` 192.168.86.50/24
   *(Procure o número depois de `inet`, ex: `192.168.0.25`)*.


<strong> ### 4. Como Acessar </strong>.

Pegue o celular, conecte no Wi-Fi e digite no navegador:
`http://SEU_IP_AQUI` (Ex: `http://192.168.0.25`)
