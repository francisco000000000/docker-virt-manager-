[![](https://github.com/m-bers/docker-virt-manager/workflows/docker%20build/badge.svg)](https://github.com/m-bers/docker-virt-manager/actions/workflows/deploy.yml)[![](https://img.shields.io/docker/pulls/mber5/virt-manager)](https://hub.docker.com/r/mber5/virt-manager)
# Docker virt-manager
### Interface de usuário da web GTK Broadway para libvirt
![Docker virt-manager](docker-virt-manager.gif)

## O que é?
virt-manager: https://virt-manager.org/
broadway: https://developer.gnome.org/gtk3/stable/gtk-broadway.html

## Recursos:
* Usa o backend GTK3 Broadway (HTML5) - não precisa de vnc, xrdp, etc.!
* Suporte a senha/frase-senha SSH via ttyd (obrigado a [@obazda20](https://github.com/obazda20/docker-virt-manager) pela ideia!) Basta clicar no ícone do terminal no canto inferior esquerdo para acessar o prompt de senha após adicionar uma conexão ssh.
<img width="114" alt="Screen Shot 2021-10-25 at 12 01 02 AM" src="https://user-images.githubusercontent.com/4750774/138649110-73c097cc-b054-424c-8fa0-d0c23540b499.png">

* Modo escuro

## Requisitos:
git, docker, docker-compose, pelo menos um host libvirt/kvm

## Uso

### docker-compose

Se o docker e libvirt estiverem no mesmo host
```yaml
services: 
  virt-manager:
    image: franciscodockers/docker-virt-manager-plus:latest
    restart: always
    ports:
      - 8185:80
    environment:
    # Set DARK_MODE to true to enable dark mode
      DARK_MODE: false

    # Set HOSTS: "['qemu:///session']" to connect to a user session
      HOSTS: "['qemu:///system']"

    # If on an Ubuntu host (or any host with the libvirt AppArmor policy,
    # you will need to use an ssh connection to localhost
    # or use qemu:///system and uncomment the below line

    # privileged: true

    volumes:
      - "/var/run/libvirt/libvirt-sock:/var/run/libvirt/libvirt-sock"
      - "/var/lib/libvirt/images:/var/lib/libvirt/images"
    devices:
      - "/dev/kvm:/dev/kvm"
```
Se o docker e libvirt estiverem em hosts diferentes
```yaml
services: 
  virt-manager:
    image: franciscodockers/docker-virt-manager-plus:latest
    restart: always
    ports:
      - 8185:80
    environment:
    # Defina DARK_MODE como "true" para habilitar o modo escuro
      DARK_MODE: false

      # Substitua as strings de conexão qemu separadas por vírgulas, por exemplo:
      # HOSTS: "['qemu+ssh://user@host1/system', 'qemu+ssh://user@host2/system']"
      HOSTS: "[]"
    # volumes:
      # Se não estiver usando autenticação por senha, substitua o local da chave privada ssh, por exemplo:
      # - /home/user/.ssh/id_rsa:/root/.ssh/id_rsa:ro
```
### Construindo a partir do Dockerfile
```bash
    git clone [https://github.com/m-bers/docker-virt-manager.git](https://github.com/francisco000000000/docker-virt-manager-.git)
    cd docker-virt-manager
    docker build -t docker-virt-manager . && docker-compose up -d
```
Vai para http://localhost:8185 no seu navegador
