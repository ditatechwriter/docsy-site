---
title: "Підхід docs-as-code для створення документації на прикладі генератора статичних сайтів Docusaurus"
linkTitle: "Docs-as-code для документації на прикладі Docusaurus"
weight: 6
description: >
  У цій статті я розповім про підхід *docs-as-code* (документація як код) для створення документації у вигляді статичного сайта за допомогою Docusaurus — генератора статичних сайтів (*static site generator*). Окрім загальної інформації про сучасний підхід до створення документації, я пропоную покрокову інструкцію, як створити сайт із документацією без знання мови програмування, а також опублікувати цей сайт в інтернеті.
---

{{% pageinfo %}}
Стаття адресована насамперед техрайтерам, але буде корисною також для команд розробників, які шукають спосіб створення внутрішнього або зовнішнього сайта з документацією для програмного продукту.
{{% /pageinfo %}}

У цій статті ви дізнаєтесь:

* Що таке підхід docs-as-code

* Як швидко створити сайт із документацією за допомогою генератора статичних сайтів Docusaurus

* Як налаштувати зовнішній вигляд сайта з документацією

* Як створювати та редагувати статті документації на сайті

* Як безкоштовно опублікувати сайт із документацією в інтернеті

* Як налаштувати CI/CD пайплайн для автоматизації публікації змін на сайті

---

## Docs-as-code

Підхід до створення документації _docs-as-code_ (документація як код) набуває все більшого поширення серед техрайтерів та інших спеціалістів, які пишуть документацію для програмних продуктів. Спочатку розберемося, що означає це поняття.

Досвідчений техрайтер Google Том Джонсон у своєму блозі [зазначає](https://idratherbewriting.com/learnapidoc/pubapis_docs_as_code.html): «Ставитись до документації як коду — означає використовувати ті самі системи, процеси та послідовність виконуваних дій з документацією, які ви використовуєте з програмним кодом». Він наводить такі ознаки підходу _docs-as-code_:

* **Робота з файлами у форматі «легкої» (lightweight) або «читабельної» (plain) розмітки.** Це насамперед Markdown (.md) — найбільш поширений формат вихідного коду документації для docs-as-code. Маркдаун — дійсно простий і читабельний формат розмітки. Наприклад, виділення жирним шрифтом робиться за допомогою двох зірочок ** на початку та в кінці слова або фрази, які потрібно виділити жирним. Таким чином файли у форматі Маркдаун можна прочитати у вихідному коді документації навіть до конвертації в HTML. Для цього не потрібний WYSIWYG редактор, що конвертує складний для прочитання код XML, як це робить Word або MadCap Flare.

* **Використання open-source (безкоштовного) генератора статичних сайтів.** На сьогодні існують десятки безкоштовних генераторів статичних сайтів з відкритим кодом. Це означає, що їхній код можна завантажити собі на комп’ютер із відкритого репозиторія в GitHub, побудувати сайт локально, щоб автоматично отримати HTML-сторінки з додаванням CSS-стилів для кольорової теми, JS-елементів тощо. Ці сторінки можна завантажити на вебсервер, щоб зібраний сайт був доступний в інтернеті. Серед найбільш відомих генераторів статичних сайтів такі: Jekyll, Hugo, Gatsby, Next.js тощо. Перелік досить довгий. Більше можна знайти на сайті [Jamstack](https://jamstack.org/generators/). Генератори можна використовувати не тільки для створення сайтів із документацією, але насамперед для блогів, лендингів тощо.

* **Робота з файлами за допомогою текстового редактора.** Техрайтери, які працюють за принципом *docs-as-code*, використовують редактори коду (IDE), якими користуються й розробники. Це може бути Visual Studio Code, WebStorm, PyCharm та інші редактори коду. У цих редакторах можна працювати з файлами у форматі Маркдаун і одночасно мати попередній перегляд кінцевого результату в HTML. Крім того, можна користуватися багатьма розширеннями, що спрощують роботу з гітом: перевірка правопису тощо.

* **Зберігання документації в репозиторії системи контролю версій.** Як і розробники, техрайтери комітять (*commit* — фіксація змін) зміни до коду документації локально та пушать (*push* — передача змін на сервер) ці зміни в гіт сервер у GitHub, GitLab, Bitbucket або іншу систему контролю версій (VCS). Крім відомих переваг системи контролю версій, як-от відстежування змін за допомогою діффів (*diff* або *difference* — візуальне виділення різниці між поточною і попередньою версіями файла), репозиторій гіт може використовуватись у CI/CD пайплайні для збирання та публікації сайта.

* **Спільна робота над документацією в репозиторії гіта команди техрайтерів.** Як і розробники, команда техрайтерів спільно працює над проєктом сайта з документацією: роблять *master* і *develop* гілки, виводять фіча бранчі (*feature branch* — гілка для роботи над окремою функціональністю, що у випадку документації може бути окрема стаття на сайті з документацією), створюють мердж ріквести (*merge request* — запит на об’єднання змін із фіча гілки в develop після закінчення написання статті) і код рев’ю лідом.

* **Автоматизація процесу побудови та публікації сайта з документацію за допомогою пайплайна CI/CD.** Зазвичай процес автоматичного будування сайта з документацією та публікації (*deploy*) на вебсервер налаштовуються спеціалістами devops або system engineers. Такі спеціалісти створюють скрипт із усіма кроками й командами генерації статичного сайта: після пуша змін у master гілку автоматично запускається скрипт, що виконує команди будування сайта на сервері та деплоїть згенеровані HTML-сторінки разом з усіма стилями CSS й елементами JS у вигляді статичного сайта з документацією.

* **Запуск тестів документації.** Як і розробники, техрайтери тестують документацію на відповідність певним вимогам: відсутність неробочих посилань, лінтер Vale з правилами перевірки на відповідність вимогам стайлгайдів Майкрософт, Гугл і власних стайлгайдів. Про це я писав у [попередній статті](../../vale/vale-styleguides/).

* **Управління процесами створення та оновлення документації за методологіями Scrum, Agile, Kanban.** Як і розробники, техрайтери працюють за спринтами (*sprint* — період, що зазвичай триває один місяць, за який розробники поставляють частину розробленої функціональності для демонстрації замовнику виконаної роботи). Техрайтери використовують Jira або інший трекер задач і проводять відповідні Scrum-церемонії (*daily standup, Sprint Planning, Retro* тощо). Часто техрайтери лінкують у джирі свої задачі з документування фіч до задач розробників.

Отже, ми з’ясували, що процес розробки сайта з документацією загалом схожий на процес розробки коду. Далі спробуємо на практиці встановити потрібні інструменти *docs-as-code*, згенерувати сайт з документацією та опублікувати його в інтернеті.

## Генератор статичних сайтів Docusaurus

Чому я обрав [Docusaurus](https://docusaurus.io/docs) як генератор статичних сайтів (*SSG — static site generator*) для сайта з документацією? Хоча мій улюблений генератор сайтів — це Hugo, який я використовую для свого власного пет проєкта (цей сайт), налаштування такого сайта хоч і добре описане в документації, але забирає багато часу. Натомість підняти сайт на Docusaurus можна дуже швидко — буквально за лічені хвилини ви зможете мати локальний сайт з документацією та почати писати туди в Маркдаун файлах. Звісно, щоб налаштувати CI/CD пайплайн і кастомізувати CSS для власних кольорових схем, шрифти, картинки тощо, а потім опублікувати сайт в інтернеті знадобиться трохи більше часу. Але не набагато більше.

Щоб установити й запустити Docusaurus:

1. Переконайтесь, що на комп’ютері встановлено Node.js. У командному рядку введіть: `node -v`

    {{< alert title="Примітка" >}}Після всіх команд тут і далі потрібно натискати Enter.{{< /alert >}}

2. Установіть [Node.js](https://nodejs.org/en/download/), якщо версія не відображається.

3. Перезавантажте комп’ютер.

4. Створіть папку, де хочете розгорнути проєкт сайта з документацією. Наприклад, у командному рядку введіть: `md my-docusaurus-projects`, щоб створити папку та `cd my-docusaurus-projects`, щоб перейти до створеної папки.

5. Створіть сайт із документацією за допомогою останньої версії генератора Docusaurus із назвою проєкта `my-site`:

    ```sh
    npx create-docusaurus@latest my-site classic
    ```

    {{< alert title="Примітка" >}}`classic` — це попередньо встановлена тема сайта.{{< /alert >}}

6. Введіть `yes`, коли з’явиться повідомлення про продовження встановлення.

    Зачекайте, поки система встановить усі залежності npm.

7. Після закінчення встановлення перейдіть до папки новоствореного проєкта сайта: `cd my-site`

8. Запустіть сайт на локальному сервері:

    ```sh
    npm start
    ```

    Сайт із документацією відкриється в браузері за адресою: [http://localhost:3000/](http://localhost:3000/).

    ![img](/docs/img/docusaurus-site.png)

## Налаштування зовнішнього вигляду сайта

Отже, ми запустили сайт локально в браузері. Папка проєкта сайта з усіма необхідними файлами знаходиться в моєму випадку за шляхом `c:\Users\Ivan_Cheban\my-docusaurus-projects\my-site`. Зараз ми змінимо:

- Назву сайта

- Зображення логотипу

- Кольорову схему теми

- Текст на домашній сторінці

Прочитати про всі налаштування сайта, створеного за допомогою Docusaurus, можна в офіційній документації: [https://docusaurus.io/docs](https://docusaurus.io/docs).

Щоб змінити назву сайта:

1. Відкрийте папку проєкта в редакторі коду VS Code.

2. Виберіть файл `docusaurus.config.js`.

    ![img](/docs/img/docusaurus-config.png)

3. Змініть назву сайта `title: 'My Site'` на свою. Наприклад: `Documentation site`.

    {{< alert title="Примітка" >}}Усі зміни відразу відображаються в браузері, тому що ми запустили сайт на локальному сервері в режимі live reload.{{< /alert >}}

4. У цьому ж файлі нижче змініть назву сайта `title: 'My Site'` в навігаційному меню.

    ![img](/docs/img/navbar.png)

Щоб змінити зображення логотипу (динозаврик):

1. Скопіюйте своє зображення логотипу до папки `c:\Users\Ivan_Cheban\my-docusaurus-projects\my-site\static\img\`.

2. Замініть зображення `logo.svg` на своє зображення з таким же іменем і розширенням файла. Наприклад, я завантажив логотип звідси: [https://www.svgrepo.com/download/205072/documents-document.svg](https://www.svgrepo.com/download/205072/documents-document.svg).

3. Перезавантажте сторінку сайта з документацією в браузері, щоб побачити новий логотип.

Щоб змінити кольорову схему теми:

1. У VS Code виберіть файл `c:\Users\Ivan_Cheban\my-docusaurus-projects\my-site\src\css\main.css`.

2. Змініть код кольору hex для параметра `--ifm-color-primary` з `#25c2a0` на `#90a3b0` або інший колір на ваш смак.

    {{< alert title="Примітка" >}}Зміни застосовуються не тільки для домашньої сторінки, а й для кольорової схеми на інших сторінках сайта (бокове меню, верхнє меню навігації, колір посилань тощо).{{< /alert >}}

Щоб змінити текст на домашній сторінці сайта:

1. Змініть підзаголовок сайта `tagline` у файлі `docusaurus.config.js` з `Dinosaurs are cool` на `How to create your documentation site with Docusaurus`.

2. Змініть назви та посилання, копірайт тощо в нижній частині сайта `footer`.

3. Якщо вам не потрібний блог на сайті з документацією, закоментуйте (`//`) рядок, щоб не відображати його в меню навігації.

    ![img](/docs/img/comment-out.png)

4. Змініть назву розділу сайта з документацією `label` з `Tutorial` на `Docs`.

5. У VS Code виберіть файл `my-site\src\components\HomepageFeatures.js` і змініть текст і зображення можливостей на домашній сторінці.

    ![img](/docs/img/homepage-features.png)

6. У VS Code виберіть файл `my-site\src\pages\index.js`, щоб змінити надпис на кнопці з `Docusaurus Tutorial - 5min` на `Start here`.

    ![img](/docs/img/action-button.png)

    Після всіх цих змін домашня сторінка буде виглядати так.

    ![img](/docs/img/start-page.png)

## Створення та редагування статей документації

Після кастомізації домашньої сторінки сайта з документацією можна починати писати власне документацію. Щоб перейти до власне документації, натисніть кнопку Start here на домашній сторінці.

Так виглядає зразок документації в шаблоні Docusaurus.

![img](/docs/img/default-intro.png)

### Розташування статей документації

Уся документація в Docusaurus міститься у вигляді файлів Маркдаун у папці `c:\Users\Ivan_Cheban\my-docusaurus-projects\my-site\docs\`.

![img](/docs/img/docs-folder.png)

### Ієрархія статей у розділі документації

Перша стаття **Tutorial Intro** — це файл `intro.md`. Його розташування в сайдбарі (*sidebar* — бокове меню) визначається параметром `sidebar_position: 1`. Назва береться з першого заголовка в тексті Маркдаун файла.

Щоб змінити ієрархічне розташування статті в сайдбарі, змініть значення параметра `sidebar_position`. Наприклад, щоб перемістити цю статтю в кінець: `sidebar_position: 4`.

![img](/docs/img/sidebar-position.png)

Щоб відкривати іншу статтю після переходу з домашньої сторінки, змініть шлях до потрібної статті у файлі `my-site\src\pages\index.js`.

Наприклад, будемо першою показувати статтю **Create a Page** файла `c:\Users\Ivan_Cheban\my-docusaurus-projects\my-site\docs\tutorial-basics\create-a-page.md`. Для цього змініть значення параметра `to` з `/docs/intro` на `/docs/tutorial-basics/create-a-page`.

![img](/docs/img/first-page.png)

### Підрозділи в розділі документації

У нашому тестовому сайті з документацією є два підрозділи: Tutorial – Basics та Tutorial – Extras. Ці підрозділи містять інші статті, які можна побачити, натиснувши на відповідний підрозділ у сайдбарі. Цим підрозділам відповідають папки **tutorial-basics** і **tutorial-extras** у папці `c:\Users\Ivan_Cheban\my-docusaurus-projects\my-site\docs\`.

Щоб додати новий підрозділ зі статтями, скопіюйте одну з цих папок і вставте її в папку **docs** із новою назвою, наприклад **my-docs**.

{{< alert title="Примітка" >}}Дотримуйтесь правил неймінгу папок і файлів: усі маленькі літери, дефіси замість пробілів.{{< /alert >}}

Далі перейменуйте Маркдаун файли в папці **my-docs**.

### Ієрархія підрозділів документації

Щоб визначити порядок розташування підрозділів зі статтями документації, змініть значення параметра position у файлі `C:\Users\Ivan_Cheban\my-docusaurus-projects\my-site\docs\tutorial-basics\_category_.json`. Наприклад, змінимо порядок підрозділів `tutorial-basics` і `tutorial-extras`. Для цього змінимо 3 на 2 для `tutorial-basics` і 2 на 3 для `tutorial-extras`.

![img](/docs/img/category-json.png)

Порядок розташування цих підрозділів зміниться в сайдбарі.

![img](/docs/img/changed-sections.png)

Ієрархія статей у межах підрозділу змінюється за допомогою параметра `sidebar_postion`, як було описано в підрозділі [Ієрархія статей в розділі документації](./#ієрархія-статей-у-розділі-документації).

## Публікація сайта з документацією

Отже, ми зробили локальний сайт із документацією та можемо переглядати його в браузері. Настав час опублікувати його в інтернеті, щоб він був доступний для перегляду за посиланням. Для цього нам знадобиться:

- Акаунт у [GitHub](https://github.com/), куди ми завантажимо вихідний код проєкта з документацією.

- Акаунт у [Netlify](https://app.netlify.com/), де ми будемо хостити наш згенерований сайт.

{{< alert title="Примітка" >}}Спочатку зареєструйтеся в GitHub, щоб можна було увійти в Netlify за допомогою акаунта GitHub.{{< /alert >}}

Крім того, потрібно завантажити та встановити гіт локально на комп’ютері: [https://git-scm.com/downloads](https://git-scm.com/downloads).

Щоб перевірити, чи встановлено у вас гіт локально, у командному рядку введіть:

```sh
git version
```

### Завантаження в GitHub

Спочатку прив’яжіть локальну папку з проєктом документації до репозиторія в GitHub.

1. Відкрийте папку проєкта з документацію у VS Code. У моєму випадку це: `c:\Users\Ivan_Cheban\my-docusaurus-projects\my-site\`

2. Перейдіть на вкладку Source Control у боковому меню VS Code.

3. Виберіть **Publish to GitHub**.

    ![img](/docs/img/publish-github.png)

4. Виберіть **Publish to GitHub public repository**.

5. Дочекайтесь, коли ваш проєкт буде опубліковано та виберіть **Open on GitHub**, щоб перейти до створеного репозиторія в GitHub.

    ![img](/docs/img/github-my-site.png)

Тепер ваш локальний проєкт синхронізовано з репозиторієм у GitHub. На жаль, усі зміни, які ви вносите локально, потрібно синхронізувати з репозиторієм у гіті вручну.

Після того, як ви внесете всі потрібні зміни:

1. У VS Code натисніть <kbd>Ctrl+Shift+P</kbd>.

2. Введіть або виберіть **Git: Commit All**.

    ![img](/docs/img/commit-all.png)

3. Введіть повідомлення, що ви змінили.

4. Натисніть <kbd>Ctrl+Shift+P</kbd>.

5. Введіть або виберіть **Git: Push**.

    Ваші зміни передано на сервер GitHub.

### Публікація сайта за допомогою Netlify

Тепер ми можемо опублікувати наш сайт із документацією через сервіс Netlify. Це безкоштовно, якщо доменне ім’я сайта буде містити netlify.app.

1. Перейдіть до https://app.netlify.com/

2. Увійдіть за допомогою вашого акаунта в GitHub.

3. Виберіть New site from Git.

    ![img](/docs/img/new-site.png)

4. Виберіть **GitHub**.

    ![img](/docs/img/github.png)

5. Авторизуйте Netlify для доступу до вашого GitHub репозиторія та виберіть репозиторій із вашим сайтом.

    ![img](/docs/img/import-github.png)

6. Виберіть **Deploy site**.

    ![img](/docs/img/deploy-netlify.png)

7. Дочекайтеся завершення публікації сайта (deploy).

8. Оскільки сайт публікується з випадковим ім’ям, як-от `inspiring-benz-dc91fd`, відразу змініть назву сайта в **Site settings**.

    ![img](/docs/img/site-settings-netlify.png)

9. Виберіть **Change site name** та введіть свою назву.

    ![img](/docs/img/change-site-name-netlify.png)

    Я ввів `ivan-documentation-example`.

10. Перейдіть за посиланням на опублікований сайт. У моєму випадку це:

    https://ivan-documentation-example.netlify.app/

## CI/CD пайплайн

Якщо ви дотримувалися кроків у попередніх розділах, то опублікували свій сайт в інтернеті. Тепер хочу пояснити, як автоматично оновлювати свій сайт із документацією за допомогою пайплайна CI/CD.

Насправді автоматичний пайплайн уже налаштовано — сервіс Netlify створив хук у ваш репозиторій GitHub і буде відстежувати будь-які зміни. Як тільки ви зробите комміт і пуш змін зі свого локального проєкта до репозиторія в GitHub, Netlify почне процес будування та деплоя сайта. Це не забирає багато часу (1-2 хвилини), оскільки все кешується, і деплоїться тільки дельта — зміни між початковою та поточною версією файлів.

Спробуйте змінити щось у файлах локально, а потім повторіть кроки для Git: Commit All і Git: Push, описані в розділі [Завантаження в GitHub](./#завантаження-в-github).

Усі зміни публікуються до гілки master. Якщо ви створите гілку develop або іншу, зміни, які ви запушите в ці гілки, не будуть відображатися на сайті, оскільки гілка master вважається Production. Звісно, усе це налаштовується. Однак пропоную всі процеси налаштування в локальній інфраструктурі залишити спеціалістам, які цим займаються: devops або system engineers. Вони створять скрипти та середовища (environments) для публікації локального інстанса сайта (корисно, щоб переглядати статті до публікації на продакшн). Спеціалісти також розберуться з налаштуванням deployment server (Octupus або інше), місцем хостинга файлів (AWS S3 бакіти або інше).

Ваша задача — створити демо для сайта з документацію та продемонструвати, як він працює. Далі вже керівництво прийме рішення, чи потрібно й надалі користуватися Confluence або іншою внутрішньою базою знань. А можливо, цей сайт із документацією доопрацюють фронтенд інженери, щоб він виконував функцію головного сайта з документацією для вашого продукту.

Підхід *docs-as-code* дає змогу користуватися всіма перевагами інструментів, якими користуються розробники програмного забезпечення. Ви можете налаштувати лінтер Vale, щоб перевіряти текст документації на відповідність вимогам стайл гайдів Майкрософт і Гугл, а також власних стайл гайдів. Про це я писав у статті: [Як перевіряти документацію за допомогою автоматичного засобу — лінтера Vale](../../vale/vale-styleguides/).