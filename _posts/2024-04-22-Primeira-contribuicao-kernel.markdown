---
layout: post
title:  "Primeira contribuição no kernel linux"
date:   2024-04-22 20:20:15 -0300
categories: IME-USP MAC0470-Software_Livre
---
Este artigo é uma exploração da minha primeira experiência contribuindo ao kernel linux, mais especificamente no subsistema IIO, necessária para um projeto na disciplina de Desenvolvimento de Software Livre (MAC0470), realizado em conjunto com Lucas Antonio.

# Objetivos do projeto
O objetivo do projeto é enviar um patch da contribuição via e-mail dos monitores da discplina para avaliação dos mesmos.

Os meus objetivos pessoais com tal contribuição e projeto é aprender e entender o processo de contribuir para um código aberto, como o do kernel linux, já que nunca tive uma experiência parecida, entendendo a realização de patches e a revisão do repositório.

# Como vou contribuir?

Os monitores da disciplinas disponibilizaram quatro sugestões para contribuições no kernel, explicando o que poderia ser modificado e como poderia se alterado.

Analizei, junto a minha dupla, as sugestões e decidimos escolher arrumar alguns warnings que estavam aparecendo em algumas partes do IIO, já que não é algo complicado, possibilitando o entendimento do processo de contribuição para que, no futuro, podemos enviar modificações mais avançadas com total clareza do processo de contribuição e revisão do repositório kernel linux.

# Modificação do código

Usando o comando kw c do kernelworkflow, foi possível ver os diferentes warnings gerados por checkpatch. O warning que decidimos modificar estava presente no arquivo `vkms_drv.c` dentro do diretório `drivers/gpu/drm/vkms/` (Considernado que estamos dentro do subsistema IIO)

Percebemos que um comentário sobre uma pequena confusão dentro do código não estava de acordo com as boas práticas e o compilador estava reclamando disso. Para o compilador, o comentário em bloco deve possuir os seus astericos `*` alinhados de maneira simétrica, com o início do bloco `/*` na mesma linha do início do comentário e o final do bloco `*/` uma linha abaixo do final do comentário.

Após alterar o código, o bloco do comentário ficou dentro das boas práticas e o compilador parou de reclamar sobre. Abaixo está o código da maneira correta, após a modificação.

{% highlight c %}

  /* FIXME: There's a confusion between bpp and depth between this and
   * fbdev helpers. We have to go with 0, meaning "pick the default", 
   * wich is XRGB8888 in all cases
   */

{% endhighlight %}

Realizamos a correção de erros e warning similares, também, nos arquivo `drivers/gpu/drm/amd/acp/include/acp_gfx_if.h`

# Finalização da contribuição

Agora apenas faltava realizar o patch e enviá-los por e-mail. Como gerenciamos o repositório via git, utilizamos essa ferramenta para enviar as alterações via e-mail.

Realizamos um `git commit` e, para criar um arquivo `*.patch` utilizamos o comando `git diff > *.patch`, desse modo conseguimos armazenar todas as contribuições em um arquivo só.

Portanto, apenas falta mandar e-mail para os monitores via o commando `git send-email`, porém não conseguimos enviar o comando pois, devido à autenticação de dois fatores, o gmail não estava aceitando tal maneira de mandar o e-mail.

Assim, com ajuda dos própios monitores, enviamos o patch via email manualmente para os para revisão. Essa primeira contribuição foi menor do que o esperado, devido a problemas de planejamento e problemas realizando os tutoriais anteriores da instalação da VM, mas no geral ainda foi algo instrutivo, e espero que em futuras experiências possa contribuir de forma mais significativa para o kernel do linux, ou mesmo para a ferramente do kernelworkflow.