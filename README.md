# Exercise 1

```
public class Rack {
        Boolean[] balls;
        int size;
        
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
