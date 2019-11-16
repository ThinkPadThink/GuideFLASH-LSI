# Гайд по перепрошивке RAID контроллера Dell PERC H310 в HBA режим (он же IT Mode)

![Фотка H310](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/h310.jpg?raw=true)
*Данный гайд является переводом другого гайда [оригинальный гайд](https://tylermade.net/2017/06/27/how-to-crossflash-perc-h310-to-it-mode-lsi-9211-8i-firmware-hba-for-freenas-unraid/)*

*__При копировании информации обязательно указывайте оригинальный источник.__* 

# Для чего это нужно?

Например у вас есть ненужный контроллер и вы хотите его использовать для простого подключения дисков или для Soft RAID (ZFS, LVM, Storage Spaces и т.п.) или просто для подключения SAS/SATA дисков.

## Что для этого нужно?
* SAS контроллер
* USB флешка, подойдёт любая, например на 1 гб.

**ВНИМАНИЕ, ЧТО ВЫ ДЕЛАЕТЕ СО СВОИМ КОНТРОЛЛЕРОМ, ВЫ ДЕЛАЕТЕ НА СВОЙ СТРАХ И РИСК!!**

# Подготовка

1) Скачиваете [Rufus](https://rufus.ie), [FreeDOS Boot Floppy](http://www.freedos.org/download/download/FD12FLOPPY.zip) и архив [LSI9211-8i](https://www.mediafire.com/file/6mtie10d9ud6675/LSI-9211-8i.zip/file);
2) Форматируете через Rufus флешку в FreeDOS как на фото ниже;
![Фотка руфус](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/rufus.jpg?raw=true)
3) Распаковываете архив с FreeDOS Boot Floppy на флешку с заменой, удаляете файл SETUP.BAT;
4) Распаковываете архив с LSI9211-8i на флешку с заменой;
5) Выключаете пк, устанавливаете контроллер и запускаете пк с загрузкой в FreeDOS;
6) Вводите комманду **megacli.exe -AdpAllInfo -aAll -page 20** через клавишу *Enter* прокручиваете до раздела  *HW Configuration* и запоминаете/фотографируете/записываете *SAS Address* как на фото, **в ином случае при дальнейших действиях вы получите кирпич заместо контроллера**;
![Фотка SAS Address](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/SAS%20Address.jpg?raw=true)
7) Вводите **megarec.exe -writesbr 0 sbrempty.bin** перед перепрошивкой;
![Фотка empty](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/empty.jpg?raw=true)
8) Вводите **megarec.exe -cleanflash 0** для зачистки прошивки и перезагрузите пк с последующей загрузкой в FreeDOS, для перезагрузки используйте комманду reboot
9) После перезагрузки вводите **sas2flsh.exe -o -f 6GBPSAS.fw**;
10) Вводите **s2fp19.exe -o -sasadd %ВАШ SAS Address%** %ВАШ SAS Address% - то что вы записали в 6 пункте, у меня же это **s2fp19.exe -o -sasadd 5с81f660dbbb1a00**;
![Фотка SAS Address](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/sasadd.jpg?raw=true)
11) Перезагрузите пк с последующей загрузкой в FreeDOS, для перезагрузки используйте комманду reboot;
12) После перезагрузки вводите **sas2flsh.exe -o -f 2118it.bin**, при прошивке вам будет задан вопрос: *Would you like to flash anyways? *, вводите y и нажмите *Enter*;
![Фотка SAS Address](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/2118it.jpg?raw=true)
![Фотка SAS Address](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/yes.jpg?raw=true)
13) Всё! перезагружайтесь в Windows/Linux и у вас он будет отображаться как надо в Windows: Dell HBA 6 Gbit/s, в Linux как-то так же. 


**PS** Данный гайд в теории так же подходит для контроллера IBM M1015 и оригинального LSI MegaRAID 9240
![M1015](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/ibm%20m1015.jpg?raw=true)
![9240](https://github.com/ThinkPadThink/GuideFLASH-LSI/blob/master/9240.jpg?raw=true)
