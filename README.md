# Bioinfo-K-Mers

Obs: esse código foi criado a partir do curso "Biology Meets Programation: Bioinformatic for Beginners", da Universidade da California. Todas as explicações aqui contidas são
de propriedade dos criadores originais.

  Operando sob o pressuposto de que o DNA é uma linguagem própria, tomemos emprestado o método de Legrand e vejamos se conseguimos encontrar alguma “palavra” surpreendentemente frequente na origem do Vibrio cholerae. Adicionamos motivos para procurar palavras frequentes no ori porque, para vários processos biológicos, certas cadeias de nucleotídeos aparecem com surpreendente frequência em pequenas regiões do genoma. Muitas vezes, isso ocorre porque certas proteínas só podem se ligar ao DNA se uma sequência específica de nucleotídeos estiver presente e, se houver mais ocorrências da sequência, é mais provável que a ligação ocorra com sucesso. (Também é menos provável que uma mutação interrompa o processo de ligação.)
  
  Por exemplo, "ACTAT" é uma substring surpreendentemente frequente de:
  
  **"ACAACTATGCATACTATCGGGAACTATCCT"**

  Usamos o termo k-mer para uma string de comprimento k e definimos PatternCount(Pattern, Text) como o número de vezes que um padrão k-mer aparece como uma substring de Text. Seguindo o exemplo acima,

  **PatternCount("ACTAT", "ACAACTATGCATACTATCGGGAACTATCCT") = 3.**

  Observe que PatternCount ("ATA", "CGATATATCCATAG") é igual a 3 (não 2), pois devemos levar em conta ocorrências sobrepostas de Padrão em Texto.
  Antes de procurar palavras frequentes, gostaríamos de calcular PatternCount(Pattern, Text). Como este é o seu primeiro algoritmo biológico, iremos orientá-lo nos detalhes. Para fazer isso, primeiro criamos uma variável inteira count que definimos como zero:

    count = 0

  Conforme ilustrado na figura abaixo, nosso plano é “deslizar uma janela” para baixo do Texto, verificando se cada substring k-mer do Texto corresponde ao Padrão. Se isso acontecer, então adicionamos 1 à contagem (adicionar 1 a uma variável é chamado de incrementá-la). O valor da contagem depois de deslizarmos a janela até o final do Texto será igual a PatternCount(Pattern, Text). A questão, então, é como converter a ideia da figura em um programa funcional. Fazer isso exigirá um pouco mais de conhecimento de Python.

  ![image](https://github.com/TahVicentini/Bioinfo---K-mer-Count/assets/103975934/bee3514c-ff90-411d-aff5-f39da9bbe5a2)

  Antes de pensar em deslizar a janela para baixo do Texto, vamos resolver o problema mais simples de determinar se o Padrão corresponde a um k-mer do Texto em uma janela fixa. Em Python, o k-mer começando na posição i de Text é denotado Text[i:i+k]. Por exemplo, se Text = "GACCATACTG", então Text[4:7] = "ATA". Python usa indexação baseada em 0, na qual o primeiro símbolo da string ocorre na posição 0 em vez de 1; como resultado, o Texto termina na posição len(Texto)-1, onde len(Texto) é o número de símbolos no Text.

Agora podemos usar uma instrução if, mostrada abaixo, para determinar se Pattern corresponde a Text[i:i+k]; se isso acontecer, então incrementamos a contagem.

    if Text[i:i+len(Pattern)] == Pattern:
      count = count+1

No código Python acima, certifique-se de observar a diferença entre o símbolo de igual (=), no qual atribuímos um valor a uma variável, e o símbolo de igual duplo (==), no qual testamos a igualdade de duas variáveis.

Em Python, o bloco recuado:

    for i in range(n):
    
itera sobre todos os valores de i entre 0 e n-1, inclusive.
Por exemplo, poderíamos usar o código a seguir para imprimir todos os números pares entre 0 e 100, inclusive.

    for number in range(51):
       print(2*number)
 
Observe novamente que usamos range(51) e não range(50) porque range(n) vai de 0 a n-1.

Em geral, o k-mer final de uma corda de comprimento n começa na posição nk; por exemplo, o 3-mer final de "GACCATACTG", que tem comprimento 10, começa na posição 10 - 3 = 7. Esta observação implica que a janela deve deslizar entre a posição 0 e a posição len(Text)-len(Pattern).

Assim, para deslizar nossa janela da posição 0 para len(Text)-len(Pattern), precisaremos de um loop for no formato:

    for i in range(len(Text)-len(Pattern)+1):

Agora estamos prontos para usar este loop for junto com nossa instrução if anterior para expandir nosso código para PatternCount.

     count = 0
         for i in range(len(Text)-len(Pattern)+1):
            if Text[i:i+len(Pattern)] == Pattern:
                count = count+1 

Para juntar tudo, colocaremos todo esse código em uma função, chamada PatternCount (veja abaixo). Veremos em breve que esta função nos ajudará a voltar a encontrar palavras frequentes na origem da replicação.

Resumidamente, uma função recebe uma entrada e retorna uma saída. Colocar código em uma função é econômico porque podemos chamar essa função posteriormente pelo nome sempre que quisermos encontrar o número de vezes que uma string ocorre como uma substring dentro de outra, em vez de ter que repetir todas as linhas de código contidas na função . A palavra-chave **def** no início de uma função sinaliza que estamos “definindo” uma função.

    def PatternCount(Text, Pattern):
        count = 0
        for i in range(len(Text)-len(Pattern)+1):
            if Text[i:i+len(Pattern)] == Pattern:
                count = count+1
        return count 


