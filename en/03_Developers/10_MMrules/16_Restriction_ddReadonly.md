###Виджет для плагина ManagerManager, позволяющий сделать поля документа и TV доступными только для чтения 

*значения отображаются при редактировании, но их невозможно изменить.*

mm_ddReadonly(string $fields, string $roles, string $templates);

####Описание параметров

Название|Описание|Допустимые значения|Значение по умолчанию|Обязателен?
--------|--------|-------------------|---------------------|-----------
fields|Поля документа (или TV), которые необходимо сделать доступными только для чтения.|{comma separated string}|—|true
roles|Роли, для которых необходимо применить виждет, пустое значение — все роли.|{comma separated string}|—|false
templates|Id шаблонов, для которых необходимо применить виджет, пустое значение — все шаблоны.|{comma separated string}|—|false


####Пояснение
Довольно редко, но иногда бывают случаи, когда мы храним какую-то информацию о документе в его TV (например, количество просмотров или скачиваний, рейтинг и т.д.). Такая информация обновляется автоматически (какой-нибудь сниппет/плагин просто сохраняет значение в TV соответствующего документа). И вот Петя вдруг решил отредактировать текст документа: открыл, начал писать, его отвлекли по работе, потом позвонили, потом срочно пришлось уехать, два часа ездил, вернулся, продолжил редактировать. Всё это время документ у него был открыт, данные о количестве скачиваний уже 100 раз успели поменяться (было 3, а стало 33), но у Пети до сих пор отображается 3. Петя закончил свою работу, нажимает «Сохранить» и… количество скачиваний перезаписывается на 3! Неприятная ситуация. Что можно сделать? Вариант 1: можно сделать какую-нибудь супер-системную группу и выставить её для тех TV, значения которых не должны редактироваться пользователями. Неплохой вариант, но не надёжный (может найтись кто-то, кто поставит себе эту группу и обязательно что-то испортит) и не подходит, если хочется видеть эти данные при редактировании документа. Именно для таких случаев и предназначен этот виджет.
Работа виджета делится на 3 части:
Перед сохранением документа (OnBeforeDocFormSave) получаются и запоминаются актуальные значения необходимых полей (из базы).
После сохранения (OnDocFormSave) записываются обратно.
JS делает поля визуально не редактируемыми.