# test7seasol.github.io

// Hello World

// Pivot number logic optimum logical task logical programm
  
    class Main {
        public static void main(String[] args) {
            System.out.println("Try programiz.pro");
            
            int start = 1;
            int end = 9;
            int pivot = 5;
            
            int temp = pivot;
            
            for(int i=start;i<=end;i++){
                if(pivot >= i){
                    System.out.print(", " + temp);
                    temp--;
                }
                else{
                    pivot++;
                    System.out.print(", " + pivot);
                   
                }
            }
            
        }
    }
