# Exercise 1

## Comentários 
Como o limite das bolas sorteadas é de apenas 0 a 59 não utilizei um sorter, como por exemplo o quickSort, em vez disso utilizei um array de boolean (Não poderia ser int pois os arrays são inicializados a 0 e sendo assim não iria ser possivel diferenciar a bola 0) e ao retirar uma bola a posição desse array da bola passa a true.
Suponho também que no assert_equal há um método de comparação entre Arrays, algo do género de Array.equals()


## Funções
#### public void add(Integer number)
Adiciona a bola, caso esteja entre os números 0 e 59, neste caso adicionar refere-se a passar o número no  array de Boolean a true, sempre que se adiciona uma bola é incrementado a variável size.

#### public int[] balls()
Função que vai devolver o array de inteiros, podemos criar um array com size fixo uma vez que já temos a variável size dentro da class. 
O resto consiste num ciclo for para confirmar quais os números que estão a true, seja esse o caso é adicionado ao sortedBalls que é o array a ser devolvido.

#### Código

```
public class Rack {
        Boolean[] balls;
        private int size;
        
        Rack(){
            balls = new Boolean[60];
        }
        
        public void add(Integer number){
            if(number >= 0 && number <= 59){
                balls[number] = true;
                size++;
            } else {
                System.out.println("Number not in range");
            }
        }
        
        public int[] balls(){
            int[] sortedBalls;
            sortedBalls = new int[size];
            int curIndex = 0;
            
            for(int index = 0; index < balls.length; index++){
                if(balls[index] != null){
                    sortedBalls[curIndex] = index;
                    curIndex++;
                }
            }
            return sortedBalls;
        }
}
```

# Exercise 2


## Funções
#### public String readNumber(int number)
Recebe o número que vai ser transformado para um formato escrito, nesta função faz verificações básicas para confirmar que o número está dentro dos limites e se o número é o 0 ou o tal "billion".
Depois de verificar o inteiro é transformado numa string e subsequentemente num array de caracteres para verificar o número total de números para mais facilmente verificar se este fica entre os milhões, milhares ou abaixo de um milhar, conforme o resultado passa para as funções adequadas.

#### public String convertUnderThousand(int number)
Logo no inicio é verificado se o número está entre o zero e o 20, pois se for esse o caso é preciso ir buscar o numero a string basicNumbers (esta contém os números de zero a vinte, pois são unicos).
Caso seja maior que esses números é necessário combinar nomes. Isto é feito através de restos de divisão por 10, para separar o número por etapas. Por exemplo, imaginemos que o número é 678, primeiro é encontrado o número 8, depois vem o 70 e no return final adicionamos o 600, ou seja, ir buscar o "six" mas é adicionado o "hundred" e então é colocado a frente os números encontrados anteriormente, neste caso "seventy-eight"

#### public String convertUnderMillion(String fullNumber, int index)
O fullNumber é a variável num formato string e o index é a subtração entre o total de digitos do número pelo número 3 (ex: 100 200, index = 6-3, index seria 3).
Para separar os milhares é utilizado o método substring juntamente com o index calculado anteriomente, para sabermos exatamente quantos números "cortar" da string original e passamos esse número para int, o restante é posto noutro inteiro. Estes dois números são enviados como argumento para o convertUnderThousand, mas no meio é posta a palavra "hundred" e assim é convertido para um número sob um milhão.
(ex: 123 456, é separado para 123 e 456)

#### public String convertUnderBillion(String fullNumber, int index)
Semelhante ao convertUnderMillion, neste caso também separamos em 2 o número, um é tratado então pelo convertUnderThousand e adicionado o "million" depois, e o restante número é enviado para o convertUnderMillion.


#### Código

##### Main
```
public void main(String[] args) {
    NumberReader nr = new NumberReader();

    // Exemplo de uma chamada
    System.out.println(12678 + "-" + nr.readNumber(12678));
}
```

##### Class NumberReader

```
public class NumberReader {
    int number;
    
    NumberReader(){
    }
    
    private final String[] basicNumbers = {
        "",
        "one ", 
        "two ",
        "three ",
        "four ",
        "five ",
        "six ",
        "seven ",
        "eight ",
        "nine ",
        "ten ",
        "eleven ",
        "twelve ",
        "thirteen ",
        "fourteen ",
        "fifteen ",
        "sixteen ",
        "seventeen ",
        "eighteen ",
        "nineteen ",
    };
    private final String[] tensNumbers = {
        "",
        "ten",
        "twenty",
        "thirty",
        "forty",
        "fifty",
        "sixty",
        "seventy",
        "eighty",
        "ninety",
    };
    
    public String readNumber(int number){
        int billion = 1000000000;
        if(number == 0){
            return "zero";
        } else if(number < 0){
            return "Number under 0";
        } else if(number > billion){
            return "Number is over a billion";
        }
        
        String fullNumber = String.valueOf(number);
        char[] digits = fullNumber.toCharArray();

        if(digits.length <= 3){
            return convertUnderThousand(number);
        } else if(digits.length <= 6) {
            return convertUnderMillion(fullNumber, digits.length - 3);
        } else if(digits.length<=9){
            return convertUnderBillion(fullNumber, digits.length - 6);
        } else if(number == billion){
            return "One billion";
        };
                  
        return convertUnderThousand(number);
    }

    public String convertUnderMillion(String fullNumber, int index){
        int thousand = Integer.parseInt(fullNumber.substring(0,index));
        int remainingInt = Integer.parseInt(fullNumber.substring(index, fullNumber.length()));
        
        return convertUnderThousand(thousand) + "thousand " + convertUnderThousand(remainingInt);
    }
    
    public String convertUnderBillion(String fullNumber, int index){
        int million = Integer.parseInt(fullNumber.substring(0,index));
        String remainingString = fullNumber.substring(index, fullNumber.length());
        
        return convertUnderThousand(million) + "million " + 
                convertUnderMillion(remainingString, 3);
    }
    
    public String convertUnderThousand(int number){
        String basic;

        if (number % 100 < 20){
          basic = basicNumbers[number % 100];
          number /= 100;
        }
        else {
          basic = basicNumbers[number % 10];
          number /= 10;
          
          if(basic == ""){
            basic = tensNumbers[number % 10] + basic;
            number /= 10;
          } else {
            basic = tensNumbers[number % 10] + "-" + basic;
            number /= 10;
          }
        }
        
        if (number == 0){
            return basic;
        }
        
        return basicNumbers[number] + "hundred " + basic;
        
    } 
    
}
```

