---
layout: post
title: "[PT] Encriptação no TEDxULisboa"
date: 2020-08-31
description: Descrição da implementação de encriptação ponto-a-ponto no TEDxULisboa
comments: true
---

Nas aplicações de mensagens da atualidade, **encriptação** é algo que muitas vezes tomamos como garantido e nem pensamos nisso.
<br>

#### **Mas, como é que as nossas mensagens são realmente armazenadas?**

Na aplicação oficial do TEDxULisboa 2020 **as mensagens são guardadas de forma segura numa base de dados usando encriptação**. Desta forma, qualquer informação pessoal e/ou sensível contida numa mensagem só é acessível pelo seu remetente e destinatário.
<br>

Comecemos por pensar na aplicação **Whatsapp**. Esta aplicação de mensagens utiliza encriptação de **ponto-a-ponto**. Este tipo de encriptação é o equivalente a ter 2 copos unidos por 1 fio, uma pessoa fala pelo copo e só a outra pessoa, na outra ponta do fio, consegue ouvir a mensagem.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/posts/2020-08-31-encriptação/whatsapp.png">
    </div>
</div>
<div class="caption">
    Imagem de <a href="https://hedgetrade.com/end-to-end-encryption-ban-attempts/" target="_blank" rel="noopener noreferrer">Edgetrade.com</a>.
</div>

Encriptação de ponto-a-ponto é um exemplo, mas o que é realmente encriptação? Não sou nenhum profissional nisto, mas tentarei explicar este conceito o melhor possível.

##### *Em criptografia, encriptação, ou cifragem, é o processo de transformar informação (purotexto) usando um algoritmo (chamado cifra) de modo a impossibilitar a sua leitura a todos exceto aqueles que possuam uma identificação particular, geralmente referida como chave. — Encriptação, Wikipédia*


Este processo envolve **chaves de encriptação**, que são, de forma simplificada, **palavras-passe**. Imaginemos que eu tenho uma mensagem, representada por uma carta. Quando se cifra (encripta) a mensagem, é como se a referida carta fosse colocada dentro dum cofre, acessível apenas com a chave certa. Na realidade, cifrar uma mensagem é a transformação de uma sequência de caracteres, através de um processo reversível, para uma outra sequência de caracteres. A chave de encriptação neste processo representa “instruções” para reverter esta transformação.

O último conceito que é importante dominar antes de explicar como está implementada a encriptação na aplicação do TEDxULisboa **é a diferença entre chaves simétricas e chaves assimétricas**.

Uma **chave simétrica** é uma sequência de caracteres que serve como “instruções” para cifrar e decifrar (desencriptar) uma sequência de caracteres (mensagem). Seria semelhante então ao cofre que falei anteriormente em que uma chave serve para abrir e fechar o cofre, cifrar e decifrar uma mensagem.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <center>
            <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/posts/2020-08-31-encriptação/keys-sym.png">
        </center>
    </div>
</div>
<div class="caption">
    Imagem de <a href="https://www.101computing.net/symmetric-vs-asymmetric-encryption/" target="_blank" rel="noopener noreferrer">101Computing.net</a>.
</div>

Uma **chave assimétrica** é diferente. É constituída por um par único de chaves: uma chave pública e outra privada. A chave pública, como o nome diz, pode ser acessível por todos e serve para cifrar uma mensagem. Porém, depois de cifrada, não é possível decifrar usando a chave pública. Para decifrar é necessário utilizar a chave privada que, como o nome diz, só o recetor da mensagem deverá possuir esta chave. Gosto de comparar este tipo de chave a uma caixa de correio, em que qualquer pessoa pode colocar cartas lá dentro, mas só o dono da mesma tem acesso ao seu conteúdo.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <center>
            <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/posts/2020-08-31-encriptação/keys-asym.png">
        </center>
    </div>
</div>
<div class="caption">
    Imagem de <a href="https://www.101computing.net/symmetric-vs-asymmetric-encryption/" target="_blank" rel="noopener noreferrer">101Computing.net</a>
</div>


<center>
<h1>
. . .
</h1>
</center>

### **Estamos agora prontos para compreender o sistema de encriptação do TEDxULisboa! 🥳**

Cada utilizador da aplicação possui uma chave assimétrica, ou seja, uma chave pública e uma chave privada. Este par de chaves é gerado aleatoriamente no telemóvel de cada pessoa durante o processo de registo e guardado na memória do mesmo. Porém, esta chave tem de ser guardada num lugar mais acessível, pois será necessário recuperar as chaves cada vez que se inicia sessão. Estas chaves são então guardadas na base de dados. A chave pública pode ser guardada sem qualquer tipo de protecção, pois, como o nome diz, é pública. A chave privada requer algum cuidado extra ao ser guardada, pois é pessoal e intransmissível. Portanto, cada chave privada é cifrada usando uma chave simétrica e depois guardada na base de dados.

O truque é gerar esta chave simétrica de forma pessoal e reproduzível para cada utilizador. Sendo assim, **a chave privada de cada utilizador é cifrada utilizando uma chave simétrica gerada a partir da palavra-passe de cada um e depois guardada na base de dados**. Desta forma, a chave simétrica que cifra a chave privada é pessoal, pois apenas cada utilizador tem conhecimento da sua palavra-passe.
<br>

*Eu sei, é muita informação duma vez, mas é necessário isto tudo para manter a segurança deste sistema de encriptação. 🤓*
<br>

#### **Agora que cada utilizador tem a sua chave pública, acessível por todos, e chave privada, acessível apenas por ele mesmo, como se trata da encriptação das mensagens?**

Uma opção seria: quando o utilizador A quer mandar uma mensagem ao utilizador B, cifra a mensagem com a chave pública do utilizador B e, desta forma, só este terá acesso à mesma. Isto seria equivalente ao utilizador A escrever uma carta e colocá-la na caixa de correio do utilizador B, à qual só este tem acesso. O problema deste método é que o utilizador A perde acesso às mensagens que enviou, portanto, este método não se adequa.
<br>
<br>

### **Como funciona então a encriptação da aplicação do TEDxULisboa?**

O utilizador A quer enviar uma mensagem ao utilizador B, então escreve uma carta e arranja um cofre. Coloca a carta no cofre e faz 2 cópias da chave do mesmo. Uma cópia fica para si mesmo (utilizador A) e outra para o utilizador B. De forma a guardar estas chaves do cofre, o utilizador A coloca a sua chave na sua caixa de correio e a chave do utilizador B na caixa de correio do mesmo. Na realidade, o que acontece é: quando o utilizador A escreve a mensagem, gera uma chave simétrica aleatória. Esta chave simétrica será utilizada para cifrar todas as mensagens da conversa entre os utilizadores A e B. A chave simétrica gerada é cifrada e armazenada, individualmente, utilizando a chave pública do utilizador A e a chave pública do utilizador B, assim, só estes terão acesso à chave simétrica.

**Este método funciona bastante bem, pois cria uma espécie de sala segura por cada conversa à qual só os participantes da mesma têm acesso**. Se quiserem adicionar o utilizador C à conversa, basta criar uma nova cópia da chave simétrica e cifrar usando a chave pública do utilizador C.

**Porém, existe um defeito neste método:** se um utilizador perder acesso à sua chave privada, por exemplo, terminando sessão ou trocando de dispositivo, e não se lembrar da sua palavra-passe, perderá a capacidade de decifrar todas as mensagens das suas conversas. Porquê? Porque sem se lembrar da sua palavra-passe, não é possível decifrar a sua chave privada na base de dados. Sem a sua chave privada, não é possível decifrar as chaves simétricas associadas às conversas das quais faz parte. E sem estas chaves, não será possível decifrar o conteúdo de cada conversa.

Este defeito é uma **consequência do nível de segurança deste método**, pois significa que as mensagens guardadas na base de dados só são acessíveis pelas pessoas certas, nem um administrador consegue ter acesso às mesmas, pois as mensagens encontram-se encriptadas na base de dados e só são decifráveis contendo acesso às chaves correspondentes.

**É assim que o TEDxULisboa mantém as mensagens seguras na base de dados**. Aconselho-te a ver um vídeo que fiz onde explico esta metodologia duma forma mais ilustrativa e que poderá complementar esta explicação:

<div>
  <div style="position:relative;padding-top:56.25%;">
    <iframe style="position:absolute;top:0;left:0;width:100%;height:100%;" src="https://www.youtube.com/embed/d_0KLxpG62E" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
</div>
<br>
