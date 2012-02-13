---
layout: post
title: Diferença entre git merge e git rebase
---

Olá, há alguns dias tive muitos problemas de conflitos ao usar o git, quando ia unir dois branchs. Sempre tinha um ou outro arquivo com conflito.

Então postei a [minha dúvida](http://groups.google.com/group/git-br/browse_thread/thread/d531975c7448e97d) na lista [git-br](http://groups.google.com/group/git-br) e na do [rails-br](http://groups.google.com/group/rails-br/browse_thread/thread/d9d041a1860cc880/922b2c5e56ccd24f?lnk=gst&amp;q=Conflitos+ao+fazer+merge#922b2c5e56ccd24f), então no rails-br me indicaram a usar o [rebase](http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html) ao invés do [merge](http://www.kernel.org/pub/software/scm/git/docs/git-merge.html), com o rebase, diminuiu e muito a quantidade de conflitos.

Agora, qual a diferença entre o git merge e o git rebase? Eis a resposta ;)

Ao se fazer um git rebase se faz a união de dois branchs mas mantendo o histórico de commits:

![Git Rebase](http://book.git-scm.com/images/figure/rebase4.png)

Enquanto que o merge apenas une os dois branchs e desconsidera o histórico:

![Git merge](http://book.git-scm.com/images/figure/rebase2.png)

Fazendo assim, o rebase mantem um histórico maior, assim gerando menos conflitos.

Por um lado, isso aumenta o número de commits e patchs por outro lado diminui bastante o índice de conflitos já que ele segue uma cronologia entre os branchs.

Eu prefiro manter o histórico de revisões é bem mais fácil de gerenciar.

Até a próxima!
