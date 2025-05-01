# nor
using System;
using System.IO;
using System.Linq;

class Program
{
    static void Main(string[] args)
    {
        // Читаем все строки из файла "17.txt" и преобразуем их в массив целых чисел
        int[] numbers = File.ReadAllLines("17.txt").Select(int.Parse).ToArray();

        // Находим минимальное трехзначное число (>=100), которое делится на 7
        // Если таких чисел нет, используем 0 (DefaultIfEmpty(0))
        int minThreeDigitDivBy7 = numbers.Where(x => x >= 100 && x % 7 == 0).DefaultIfEmpty(0).Min();

        // Находим минимальное четырехзначное число (>=1000)
        // Если таких чисел нет, используем 0 (DefaultIfEmpty(0))
        int minFourDigit = numbers.Where(x => x >= 1000).DefaultIfEmpty(0).Min();

        // Вычисляем последнюю цифру минимального четырехзначного числа (по модулю, чтобы избежать отрицательных)
        int lastDigitOfMinFourDigit = Math.Abs(minFourDigit % 10);

        // Инициализируем счетчик подходящих пар
        int count = 0;

        // Инициализируем переменную для хранения максимальной суммы элементов пар (начинаем с минимального возможного int)
        int maxSum = int.MinValue;

        // Проходим по всем парам подряд идущих чисел в массиве
        for (int i = 0; i < numbers.Length - 1; i++)
        {
            // Первое число в паре
            int a = numbers[i];

            // Второе число в паре
            int b = numbers[i + 1];

            // Проверяем условие: хотя бы одно число в паре меньше minThreeDigitDivBy7
            if (a < minThreeDigitDivBy7 || b < minThreeDigitDivBy7)
            {
                // Вычисляем произведение чисел в паре
                int product = a * b;

                // Находим последнюю цифру произведения (по модулю)
                int lastDigitOfProduct = Math.Abs(product % 10);

                // Проверяем, совпадает ли последняя цифра произведения с последней цифрой minFourDigit
                if (lastDigitOfProduct == lastDigitOfMinFourDigit)
                {
                    // Увеличиваем счетчик подходящих пар
                    count++;

                    // Вычисляем сумму чисел в паре
                    int sum = a + b;

                    // Обновляем максимальную сумму, если текущая сумма больше
                    if (sum > maxSum)
                    {
                        maxSum = sum;
                    }
                }
            }
        }

        // Выводим результат: количество подходящих пар и максимальную сумму
        Console.WriteLine($"{count} {maxSum}");
    }
}
