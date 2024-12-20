# CompanyCoreLib
using System;



namespace CompanyCoreLib
{
    public class Analytics
    {
    }
    public List<DateTime> PopularMonths(
        List<DateTime> dates)
    {
        var dateTimeWithCounterList =
            new List<Tuple<DateTime, int>>();

        int previousYear = DateTime.Now.Year - 1;
        foreach (DateTime iterDate in dates)
        {
            if (iterDate.Year == previousYear)
            {
                // вычисляем начало месяца для текущей даты
                var dateMonthStart = new DateTime(
                    iterDate.Year,  // год
                    iterDate.Month, // месяц
                    1, 0, 0, 0);    // день

                // ищем эту дату во временном списке
                var index = dateTimeWithCounterList
                    .FindIndex(
                        item => item.Item1 == dateMonthStart);

                // кортежи можно создавать по-разному
                if (index == -1)
                {
                    // такой даты нет - добавляю (используя конструктор)
                    dateTimeWithCounterList.Add(
                        new Tuple<DateTime, int>(
                            dateMonthStart, 1));
                }
                else
                {
                    /* 
                        дата есть - увеличиваем счетчик
                        свойства кортежа неизменяемые, 
                        поэтому перезаписываем текущий элемент
                        новым кортежем, 
                        который создаем статическим методом
                    */
                    dateTimeWithCounterList[index] = Tuple.Create(
                        dateTimeWithCounterList[index].Item1,
                        dateTimeWithCounterList[index].Item2 + 1);
                }
            }
        }

        return dateTimeWithCounterList
            .OrderByDescending(item => item.Item2)
            .ThenBy(item => item.Item1)
            .Select(item => item.Item1)
            .ToList();
    }
