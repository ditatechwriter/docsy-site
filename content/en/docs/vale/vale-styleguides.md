---
title: "Style guides, linters, and Vale: Why do tech writers need this?"
linkTitle: "Style guides, linters, and Vale"
weight: 3
description: >
  Read about the style guides the tech writers use in their work and why it's important. Also, read about the linter for tech writers. Configure the Vale linter for checking how your texts comply with the recommendations in the style guides of Google and Microsoft. As a bonus, lean how to create your own style guide for Vale.
---

{{% pageinfo %}}
Important: This article can be useful not only for tech writers but also for all who deal with writing documentation in English: developers, QA engineers, business analysts, and others. Not all teams have tech writers. That's why these recommendations can add quality to your documentation.
{{% /pageinfo %}}

In this article, you learn about:

* Style guides for tech writers.

* Linters in general and linters for tech writers (by the example of Vale).

* How to use Vale for checking your texts if they comply with the recommendations in the style guides of Microsoft and Google.

* How to create your own style guide for Vale.

---

## What is style guide?

Style guide is a set of recommendations about styles, usage of specific words, phrases, terms. It's a written agreement for consistent writing and look of your documentation. Style guides are *a single source of truth* when different people have their own vision on using certain words in the text or about the formatting of documentation.

Tech writers use the recommendations from several generally accepted style guides: [Microsoft Style Guide](https://docs.microsoft.com/en-us/style-guide/), [Google developer documentation style guide](https://developers.google.com/style), etc. One of the oldest and the most well-known style guides is [The Chicago Manual of Style](https://www.chicagomanualofstyle.org/book/ed17/frontmatter/toc.html). This style guide has been published since 1906 and is more than 1000 pages long.

Mind that recommendations in the style guides from Microsoft and others are recommendations in the first place. These recommendations are preferable but not mandatory. Very often there are in-house style guides in the companies. The tech writers' team approve their internal rules on using certain terms, name conventions, punctuation. However, most of tech writers accept the well-known style guides and don't reinvent the wheel.

See these examples from style guides.

1. Microsoft [recommends](https://docs.microsoft.com/en-us/style-guide/capitalization) capitalizing only the first word of headings and titles. For example, the heading of this section is: *What is a style guide?* Google [recommends](https://developers.google.com/style/capitalization#capitalization-in-titles-and-headings) the same thing.

2. According to the style guides by [Microsoft](https://docs.microsoft.com/en-us/style-guide/capitalization#sentence-style-capitalization-in-titles-and-headings) and [Google](https://developers.google.com/style/capitalization#capitalization-in-titles-and-headings), if a title or heading includes a colon, you should capitalize the first word after it. For example, the title of this article is *Style guides, linters, and Vale: Why do tech writers need this?*

3. [Microsoft](https://docs.microsoft.com/en-us/style-guide/punctuation/commas#use-a-comma) and [Google](https://developers.google.com/style/commas#serial-commas) style guides recommend using so called 'oxofrd comma' (or serial comma) before the conjunction in a list of three or more items: *I dedicate this book to my parents, Ayn Rand, and God.*

To summarize, style guides provide useful guidance for tech writers and all who want to write in accordance with the acknowledged standards for technical documentation. Knowldge of style guides is one of the competencies for a tech writer, even a beginner. Seasoned tech writers create style guides in the company where they work. However, they all are guided by the well-known style guides as their reference.

Now, when you understand what style guides tech writers use, here is the question: how to apply these recommendations in practice? Surely, you can read the Microsoft Manual of Style as a book: memorize and take notes of the most useful recommendations. However, this is a long way: the printed Manual of Style is almost 500 pages. And don't forget that Google has its own style guide. Experienced tech writers know the most important recommendations by heart. However, even they make mistakes or don't see them in the texts when editing as we're all just people.

Luckily, we've the tools that automate checking texts for their compliance with the recommendations in the style guides from Microsoft and Google. The Vale linter is one of these tools.

## What are linters?

Linter *(derived from lint)* is a tool for automatic analysis of code for its compliance with certain requirements and rules: syntax, style, etc. This term was invented in 1978 by the programmer [Stephen Johnson](https://en.wikipedia.org/wiki/Stephen_C._Johnson) for searching for errors in the code of the С language. Literally, lints are small particles of fiber from the clothes made of cotton. Stephen compared such particles that are captured in the filter of the drier when you wash your clothes in the washing machine with small errors in your code. These errors can cause serious problems during compilation.

Modern developers make extensive use of linters to meet the syntax requirements of certain programming languages and to detect errors in their code. ESLint is one of the most used linters for JavaScript: more than [16 mln users](https://www.npmtrends.com/jslint-vs-jshint-vs-eslint-vs-tslint-vs-@typescript-eslint/eslint-plugin) downloaded this tool in the first months of 2021.

Now is where the fun begins: there are linters not only for code but also for texts. Vale is one of such linters. This tool was created by [Joseph Kato](https://github.com/jdkato) for the markup languages: Markdown, HTML, etc. Tech writers use Vale linter for checking if their texts meet the requirements of style guides: Microsoft, Google, etc.

## Vale

[Vale](https://docs.errata.ai/vale/about) linter is a command line tool that checks if texts meet the requirements of style guides or your own rules. For those who don't like using command prompt, there's a commercial version called [Vale Server](https://errata.ai/vale-server/). However, in this article I'm discussing a [free version](https://github.com/errata-ai/vale) of Vale linter.

How does it work?

### Prerequisites

1. You have texts written in the Markdown format .md. Vale also supports HTML, reStructuredText, AsciiDoc, DITA, XML. For more information, see Vale-[supported formats](https://docs.errata.ai/vale/scoping#formats).

2. You need to check if your texts in Markdown files meet the requirements of style guides: Microsoft,  Google.

3. Download the [styles](https://github.com/ivancheban/docsy-site/tree/master/styles) folder with style guide rules from this [GitHub repository](https://github.com/ivancheban/docsy-site).

4. Download the configuration file [.vale.ini](https://github.com/ivancheban/docsy-site/blob/master/.vale.ini).

5. Copy the **styles** folder and the **.vale.ini** config file to the root of your project with the Markdown files that you want to check. Usually, this is your documentation project folder.

    ![img](/docs/img/test-vale.png)

6.	Install Vale: [installation instructions](https://docs.errata.ai/vale/install). Check if Vale is installed: `vale --v`.

    ![img](/docs/img/vale-v.png)

### Using Vale

Let's check some file in Markdown. I copied one Markdown file to the test-vale folder — **jekyll.md**. This is my [article](https://docsy-site.netlify.app/docs/static-site-generators/jekyll/) про генератор статичних файлів Jekyll. Я хочу перевірити, наскільки в цій статті я дотримувався рекомендацій стайлгайдів Майкрософт і Гугл. Ну що, поїхали?

Можна скористатися командним рядком, але там не так красиво підсвічуються помилки та зауваження стайлгайдів.

![img](/docs/img/vale-cmd.png)

Я використовую редактор коду VSCode для написання та редагування статей у форматі Markdown. У VSCode процес перевірки виглядає набагато краще.

1. У VSCode відкрийте папку проекта.

2. У терміналі VSCode введіть:

```sh
vale filename.md
```

де `filename.md` — ваш файл у форматі Markdown, який потрібно перевірити.

![img](/docs/img/vale-jekyll.png)

Як бачимо, у терміналі VSCode виводяться попередження (warning) жовтим кольором і помилки (error) червоним. Також указані рядки, у яких знайдено помилки. Наприклад, в 11-му рядку (11:1) я вжив займенник our (*Our goal is…*). Написання від першої особи множини (*we, our, us, let’s*) — це не помилка, але й не рекомендується відповідно до стайлгайдів [Майкрософт](https://docs.microsoft.com/en-us/style-guide/grammar/person#avoid-first-person-plural) і [Гугл](https://developers.google.com/style/pronouns#personal-pronouns). Натомість стайлгайди рекомендують використовувати другу особу (*you, your*): *Your goal is…*

Звісно, що вирішувати вам: дослухатися до рекомендацій стайлгайдів або писати, як вважаєте за краще самі. Іноді трапляються *false positives* — хибні спрацьовування, коли помилки немає. Однак головна мета цієї перевірки — привернути вашу увагу до потенційної проблеми. Наведу ще один приклад, де вже не попередження, а червона помилка.

![img](/docs/img/vale-terminal.png)

Тут у 54-му рядку я написав: *… let's assume that we have Ruby installed.* Майкрософт [рекомендує](https://docs.microsoft.com/en-us/style-guide/word-choice/use-contractions) писати скорочення *we’ve* замість *we have*.

### Розширення Vale для VSCode

Замість того, щоб вводити в терміналі VSCode команду `vale filename.md` щоразу, коли вам потрібно перевірити файл у форматі Markdown на відповідність вимогам стайлгайдів, установіть розширення (extension) Vale для VSCode.

1. У розділі розширень VSCode знайдіть і установіть розширення Vale.

2. Налаштуйте конфігурацію розширення:

    a. Виберіть **Use Vale’s CLI instead of Vale Server**.

    b. Введіть шлях до папки проекта, де лежить файл **.vale.ini**. У моєму випадку це: `c:\Users\ivanc\test-vale`.

    c. У **Vale CLI: Path** введіть `vale`.

    ![img](/docs/img/vale-extension-config.png)


Тепер Vale буде перевіряти всі файли у форматі Markdown, які ви відкриваєте в редакторі VSCode. Розширення буде посилатися на папку **styles** і файл конфігурації **.vale.ini**, але тепер не треба копіювати ці файли до будь-якого проекта з файлами Markdown для перевірки.

Сама перевірка буде здійснюватись автоматично, коли ви відкриєте будь-який файл Markdown у VSCode. Vale буде підкреслювати слова, у яких знайдено проблеми. Ви можете навести курсор на це підкреслення або перейти на вкладку PROBLEMS у терміналі VSCode.

![img](/docs/img/vale-problems.png)

Можна також переглянути правило відповідного стайлгайда, якщо вибрати **View rule**. Відкриється файл у форматі YML, що лежить у папці відповідного стайлгайда в папці **styles**. У файлі є посилання на це правило в стайлгайді.

![img](/docs/img/vale-rule.png)

### Створення власного стайлгайда

Отже, ми з’ясували, що можемо перевіряти тексти на відповідність стайлгайдам Майкрософт і Гугл. А як щодо власних стайлгайдів? Це також можливо. Можна створювати власні правила та регулярні вирази. Як зразок можете використати існуючі YML-файли правил зі стайлгайдів Майкрософт, Гугл тощо.

Щоб створити власне правило:

1. Створіть папку з назвою вашого власного стайлгайда. Наприклад, **my-styleguide**.

2. Покладіть папку **my-styleguide** до папки **styles** з усіма іншими стайлгайдами.

3. Відкрийте файл конфігурації **.vale.ini** у Блокноті.

4. Додайте назву папки свого стайлгайда **my-styleguide** до переліку стайлгайдів.

    ![img](/docs/img/vale-ini.png)

5. Збережіть файл конфігурації **.vale.ini** у Блокноті.

6. У папці вашого стайлгайда створіть YML-файл правила з такою конфігурацією.

    ![img](/docs/img/rule-1.png)

7. Збережіть його з назвою, наприклад, **rule-1.yml**.

    ![img](/docs/img/vale-rule-1.png)

    Тут ми створюємо правило, щоб Vale нам видавав попередження, якщо написано *web-site* замість *website*, *dou* замість *DOU* та *e-mail* замість *email* незалежно від регістру (великими чи малими літерами).

8. Відкриваємо наш файл Markdown у VSCode.

    ![img](/docs/img/vale-check.png)

    Бачимо, що правило нашого стайлгайда спрацювало: Vale видав усі попередження щодо неправильного написання *web-site*, *e-mail* та *dou*.

## Що далі?

Усі перевірки, що я навів у попередніх розділах стосуються локальних файлів у форматі Markdown. Лінтер Vale, як і лінтери для коду, можна вбудувати в пайплайн CI/CD, щоб під час кожного коміту та пушу (збереження та передача локальних змін на сервер за допомогою git) відбувалася перевірка лінтером Vale. За наявності помилок ви не зможете передати зміни на сервер. Пайплайн CI/CD можна налаштувати для GitHub, GitLab. Особисто я цього не роблю і перевіряю свої файли локально. Однак знаю, що так працюють техрайтерські команди, що пишуть документацію для [GitLab](https://docs.gitlab.com/ee/development/documentation/testing.html#vale), [Spotify](https://github.com/backstage/backstage) та інших продуктів.

До речі, можете зайти в їхні відкриті репозиторії та подивитися конфігурацію перевірок за допомогою лінтера Vale. Крім того, ви можете додати додаткові стайлгайди до моєї конфігурації. Ось перелік доступних репозиторіїв з [офіційно підтримуваними стайлгайдами](https://github.com/errata-ai/styles#available-styles), з яких я брав стайлгайди Майкрософт і Гугл. Переходите за посиланнями до репозиторія і завантажуєте звідти папку з правилами. Наприклад, [цю папку для стайлгайда Joblint](https://github.com/errata-ai/Joblint/tree/master/Joblint). У цьому стайлгайді правила, за якими Vale перевіряє текст описів вакансій на наявність сексизмів, культурних ляпів, рекрутерських фейлів тощо.

Ще одна цікава можливість експериментувати зі створенням правил для Vale — їхній сайт [Vale Studio](https://vale-studio.errata.ai/). Тут можна задати правила та регулярні вирази, а також відразу подивитися результат, як відпрацює правило.

Сподіваюся, що ця стаття допоможе вам автоматизувати перевірку документації на відповідність вимогам стайлгайдів, а також створити власні правила для перевірки лінтером Vale. Пам’ятайте, що людині властиво помилятися, а такі засоби перевірки як Vale допомагають усунути людський фактор.
