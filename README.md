# Exercise 1

## Comentários 
Como o limite das bolas sorteadas é de apenas 0 a 59 não utilizei um sorter, como por exemplo o quickSort, em vez disso utilizei um array de boolean (Não poderia ser int pois os arrays são inicializados a 0 e sendo assim não iria ser possivel diferenciar a bola 0) e ao retirar uma bola a posição desse array da bola passa a true.
Suponho também que no assert_equal há um método de comparação entre Arrays, algo do género de Array.equals()


## Funções
#### public void add
Adiciona a bola, caso esteja entre os números 0 e 59, neste caso adicionar refere-se a passar o número no  array de Boolean a true, sempre que se adiciona uma bola é incrementado a variável size.

#### public int[] balls
Função que vai devolver o array de inteiros, podemos criar um array com size fixo uma vez que já temos a variável size dentro da class. 
O resto consiste num ciclo for para confirmar quais os números que estão a true, seja esse o caso é adicionado ao sortedBalls que é o array a ser devolvido.

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

