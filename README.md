Домашнее задание ШРИ "Инфраструктура"

🟥 Задание:
1. Настройте линтер для соответствия сообщений о коммитах формату conventional commits

🟩 Решение:
Был использован commitlint, коммит невозможно создать, пока он не будет соответствовать нужному формату

🔳🔳🔳🔳🔳🔳🔳🔳🔳🔳

🟥 Задание:

2. Настройте автоматический запуск проверок в CI для пулл реквестов

🟩 Решение: 
В файле .github/workflows/ci.yml описана работа build-and-test, которая запускается только при пулл-реквесте в master. В ней устанавливаются зависимости и браузеры для playwright, далее происходят юнит и e2e тесты. Браузер при этом запускается в headless формате и репорт не показывается(но показывается, если тесты запустить самостоятельно).
Если тесты не прошли, merge будет невозможен

🔳🔳🔳🔳🔳🔳🔳🔳🔳🔳

🟥 Задание:

3. Настройте релизный процесс

🟩 Решение:
1) Был использована библиотека Semantic Release, она позволяет сконфигурировать релиз так, чтобы при каждом релизе создавался changelog и добавлялся тэг в нужном формате. Его конфиг можно посомтреть в файле release.config.js. Версия релиза обновляется когда в коммите обозначены категории "feat"(второе число в номере версии), "fix" (третье число) или информация с пометкой "BREAKING CHANGE"(первое число)
2) Также был написан скрипт для того, чтобы при каждом релизе создавался issue с информацией. С помощью скрипта, который запускается в процессе работы actions во время релиза создается issue, в которой есть вся нужная информация: дата и время релиза, юзер, который релиз выпустил(технически последним юзером всегда считается semantic-release-bot, поэтому он исключен из возможных релизеров, чтобы в release note всегда сохранялся конкретный человек, ответственный за (запушивший) релиз), релизная версия, changelog и ссылка на результаты тестов. Если в коммит не пренадлежит к категориям, которые триггерят новый релиз новая Issue не создается, а происходит поиск старой такой же версии, если таковая находится — вся информация выводится в комментарий к той же Issue
3) Перед релизом(созданием новой версии и issue) запускается работа с тестами, там происходит то же самое, что и в случае с PR
4) Деплой на gh-pages происходит как отдельная работа, которая выполняется только после успешного релиза. В ней используется github-pages-deploy-action, что облегчает процесс деплоя. После успешного(!!) деплоя Issue закрывается
🟥 Задание:

4. секреты должны храниться защищенно

🟩 Решение:
Секреты организованы через secrets на гитхабе, во всех скриптах токены прокидываются через переменные окружения
