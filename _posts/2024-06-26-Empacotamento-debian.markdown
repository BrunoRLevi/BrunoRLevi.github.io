---
layout: post
title:  "Primeira experiência com pacotes no debian"
date:   2024-06-26 23:06:00 -0300
categories: IME-USP MAC0470-Software_Livre
---

# Contextualização

O debian é um distribuição de software livre do linux, com mais de 6 mil colaboradores, e especialmente conhecido pelo seu sistema de gestão de pacotes.

Como parte da disciplinas de Software livre, segui o tutorial 1 do Joenio Marques da Costa, um engenheiro de software que também apoia software livre e desenvolver para o debian.
https://joenio.me/tutorial-pacote-debian-parte1/

# Tutorial 1
Inicialmente usei o podman para usar um container para o desenvolvimento, mas isso gerou problemas no fim do tutorial e eventualmente decidi refazer do começando usando o docker para gerar o container. 

O tutorial em si foi bem simples, primeiramente instalei as dependências necessárias, depois configurei o git, o dh-maker-perl, que cria os arquivos necessários para a criação de pacotes, e o cpan, que é um repositório com diversos modulos e bibliotecas diferentes escritos em Perl. Depois criei a versão inicial do pacote, que consistia da bibilioteca Acme::HelloWorld, usando o comando dh-make-perl. Em seguida foi necessário mudar as informações de algunas arquivos gerados, como colocar o ano da licença certo, mudar a versão do sendo utilizada, e remover alguns avisos desnecessários. Após isso só restou dar commit nas mudanças, verificar se tinha algum problema com o cme check e realmente construir o pacote. No geral foi algo bem simples, e o pacote era apenas um HelloWorld, mas já deu para ter uma noção do que é necessário para criar pacotes no debian.

# Tutorial 2
