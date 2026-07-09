layout/theme.liquid - Главный Layout всей темы. Формирует HTML-документ, подключает CSS/JS, выводит {{ content_for_layout }} и является точкой входа для рендера всех страниц.

templates/index.json - дефолтный основной шаблон для страницы.

sections/header.liquid - Представляет самостоятельную Section, потому что пользователь может управлять ею через Theme Editor. Помимо HTML содержит Schema, определяющую настройки.

snippets/icon-search.liquid - переиспользуемый кусок кода (форма поиска по сайту)

sections/footer-group.json - описание структуры и настроек для футер

sections/image-banner.liquid - Реализует Hero Banner как независимую Section, которую можно добавить, удалить или настроить через Theme Editor. Содержит Liquid-разметку и Schema для настроек.

templates/product.json - дефолтный шаблон отображения страницы продукта.

sections/main-product.liquid - дефолтный главный шаблон отображение отсновной секции для страницы продукта

snippets/card-product.liquid - повторяющийся код для отображения карточки продукта. Принимает входящие параметры.