Заметки
=======

Используется программатор-отладчик ST-Link V2 (клон с AliExpress).
ST-Link использует пины PA13 (SWDIO) и PA14 (SWCLK) для SWD (Serial Wire Debug). Этого достаточно для прошивки микроконтроллера, но не достаточно для отладки - необходимо еще одно соединение (SWO). В идеале еще нужен Reset для особых случаев - если выводы SWDIO, SWCLK сконфигурированы и используются в иных целях без ресета не прошьешь, но можно нажимать ресет вручную (главное вовремя отпустить).

Можно припаять новый вывод SWO через резистор к ноге МК программатора ST-Link ([видео](https://www.youtube.com/watch?v=iC2-0Md-6yg)).


STM32F103C8T6 Blue Pill
-----------------------

[Официальная страница на сайте ST.](https://www.st.com/content/st_com/en/products/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus/stm32-mainstream-mcus/stm32f1-series/stm32f103/stm32f103c8.html)

Полезные пины:
* PA13 - SWDIO
* PA14 - SWCLK
* PB3 - SWO
* PC13 - on-board LED

Встроенный светодиод сидит на выводе PC13, хардверно подтянут к питанию через резистор (инвертирует сигналы - при подачи напряжения гаснет, при замыкании на землю - горит), работает и в push-pull и в open-drain режимах.


STM32 ST-LINK Utility
---------------------

Много настроек, легко пользоваться, позволяет залить/скачать прошивку, разблокировать (unlock) флэш память. Но в целом не нужна.

Если при прошивке что-то пошло не так и МК не реагирует и не прошивается, скорей всего пины SWD сконфигурированы под другие нужды. Чтобы в этом случае прошить утилитой STM32 ST-LINK Utility в настройке указать Connect under reset, далее:
* зажать ресет на МК
* нажать Connect to target в программе
* отпустить ресет

То же самое можно провернуть и через openocd (но там нужно вовремя отпустить reset).

#### Обновление прошивки самого программатора  ST-Link V2
В ST-LINK Utility можно посмотреть версию прошивки программатора. Стоит обновить китайскую прошивку на последнюю официальную.


Open source version of the STMicroelectronics STlink Tools
----------------------------------------------------------

https://github.com/stlink-org/stlink

Есть возможность узнать Serial number программатора.


Openocd
=======

Запускает сервер с постоянной связью с программатором (в нашем случае ST-Link V2) в монопольном режиме (устройство будет недоступно для других приложений). Связываясь с сервером и отправляя ему команды можно считывать информацию из МК, прошивать его, перезагружать и т.д.

Установка
---------

Репозиторий: https://sourceforge.net/p/openocd/code/ci/master/tree/

Требуется для работы:
- make
- libtool
- pkg-config >= 0.23 (or compatible)

Требуется для компилирования из исходников:
- autoconf >= 2.64
- automake >= 1.14
- texinfo

Посмотреть версию установленной программы (в примере automake) можно командой:
```sh
apt-cache show automake | grep Version

```

Полезные команды на тему:
```sh
# расположение исполняемого файла
which [prog_name]

# узнать, что за программа
whatis [prog_name]
```

##### Установка
Из корневой папки репозитория выполнить:
```sh
./bootstrap
./configure [options]
make
sudo make install
```

Опции конфигурации можно посмотреть командой ```./configure --help```. В большинстве случаев ```./configure``` без опций походит.

##### Настройка разрешений:
Чтобы получать доступ к программатору без sudo прав нужно файл ```contrib/60-openocd.rules``` добавить в ```/etc/udev/rules.d```


Настройка и запуск сервера openocd
----------------------------------

Параметр hla_vid_pid в конфиге интерфейса (stlink.cfg) это пара значений (idVendor idProduct). Их можно увидеть, подключив программатор к компу и запустив команду lsusb.
```
Bus 005 Device 005: ID 0483:3748 STMicroelectronics ST-LINK/V2

где
idVendor = 0483
idProduct = 3748
```

Нужно заменить эти значения в файле на актуальные.

ST-Link со старой прошивкой может иметь нечитаемые ASCII символы в своем ID. При прошивке могут возникать ошибки.
Лечение: в файле tool-openocd\scripts\interface\stlink.cfg указать серийный номер в явном виде (раскоментировать строку)
```
hla_serial "\xaa\xbc\x6e\x06\x50\x75\xff\x55\x17\x42\x19\x3f"
```

Доступные варианты интерфейсов, протоколов и т.д. можно получить добавив к соответствующей команде опцию list
```sh
openocd -c "transport list"
```



##### Запуск openocd сервера

Простейшая команда:
```sh
openocd -f /usr/local/share/openocd/scripts/interface/stlink-v2.cfg -c "transport select hla_swd" -f /usr/local/share/openocd/scripts/target/stm32f1x.cfg
```

Мой вариант:
```sh
openocd -f /home/konstantin/STM32/stlink-v2-clone.cfg -f /usr/local/share/openocd/scripts/target/stm32f1x.cfg

# в клон стлинк-конфига дописано transport select hla_swd
```

##### Подключение к openocd серверу

```sh
telnet localhost 4444
```

Основные команды для сохранения текущей прошивки и заливки новой:
```sh
# перезагрузка и остановка работы МК (необходимо для манипуляций с прошивкой)
> reset init
# или (в мануале рекомендуется именно init)
> reset halt

# сохранить в файл текущую прошивку
> dump_image dump.bin 0x08000000 0x1ffff

# стирание старой прошивки (помогает избежать неожиданных багов в работе МК)
# и загрузка новой прошивки начиная с указанного адреса флеш памяти
> flash write_image erase maple_mini_boot.bin 0x08000000

# перезагрузка и запуск работы МК
> reset run

# отключиться от сервера
> exit

# завершить работу сервера
> shutdown
```

Еще команды:
```sh
# в некоторых МК несколько областей флеш памяти
# чтобы посмотреть список доступных областей флэша
> flash list

# подробней
> flash banks

# подробная информация по области флеша 0
> flash info 0

# приостановка работы МК (например для считывания данных регистров)
> halt

# возобновление работы МК (с момента остановки)
> resume

# прочитать значение регистров МК
> reg
```

Подробней про команды манипуляции с прошивкой: http://openocd.org/doc/html/Flash-Commands.html

##### Разовая прошивка с помошью openocd

OpenOCD команда запуска сервера из platformio (разовая прошивка):
```sh
openocd -d2 -s C:\Users\Konstantin\.platformio\packages\tool-openocd/scripts -f interface/stlink.cfg -c "transport select hla_swd" -f target/stm32f1x.cfg -c "reset_config none" -c "program {.pio\build\genericSTM32F103C8\firmware.elf} verify reset; shutdown;"

# где
# OpenOCD ==  запуск сервера отладки со следующими параметрами
# -d2  	==  подробность вывода дебаг-информации уровень 2
# -s C:\Users\Konstantin\.platformio\packages\tool-openocd/scripts  ==  указываем путь до папки с конфигами
# -f interface/stlink.cfg  		==  конфиг интерфейса
# -c "transport select hla_swd"  	==  выбор типа подключения
# -f target/stm32f1x.cfg  		==  конфиг микроконтроллера
# -c "reset_config none"  		==  установка режима вывода лапки reset программатора (none, значит не используется)
# -c "program {.pio\build\genericSTM32F103C8\firmware.elf} verify reset;  	==  загрузка прошивки, проверка, перезапуск МК (софтверный)
# shutdown;"  	==  прекращение работы сервера
```

Команда-скрипт [program](http://openocd.org/doc/html/Flash-Programming.html "Flash-Programming") упрощает разовую загрузку прошивки:
```sh
program filename [preverify] [verify] [reset] [exit] [offset]

# где
# filename  - путь до файла с прошивкой (bin, hex, elf)
# preverify (опционально) - проверка на совпадение прошивок (не прошивает если уже та же прошивка залита)
# verify (опционально) - проверка прошивки после загрузки
# reset (опционально) - неперезапуск МК после прошивки (софрверный??)
# exit (опционально) - прекращение работы openocd сервера после прошивки
# offset (опционально) - отступ от начала памяти МК (обычно 0x08000000)
```

Исходя из этого можно записать простую команду для разовой прошивки:
```sh
openocd -d2 -f /usr/local/share/openocd/scripts/interface/stlink-v2.cfg -c "transport select hla_swd" -f /usr/local/share/openocd/scripts/target/stm32f1x.cfg -c "reset_config none" -c "program {path/to/firmware.elf} verify reset exit"
```

##### Прошивка МК с несконфигурированными выводами SWD

Если ноги SWD интерфейса сконфигурированы под другие нужды, то зажать reset на МК и держать до ввода команды reset init (или reset halt), отпустить сразу перед нажатием на enter или одновременно.

#### Прошивка с помощью ESP-Prog (на чипе FTDI FT2232H) через JTAG разъем

[Инструкция](https://www.allaboutcircuits.com/technical-articles/getting-started-with-openocd-using-ft2232h-adapter-for-swd-debugging/).

Конфиг интерфейса ESP-Prog можно получить, установив [тулы ESP-IDF для дебага ESP](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html). Нужный файл можно найти командой:
```sh
# по умолчанию все файлы ESP-IDF устанавливаются в ~/.espressif
# нам нужет файл с именем вида esp32_devkitj_v1.cfg
find ~/.espressif/ -iname "*esp32_devkitj*"
# пример:
# ~/.espressif/tools/openocd-esp32/v0.10.0-esp32-20200420/openocd-esp32/share/openocd/scripts/interface/ftdi/esp32_devkitj_v1.cfg
```

Чтобы использовать SWD для отладки через JTAG разъем нужно добавить в конфиг команду "transport select swd":
```sh
# #
# # Driver for the FT2232H JTAG chip on the Espressif DevkitJ board
# # (and most other FT2232H and FT232H based boards)
# #


adapter driver ftdi
ftdi_vid_pid 0x0403 0x6010 0x0403 0x6014

# interface 1 is the uart
ftdi_channel 0

transport select swd

# TCK, TDI, TDO, TMS: ADBUS0-3
# LEDs: ACBUS4-7

ftdi_layout_init 0x0008 0xf00b
ftdi_layout_signal LED -data 0x1000
ftdi_layout_signal LED2 -data 0x2000
ftdi_layout_signal LED3 -data 0x4000
ftdi_layout_signal LED4 -data 0x8000

ftdi_layout_signal SWD_EN -data 0

# ESP32 series chips do not have a TRST input, and the SRST line is connected
# to the EN pin.
# The target code doesn't handle SRST reset properly yet, so this is
# commented out:
# ftdi_layout_signal nSRST -oe 0x0020

reset_config none

# The speed of the JTAG interface, in KHz. If you get DSR/DIR errors (and they
# do not relate to OpenOCD trying to read from a memory range without physical
# memory being present there), you can try lowering this.
#
# On DevKit-J, this can go as high as 20MHz if CPU frequency is 80MHz, or 26MHz
# if CPU frequency is 160MHz or 240MHz.

# speed in kHz
adapter speed 20000


```

Чтобы получить доступ к устройству без sudo прав нужно установить соответствующие udev rules, например от PlatformIO [инструкция здесь](https://docs.platformio.org/en/latest/faq.html#faq-udev-rules), сам файл [здесь](https://raw.githubusercontent.com/platformio/platformio-core/master/scripts/99-platformio-udev.rules).

##### Подключение SWD через разъем JTAG

**Не забываем переключить джаймпер питания в положение 3.3 В**
```
---------------------
|TMS TCK TDO TDI ???|
|Vcc Gnd Gnd Gnd Gnd|
--------     --------

ESP-Prog            STM32
Vcc --------------- Vcc
GND --------------- GND
TCK --------------- SWDCLK
TDO --------------- SWDIO
TDI --- 470 Ohm --- SWDIO

```
Подключать TDI - SWDIO нужно через резистор 220-470 Ом.

#### Eclipse OpenOCD plug-in

[Официальная страничка и настройки](https://gnu-mcu-eclipse.github.io/debug/openocd/). Внимание! На страничке есть ссылки на старые (запрещенные к использованию) версии некоторых пакетов.

Prerequisites:
* snap
* [Node.js version >= 8.0](https://github.com/nodesource/distributions/blob/master/README.md#snapinstall)
* [xpm (xPack package manager)(https://www.npmjs.com/package/xpm)
* [The xPack OpenOCD](https://github.com/xpack-dev-tools/openocd-xpack)https://xpack.github.io/openocd/install/

Есть [старая версия xPack OpenOCD](https://github.com/xpack-dev-tools/openocd), которая [запрещена](https://gnu-mcu-eclipse.github.io/openocd/install/) к использованию. Не путать проекты!!


Prerequisites simple installation:
```sh
# Node.js install

# channel - number of major version of Node.js
sudo snap install node --classic --channel=10


# xpm install

# by default xpm installation folder is /usr/local/bin/xpm
# set environment for xpm to be installed to ${HOME}/opt/npm/xpm (recommended)
mkdir -p "${HOME}"/opt/npm
npm config set prefix "${HOME}"/opt/npm
echo 'export PATH="${HOME}"/opt/npm/bin:${PATH}' >> "${HOME}"/.profile
source "${HOME}"/.profile
# install
npm install --global xpm


# OpenOCD xPack install

xpm install --global @xpack-dev-tools/openocd@latest
# the binary will be around here:
# ~/opt/xPacks/@gnu-mcu-eclipse/openocd/0.10.0-12.1/.content/bin/
# and scripts:
# /home/konstantin/opt/xPacks/@gnu-mcu-eclipse/openocd/0.10.0-12.1/.content/scripts/


```

Затем установить плагин ARM GNU for Eclipse через Eclipse Marketplace.

В конфиге stm32f1x.cfg найти ```set _CPUTAPID 0x3ba00477``` и заменить на ```set _CPUTAPID 0x4ba00477``` для поддержки китайского клона CKS32F103C8T6.

##### GDB

Найти бинарник arm-none-eabi-gdb и поставить вместо gdb по умолчанию в конфиге дебаггера.
```sh
locate arm-none-eabi-gdb | grep arm-none-eabi-gdb$
# output:
# /opt/st/stm32cubeide_1.3.0/plugins/com.st.stm32cube.ide.mcu.externaltools.gnu-tools-for-stm32.7-2018-q2-update.linux64_1.0.0.201904181610/tools/bin/arm-none-eabi-gdb
```


------------------------------------------------------------------

STM32CubeIDE
============

К сожалению встроенный GDB сервер отладки "из коробки" не заработал с китайским STLink'ом.

##### Дебаг через внешний openocd сервер

после запуска openocd сервера в настройках CubeIDE->Debugg Configurations->Debugger установить:
* Connect to remote GDB server
* Host name = localhost
* Port number = 3333
* Debug Probe = ST-LINK (OpenOCD)

##### Базовая конфигурация Blue Pill STM32F103C8T6 в CubeMX
RCC :	установить High Speed Clock (HSC) = Crystal чтобы включить тактирование с внешнего резонатора (8 МГц), установленного на плате.
		Low Speed Clock  используется для часов реального времени. Это не обязательно, т.к. все основные функции времени (в т.ч. RTOS) используют sysclk вместо RTC.
SYS :	установить Debug = Serial Wire при подключении через 2 провода (SWCLK, SWDIO) или Trace Asynchronous Sw при наличии SWO контакта.
Clock Configuration : установить требуемую тактовую частоту тактирования SYSCLK (MHz) и нажать Enter - программа сама подберет коэффициенты множителей/делителей.


##### Serial соединение для прослушивания порта в линуксе:

Программа **screen**:
```sh
screen /dev/ttyUSB0 115200

# отключиться от сессии (сама сессия не прерывается)
Ctrl+A, затем D

# закрыть сессию
Ctrl+A, затем K, затем Y

# повторно подключиться к сессии
screen -r
```


Программа **putty**:
```sh
putty /dev/ttyUSB0 -serial -sercfg 115200,8,n,1,N &
```

Параметры указываются после флага -sercfg через запятую в любом порядке:
* 115200 - baud rate,
* 8 - bits,
* 1 - stop bit,
* n - parity (буква в нижнем регистре),
* N - flow control (буква в верхнем регистре)

Программа открывает свое окно, поэтому, чтобы не занимать терминал добавляем в конце ```&``` (выполнение в background).

Подробнее про параметры: ```man putty``` (в самом конце).


Работа со строками
------------------

Программируя STM32 придется работать со строками в стиле Си (не Си++). При этом никаких проверок выхода индекса за размер строки нет. Вся работа с памятью делается вручную. Основные команды:

Длина строки:
```C
strlen (const char *)
```

Дописать строку str2 в конец строки str1:
```C
char *strcpy (char *str1, const char *str2);
```

Дописать n символов из начала строки str2 в конец строки str1:
```C
char *strncpy (char *str1, const char *str2, size_t n);
```

TODO: добавить про аллокацию памяти под строки.

Числа в строки. Вручную:

```C
char * toArray(int number)
{
    int n = log10(number) + 1;
    int i;
  char *numberArray = calloc(n, sizeof(char));
    for ( i = 0; i < n; ++i, number /= 10 )
    {
        numberArray[i] = number % 10;
    }
    return numberArray;
}
```

##### Готовая функция форматирования:
```C
sprintf(yourCharArray, "%ld", longIntNumber);
sprintf(yourCharArray, "%.2Lf", longDoubleNumber); // .2 - точность
// в yourCharArray должно быть достаточно места!!
//Иначе будет запись в неизвестное место в памяти без предупреждений.
```

или просто перевод числа в строку (не работает):
```C
itoa(number, yourCharArray, numberBase);
```

Форматирование вывода через UART и SWO
--------------------------------------

Вывод через UART с помощью функции printf() для тулчейнов на базе gcc (для сборок на основе других компиляторов см. документ [AN4989 Application note](https://www.st.com/resource/en/application_note/dm00354244-stm32-microcontroller-debug-toolbox-stmicroelectronics.pdf "AN4989. Application note. PDF"), стр. 58). Подходит для тулчейнов STM32CubeIDE.

Форматирование и вывод 2 в 1!

```C
#include "stdio.h"
// подключаем библиотеку с функцией printf()
// в CubeIDE подключена по умолчанию


// переопределяем функцию вывода 1 символа
int __io_putchar(int ch)
{
	HAL_UART_Transmit(&UartHandle, (uint8_t *)&ch, 1, 0xFFFF);
	return ch;
}

// опционально переопределить функцию _write(), чтобы syscall.c не подключился к проекту
// экономит 0,48 КБ флэш памяти
int _write(int file, char *ptr, int len)
{
int DataIdx;
 for (DataIdx = 0; DataIdx < len; DataIdx++){ __io_putchar( *ptr++ );}
 return len;
}
```

Вывод через SWO

Unit Tests
----------

Предупреждения (Warning) на стадии компиляции могут стать ошибками на стадии выполнения. Для максимального исключения всех возможных ошибок можно использовать флаги компилирования: -Wall -Wextra -Wshadow -Wpedantic -Werror

Для написания тестов в стандартной библиотке переферии ST есть специальный макрос:
```C
#define assert_param(expr) ((expr) ? (void)0 : assert_failed((uint8_t *)__FILE__, __LINE__))
```
Где expr - выражение, которое необходимо проверить на истинность. Если это утверждение (assert) не проходит, срабатывает функция assert_failed(), определение которой подправить в конце файла main.c (по умолчанию тело функции пустое):
```c
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     tex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

Таже в стандартной библиотеке Си assert.h есть макросы:
```C
// макро отключается удалением определения USE_FULL_ASSERT
assert_param(expr);

// макрос отключается определением NDEBUG
assert(expr);
```

Timer interrypt
---------------

Настройка в CubeMX
* В настройках выбранного таймера нужно включить тактирование таймера
* На схеме тактирования Clock Configuration проверить/настроить частоту соответствующей группы таймеров (APBx Timer Clocks).
* Далее в настройках таймера поставить делитель частоты (prescaler),
* период счета в тиках (Count period Autoreload),
* а в Trigger Event Selection поставить Update Event (обычное прерывание по таймеру).

Настройка в коде.
* Таймер уже проинициализарован, спасибо CubeMX.
* Запускаем таймер структурой htimX (где Х - номер таймера), созданной CubeMX.
* Запускаем обработку прерываний таймера.
```C
// пример для таймера TIM3
HAL_TIM_Base_Start(&htim3);     // запускает счет таймера TIM3
LL_TIM_ClearFlag_UPDATE(TIM3);  // сбрасывает флаг прерывания (при перезагрузке МК он стоит)
HAL_TIM_Base_Start_IT(&htim3);  // включает обработку прерываний по таймеру и (!!) запускает счет

HAL_TIM_Base_Stop(&htim3);  // останавливает счет таймера TIM3
TIM3->CNT = 0;              // обнуляет счетчик таймера

// если нужно включить прерывания, не запуская сам таймер:
__HAL_TIM_ENABLE_IT(&htim3, TIM_IT_UPDATE);  // включает обработку прерываний типа Event Update на таймере TIM3

/* типы прерываний (__INTERRUPT__):
TIM_IT_UPDATE: Update interrupt
TIM_IT_CC1: Capture/Compare 1 interrupt
TIM_IT_CC2: Capture/Compare 2 interrupt
TIM_IT_CC3: Capture/Compare 3 interrupt
TIM_IT_CC4: Capture/Compare 4 interrupt
TIM_IT_COM: Commutation interrupt
TIM_IT_TRIGGER: Trigger interrupt
TIM_IT_BREAK: Break interrupt
*/
```

После настройки в  CubeMX прерывания по таймеру и гнерации кода, в файле Core/Src/stm32f1xx_it.c появится обработчик прерывания. Свой код можно вставить как до обработки прерывания (снятие флага, обнуление счетчика и т.д.), так и после:

```c
void TIM3_IRQHandler(void)
{
  /* USER CODE BEGIN TIM3_IRQn 0 */

  /* USER CODE END TIM3_IRQn 0 */
  HAL_TIM_IRQHandler(&htim3);
  /* USER CODE BEGIN TIM3_IRQn 1 */

  /* USER CODE END TIM3_IRQn 1 */
}
```

##### Get system clock frequency
```c
// какие еще есть сокращения
// посмотреть на вкладке Clock в CubeMX
printf("SYSCLK=%d\n", HAL_RCC_GetSysClockFreq());
printf("HCLK=%d\n", HAL_RCC_GetHCLKFreq());
printf("APB1=%d\n", HAL_RCC_GetPCLK1Freq());
printf("APB2=%d\n", HAL_RCC_GetPCLK2Freq());
```

### Добавление сторонней библиотеки
Как добавить стороннюю библиотеку в проект STM32CubeIDE

Project > Properties > C/C++ Build > Settings > Tool Settings > MCU GCC Linker > Libraries add the .a library name and the path to that file

### Разное

```
// Turn on timer and counting
TIM2->DIER |= TIM_DIER_TIE;
TIM2->DIER |= TIM_DIER_UIE;
TIM2->CR1 = TIM_CR1_CEN;
```


##### HARDWARE

SPI : Transmit Only Master, 4 Baud Rate, 4 Bits, MSB First

GPIO : Level Low, Output Push Pull, Speed Low, No pull-up and no pull-down



##### Работа с дисплеем ST7789
MAIN.H DEFINES
```
/* Private defines -----------------------------------------------------------*/
/* USER CODE BEGIN Private defines */
#define BLK_Pin GPIO_PIN_2
#define BLK_GPIO_Port GPIOA
#define DC_Pin GPIO_PIN_3
#define DC_GPIO_Port GPIOA
#define RES_Pin GPIO_PIN_4
#define RES_GPIO_Port GPIOA
/* USER CODE END Private defines */
```

st7789.h / font5x7.h / font7x11.h Includes
```
// change to your controller hal library header file
#include "stm32f1xx_hal.h"
```

st7789.c Includes
```
// Include GPIO declaration
#include "main.h"
```

MAIN Init
```
// Turn on backlight
HAL_GPIO_WritePin(dispBLK_GPIO_Port, dispBLK_Pin, GPIO_PIN_SET);
// Display initialisation
ST7789_Init(240, 240);
```
