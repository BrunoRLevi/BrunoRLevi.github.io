---
layout: post
title:  "Primeira contribuição para o kw"
date:   2024-06-22 18:15:00 -0300
categories: IME-USP MAC0470-Software_Livre
---

Este artigo é uma exploração da minha experiência contribuindo ao kw ou kworkflow, sendo isso o segundo projeto na disciplina de Desenvolvimento de Software Livre (MAC0470), realizado em conjunto com Lucas Antonio.

# Objetivos do projeto

O objetivo do projeto é enviar um patch da contribuição via e-mail dos monitores da discplina para avaliação dos mesmos.
O que é o kworkflow

O kworkflow é uma coleção de scripts usados para auxiliar o desenvolvimento no kernel Linux, criado por estudantes do FLUSP.

#Como vou contribuir?

Os monitores da disciplinas disponibilizaram divesas sugestões para contribuições no kernel, explicando o que poderia ser modificado e como poderia se alterado. Depois de considerar algumas diferentes possíveis contribuições, como consertar o comando kw build –info, que não imprimia o nome customizado corretamente, ou melhorar a cobertura de testes em partes com cobertura baixa, decidimos melhorar o coding style de diversos arquivos. Inicialmente fizemos isso substituindo 10 instâncias de echo em 4 arquivos diferentes para printf, porque o printf é mais consistente em diferentes plataforma do que o echo, mas depois percebemos que apenas fizemos a modificação em arquivos teste, o que era desnecessário. Decidimos então adicionar mensagens de erro seguindo o padrão errno

#Modificação do código

Usango grep -r return, achamos diversos lugares onde não havia comentário nas linhas de retorno, então os adicionamos em vários arquivos. Um exemplo seria mudar 

  return 22

para

  return 22 # EINVAL

Fizemos essa correção mais 24 vezes em 16 arquivos diferentes, tornando o kw um pouco mais condizente com as regras de coding style.

# Finalização da contribuição

Por fim apenas demos commit nas mudanças, abrimos um PR, e depois de alguns ajustes na mensagem de commit e na descrição, o PR foi [aceito][link-pr].

[link-pr] https://github.com/kworkflow/kworkflow/pull/1112
