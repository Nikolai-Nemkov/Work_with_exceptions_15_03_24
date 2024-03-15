# Work_with_exceptions_15_03_24

## Урок 3. Продвинутая работа с исключениями в Java
Напишите приложение, которое будет запрашивать у пользователя следующие данные, разделенные пробелом:

Фамилия Имя Отчество дата _ рождения номер _ телефона пол

Форматы данных:

фамилия, имя, отчество - строки
дата _ рождения - строка формата dd.mm.yyyy
номер _ телефона - целое беззнаковое число без форматирования
пол - символ латиницей f или m.

Приложение должно проверить введенные данные по количеству. Если количество не совпадает, вернуть код ошибки, обработать его и показать пользователю сообщение, что он ввел меньше и больше данных, чем требуется.

Приложение должно распарсить полученную строку и выделить из них требуемые значения. Если форматы данных не совпадают, нужно бросить исключение, соответствующее типу проблемы. Можно использовать встроенные типы java и создать свои. Исключение должно быть корректно обработано, пользователю выведено сообщение с информацией, что именно неверно.

Если всё введено и обработано верно, должен создаться файл с названием, равным фамилии, в него в одну строку должны записаться полученные данные, вида
<Фамилия> <Имя> <Отчество> <дата _ рождения> <номер _ телефона> <пол>

Однофамильцы должны записаться в один и тот же файл, в отдельные строки.
Не забудьте закрыть соединение с файлом.
При возникновении проблемы с чтением-записью в файл, исключение должно быть корректно обработано, пользователь должен увидеть стектрейс ошибки. 

## Решение

 
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // Создаем Мар для хранения данных однофамильцев

        Map<String, String> data = new HashMap<>();

        while (true) {
            System.out.println("Введите данные (Фамилия Имя Отчество дата рождения номер_телефона пол):");
            String input = scanner.nextLine();

            // Проверяем, является ли введенная строка пустой

            if (input.isEmpty()) {
                break;
            }
                // Разделяем введенные данные по пробелу

            String[] parts = input.split(" ");

            // ПРоверяем количество введенных данных

            if (parts.length != 6) {
                System.out.println("Ошибка ввода данных. Попробуйте еще раз.");
                continue;
            }

            String lastName = parts[0];
            String firstName = parts[1];
            String patronymic = parts[2];
            String dateOfBirth = parts[3];
            String phoneNumber = parts[4];
            String gender = parts[5];

            // Поверяем форматы данных

            if (!isValidDate(dateOfBirth) || !isValidPhoneNumber(phoneNumber) || !isValidGender(gender)) {
                System.out.println("Ошибка ввода данных. Поробуйте еще раз.");
                continue;
            }

            // Формируем строку для записи в файл

            String dataString = lastName + " " + firstName + " " + patronymic + " " + dateOfBirth + " " + phoneNumber + " " + gender;

            // Записываем данные в файл
            try {

                // Откроем файл на дозапись

                BufferedWriter writer = new BufferedWriter(new FileWriter("data.txt", true));
                writer.write(dataString);
                writer.newLine();
                writer.close();
            } catch (IOException e) {
                System.out.println("Ошибка при записи в файл,");

                e.printStackTrace();
            }

            // Добавляем данные в Мар для однофамильцев
            data.put(lastName, dataString);
        }
        // Выводим данные однофамильцев

        for (Map.Entry<String, String> entry : data.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        // Закрываем соединение с пользовательским вводом

        scanner.close();
    }
    // Проверка формата даты (dd.mm.yyyy)

    private static boolean isValidDate(String date) {
        String regex = "\d{2}\.\d{2}\.\d{4}";
        return date.matches(regex);
    }

    // Проверка формата номера телефона (целое беззнаковое число без форматирования)

    private static boolean isValidPhoneNumber(String phoneNumber) {
        String regex = "\d+";
        return phoneNumber.matches(regex);
    }

    // Проверка формата пола (f или m)
    private static boolean isValidGender(String gender){

        return gender.equals("f") || gender.equals("m");
    }

}

