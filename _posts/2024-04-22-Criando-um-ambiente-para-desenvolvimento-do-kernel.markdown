---
layout: post
title:  "Criando um ambiente para desenvolvimento do kernel"
date:   2024-04-22 20:20:15 -0300
categories: IME-USP MAC0470-Software_Livre
---

Para o desenvolvimento de qualquer kernel, é importante criar um ambiente para evitar possíveis defeitos permanentes no seu computador. Desse modo, tive que criar um ambiente estável para realizar projetos da disciplina de Desenvolvimento de Software (MAC0470) que envolvem o desenvolvimento do kernel linux.

# Tutorias do FLUSP

Utilizei como base os tutoriais elaborados pelo Rodrigo Siqueira no site do Flusp para [criar a máquiva virtual][qemu-kernel], [compilar e instalar o kernel linux][compilar-kernel] e [entender e instalar módulos][entender-modulos].

Completar os tutoriais acabou sendo muito mais demorado e frustante do que o esperado, mas mesmo assim resolver os problemas enfrentados me tornou mais experiente e me deu mais conhecimento sobre como o kernel do linux funciona.

# Criando a máquina virtual

Esse primeiro tutorial foi o mais longo dos três, e o que deu mais problemas.  Primeiramente foi necessário criar uma imagem e bootar a VM, usando o QEMU.
Inicialmente, criei uma imagem de 15G e formato qcow2 e tentei instalar Debian Linux, porém a instalação e boot do Debian demorou mais do que eu esperava e não consegui realizar em um tempo razoável. Desse modo, desisti de instalar o debian e baixei a ISO do linux Ubuntu 22.04, conseguindo instalar e bootar de maneira mais rápida e prática do que o Debian.

Em seguida criei um script que começava a VM usando virt-install, e depois foi necessário configurar acesso SSH a VM, gerando as chaves SSH e pegando o IP da VM, o que habilitava um daemon que cuidava de tudo no background.

# Compilando e instalando o kernel

Esse tutorial era sobre montar o kernel do Linux dentro da VM após a instalação da mesma. Primeiramente foi necessário clonar o kernel do linux e depois montá-lo, organizando todo o sistema de arquivos. Tive vários problemas com os comandos guest mount e unmount o que foi bem custoso em tempo e ao mesmo tempo frustrante. Em seguida também instalei os módulos do kernel, e depois modifiquei o script que começava a VM para apontar para a nova imagem do kernel.

#  Entender e instalar módulos

Essa parte do tutorial foi bem rápida e simples, tratava-se apenas de criar dois módulos simples para entender o formato dos módulos nos linuxs, e quais arquivos precisam ser modificados para que os módulos sejam realmente adicionados.

[qemu-kernel]: https://flusp.ime.usp.br/kernel/qemu-libvirt-setup/
[compilar-kernel]: https://flusp.ime.usp.br/kernel/build-linux-for-arm/
[entender-modulos]: https://flusp.ime.usp.br/kernel/modules-intro/