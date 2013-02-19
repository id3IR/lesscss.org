#Індекс

	escape(@string);               // URL кодування рядку
	e(@string);                    // екранування вмісту рядка
	%(@string, values...);         // форматування рядка
	
	unit(@dimension, [@unit: ""]); // видалити чи змінити одиниці вимірювання
	color(@string);				   // розбір рядка в колір
	
	ceil(@number);                 // округлити "вверх" до цілого значення
	floor(@number);                // округлити "вниз" до цілого значення
	percentage(@number);           // перетворення у відсотки %, напр. 0.5 -> 50%
	round(number, [places: 0]);	   // оуркглити із точністю places

	rgb(@r, @g, @b);                             // конвертація в колір
	rgba(@r, @g, @b, @a);                        // конвертація в колір
	argb(@color);                                // створити #AARRGGBB
	hsl(@hue, @saturation, @lightness);          // створити колір
	hsla(@hue, @saturation, @lightness, @alpha); // створити колір
	hsv(@hue, @saturation, @value);              // створити колір
	hsva(@hue, @saturation, @value, @alpha);     // створити колір
	
    hue(@color);        // повертає `hue` канал кольору @color
    saturation(@color); // повертає `saturation` канал кольору @color
    lightness(@color);  // повертає 'lightness' канал кольору @color
    red(@color);        // повертає 'red' канал кольору @color
    green(@color);      // повертає 'green' канал кольору @color
    blue(@color);       // повертає 'blue' канал кольору @color
    alpha(@color);      // повертає 'alpha' канал кольору @color
    luma(@color);       // повертає 'luma' значення (перцептивну яскравість) кольору @color
	
    saturate(@color, 10%);                  // повертає колір на 10% *більш* насичений
    desaturate(@color, 10%);                // повертає колір на 10% *менш* насичений
    lighten(@color, 10%);                   // повертає колір на 10% *світліший*
    darken(@color, 10%);                    // повертає колір на 10% *темніший*
    fadein(@color, 10%);                    // повертає колір на 10% *менш* прозорий
    fadeout(@color, 10%);                   // повертає колір на 10% *більш* прозорий
    fade(@color, 50%);                      // повертає колір @color із прозорістю 50%
    spin(@color, 10);                       // повертає на 10 градусів більший у відтінку колір
    mix(@color1, @color2, [@weight: 50%]);  // поверає суміш @color1 та @color2
	greyscale(@color);                      // повертає сірий, 100% ненасичений колір
    contrast(@color1, [@darkcolor: black], [@lightcolor: white], [@threshold: 43%]); 
	                                        // повертає @darkcolor якщо @color1 яскравіший на 43%  
		                                    // у іншому випадку повертає @lightcolor

	multiply(@color1, @color2);
	screen(@color1, @color2);
	overlay(@color1, @color2);
	softlight(@color1, @color2);
	hardlight(@color1, @color2);
	difference(@color1, @color2);
	exclusion(@color1, @color2);
	average(@color1, @color2);
	negation(@color1, @color2);
	
#Функції роботи із текстом
###escape

Застосовує [URL-кодування](http://en.wikipedia.org/wiki/Percent-encoding) до спеціальний символів у вхідному рядку. 

* Наступні символи - виключення і не кодуються: `,`, `/`, `?`, `@`, `&`, `+`, `'`, `~`, `!` та `$`. 
* Найчастіші символи що кодуються: `<space>`, `#`, `^`, `(`, `)`, `{`, `}`, `|`, `:`, `>`, `<`, `;`, `]`, `[` та `=`.

Параметри:

* `string`: Рядок для кодування

Повертає: екранований `string` без лапок.

Приклад:

    escape('a=1')

Результат:

    a%3D1
    
Примітка: Поведінка функції, якщо параметр не рядок, або не визначений. 
Поточна реалізаціє повертає `undefined` для кольору і незмінене вхідне значення 
для аргументів іншого типу. Така поведінка може змінитися у наступних версіях.

###e
CSS екранування, синтакс подібний до `~"value"`. Функція очікує аргументом рядок 
і повертає його вміст "як є", але без лапок. Це можна використовувати для виводу 
CSS значення котре має або не валідний CSS синтакс, або використовує синтакс, 
який не підтримується LESS.

Параметри:

* `string`: Рядок для екранування

Повертає: `string` без лапок.

Приклад:

    filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";

Результат:

    filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
    
Примітка: Функція приймає, також, `~""` екрановані значення і числа, як параметри. 
Будь-що інше призведе до помилок.

###% format
Функція `%("format", arguments ...)` форматує рядок. Перший аргумент - рядок із плейсхолдерами (placeholders). 
Всі плейсхолдери починаються із символу відсотка `%`, за яким йде літера `s`,`S`,`d`,`D`,`a`, чи `A`. 
Інші аргуманти містять значення для заміни плейсхолдерів. Якщо потрібно вивести символ відсотку, екрануйте його іншим символом відсотку `%%`.

Використовуйте плейсхолдери у верхньому регістрі, якщо потрібно екранувати спеціальні символи у їх utf-8 escape коди. 
Функція екранує всі спеціальні символи, крім `()'~!`. Пробіл кодується як `%20`. Плейсхолдери у нижньому регістрі залишають спеціальні символи "як є". 

Плейсхолдери:
* d, D, a, A - можуть бути замінені будь аргументами будь-якого типу (колір, число, екранована змінна, вираз, ...). 
Якщо ви використовуєте їх у комбінації із рядком, буде використаний рядок повністю, із лапками. 
Тим не менше, лапки будуть вставлені у рядок "як є", не екрановані "/" чи подібно.
* s, S - можуть бути замінені аргументами будь-якого типу, крім кольору. 
Якщо використовувати їх у комбінації із рядком, тільки значення рядка буде використовуватися - лапки пропустяться.

Параметри:

* `string`: формат рядка із плейсхолдерами,
* `anything`* : значення для заміни плейсхолдерів.

Повертає: форматований рядок `string`.

Приклад:

    format-a-d: %("repetitions: %a file: %d", 1 + 2, "directory/file.less");
    format-a-d-upper: %('repetitions: %A file: %D', 1 + 2, "directory/file.less");
    format-s: %("repetitions: %s file: %s", 1 + 2, "directory/file.less");
    format-s-upper: %('repetitions: %S file: %S', 1 + 2, "directory/file.less");


Результат:

    format-a-d: "repetitions: 3 file: "directory/file.less"";
    format-a-d-upper: "repetitions: 3 file: %22directory%2Ffile.less%22";
    format-s: "repetitions: 3 file: directory/file.less";
    format-s-upper: "repetitions: 3 file: directory%2Ffile.less";
    
#Змішані функції
###color
Парсить колір, таким чином, рядок, що репрезентує колір, стає кольором.

Параметри:

* `string`: рядок із кольором

Приклад:

    color("#aaa");

Результат:

    #aaa

###unit
Видалити або змінити одиниці виміру

Параметри:

* `dimension`: число із, або без, точності
* `unit`: Опціонально: одиниця виміру, в яку конвертувати, або, якщо пропущено, видалити одиницю виміру

Приклад:

    unit(5, px)

Результат:

    5px
	
Приклад:

    unit(5em)

Результат:

    5

#Математичні функції
###ceil
Округлення "вверх" до наступного, більшлго цілого значення.

Параметри:

* `number`: Дійсне число.

Повертає: `ціле число`

Приклад:

    ceil(2.4)

Результат:

    3

###floor
Округлення числа "вниз" до попереднього, меншого цілого числа.

Параметри:

* `number`: Дійсне число.

Повертає: ціле число `ціле число`

Приклад:

    floor(2.6)

Результат:

    2
###percentage
Конвертує дійсне число у відсотки.

Параметри:

* `number`: Дійсне число.

Повертає: `string`

Приклад:

    percentage(0.5)

Результат:

    50%
###round
Округлення.

Параметри:

* `number`: Дійсне число.
* `decimalPlaces`: Опціонально: Точність округлення. За замовчуванням 0.

Повертає: `number`

Приклад:

    round(1.67)

Результат:

    2
	
Приклад:

    round(1.67, 1)

Результат:

    1.7
#Функції кольору
##Визначення кольору
###rgb
Створює об’єкт непрозорого кольору із десяткових red, green та blue (RGB) значень. 
Стандартне значення кольору HTML/CSS формату можна також використовувати, наприклад `#ff0000`.

Параметри:

* `red`: Ціле цисло 0-255 або відсоток 0-100%.
* `green`: Ціле цисло 0-255 або відсоток 0-100%.
* `blue`: Ціле цисло 0-255 або відсоток 0-100%.

Повертає: `колір`

Приклад:

    rgb(90, 129, 32)

Результат:

    #5a8120
###rgba
Створює об’єкт прозорого кольору із десяткових red, green, blue та alpha (RGBA) значень.

Параметри:

* `red`: Ціле число 0-255 або відсоток 0-100%.
* `green`: Ціле число 0-255 або відсоток 0-100%.
* `blue`: Ціле число 0-255 або відсоток 0-100%.
* `alpha`: Число 0-1 чи відсоток 0-100%.

Повертає: `колір`

Приклад:

    rgba(90, 129, 32, 0.5)

Результат:

    rgba(90, 129, 32, 0.5)
###argb
Створює hex зображення кольору у форматі `#AARRGGBB` (**НЕ** `#RRGGBBAA`!).

Параметри:

* `color`: Об’єкт кольору.

Повертає: `string`

Приклад:

    argb(rgba(90, 23, 148, 0.5));

Результат:

    #805a1794
###hsl
Створює об’єкт непрозорого кольору із hue, saturation та lightness (HSL) значень.

Параметри:

* `hue`: Ціле число 0-360, градуси.
* `saturation`: Відсоток 0-100% або число 0-1.
* `lightness`: Відсоток 0-100% або число 0-1.

Повертає: `колір`

Приклад:

    hsl(90, 100%, 50%)

Результат:

    #80ff00
	
Це корисно, якщо потрібно створити новий колір, базований на каналі іншого кольору, наприклад:

    @new: hsl(hue(@old), 45%, 90%);

`@new` буде мати значення *hue* із `@old` і власні saturation та lightness.
	
###hsla
Створює об’єкт прозорого кольору із hue, saturation, lightness та alpha (HSLA) значень.

Параметри:

* `hue`: Ціле число 0-360, градуси.
* `saturation`: Відсоток 0-100% або число 0-1.
* `lightness`: Відсоток 0-100% або число 0-1.
* `alpha`: Відсоток 0-100% або число 0-1.

Повертає: `колір`

Приклад:

    hsl(90, 100%, 50%, 0.5)

Результат:

    rgba(128, 255, 0, 0.5)
###hsv
Створює об’єкт непрозорого кольору із hue, saturation та value (HSV) значень. Візьміть до уваги, що це не те ж, що і `hsl`.

Параметри:

* `hue`: Ціле число 0-360, градуси.
* `saturation`: Відсоток 0-100% або число 0-1.
* `value`: Відсоток 0-100% або число 0-1.

Повертає: `колір`

Приклад:

    hsv(90, 100%, 50%)

Результат:

    #408000

###hsva
Створює об’єкт прозорого кольору із hue, saturation, value та alpha (HSVA) значень. Візьміть до уваги, що це не те ж, що і `hsla`.

Параметри:

* `hue`: Ціле число 0-360, градуси.
* `saturation`: Відсоток 0-100% або число 0-1.
* `value`: Відсоток 0-100% або число 0-1.
* `alpha`: Відсоток 0-100% або число 0-1.

Повертає: `колір`

Приклад:

    hsva(90, 100%, 50%, 0.5)

Результат:

    rgba(64, 128, 0, 0.5)

##Інформація каналу кольору
###hue
Обрахувати канал hue об’єкту кольору.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `ціле число` 0-360

Приклад:

    hue(hsl(90, 100%, 50%))

Результат:

    90
###saturation
Обрахувати канал saturation об’єкту кольору.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `відсотки` 0-100

Приклад:

    saturation(hsl(90, 100%, 50%))

Результат:

    100%
###lightness
Обрахувати канал lightness об’єкту кольору.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `відсотки` 0-100

Приклад:

    lightness(hsl(90, 100%, 50%))

Результат:

    50%
###red
Обрахувати канал red об’єкту кольору.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `ціле число` 0-255

Приклад:

    red(rgb(10, 20, 30))

Результат:

    10
###green
Обрахувати канал green об’єкту кольору.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `ціле число` 0-255

Приклад:

    green(rgb(10, 20, 30))

Результат:

    20
###blue
Обрахувати канал blue об’єкту кольору.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `ціле число` 0-255

Приклад:

    blue(rgb(10, 20, 30))

Результат:

    30
###alpha
Обрахувати канал alpha об’єкту кольору.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `дійсне число` 0-1

Приклад:

    alpha(rgba(10, 20, 30, 0.5))

Результат:

    0.5
###luma
Обраховує значення [luma](http://en.wikipedia.org/wiki/Luma_(video)) (сприйняття яскравості) для об’єкту кольору. Використовується SMPTE C / Rec. 709 коефіцієнти, оскільки рекомендовані [WCAG 2.0](http://www.w3.org/TR/2008/REC-WCAG20-20081211/#relativeluminancedef). Ці розрахунки також використовуються у функції контрасту.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `відсотки` 0-100%

Приклад:

    luma(rgb(100, 200, 30))

Результат:

    65%
##Операції із кольором
Функції операції із коьлором приймають параметри в одиницях виміру таких же, як і значення, 
які вони змінюють і відотки опрацьовуються, як абсолютні значення, тобто збільшення 
10% відсоткового значення на 10% відсотків дасть у результаті 20%, а не 11%, і значння будуть 
у їх допустимих межах, тобто не вийдуть за рамки допустимих значень. Де вказані значання, що повертаються, 
ми також навели формати, для того щоб прояснити, що робить кожна функція, 
на додаток до hex варіантів, з якими, зазвичай, проводиться робота..
###saturate
Збільшення насиченості кольору на абсолютне значення.

Параметри:

* `color`: Об’єкт кольору.
* `amount`: Відсоток 0-100%.

Повертає: `колір`

Приклад:

    saturate(hsl(90, 90%, 50%), 10%)

Результат:

    #80ff00 // hsl(90, 100%, 50%)

###desaturate
Зменшення насиченості кольору на абсолютне значення.

Параметри:

* `color`: Об’єкт кольору.
* `amount`: Відсоток 0-100%.

Повертає: `колір`

Приклад:

    desaturate(hsl(90, 90%, 50%), 10%)

Результат:

    #80e51a // hsl(90, 80%, 50%)
###lighten
Збільшення яскравості кольору на абсолютне значення.

Параметри:

* `color`: Об’єкт кольору.
* `amount`: Відсоток 0-100%.

Повертає: `колір`

Приклад:

    lighten(hsl(90, 90%, 50%), 10%)

Результат:

    #99f53d // hsl(90, 90%, 60%)
###darken
Зменшення яскравості кольору на абсолютне значення.

Параметри:

* `color`: Об’єкт кольору.
* `amount`: Відсоток 0-100%.

Повертає: `колір`

Приклад:

    darken(hsl(90, 90%, 50%), 10%)

Результат:

    #66c20a // hsl(90, 90%, 40%)
###fadein
Зменшення прозорості (чи збільшення непрозорості) кольору, зробити його більш непрозорим. 
Не має ніякого ефекту на повністю непрозоорих кольорах. Для застосування алгоритму 
у іншому напрямку використовуйте `fadeout`.

Параметри:

* `color`: Об’єкт кольору.
* `amount`: Відсоток 0-100%.

Повертає: `колір`

Приклад:

    fadein(hsla(90, 90%, 50%, 0.5), 10%)

Результат:

    rgba(128, 242, 13, 0.6) // hsla(90, 90%, 50%, 0.6)
###fadeout
Збільшення прозорості (або зменшення непрозорості) кольору, зробити його менш непрозорим. 
Для застосування алгоритму у іншому напрямку використовуйте `fadein`.

Параметри:

* `color`: Об’єкт кольору.
* `amount`: Відсоток 0-100%.

Повертає: `колір`

Приклад:

    fadeout(hsla(90, 90%, 50%, 0.5), 10%)

Результат:

    rgba(128, 242, 13, 0.4) // hsla(90, 90%, 50%, 0.6)
###fade
Встановити абсолютне значення прозорості лля кольору. Застосовне до кольорів, не 
зважаючи на те чи мають вони значення прозорості, або ні.

Параметри:

* `color`: Об’єкт кольору.
* `amount`: Відсоток 0-100%.

Повертає: `колір`

Приклад:

    fade(hsl(90, 90%, 50%), 10%)

Результат:

    rgba(128, 242, 13, 0.1) //hsla(90, 90%, 50%, 0.1)
###spin
Повернути кут відтінку (hue) у іншому напрямку. Кут відтінку приводиться до вигляду у межах 0-360, 
тобто кути від 360 до 720 та в межах 0-360 дадуть один і той самий результат. 
Візьміть до уваги, що до кольорів застосовується RGB перетворення, тобто значення hue не зберігається для сірих кольорів
(оскільки відтінок не має сенсу, якщо немає насичення), тому переконайтеся, що застосовуєте функції таким чином, щоб зберегти відтінок,
наприкад, не робіть наступним чином:

    @c: saturate(spin(#aaaaaa, 10), 10%);

Правильно:

    @c: spin(saturate(#aaaaaa, 10%), 10);
	
Кольори завжди повертаються, як RGB значення, тому застосування `spin` до сірого кольору не дасть результату.

Параметри:

* `color`: Об’єкт кольору.
* `angle`: Кількість градусів для повороту (+ or -).

Повертає: `колір`

Приклад:

    spin(hsl(10, 90%, 50%), 20)
    spin(hsl(10, 90%, 50%), -20)

Результат:

    #f27f0d // hsl(30, 90%, 50%)
    #f20d33 // hsl(350, 90%, 50%)
###mix
Змішати два кольори разом, пропорційно. Прозорість також обраховується за пропорціями.

Параметри:

* `color1`: Об’єкт кольору.
* `color1`: Об’єкт кольору.
* `weight`: Опціонально, Відсоток - точка балансу між двома кольорами, за замовчуванням 50%.

Повертає: `колір`

Приклад:

    mix(#ff0000, #0000ff, 50%)
    mix(rgba(100,0,0,1.0), rgba(0,100,0,0.5), 50%)

Результат:

    #800080
    rgba(75, 25, 0, 0.75)
###greyscale
Remove all saturation from a color; the same as calling `desaturate(@color, 100%)`. Because the saturation is not affected by hue, the resulting color mapping may be somewhat dull or muddy; `luma` may provide a better result as it extracts perceptual rather than linear brightness, for example `greyscale('#0000ff')` will return the same value as `greyscale('#00ff00')`, though they appear quite different in brightness to the human eye.

Параметри:

* `color`: Об’єкт кольору.

Повертає: `колір`

Приклад:

    greyscale(hsl(90, 90%, 50%))

Результат:

    #808080 // hsl(90, 0%, 50%)
###contrast
Вибрати, який із двох кольорів більш контрастний від іншого. Це корисно для того 
щоб переконатися, що колір читається на фоні, що також корисно для забеспечення доступності.
Ця функція працює так само як [contrast function in Compass for SASS](http://compass-style.org/reference/compass/utilities/color/contrast/). 
У відповідності до [WCAG 2.0](http://www.w3.org/TR/2008/REC-WCAG20-20081211/#relativeluminancedef), 
кольори порівнються, використовуючи їх значення luma, а не яскравість.

Параметри:

* `color`: Об’єкт кольору з яким порівнювати.
* `dark`: optional - Темніший колір (за замовчуваннями чорний).
* `light`: optional - Світліший колір (за замовчуваннями білий).
* `threshold`: optional - Відсоток 0-100%, що вказує де знаходиться перехід від 
"темного" до "світлого" (за замовчуваннями 43%). Використовується для "зсуву" контрасту, 
наприклад, для того, щоб було можливим вирішити, чи 50% сірий фон дає у результаті чорний чи білий текст.
Встановіть значення меншим для 'світліших' палітер та більшим для 'темніших' палітер. За замовчуванням 43%.

Повертає: `колір`

Приклад:

    contrast(#aaaaaa)
    contrast(#222222, #101010)
    contrast(#222222, #101010, #dddddd)
    contrast(hsl(90, 100%, 50%),#000000,#ffffff,40%);
    contrast(hsl(90, 100%, 50%),#000000,#ffffff,60%);

Результат:

    #000000 // чорний
    #ffffff // білий
    #dddddd
    #000000 // чорний
    #ffffff // білий

##Змішування кольорів
Ці операції _однакові_ до змішування мод, як у редакторах, на кшталт Photoshop, Firework or GIMP,
тому можна використовувати її для того, щоб зробити CSS кольори більш органічними до зображень.

###multiply
Множення двох кольорів. Для кожного із двох кольорів RGB канал перемножується, 
а потім ділиться на 255. Результатом є темніший колір.

Параметри:

* `color1`: Об’єкт кольору для множення.
* `color2`: Об’єкт кольору для множення.

Повертає: `колір`

Приклади:

    multiply(#ff6600, #000000);
    
![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/000000/ffffff&text=000000)
    
    multiply(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/331400/ffffff&text=331400)

    multiply(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/662900/ffffff&text=662900)

    multiply(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/993d00/ffffff&text=993d00)

    multiply(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/cc5200/ffffff&text=cc5200)

    multiply(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)

    multiply(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)

    multiply(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/006600/ffffff&text=006600)

    multiply(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)

###screen
Ефект, протилежний до множення. Результатом є яскравіший колір.

Параметри:

* `color1`: Об’єкт кольору для операції _screen_.
* `color2`: Об’єкт кольору для операції _screen_.

Повертає: `колір`

Приклад:

    screen(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)

    screen(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/ff8533/ffffff&text=ff8533)

    screen(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/ffa366/ffffff&text=ffa366)

    screen(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/ffc299/000000&text=ffc299)

    screen(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/ffe0cc/000000&text=ffe0cc)

    screen(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/ffffff/000000&text=ffffff)

    screen(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)

    screen(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/ffff00/ffffff&text=ffff00)

    screen(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/ff66ff/000000&text=ff66ff)

###overlay
Комбінує ефект `multiply` та `screen`. Відносно робить світлі канали світлішими і темні канали темнішими.
**Примітка**: Результат умови визначається за першим параметром.

Параметри:

* `color1`: Об’єкт кольору для перекриття. Також визначає результат: світліший, чи темніший.
* `color2`: Об’єкт кольору для _перекривання_.

Повертає: `колір`

Приклад:

    overlay(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)

    overlay(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/ff2900/ffffff&text=ff2900)

    overlay(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/ff5200/ffffff&text=ff5200)

    overlay(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/ff7a00/ffffff&text=ff7a00)

    overlay(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/ffa300/ffffff&text=ffa300)

    overlay(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/ffcc00/ffffff&text=ffcc00)

    overlay(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)

    overlay(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/ffcc00/ffffff&text=ffcc00)

    overlay(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)

###softlight
Подібно до `overlay`, але дозволяє уникнути у результаті цілком білого, або цілком чорного кольору.

Параметри:

* `color1`: Об’єкт кольору для "_пом’якшення_".
* `color2`: Об’єкт кольору що буде "_пом’якшений_".

Повертає: `колір`

Приклад:

    softlight(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/ff2900/ffffff&text=ff2900)

    softlight(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/ff4100/ffffff&text=ff4100)

    softlight(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/ff5a00/ffffff&text=ff5a00)

    softlight(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/ff7200/ffffff&text=ff7200)

    softlight(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/ff8b00/ffffff&text=ff8b00)

    softlight(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/ffa300/ffffff&text=ffa300)

    softlight(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/ff2900/ffffff&text=ff2900)

    softlight(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/ffa300/ffffff&text=ffa300)

    softlight(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/ff2900/ffffff&text=ff2900)

###hardlight
Подібно до `overlay`, але використовує другий колір, для визначення світлих і 
темних каналів, замість того, щоб використовувати перший колір.

Параметри:

* `color1`: Об’єкт кольору для перекриття іншого.
* `color2`: Об’єкт кольору який буде _перекритим_. Також визначальний колір для результату: світлішого чи темнішого.

Повертає: `колір`

Приклад:

    hardlight(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/000000/ffffff&text=000000)

    hardlight(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/662900/ffffff&text=662900)

    hardlight(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/cc5200/ffffff&text=cc5200)

    hardlight(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/ff8533/ffffff&text=ff8533)

    hardlight(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/ff2900/ffffff&text=ff2900)

    hardlight(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/ffffff/000000&text=ffffff)

    hardlight(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)

    hardlight(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)

    hardlight(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)

###difference
Віднімає другий колір від першого. Операція виконується для кожного RGB каналу. 
Результатом є темніший колір.

Параметри:

* `color1`: Об’єкт кольору, ділене.
* `color2`: Об’єкт кольору, дільник.

Повертає: `колір`

Приклад:

    difference(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)

    difference(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/cc3333/ffffff&text=cc3333)

    difference(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/990066/ffffff&text=990066)

    difference(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/663399/ffffff&text=663399)

    difference(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/3366cc/ffffff&text=3366cc)

    difference(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/0099ff/ffffff&text=0099ff)

    difference(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/006600/ffffff&text=006600)

    difference(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/ff9900/ffffff&text=ff9900)

    difference(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/ff66ff/000000&text=ff66ff)

###exclusion
Подібно до `difference` із меншим контрастом.

Параметри:

* `color1`: Об’єкт кольору, ділене.
* `color2`: Об’єкт кольору, дільник.

Повертає: `колір`

Приклад:

    exclusion(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)

    exclusion(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/cc7033/ffffff&text=cc7033)

    exclusion(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/997a66/ffffff&text=997a66)

    exclusion(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/668599/ffffff&text=668599)

    exclusion(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/338fcc/ffffff&text=338fcc)

    exclusion(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/0099ff/ffffff&text=0099ff)

    exclusion(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/006600/ffffff&text=006600)

    exclusion(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/ff9900/ffffff&text=ff9900)

    exclusion(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/ff66ff/000000&text=ff66ff)

###average
Обчислити середнє значення двох кольорів. Операція виконується по-канально (RGB).

Параметри:

* `color1`: Об’єкт кольору.
* `color2`: Об’єкт кольору.

Повертає: `колір`

Приклад:

    average(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/803300/ffffff&text=803300)

    average(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/994d1a/ffffff&text=994d1a)

    average(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/b36633/ffffff&text=b36633)

    average(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/cc804d/ffffff&text=cc804d)

    average(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/e69966/ffffff&text=e69966)

    average(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/ffb380/000000&text=ffb380)

    average(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/ff3300/ffffff&text=ff3300)

    average(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/80b300/ffffff&text=80b300)

    average(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/803380/ffffff&text=803380)

###negation
Протилежний до `difference` ефект. Результатом є яскравіший колір. 
**Примітка**: Під ефектом _opposite_ не розуміється ефект _інвертування_ як результат операції _додавання_.

Параметри:

* `color1`: Об’єкт кольору, ділене.
* `color2`: Об’єкт кольору, дільник.

Повертає: `колір`

Приклад:

    negation(#ff6600, #000000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/000000/ffffff&text=000000)
![Color 3](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)

    negation(#ff6600, #333333);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/333333/ffffff&text=333333)
![Color 3](http://placehold.it/100x40/cc9933/ffffff&text=cc9933)

    negation(#ff6600, #666666);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/666666/ffffff&text=666666)
![Color 3](http://placehold.it/100x40/99cc66/ffffff&text=99cc66)

    negation(#ff6600, #999999);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/999999/ffffff&text=999999)
![Color 3](http://placehold.it/100x40/66ff99/ffffff&text=66ff99)

    negation(#ff6600, #cccccc);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/cccccc/000000&text=cccccc)
![Color 3](http://placehold.it/100x40/33cccc/ffffff&text=33cccc)

    negation(#ff6600, #ffffff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ffffff/000000&text=ffffff)
![Color 3](http://placehold.it/100x40/0099ff/ffffff&text=0099ff)

    negation(#ff6600, #ff0000);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/ff0000/ffffff&text=ff0000)
![Color 3](http://placehold.it/100x40/006600/ffffff&text=006600)

    negation(#ff6600, #00ff00);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/00ff00/ffffff&text=00ff00)
![Color 3](http://placehold.it/100x40/ff9900/ffffff&text=ff9900)

    negation(#ff6600, #0000ff);

![Color 1](http://placehold.it/100x40/ff6600/ffffff&text=ff6600)
![Color 2](http://placehold.it/100x40/0000ff/ffffff&text=0000ff)
![Color 3](http://placehold.it/100x40/ff66ff/ffffff&text=ff66ff)
