# Занятие 3. Потоки ввода/вывода. Функции.

1. [Потоки ввода/вывода](#stream)
    1. [Что такое потов ввода/вывода](#stream0)
    2. [Потоки в Java](#stream1)
    3. [Байтовые Потоки](#stream2)
      1. [Класс InputStream](#stream3)
      2. [Класс OutputStream](#stream4)
    4. [Работа с файлами](#stream6)
    5. [Символьные потоки](#stream8)
      1. [Класс Reader](#stream9)
      2. [Класс Writer](#stream10)
    6. [Стрруктура java.io](#stream12)
    7. [Стандартые потоки](#stream13)
      1. [Стандартный поток вывода](#stream14)
      2. [Стандартный поток ввода](#stream15)

<a name="stream"/>

## Потоки ввода/вывода

<a name="stream0"/>

### Что такое потов ввода/вывода

Отличительной чертой многих языков программирования является работа с файлами и потоками. В Java основной функционал работы с потоками сосредоточен в классах из пакета **java.io**.

Ключевым понятием здесь является понятие **потока**. Хотя понятие "поток" в программировании довольно перегружено и может обозначать множество различных концепций. В данном случае применительно к работе с файлами и вводом-выводом мы будем говорить о **потоке (stream)**, как об абстракции, которая используется для чтения или записи информации (файлов, сокетов, текста консоли и т.д.).

Пакет **java.io** содержит почти каждый класс, который может потребоваться Вам для совершения ввода и вывода в Java. Все данные потоки представлены потоком ввода и адресом вывода. Поток в пакете **java.io** осуществляет поддержку различных данных, таких как примитивы, объекты, локализованные символы и т.д.

Поток связан с реальным физическим устройством с помощью системы ввода-вывода Java. У нас может быть определен поток, который связан с файлом и через который мы можем вести чтение или запись файла. Это также может быть поток, связанный с сетевым сокетом, с помощью которого можно получить или отправить данные в сети. Все эти задачи: чтение и запись различных файлов, обмен информацией по сети, ввод-ввывод в консоли мы будем решать в Java с помощью потоков.

Объект, из которого можно считать данные, называется **потоком ввода**, а объект, в который можно записывать данные, - **потоком вывода**. Например, если надо считать содержание файла, то применяется **поток ввода**, а если надо записать в файл - то **поток вывода**.

<a name="stream1"/>

### Потоки в Java

Потоки в Java определяются в качестве последовательности данных. Существует два типа потоков:

* **InputStream** – поток ввода используется для считывания данных с источника.
* **OutputStream** – поток вывода используется для записи данных по месту назначения.

Java предоставляет сильную, но гибкую поддержку в отношении ввода/вывода, связанных с файлами и сетями.

<a name="stream2"/>

### Байтовые Потоки

Потоки байтов в Java используются для осуществления ввода и вывода 8-битных байтов.

<a name="stream3"/>

#### Класс InputStream

Класс **InputStream** является базовым для всех классов, управляющих байтовыми потоками ввода. Рассмотрим его основные методы:

* **int available()**: возвращает количество байтов, доступных для чтения в потоке

* **void close()**: закрывает поток

* **int read()**: возвращает целочисленное представление следующего байта в потоке. Когда в потоке не останется доступных для чтения байтов, данный метод возвратит число -1

* **int read(byte[] buffer)**: считывает байты из потока в массив buffer. После чтения возвращает число считанных байтов. Если ни одного байта не было считано, то возвращается число -1

* **int read(byte[] buffer, int offset, int length)**: считывает некоторое количество байтов, равное length, из потока в массив buffer. При этом считанные байты помещаются в массиве, начиная со смещения offset, то есть с элемента buffer[offset]. Метод возвращает число успешно прочитанных байтов.

* **long skip(long number)**: пропускает в потоке при чтении некоторое количество байт, которое равно number

<a name="stream4"/>

#### Класс OutputStream

Класс OutputStream является базовым классом для всех классов, которые работают с бинарными потоками записи. Свою функциональность он реализует через следующие методы:

* **void close()**: закрывает поток

* **void flush()**: очищает буфер вывода, записывая все его содержимое

* **void write(int b)**: записывает в выходной поток один байт, который представлен целочисленным параметром b

* **void write(byte[] buffer)**: записывает в выходной поток массив байтов buffer.

* **void write(byte[] buffer, int offset, int length)**: записывает в выходной поток некоторое число байтов, равное length, из массива buffer, начиная со смещения offset, то есть с элемента buffer[offset].

<a name="stream5"/>

#### Пример

      import java.io.*; // указываем, что будем использовать IO Api

      public class InputOutputStreamExam {

          private InputStream inputstream;    // класс для чтения файла

          private OutputStream outputStream;  // класс для записи в файл

          private String path;                // путь к файлу который будем читать и записывать

          public InputOutputStreamExam(String path) {
              this.path = path;
          }

          // чтение файла используя InputStream
          public void read() throws IOException {
              // инициализируем поток на чтение
              inputstream = new FileInputStream(path);

              // читаем первый символ в байтах (ASCII)
              int data = inputstream.read();
              char content;
              // по байтово читаем весь файл
              while(data != -1) {
                  // преобразуем полученный байт в символ
                  content = (char) data;
                  // выводим посимвольно
                  System.out.print(content);
                  data = inputstream.read();
              }
              // закрываем поток
              inputstream.close();
          }

          // запись в файл используя OutputStream
          public void write(String st) throws IOException {
              // инициализируем поток для вывода данных
              // что позволит нам записать новые данные в файл
              outputStream = new FileOutputStream(path);
              // передаем полученную строку st и приводим её к byte массиву.
              outputStream.write(st.getBytes());
              // закрываем поток вывода
              // только после того как мы закроем поток данные попадут в файл.
              outputStream.close();
          }

      }

<a name="stream6"/>

### Работа с файлами

Не смотря на множество классов, связанных с потоками байтов, наиболее распространено использование следующих классов: FileInputStream и FileOutputStream.

<a name="stream7"/>

#### Пример

    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;

    public class FileInputOutputStream {

        // Класс для работы потоком вывода из файла
        private FileInputStream inputStream;

        // Класс для работы потоком ввода в файл
        private FileOutputStream outputStream;

        // полный путь к файлу
        private String path;

        public FileInputOutputStream(String path) {
            this.path = path;
        }

        public void read() throws IOException {
            // инициализируем поток вывода из файлу
            inputStream = new FileInputStream(path);

            // читаем первый символ с потока байтов
            int data = inputStream.read();
            char content;

            // если data будет равна 0 то это значит,
            // что файл пуст
            while(data != -1) {
                // переводим байты в символ
                content = (char) data;

                // выводим полученный символ
                System.out.print(content);

                // читаем следующий байты символа
                data = inputStream.read();
            }

            // закрываем поток чтения файла
            inputStream.close();
        }

        public void write(String st) throws IOException {
            // открываем поток ввода в файл
            outputStream = new FileOutputStream(path);

            // записываем данные в файл, но
            // пока еще данные не попадут в файл,
            // а просто будут в памяти
            outputStream.write(st.getBytes());

            // только после закрытия потока записи,
            // данные попадают в файл
            outputStream.close();
        }

    }

<a name="stream8"/>

### Символьные потоки

Потоки байтов в Java позволяют произвести ввод и вывод 8-битных байтов, в то время как потоки символов используются для ввода и вывода 16-битного юникода.

<a name="stream9"/>

#### Класс Reader

Класс Reader предоставляет функционал для чтения текстовой информации. Рассмотрим его основные методы:

* **void close()**: закрывает поток ввода

* **int read()**: возвращает целочисленное представление следующего символа в потоке. Если таких символов нет, и достигнут конец файла, то возвращается число -1

* **int read(char[] buffer)**: считывает в массив buffer из потока символы, количество которых равно длине массива buffer. Возвращает количество успешно считанных символов. При достижении конца файла возвращает -1

* **int read(CharBuffer buffer)**: считывает в объект CharBuffer из потока символы. Возвращает количество успешно считанных символов. При достижении конца файла возвращает -1

* **int read(char[] buffer, int offset, int count)**: считывает в массив buffer, начиная со смещения offset, из потока символы, количество которых равно count

* **long skip(long count)**: пропускает количество символов, равное count. Возвращает число успешно пропущенных символов

<a name="stream10"/>

#### Класс Writer

Класс Writer определяет функционал для всех символьных потоков вывода. Его основные методы:

* **Writer append(char c)**: добавляет в конец выходного потока символ c. Возвращает объект Writer

* **Writer append(CharSequence chars)**: добавляет в конец выходного потока набор символов chars. Возвращает объект Writer

* **void close()**: закрывает поток

* **void flush()**: очищает буферы потока

* **void write(int c)**: записывает в поток один символ, который имеет целочисленное представление

* **void write(char[] buffer)**: записывает в поток массив символов

* **void write(char[] buffer, int off, int len)** : записывает в поток только несколько символов из массива buffer. Причем количество символов равно len, а отбор символов из массива начинается с индекса off

* **void write(String str)**: записывает в поток строку

* **void write(String str, int off, int len)**: записывает в поток из строки некоторое количество символов, которое равно len, причем отбор символов из строки начинается с индекса off

<a name="stream11"/>

#### Пример

Не смотря на множество классов, связанных с потоками символов, наиболее распространено использование следующих классов: FileReader и FileWriter. Не смотря на тот факт, что внутренний FileReader использует FileInputStream, и FileWriter использует FileOutputStream, основное различие состоит в том, что FileReader производит считывание двух байтов в конкретный момент времени, в то время как FileWriter производит запись двух байтов за то же время.

    import java.io.*;
    public class Test {

       public static void main(String args[])throws IOException {
          File file = new File("Example.txt");

          // Создание файла
          file.createNewFile();

          // Создание объекта FileWriter
          FileWriter writer = new FileWriter(file);

          // Запись содержимого в файл
          writer.write("Это простой пример,\n в котором мы осуществляем\n с помощью языка Java\n запись в файл\n и чтение из файла\n");
          writer.flush();
          writer.close();

          // Создание объекта FileReader
          FileReader fr = new FileReader(file);
          char [] a = new char[200];   // Количество символов, которое будем считывать
          fr.read(a);   // Чтение содержимого в массив

          for(char c : a)
             System.out.print(c);   // Вывод символов один за другими
          fr.close();
       }
    }


<a name="stream12"/>

### Стрруктура java.io

| InputStream | OutputStream | Reader | Writer |
|:-:|:-:|:-:|:-:|
| FileInputStream  | FileOutputStream   | FileReader  | FileWriter |
| BufferedInpytStream  | BufferedOutputStream  | BufferedReader  | BufferedWriter  |
| ByteArrayInpytStream  | ByteArrayOutputStream  |  CharArrayReader  | CharArrayWriter  |
| FilterInputStream  | FilterOutputStream  | FilterReader  | FilterWriter  |
| DataInputStream   | DataOutputStream  |   |   |
| ObjectInputStream   | ObjectOutputStream  |   |   |


<a name="stream13"/>

### Стандартые потоки

Все языки программирования обеспечивают поддержку стандартного ввода/вывода, где программа пользователя может произвести ввод посредством клавиатуры и осуществить вывод на экран компьютера. Если вы знакомы с языками программирования C либо C++, вам должны быть известны три стандартных устройства STDIN, STDOUT и STDERR. Аналогичным образом, Java предоставляет следующие три стандартных потока:

Стандартный ввод – используется для перевода данных в программу пользователя, клавиатура обычно используется в качестве стандартного потока ввода, представленного в виде System.in.
Стандартный вывод – производится для вывода данных, полученных в программе пользователя, и обычно экран компьютера используется в качестве стандартного потока вывода, представленного в виде System.out.
Стандартная ошибка – используется для вывода данных об ошибке, полученной в программе пользователя, чаще всего экран компьютера служит в качестве стандартного потока сообщений об ошибках, представленного в виде System.err.

<a name="stream14"/>

#### Стандартный поток вывода

Для создания потока вывода в класс System определен объект out. В этом объекте определен метод println, который позволяет вывести на консоль некоторое значение с последующим переводом консоли на следующую строку:

    System.out.println("Hello world");

В метод println передается любое значение, как правило, строка, которое надо вывести на консоль. При необходимости можно и не переводить курсор на следующую строку. В этом случае можно использовать метод System.out.print(), который аналогичен println за тем исключением, что не осуществляет перевода на следующую строку.

    System.out.print("Hello world");

Но с помощью метода System.out.print также можно осуществить перевод каретки на следующую строку. Для этого надо использовать escape-последовательность \n:

Если у нас есть два числа, и мы хотим вывести их значения на экран, то мы можем, например, написать так:

    int x=5;
    int y=6;
    System.out.println("x="+x +"; y="+y);

Но в Java есть также функция для форматированного вывода, унаследованная от языка С: System.out.printf(). С ее помощью мы можем переписать предыдущий пример следующим образом:

    int x=5;
    int y=6;
    System.out.printf("x=%d; y=%d \n", x, y);

В данном случае символы %d обозначают спецификатор, вместо которого подставляет один из аргументов. Спецификаторов и соответствующих им аргументов может быть множество. В данном случае у нас только два аргумента, поэтому вместо первого %d подставляет значение переменной x, а вместо второго - значение переменной y. Сама буква d означает, что данный спецификатор будет использоваться для вывода целочисленных значений типа int.

Кроме спецификатора %d мы можем использовать еще ряд спецификаторов для других типов данных:

* %x: для вывода шестнадцатеричных чисел

* %f: для вывода чисел с плавающей точкой

* %e: для вывода чисел в экспоненциальной форме, например, 1.3e+01

* %c: для вывода одиночного символа

* %s: для вывода строковых значений

Например:

    String name = "Иван";
    int age = 30;
    float height = 1.7f;

    System.out.printf("Имя: %s   Возраст: %d лет   Рост: %.2f метров \n", name, age, height);

При выводе чисел с плавающей точкой мы можем указать количество знаков после запятой, для этого используем спецификатор на %.2f, где .2 указывает, что после запятой будет два знака. В итоге мы получим следующий вывод:

    Имя: Иван   Возраст: 30 лет   Рост: 1,70 метров


<a name="stream15"/>

#### Стандартный поток ввода

Для ввода данных с клавиатуры в Java имеется стандартный поток ввода - **System.in**.

Но у System.in есть минус – он позволяет считать с клавиатуры только коды символов. Чтобы обойти эту проблему и считывать большие порции данных за один раз, можно использовать более сложную конструкцию:

    InputStream inputStream = System.in;
    Reader inputStreamReader = new InputStreamReader(inputStream);
    BufferedReader bufferedReader = new BufferedReader(inputStreamReader);

    String str = bufferedReader.readLine(); //читаем строку с клавиатуры

Более компактная версия записи:

    BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

    String name = reader.readLine();