---
layout: post
title:  "Segunda contribuição para o kw"
date:   2024-06-22 18:34:00 -0300
categories: IME-USP MAC0470-Software_Livre
---

Este artigo é uma exploração da minha segunda experiência contribuindo ao kw ou kworkflow, sendo isso o terceiro projeto na disciplina de Desenvolvimento de Software Livre (MAC0470), realizado em conjunto com Lucas Antonio.

# Objetivos do projeto

Nessa segunda fase da disciplina, os alunos poderiam decidir fazer uma outra modificação no kernel do linux, ou continuar com o kw. Decidimos continuar com o kw, pois era mais simples já que tinhamos um dos mantenedores como monitor. Essa segunda contribuição foi a primeira de duas contribuições que decidimos fazer.

#Como vou contribuir?

Escolhemos resolver o [issue 1014][link-issue] sobre a falta de mensagens de notificação quando criando imagens, quando fazendo testes de integração, já que essas poderiam ocupar bastante espaço sem o usuário saber.

#Modificação do código

Primeiramente procuramos como o obter o id e tamanho da imagem criada pelo podman, e depois adicionamos as mensagens de warning sobre a distribuição do container sendo criado, o ID da imagem, o caminho para a imagem e o tamanho da imagem. Também adicionamos um código que convertia o tamanho da imagem em bytes para a unidade apropriada, que nesse caso era GB. Além disso adicionamos outro warning informando o usuário do comando para dar clear na cache.

Na criação das imagens adicionamos as seguintes linhas:

    size_unit=$(podman image inspect --format '{{ .Size }}' "kw-${distro}")
    id=$(podman image inspect --format '{{ .Id }}' "kw-${distro}")

    # Converting size in bytes to the appropriate units
    if [[ "$size_unit" -lt 1024 ]]; then
      size="${size_unit} B"
    elif [[ "$size_unit" -lt "$(bc <<< 1024^2)" ]]; then
      size_unit=$(printf "scale=2; %s / %s\n" "${size_unit}" "1024" | bc)
      size="${size_unit} KB"
    elif [[ "$size_unit" -lt "$(bc <<< 1024^3)" ]]; then
      mb_size=$(bc <<< 1024^2)
      size_unit=$(printf "scale=2; %s / %s\n" "${size_unit}" "$mb_size" | bc)
      size="${size_unit} MB"
    else
      gb_size=$(bc <<< 1024^3)
      size_unit=$(printf "scale=2; %s / %s\n" "${size_unit}" "$gb_size" | bc)
      size="${size_unit} GB"
    fi

    warning "- Cached container distro: ${distro}"
    warning "- Image ID: ${id}"
    warning "- Containerfile path: ${file}"
    warning "- Size: ${size}"

Após a criação de todas as imagens adicionamos as seguintes linhas:

    warning ''
    warning 'To clear all cache, run the command:'
    warning '- ./run_tests.sh --integration clear-cache'

# Finalização da contribuição

Em seguida, demos commit nas mudanças, e abrimos o [PR 1132][link-PR]. Tivemos que fazer várias mudanças desde a versão inicial, como melhorar o coding style, mudar a maneira que as mensagens estavam sendo imprimidas para ficarem mais legíveis, mudar de printf para warning e outras coisas. No momento o PR ainda está aguardando ser aceito.

[link-issue]: https://github.com/kworkflow/kworkflow/issues/1014
[link-PR]: https://github.com/kworkflow/kworkflow/pull/1132
