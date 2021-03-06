# MC723: Exercício 3
##Nome: Caio Vinícius Piologo Véras Fernandes
##RA: 145574

##Contando instruções
Inicialmente foi criado um arquivo chamado hello.c no qual foi incluída apenas um comando para imprimir a linha "Hello World!". Dessa forma, foi modificado o arquivo mips_isa.cpp para contar o número de instruções add no arquivo executado.
Obteve a seguinte saída pro hello.c:

ArchC: -------------------- Simulation Finished --------------------

DBG: Add operations = 0

DBG: @@@ end behavior @@@

Em seguida foi incluída uma soma no arquivo hello.c. para criar duas variáveis novas em que uma terá o valor da nova mais um.
Assim o simulador teve a seguinte saída:

ArchC: -------------------- Simulation Finished --------------------

DBG: Add operations = 0

DBG: @@@ end behavior @@@

Isso ocorreu pois a função de soma chamada não foi a ac_bahavior(add), mas sim as suas outras formas (ac_behavior(addu), ac_behavior(addi), etc). Caso incluída uma soma em todas essas chamadas de soma, a simulação terá a seguinte saída:

Assim o simulador teve a seguinte saída:

ArchC: -------------------- Simulation Finished --------------------

DBG: Add operations = 0x286

DBG: @@@ end behavior @@@

Mostrando que as funções de soma foram chamadas durante a execução do programa.

Aprofundando-se um pouco mais na teoria, podemos descobrir que as operações aritméticas de soma não costumam usar a instrução add(soma com sinal), mas sim a addu (soma sem sinais). Isso acontece pois caso ocorra um overflow na operação com sinal, um erro será gerado e a política em C é a que o programador deva cuidar do erro, caso esse erro não seja tratado será então utilizada a soma sem sinal.

##Avaliando Desempenho
Em seguida serão executados alguns benchmarks procurando avaliar o CPI médio de cada um através do número de instruções. Para isso o acsim será executado com uma flag -s que mostra o número de instruções de cada etapa do código (acsim mips.ac -abi -s).

Para o cálculo do CPI (ciclos por instrução) médio do benchmark a tabela seguinte foi utilizada:

| Categoria                  | CPI médio     |
| -------------              |:-------------:|
| Acesso à memória           | 10            |
| Controle (branch/jump)     | 3             |
| Outras                     | 1             |

######jpeg coder
O benchmark acima foi executado com o seguinte comando

../../../../mips.x --load=jpeg-6a/cjpeg -dct int -progressive -opt -outfile output_small_encode.jpeg input_small.ppm 

para obter os dados referentes à execução do runme_small.sh

Disso, foi retirado as seguintes informações:
- INSTRUCTIONS : 29857500
- CPI : 125750804

###### fft

Da mesma forma, o benchmark acima foi executado com o comando

../../../../mips.x --load=fft 4 4096 > output_small.txt

Retirando os seguintes dados:
- INSTRUCTIONS : 539993292
- CPI : 1878273199

###### gsm coder

Por fim, foi executado o último benchmark com o seguinte comando:

../../../../mips.x --load=bin/toast -fps -c data/large.au > output_large.encode.gsm

Obtendo os seguintes dados:
- INSTRUCTIONS : 1484477202
- CPI : 565405804
