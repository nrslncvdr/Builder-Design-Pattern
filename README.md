### Kurucu (Builder) Tasarım Deseni

Builder tasarım deseni creational design patterns grubuna ait, birden fazla özellikli kompleks yapılardaki bir sınıfın, bir nesnenin ihtiyaca göre farklı farklı parametrelerle oluşturulmasında kullanılan bir modeldir. Mesela bir nesne, ilgili sınıfın içerisinde kurucu metot (Constructor) ile elde edildiğine göre ihtiyaç duyulan tüm nesneler için kurucu metodu tekrar tekrar oluşturmak aklımıza gelebilir fakat bu şekilde onlarca kurucu metot oluşturmuş oluruz. Oluşturulan bu kurucu metotların ne zaman kullanılacağı, gerekli mi değil mi gibi soruları beraberinde getirecektir.

Kısacası karmaşık bir nesneyi oluşturan parçaları üretip sonrada onları adım adım birleştiren bir üretici nesne yapmamızı sağlayarak bizi bu sorunlardan kurtarıyor.


![image](https://user-images.githubusercontent.com/58823845/71293546-cf603500-2386-11ea-8830-25cf7936b5f0.png)

File: Packing.java
```java
public interface Packing
{  
       public String pack();  
       public int price();  
} 
```
```Packing``` isiminde bir interface tanımlıyoruz ve içinde``` pack()``` ve ```price()``` adlı iki adet metot barındırıyor.

File: CD.java
```java
public abstract class CD implements Packing
{  
       public abstract String pack();  
}  
```
Oluşturduğumuz ```Packing``` interface sınıfını oluşturduğumuz abstract(soyut) olan ```CD``` sınıfına implements ediyoruz.

File: Company.java
```java
public abstract class Company extends CD
{  
       public abstract int price();  
}  
```
```Company``` adlı bir abstract daha oluşturup ```CD``` abstract sınıfı buna extends ediyoruz. Böylelikle ata sınıflarının içerisinde olan metotları kullanabilcek.

File: Sony.java
```java
public class Sony extends Company
{  
    @Override  
    public int price()
    {   
        return 20;  
    }  
    @Override  
    public String pack()
    {  
        return "Sony CD";  
    }         
}//End of the Sony class.  
```
```Sony``` sınıfına ```Company``` sınıfını extends ediyoruz ve ```price()``` ve ```pack()``` metotlarını bu sınıf içerisinde değiştiriyoruz.

File: Samsung.java
```java
public class Samsung extends Company 
{  
    @Override  
    public int price()
    {   
        return 15;  
    }  
    @Override  
    public String pack()
    {  
        return "Samsung CD";  
    }         
}//End of the Samsung class. 
```
```Sony``` sınıfında yaptığımız işlemin aynısını ```Samsung``` sınıfı içerisinde kendi özelliklerine göre dolduruyoruz.

File: CDType.java
```java
import java.util.ArrayList;  
import java.util.List;  
public class CDType 
{  
     private List<Packing> items=new ArrayList<Packing>();  
     public void addItem(Packing packs) 
     {    
          items.add(packs);  
     }  
     public void getCost()
     {  
         for (Packing packs : items) 
         {  
             packs.price();  
         }   
      }  
      public void showItems()
      {  
            for (Packing packing : items)
            {  
                 System.out.print("CD name : "+packing.pack());  
                 System.out.println(", Price : "+packing.price());  
            }       
       }     
}//End of the CDType class.  
```
```CDType``` sınıfında items adında bir liste oluşturuyoruz ve bu listeye, Pack tipli gelen verileri ekliyoruz,daha sonrasında bu verileri getCost ile ```price()``` değerlerini geri döndürüyoruz. ```showItems()``` metoduyla ise bunları ekrana yazdırıyoruz.

File: CDBuilder.java
```java
public class CDBuilder 
{  
    public CDType buildSonyCD()
    {   
          CDType cds=new CDType();  
          cds.addItem(new Sony());  
          return cds;  
    }  
    public CDType buildSamsungCD()
    {  
          CDType cds=new CDType();  
          cds.addItem(new Samsung());  
          return cds;  
    }  
}// End of the CDBuilder class.  
```
Kurucu sınıfımız olan ```CDBuilder``` sınıfını oluşturuyoruz. Bu sınıf içerisinde iki farklı metot oluşturuyoruz. Burada ilgili metotlar içerisinde uygun türün nesnesi oluşturuluyor ve geri döndürülüyor.

File: BuilderDemo.java
```java
public class BuilderDemo
{  
     public static void main(String args[])
     {  
         CDBuilder cdBuilder=new CDBuilder();  
         CDType cdType1=cdBuilder.buildSonyCD();  
         cdType1.showItems();  

         CDType cdType2=cdBuilder.buildSamsungCD();  
         cdType2.showItems();  
     }  
}  
```
Main bölümünde ise ```CDBuilder``` sınıfımızı kullanarak istediğimiz kurucu nesneyi oluşturup, içeriğini doldurabiliyoruz.

OutPut:

CD name : Sony CD, Price : 20  
CD name : Samsung CD, Price : 15  

### Ön Yüz (Facade) Tasarım Deseni

Facade tasarım deseni structural design patterns grubuna ait, istemci tarafından alt sınıfların direkt olarak kullanılması yerine bir başka nesne oluşturup alt sistemlerin parçalarını oluşturan sınıfları istemciden soyutlayarak kullanımı daha da kolay hale getirmek amacıyla yazılır.
Yani kısaca facade istemciden gelen talepleri alt sisteme ileten, alt sistemin dışarıya olan bir kapısıdır.

![image](https://user-images.githubusercontent.com/58823845/71294865-e99c1200-238a-11ea-9ea1-82cded6c74d3.png)

File: Strategy.java
```java
public interface Strategy 
{
   public int doOperation(int num1, int num2);
}
```
```Strategy``` adlı bir interface ve içerisine 2 adet parametresi olan ```doOperation()``` metodunu oluşturuyoruz. Bu sınıf bizim facade işlevini görecek olan sınıfla iletişime geçecektir.

File: OperationAdd.java
``` java
public class OperationAdd implements Strategy
{
   @Override
   public int doOperation(int num1, int num2) 
   {
      return num1 + num2;
   }
}
```
```OperationAdd``` sınıfına oluşturmuş olduğumuz interface implements ediyoruz ve bu sınıf içerisinde ```doOperation()``` adli metotu override ederek içeriğini dolduruyoruz.

File: OperationSubstract.java
```java
public class OperationSubstract implements Strategy
{
   @Override
   public int doOperation(int num1, int num2) 
   {
      return num1 - num2;
   }
}
```
```OperationSubstract sınıfında``` aynı işlemi yapıyoruz ve ```doOperation()``` metodunun içeriğini uygun şekilde değiştiriyoruz.

File: OperationMultiply.java
```java
public class OperationMultiply implements Strategy
{
   @Override
   public int doOperation(int num1, int num2)
   {
      return num1 * num2;
   }
}
```
File: Context.java
```java
public class Context 
{
     private Strategy strategy;

     public Context(Strategy strategy)
     {
        this.strategy = strategy;
     }
     public int executeStrategy(int num1, int num2)
     {
        return strategy.doOperation(num1, num2);
     }
}
```
Facade işlevini yerine getirecek olan sınıftır. Kurucu metot içerisine parametre olarak ```Strategy``` tipli bir değişkeni alıyor. ```executeStrategy()``` metodu ile gelen değerleri alt sınıflardaki metotlara yollar böylelikle facade, istemci ile bu alt sınıflar arasında bir kapı görevi görmüş olur.

File: StrategyPatternDema.java
```java
public class StrategyPatternDemo 
{
   public static void main(String[] args) 
   {
      Context context = new Context(new OperationAdd());		
      System.out.println("10 + 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationSubstract());		
      System.out.println("10 - 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationMultiply());		
      System.out.println("10 * 5 = " + context.executeStrategy(10, 5));
   }
}
```
Facade görevini yerine getiren ```Context``` sınıfından bir nesne  üretirken aynı zamanda parametre olarak alt sınıflardan hangisini oluşturacaksak onu yeni nesne isteği şeklinde yolluyoruz ve bunu ekrana yazdırıyoruz.

OutPut:

10 + 5 = 15  
10 - 5 = 5  
10 * 5 = 50
