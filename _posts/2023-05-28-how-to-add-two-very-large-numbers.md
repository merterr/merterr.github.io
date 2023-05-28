---
layout: post
title: C# İle Çok Büyük İki Sayı Nasıl Toplanır?

---

C# ile decimal tipindeki en büyük sayıyı ekrana yazdıralım.
`Console.WriteLine(decimal.MaxValue);` kodu bize *"79228162514264337593543950335"* çıktısını verir.

Peki bu sayıyı kendisi ile toplayıp ekrana yazdırmaya çalışalım.

`Console.WriteLine(79228162514264337593543950335 + 79228162514264337593543950335);` kodu bize *"Overflow in constant value computation"* uyarısı verir.

Peki sayıyı kendisi ile nasıl toplayabilir?

Cevap şöyle olabilir;

Elimizde iki çok büyük sayı olsun. Bu sayıları string'e dönüştürüp döngüye sokarak kağıt kalem ile ilkokulda yaptığımız gibi toplayabiliriz.

Bu senaryonun C# kodunu yazmaya başlayalım.

Sayılardan birinin uzunluğunun diğerinden küçük olmasını engellemek için bir fonksiyon yazalım.

```csharp
private static string ResizeShortNumber(int length, string number)
        {
            var sb = new StringBuilder();
            for (var l = 0; l < length - number.Length; l++)
                sb.Append('0');
            sb.Append(number);
            return sb.ToString();
        }
```

Dışardan iki sayıyı *string* olarak alalım ve toplama işlemini gerçekleştirelim.

```csharp
private static string AddTwoVeryLargeNumbers(string number1, string number2)
        {
            int length;
            byte tempSum = 0;
            var sum = new List<byte>();
            
            if(number1.Length > number2.Length){
                length = number1.Length;
                number2 = ResizeShortNumber(length, number2);
            }    
            else if(number2.Length > number1.Length) {
                length = number2.Length;
                number1 = ResizeShortNumber(length, number1);
            }
            else
                length = number1.Length;
            
            for(var i = length - 1; i>= 0; i--){
                tempSum += (byte)char.GetNumericValue(number1[i]);
                tempSum += (byte)char.GetNumericValue(number2[i]);
                
                if(tempSum > 9){
                    tempSum -= 10;
                    sum.Add(tempSum);
                    if(i == 0)
                        sum.Add(1);
                    else
                        tempSum = 1;
                }
                else{
                    sum.Add(tempSum);
                    tempSum = 0;
                }
            }
		
            var result = string.Empty;
            for (var j = sum.Count - 1; j >= 0; j--)
                result += sum[j];
            return result;
        }

```

Toplamı ekrana yazdıralım.
```csharp
namespace Test
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("sum: {0}", 
                AddTwoVeryLargeNumbers(
                    decimal.MaxValue.ToString(),
                    decimal.MaxValue.ToString()
                )
            ); // sum: 158456325028528675187087900670
            
        }
    }
}
```

İyi Çalışmalar!