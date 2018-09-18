FAQ 
===

##### Основные сокращения

**ATLAS** = A Toroidal LHC ApparatuS. Название эксперимента на LHC и большого детектора, состоящего из нескольких слоев (для измерения разных величин и детектирования частиц разного типа). [Источник.](https://ru.wikipedia.org/wiki/Эксперимент_ATLAS)

**IBL** = Insertable B-Layer. Новый слой детекторов для ATLAS, установленный на расстоянии ~3.2 см от оси пучка, между остальными детекторами и новой трубой с пучком (меньшего радиуса). Зачем? - для увеличения плотности пикселей (пространственного позиционирования), также новые детекторы сильнее защищены от повышенного (относительно предыдущего эксперимента) уровня излучений (т.к. расстояние до пучка меньше, плюс планируется увеличить мощность пучка). [Источник.](https://arxiv.org/abs/1012.2742)

**Варианты сенсоров:**

* Double Chip Planar sensor.
* Single Chip 3D sensor CNM. (SC CNM)
* Single Chip 3D sensor FBK. (SC FBK)

**CNM** = Centro Nacional de Microelectronica. IBM-CNM. Институт микроэлектроники Барселоны (Испания). [Источник 1.](https://www.researchgate.net/publication/324612183_Small_pitch_3D_devices) [Источник 2 (pdf).](http://www-hep.phys.sinica.edu.tw/~hstd8/Slides/1206_Grinstein_sgrinstein_HSTD8_2011.pdf) [Сайт института.](http://www.cnm.es/)

**FBK** = Fondazione Bruno Kessler. Топовый НИИ Италии (г. Тренто). Источник 1 и 2 - см. **CNM**. [Сайт института.](https://www.fbk.eu/en/)

**FE** = Font End Chip (readout electronic).
 
**FE-I3** = Старый вариант детектора. Размеры: 7.6х10.8 мм (18 рядов по 160 пикселей).

**FE-I4A** = Новый вариант детектора. Размеры: 20.2х18.8 мм (80 рядов по 336 пикселей).

**Double Chip Planar** = Новый вариант детектора. Размеры: 41.4х18.8 мм.

-----------------------------

[Следующая информация взята из презентации (pdf)](https://indico.cern.ch/event/379103/contributions/902054/attachments/756211/1037368/USBpix_Hardware.pdf)

**MIO** = Multi-IO Board. Плата с USB интерфейсом и программируемым FPGA микроконтроллером, который проводит все тесты, управляет чипами c пиксельными детекторами в соответствии с записанной микропрограммой.

**MIO2** = Старая версия с USB 2.0

**MIO3** = Новая версия с USB 3.0 на других чипах. Больше памяти FPGA.

**BIC** = IBL Module Burn In Card. Позволяет подключать к MultiIOBoard до четырех чипов FE-I4 

**FE-I4** Adapter Card = Позволяет подключать к MultiIOBoard один чип FE-I4 

**MMC** = Multi-chip Module Card. Альтернатива плате MultiIOBoard. Позволяет напрямую подключать 16 и более чипов FE-I4.

-----------------------------

##### Информация по чипам

Чип - контроллер  USB в плате MIO2: **CY7C68013A**.

**EEPROM** = Electrically Erasable Programmable Read-Only Memory. Электрически стираемое перепрограммируемое ПЗУ. ЭнергоНЕзависимая память чипа CY7C1061AV33-10ZXC в плате MIO2 (версии 1.03 12/2008) для хранения прошивки USB контроллера. [Спецификации чипа](http://www.cypress.com/part/cy7c1061av33-10zxc),  [Описание чипа (pdf).](http://www.cypress.com/files/cy7c1061av33-16-mbit-1m-x-16-static-rampdf)

**Прошивка (uC Firmware)** для EEPROM лежит в папке USBpix/config: 
* файлы **usbpixi3.hex** и **usbpixi4.hex** для платы **MIO2**; 
* файл **SILABFX3.img** для платы **MIO3**.

**EZ-USB FX3 Software Development Kit** = программа фирмы Cypress для прошивки некоторых контроллеров этой фирмы. Подходит для MIO3, можно использовать под linux. Аналогичной для MIO2 нет. [Источник (см. раздел "uC Firmware")](https://twiki.cern.ch/twiki/bin/view/Main/AtlasEdinburghGroupHardwareUpgradeDocumentationModTest)

**SiUSBman** = программа для прошивки (**только для Windows**) USB-контроллера платы MIO2, часть программного пакета USBpix. Компилируется автоматически вместе с STcontrol только если установлен **Qwt** и настроена переменная окружения **QWTDIR**.

**FPGA** = Field-Programmable Gate Array. Программируемая логическая интегральная схема (ПЛИС), конфигурация которой может быть загружена после включения питания. ЭнергоЗависимая память чипа Xilinx Spartan3 FPGA - XC3S1000 FGG320 4C платы MIO2 (версии 1.03 12/2008), для хранения программы-алгоритма тестирования пиксельного (одного или нескольких по очереди) детектора. Микропрограмма (файл вида usbpixi4.bit) загружается автоматически в процессе инициализации контроллера в программе STcontrol.

**Микропрограмма (FPGA configuration)** лежит в папке USBpix/config: 
* файлы usbpixi3.bit и usbpixi4.bit для платы MIO2; 
* файл mmc3.bit для платы MMC3; 
* файл mio3.bit для платы MIO3.

**Очевидно**, что прошивки и микропрограммы могут отличаться в зависимости от установленной версии STcontrol. Информации о совместимости различных версий плат MIO, MMC, и т.д. с различными версиями программы STcontrol нет.

----------------------------------------------------

##### Инструкция по прошивке MIO2 и MIO3 контроллеров 
Взято с http://icwiki.physik.uni-bonn.de/twiki/bin/view/Systems/UsbPix:

**MIO2 (USBpix2) system:** use SiUSBman as provided with the USBpix code. Flashing the microcontroller is currently supported only under Microsoft Windows. Perform the following steps:

* Connect the USBpix-board, LED6 should turn green.
* Open "SiUSBman.exe" in the bin-folder or use the Start Menu entry. The MultiIOBoard should appear in the list.
* In the "Options" menu select "Expert mode".
* Select the "USB firmware" tab in the lower part of the window.
* Click on the file browser button ("...") next to the "Flash EEPROM" button and browse to "USBPix/host/tags/release-[x.y]/config" and select the file "usbpix.hex".
* Press the "Flash EEPROM" button. IMPORTANT: Be patient! Do not touch the system while performing the update!
* Press the switch "S1" on the MultiIOBoard to restart the microcontroller.
* The microcontroller will now boot up with the new firmware. The MultiIOBoard should appear in the list showing the new firmware version.

**MIO3 or MMC3 (USBpix3) system:** use programmes as provided by Cypress. Perform the following steps:

* Silab image should not yet be loaded (USB vendor ID will be 04b4 instead of 5312); otherwise device needs to be reset, consult an expert.
* Windows: CYUSB3 driver usually self-installs. Start CyControl from EZ-USB FX3 SDK
* Linux: start cyusb_linux
* Upload image: Program → FX3 → RAM and upload SILABFX3.img as provided with board delivery.
* Check with device manager (windows) or lsusb (linux) that vendor ID is now 5312.
* Important notes:
* Image will disappear from RAM after each power cycle.
* Permanent storage possible by loading image into SPI flash – since image is not yet final, we do not recommend using this option yet.

----------------------------------------------------


