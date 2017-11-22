# vetis.api #

1С доступ к ГИС Меркурий через Ветис.API http://api.vetrf.ru

Код основан на исходниках:
- http://infostart.ru/public/560823/
- http://vetrf.ru/vetrf-forum/posts/list/6955.page

## Версии ##
- Ветис.API: v1.4, 2.0
- 1C: v8.3.10
- мобильная 1C: v8.3.10

## Термины ##
- сервис - ГИС Меркурий

## Особенности ##

Код для доступа к Ветис.API:
- сейчас упор и тестирование будет делаться в основном на версию 2.0, версия 1.4 будет добавляться в коде без проверки работоспостобности пока 2.0 не заработает полностью
- не зависит от конфигурации
- расположен в общих модулях, имена соответствуют пространствам имен Ветис.API, имена с суффиксом "_2_0" работают только с версией 2.0, остальные - 1.4
- используется только часть API для оптовой торговли замороженным мясом (не будет производства, живых животных, молока и пр.)

Код привязанный к метаданным 1С:
- ориентирован на УТ 10.3
- конвертация значений находится в общих модулях (имена соответствуют пространствам имен Ветис.API с суффиксом "Слой1с")
- партионный учет не используется, остатки партий сервиса списываются в ручном режиме выбором в документе ВетисТранспортнаяПартия

Метаданные:
- могут использоваться как самостоятельная конфигурация (как вариант конвертацией из основной базы выгрузить справочники)
- реквизиты ВСД для упрощения работы с метаданными сгруппированы в табличные части (Товары, ВСД v1.4, Разрешение v1.4), в ТЧ всегда одна строка
- остатки партий хранятся в регистре сведений ВетисСкладскойЖурналРС, движений по этому регистру нет, регистр заполняется из сервиса, эти остатки используются только для транспотрных партий

Сопоставление справочников (<в метаданных> - <в Ветис.API> - <типовая УТ 10.3>):
- Контрагенты - BusinessEntity - Контрагенты;
- ВетисПредприятия - Enterprise - возможно типовой Склады, но нужны еще склады контрагентов;
- ВетисPackingForm - PackingForm - возможно как то можно использовать ЕдиницыИзмерения, но, скорее всего, это отдельный справочник;
- ВетисPurpose - Purpose - отдельный;
- КлассификаторЕдиницИзмерения - Unit - КлассификаторЕдиницИзмерения;
- КлассификаторСтранМира - Country - КлассификаторСтранМира;
- НоменклатурныеГруппы - SubProduct - близок к типовому НоменклатурныеГруппы;
- Номенклатура - ProductItem - близок к типовому Номенклатура;
- ВетисTransport - Transport - скорее всего, это отдельный справочник;
- Пользователи - User - Пользователи

Справочники Контрагенты, ВетисПредприятия, Номенклатура, НоменклатурныеГруппы, КлассификаторЕдиницИзмерения, КлассификаторСтранМира, ВетисPackingForm, ВетисPurpose, ВетисRegion, ВетисDistrict, ВетисLocality, ВетисIncorporationForm, ВетисRegionalizationCondition, ВетисAnimalDisease необходимо сопоставить с соответствующими справочниками Меркурия.
Для справочников Контрагенты, ВетисПредприятия, Номенклатура, НоменклатурныеГруппы, КлассификаторЕдиницИзмерения, КлассификаторСтранМира для сопоставления надо пользоваться обработкой ВетисУстановкаСоответствияСправочников.
Справочники ВетисPackingForm, ВетисPurpose, ВетисRegion, ВетисDistrict, ВетисLocality, ВетисIncorporationForm, ВетисRegionalizationCondition, ВетисAnimalDisease заполняются из сервиса в своих формах списка по кнопке "Заполнить".
Справочник ВетисTransport сопоставлять не надо, заполняется вручную.
Для всех справочников соответствие хранится в регистре сведений ВетисСоответствие.

## Объединение с УТ 10.3 ##
 - при повторном объединении в настройках указать "Разрешить удаление объектов основной конфигурации"
 - снять все флажки
 - выбрать Действия/Отметить по подсистемам файла
 - поставить флажок на подсистеме "Ветис" и нажать кнопку "Установить"
 - проверить все выбранные объекты, включить удаленные из поставки
 - нажать "Выполнить" / "Продолжить"
 - в процедуру ПередНачаломРаботыСистемы вставить текст ВетисВызовСервера.Инициализировать();

## Настройка ##
 будет описана позже

## Контакты ##

skype: mevgenym
email: mevgenym@mail.ru
telegram: https://t.me/mevgenym

## Лицензия ##

Данное программное обеспечение распространяется на условиях GNU General Public License v3.0.
