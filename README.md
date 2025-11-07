# Resumo de criptografia clássica (algoritmos simétricos)

# Introdução a Criptografia | Criptografia Clássica

A criptografia é um modo de desenvolver e usar algoritmos que codificam informações para proetege-las de terceiros, de modo que apenas aqueles com a permissão deva ter a habilidade de descriptografar e ler a informação compartilhada. 

Derivada da palavra grega *"kryptos"*, que significa "oculto" a criptografia é traduzida como "escrita oculta", na prática, ela é usada para transformar mensagens ilegíveis em legíveis para apenas aqueles que possuem a **chave**. 

Mas o que é essa chave? na criptografia, ela é um valor secreto usado para codificar ou decodificar informações, garantindo desta forma de que apenas quem possui acesso a essa chave possa ler ou escrever informações criptografadas, mantendo um canal de comunicação. Essa chave poder ser **simétrica**, quando é a mesma chave para as duas pessoas do canal de comunicação, ou **assimétrica** que possui pares de chaves pública e privada.

## Criptografia simétrica clássica 

Na criptografia simétrica, tanto o remetente da mensagem quanto o receptor compartilham da mesma chave para codificar e desencodificar uma mensagem e manter o canal de comunicação seguro de terceiros. 

Um exemplo clássico da criptografia simétrica é a **Cifra de César** que basicamente pega uma sentença e reorganiza as letras baseado em um deslocamento das letras do alfabeto. Por exemplo, se eu quero criptografar a palavra GATO, eu sigo um algoritmo simples: 

- 1° Passo: escolher a chave (valor do deslocamento).
- 2° Passo: identificar a posição de cada letra no alfabeto.
- 3° Passo: substituir pela nova letra 

> É importante saber que se a chave ultrapassar "Z", reinicia o alfabeto 

Assim, se eu escolher minha chave como 3 e eu quero criptografar a palavra GATO, eu teria o resultado de: 

- G -> J
- A -> D
- T -> W
- O -> R 

Portando, GATO seria **JDWR**. 

Como é um algoritmo trivial, para melhor aprendizado desenvolvemos um algoritmo em python que demonstra a Cifra de César. 

Perceba que os dois algoritmos são praticamente iguais, a mudança é apenas que na descriptografia a gente subtrai o index do alfabeto, para achar o valor correto. 

Embora hoje não se usa o algoritmo da Cifra de César em projetos industriais e reais, ele demonstra perfeitamente a a criptografia simétrica, por que tanto para descriptografar quanto para criptografar uma palavra, é necessário que os dois saibam o valor exato da chave, se você quiser realizar testes nesse algoritmo você verá que se o valor da chave for diferente para cada lado (criptografando e descriptografando), a informação muda. 

Algumas das principais virtudes em utilizar um algoritmo simétrico para sua criptografia é:

- Velocidade 
- Eficiência 
- Confidencial

## O problema da criptografia simétrica 

A principal vantagem da criptografia simétrica sob assimétrica é a simplicidade em desenvolver e resolver, o que a torna mais veloz também. No entanto, a criptografia simétrica é considerada menos segura, pois depende da troca de chaves de modo super seguro. Qualquer pessoa que obtenha a chave simétrica, pode acessar os dados, sem que as partes principais do canal de comunicação saiba. 

Considere um sistema com inicialmente 2 usuários, Alice e Bob, que desejam se comunicar usando criptografia simétrica, mas um espião consegue acesso a chave de criptografia que Alice e Bob definiram, após a criptografia da mensagem de Alice, o espião consegue captar a mensagem criptografada e descriptografar (pois tem a chave) sem que Alice nem Bob saibam que há um espião

O problema pode ser interpretado pela seguinte imagem: 

![Comunicação Alice e Bob com um espião](./imgs/workflowcriptsimetrica.png)
Fonte: Os autores

Além do mais, avanços tecnologicos em Inteligência Artificial e Computação Quântica ameaçam cada vez mais esses sistemas simétricos, com isso, partimos para nosso próximo tópico. 

## Protocolo BB84 para distribuir chaves de um jeito seguro
 
O protocolo BB84, criado por Charles Bennett e Gilles Brassard em 1984 (daí o seu nome), utiliza propriedades da mecânica quântica para fazer uma criptografia completamente segura. 

É importante entender que o protocolo BB84 não é de fato um algoritmo de criptografia quântica, e sim, um protocolo para troca segura de chaves. Esse protocolo permite que duas partes (Alice e Bob) gerem uma chave em comum sem a necessidade de um canal secreto estabelecido. É padrão, após usar esse protocolo quântico, usar algum algoritmo clássico para troca de mensagens. Desenvolveremos o protocolo em condições ideais, em canais sem ruídos.

É bem evidente que qualquer informação clássica pode ser copiada, porém, na mecânica quântica é **impossível** copiar uma informação **quântica**, segundo o *no-cloning theorem*, em sistemas quânticos, não é possível nem mesmo obter informação de um estado quântico genérico sem que interfira em todo o sistema. 

Podemos citar ainda, o o Princípio da Incerteza de Heisenberg, um dos pilares da mecânica quântica, que nos diz que é impossível conhecer com precisão a posição e o momento(velocidade) de uma partícula ao mesmo tempo, essa incerteza é aproveitada na criptografia quântica. 

Para exemplificar melhor, o protocolo pode ser dividido em algumas etapas de desenvolvimento:

### O envio da mensagem 

Na primeira etapa, Alice envia uma sequência de bits para Bob, esses bits são enviados através de fótons, que podem estar polarizados de duas formas: 
- Retilínea (+): os fótons estão polarizados em 0 ou 90°.
- Diagonal (x): os fótons são polarizados em 45° ou 135°.

Na computação, associamos valores lógicos (0 ou 1) para esses fótons, por exemplo, zero para fótons polarizados em 0 ou 45° e um para fótons polarizados em 90° ou 135°. 

Para medir o fóton polarizados que contém a mensagem de Alice, Bob escolhe uma base aleatória para medir e tentar descobrir quais bits ela enviou, porém, é necessário que a base em que Bob irá medir seja a mesma que Alice enviou o bit, se não, a informação que Bob irá ter será diferente da que Alice queria transmitir. 

Assim, se ele escolher a base certa, ele obtém o mesmo bit que Alice enviou, se não, o resultado é aleatório. Mesmo em um sistema sem espionagem, a transmissão da mensagem contém um erro de 25%, já que, 50% das vezes Bob irá escolher a chave errada, de forma que as outras 50% das vezes ele escolhe a correta, logo: 

> 50% (bases erradas) × 50% (chance de erro) = 25% de bits incorretos.

Portanto, a discrepância esperada entre as chaves(chamadas de *raw key* ou chave bruta, pois a sequência de bits que a Alice enviou e o Bob mediu, não são iguais nas duas pontas pois Bob nem sempre escolheu a base certa) de Alice e Bob é de 25%, mesmo sem interferência.

Para Bob entender exatamente o que Alice quer dizer, chegamos na segunda etapa do protocolo: 

### Reconciliação de Bases 

Essa etapa é uma comunicação pública entre as duas partes, Bob envia as bases que ele escolheu, sem que revele o resultado da medição, a partir daí, Alice informa qual polarizador ela escolheu, sem revelar também o qubit que ela enviou. Dessa forma, os dois mantêm os bits com as bases iguais e formam uma chave com eles, chamadas de *Shifted key* ou "Chave Filtrada".

Esta etapa é essencial pois reduz a chave bruta inicial pela metade, mesmo que 75% da chave estar correta, somente 50% representa a informação real de Alice, já que o restante é os erros das bases aleatórias que Bob escolheu. 

Em seguida, é necessário assegurar se existe um terceiro(espião) interceptando a comunicação, iniciaremos a terceira etapa: 

### Eve, a espiã 

Aplica-se algoritmos clássicos para captar se há a presença de Eve, uma espiã, e para corrigir os erros. 

Quando Alice e Bob divulgam um subconjunto aleatório da chave e comparam, eles verificam a taxa de erro, que é chamada de QBER (*Quantum bit error rate*), se Eve tentar capturar essa informação, ela muda, segundo a Teoria da Medida. Se haver essa mudança na informação, Alice e Bob identificam que há um espião e recomeçam o protocolo, se não, eles descartam os bits utilizados na verificação e continuam com o protocolo. 

É importante salientar novamente que exemplificamos o protocolo em condições ideais, em situações reais seria possível ter um canal com ruído com equipamentos com defeitos, fazendo com que a informação que Bob recebesse fosse um pouco diferente do qubit entendido por Alice, assim seria necessário uma quarta etapa com algum algoritmo de correção de erros. 

Como citado, quando Alice e Bob expôem a chave no canal de comunicação, ela é reduzida afim de obter a informação real, fica claro que a chave que Alice deve transmitir tem que ser maior do que a chave desejada. 

### A chave 

Para melhores fins didáticos, a tabela a seguir demonstra um exemplo de distribuição de chaves: 

| Bits enviados  | 1 | 0 | 0 | 1 | 0 | 1 | 1 | 1 | 0 | 0 | 1 | 0 | 1 |
|----------------|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Base Alice     | + | + | x | x | + | x | + | x | x | x | + | x | + |
| Base Bob       | x | + | x | x | x | + | + | x | + | + | x | + | x |
| Chave          | 0 | 0 | 1 |   | 1 | 1 | 0 |   | 1 |   |   |   | 1 |

A primeira linha é a chave de bits que Alice queria comunicar a Bob, a segunda, a base de polarização que ela utilizou, na terceira linha estão as bases aleatórias que Beto escolheu e mediu. 

Após Alice enviar os bits em fótons polarizados e Bob medir com as bases aleatórias. Bob expõe em um canal público as bases usadas na medição e Alice confirmar em quais casos ela utilizou a mesma base para polarizar o fóton, eles montam uma chave somente com as medições corretas, que no exemplo da tabela será 00111011, nota-se que alguns bits foram perdidos nesse processo de achar bits iguais, naturalmente a quantidade de fótons enviados seria bem maior do que no exemplo. 

Se Eve tentar pegar os fótons de Alice e medi-los, isso pertubaria o sistem em um todo, que enviaria qubits danificados a Bob, é importante lembrar que graças a princípios da mecânica quântica não é possível que Eve faça cópia do fóton antes de medi-lo, Eve não consegue também advinhar ou descobrir o polarizador de Alice pois isso é totalmente aleatório, e mesmo que Eve faça uma medição com base aleatória, ela poderá escolher a errada e não obterá a informação correta.

No final do protocolo, quando Bob e Alice comparam sua chave filtrada, eles poderão perceber que alguns qubits estão diferentes, mesmo com Bob medindo na mesma base que Alice, isso demonstra que Eve interceptou o sistema e a chave está insegura, eles então repetem o protocolo até encontrar uma chave segura. 

Um ponto importante é que todo sistema de distribuição de chaves precisa de autentificação, senão, Eve poderia mentir para Bob se passando por Alice ou vice-versa. O protocolo quântico BB84 poderia ter uma pequena sequência de 0 e 1, em que sirva como um "RG" para Alice e Bob terem certeza de que são eles. Então antes de ocorrer o protocolo BB84 em si, é necessário que tenha a troca de autentificação entre as duas partes. 

Concluindo, a principal vantagem de utilizar o protocolo BB84 é que é perceptível a presença de terceiros antes que a mensagem valiosa seja trocada, por esse motivo o protocolo serve apenas para gerar chaves e não trocar mensagens em si. 

