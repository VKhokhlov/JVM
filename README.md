#Домашнее задание по теме «JVM. Организация памяти, сборщики мусора, VisualVM»

***
    public class JvmComprehension {
    
        public static void main(String[] args) {
            int i = 1;                      // 1
            Object o = new Object();        // 2
            Integer ii = 2;                 // 3
            printAll(o, i, ii);             // 4
            System.out.println("finished"); // 7
        }
    
        private static void printAll(Object o, int i, Integer ii) {
            Integer uselessVar = 700;                   // 5
            System.out.println(o.toString() + i + ii);  // 6
        }
    }
***
0. Загрузка класса JvmComprehension
   * подсистема ClassLoader'ов загружает статическую часть класса JvmComprehension;
   * данные о классе JvmComprehension помещаются в Metaspace;
   * в стеке создается фрейм для метода main;
   * параметр args (ссылка) помещается в стек;
   * значение параметра args помещается в кучу;
   * вызывается метод main.
1. int i = 1;
   * В стек (фрейм main) помещается int (переменная i).
2. Object o = new Object();
   * подсистема ClassLoader'ов загружает класс Object;
   * данные о классе Object помещаются в Metaspace;
   * в стек (фрейм main) помещается ссылка на Object (переменная o);
   * в кучу помещается объект Object.
3. Integer ii = 2;
   * в стек (фрейм main) помещается ссылка на Integer (переменная ii);
   * в кучу помещается объект Integer.
4. printAll(o, i, ii);
   * в стеке создается фрейм для метода printAll;
   * в стек (фрейм printAll) помещаются значение i и ссылки o и ii;
   * вызывается метод printAll.
5. Integer uselessVar = 700;
   * в стек (фрейм printAll) помещается ссылка на Integer (переменная uselessVar);
   * в кучу помещается объект Integer.
6. System.out.println(o.toString() + i + ii);
   * подсистема ClassLoader'ов загружает статическую часть класса System;
   * данные о классе System помещаются в Metaspace;
   * в стеке создается фрейм для метода System.out.println;
   * в результате вычисления выражения (o.toString() + i + ii), в куче создается объект String;
   * в стек (фрейм println) помещается ссылка на String (созданный на предыдущем шаге);
   * вызывается метод System.out.println.
   * после вызова метода println, значение его параметра является мусором и может быть удалено сборщиком.
   * после выполнения этой строки метод printAll завершится, будет удален фрейм стека для этого метода, 
   объект Integer будет являться мусором и может быть удален сборщиком. 
7. System.out.println("finished");
   * из Metaspace получаются данные класса System, т.к. он уже был загружен ранее;
   * в стеке создается фрейм для метода System.out.println;
   * в куче создается объект String ("finished");
   * в стек (фрейм println) помещается ссылка на String (созданный на предыдущем шаге);
   * вызывается метод System.out.println;
   * после вызова метода println, значение его параметра является мусором и может быть удалено сборщиком.