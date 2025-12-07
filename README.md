# 🚀 Projeto: Servidor Web Intranet com Linux e Nginx

Este projeto documenta a criação de um servidor web doméstico rodando em uma VM Linux, configurado para ser acessível por qualquer dispositivo na rede local (Intranet).

## 📸 Topologia e Resultado Final


<img width="783" height="361" alt="Image" src="https://github.com/user-attachments/assets/7c3f4293-ecd7-4aa0-a8d6-6d7a36b71098" />

![Image](https://github.com/user-attachments/assets/9f6dc6b7-09f0-4c5d-b356-2582b9e7dd76)

## 🛠️ Tecnologias e Ferramentas
* **Hypervisor:** VMware Workstation (Modo Bridge)
* **SO:** Ubuntu Server 24.04 LTS
* **Serviços:** Nginx (Web), OpenSSH (Acesso Remoto)
* **Rede:** IPv4 Estático Manual



# 🏃‍♂️ Primeiro passo é criar uma máquina com virtual ubuntu server

Configurações mínimas ja seram suficientes
Nossa configuração mais importante é a de rede, nossa VM tem q este em modo BRIDGE

* Explicação rapida sobre modo BRIDGE
O modo Bridge faz sua máquina virtual (VM) agir como se fosse um dispositivo físico independente conectado diretamente ao seu roteador, "ignorando" o PC hospedeiro
Como funciona: A VM ganha acesso direto à rede física e solicita um IP próprio ao seu roteador (DHCP), ficando na mesma faixa de IP do seu computador (ex: PC 192.168.1.10, VM 192.168.1.11). Para que serve: Permite que a VM seja visível e acessível por outros dispositivos na rede (como seu celular ou outro hosts conectadop a intranet).

# ⚙️ Placa em modo BRIDGE 

🛠️ Primeiro problema e troboshoting
Nesta etapa nosso virtualizado (VMware) estava colocado em modo automático e não encontravam o range de IP da nossa rede. Necessário colocar o IP e o gateway (Roteador doméstico) manualmente. (OBS: Caso nao saiba o IP e o gateway da sua rede use o CMD e atraves do comando IPconfig irar mostrar).

<img width="567" height="279" alt="Image" src="https://github.com/user-attachments/assets/74ed8abd-3604-44b6-bb85-0d376ba80b7f" />

🛠️ segundo problema e troboshoting
Caso voc esteja usando o WIFI sera necessário a configuração para o VMware encontra a sua placa de rede ou WIFI
Va até a aba “Edit” > após va até a opção “VIRTUAL NETWORK EDITOR”

<img width="344" height="222" alt="Image" src="https://github.com/user-attachments/assets/4adb980b-94b1-4615-a2ad-eaa078e527fa" />

Clique em “charge Settings” para habilitar os privilégios de administrador (Neste caso estou usando Windows). 

<img width="567" height="291" alt="Image" src="https://github.com/user-attachments/assets/33fbf573-e7b8-4694-946d-096f04198a2a" />

Veja que minha VM ubuntu esta configurada para pegar a rede bridge da minha placa WI-FI, caso esteja usando conexão via cabo de rede, necessário usar sua placa de rede. 

<img width="546" height="207" alt="Image" src="https://github.com/user-attachments/assets/b1ecae32-6821-495e-aa91-dc06d50ec8de" />


⚙️ Configurando e adicionando nossa VM a rede manualmente 

<img width="567" height="262" alt="Image" src="https://github.com/user-attachments/assets/acdaac5b-8b16-4a91-be5d-662bed6de4f9" />


Como e uma rede doméstica não necessitamos de proxy 


# 💻 Agora com nossa VM já configurada, vamos aos comandos para subir o nosso servidor WEB Nginx

<strong> 1. Instalação do Servidor (No Terminal Linux) </strong>.

Vamos baixar todas as atualizações via Bash: `sudo apt update'`

Vamos baixar o pacote nginx: `sudo apt install nginx'`

Vamos confirma se o pacote esta rodando: `sudo systemctl status nginx '`

<img width="1013" height="263" alt="Image" src="https://github.com/user-attachments/assets/d7397abe-6a1e-44b7-897e-bbbfa4ee4a0d" />



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

<img width="1066" height="747" alt="Image" src="https://github.com/user-attachments/assets/584205b1-4a1f-4862-ad75-1cfa5bdcf163" />


🛠️ Terceiro problema e troboshoting
Obs: caso esteja usando direto no CLI ubuntu server sem interface grafica, sugirimos baixar o pacote SSH, e usar o CMD na sua maquina fisica no windows. assim habilitando o ctrl+c crtl-V


<strong> 3. Configuração de Rede e Firewall </strong>.

Para que o celular consiga acessar, libere a porta e descubra seu IP.

1. Liberar porta 80:Bash
    
    `sudo ufw allow 'Nginx HTTP'`
    
2. Descobrir seu IP:Bash
    
    `ip addr` 192.168.86.50/24
    
    *(Procure o número depois de `inet`, ex: `192.168.0.25`)*.


<strong> ### 4. Como Acessar </strong>.

Pegue o celular, conecte no Wi-Fi e digite no navegador:
`http://SEU_IP_AQUI` (Ex: `http://192.168.0.25`)











