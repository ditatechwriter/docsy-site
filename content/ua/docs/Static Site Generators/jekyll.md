---
title: "Jekyll"
linkTitle: "Jekyll"
weight: 1
description: >
  Коли я вперше почав вивчати CI / CD та SSG, першим генератором, про який я дізнався, був Jekyll. Я використав [Початок роботи з темою документації для Jekyll](https://idratherbewriting.com/documentation-theme-jekyll/) Тома Джонсона, відомого гуру технічного письма, відомого завдяки своєму [блогу I'd Rather Be Writing](https://idratherbewriting.com/).
  Тож, без зайвих слів, давайте створимо наш перший портал документації, використовуючи тему Тома для Jekyll. Я спробую пояснити навіть очевидні речі.
---

{{% pageinfo %}}
Наша мета - створити та опублікувати сайт документації за допомогою Jekyll та Netlify. Кінцевий результат буде виглядати так: https://sample-jekyll.netlify.app
{{% /pageinfo %}}

## Завантажте тему з репозиторію GitHub

1. Зареєструйтеся на GitHub.

![img](/docs/img/sign-up-GitHub.png)

2. Якщо у вас вже є обліковий запис, увійдіть.

![img](/docs/img/sign-in-GitHub.png)

3. Перейдіть до [репозиторія Тома](https://github.com/tomjoht/documentation-theme-jekyll).

![img](/docs/img/tom-repo.png)

4. Натисніть кнопку **Code** і виберіть **Download ZIP**.

![img](/docs/img/download-zip.png)

5. Збережіть ZIP-файл на своєму комп’ютері та розпакуйте вміст, де вам заманеться. Тепер у вас є папка з кодом та вмістом. Давайте приступимо до створення нашого сайту з усього цього.

---

## Встановіть Ruby на Windows

> Перш ніж ми встановимо Jekyll, який компілює наш сайт, нам потрібно встановити Ruby. Jekyll — це програма на основі Ruby, для запуску якої потрібен Ruby.

1. Перейдіть до [RubyInstaller for Windows](https://rubyinstaller.org/downloads/).
2. Встановіть рекомендовану  версію **Ruby+Devkit 2.6.X (x64)**.

![img](/docs/img/ruby-installer.png)

3. Встановіть все за замовчуванням.

![img](/docs/img/installation-ruby.png)

3. Після завершення встановлення ви побачите цей екран командного рядка. Натисніть `Enter` двічі, коли потрібно буде підтвердити вибір.

![img](/docs/img/ruby-installed.png)

4. Коли інсталяція в командному рядку завершиться, припустимо, що ми встановили Ruby. Якщо ви хочете переконатися, відкрийте командний рядок і введіть `ruby -v` і натисніть `Enter`.

![img](/docs/img/check-ruby-version.png)

---

## Встановіть Jekyll

1. Щоб встановити Jekyll, введіть `gem install jekyll` у командному рядку та натисніть `Enter`.

2. Перевірте, чи правильно встановлено Jekyll: введіть `jekyll -v` і натисніть `Enter`.

![img](/docs/img/check-jekyll-version.png)

---

## Встановіть Bundler

1. Перейдіть до каталогу, куди ви завантажили проект для Jekyll.

2. Видаліть існуючі файли `Gemfile` і `Gemfile.lock`.

![img](/docs/img/project-folder.png)

### Змініть шлях до проекту

По-перше, вам потрібно змінити каталог у командному рядку.

1. У провіднику скопіюйте шлях до розпакованої папки з вашим проектом.

![img](/docs/img/path-to-project-folder.png)

2. У командному рядку введіть `cd` та клацніть правою кнопкою миші, щоб вставити скопійований шлях.

3. Натисніть `Enter`, щоб змінити каталог. Тепер ви можете виконувати команди в каталозі проекту.

![img](/docs/img/paste-path-command-prompt.png)

---

### Встановіть Bundler

1. Щоб встановити Bundler, введіть `gem install bundler` і натисніть `Enter`.

![img](/docs/img/gem-install-bundler.png)

2. Введіть такі команди:

```
bundle init
bundle install
```

![img](/docs/img/bundle-init-bundle-install.png)

Ці команди створили нові файли `Gemfile` у папці проекту.

3. Відкрийте `Gemfile` за допомогою Блокнота.

![img](/docs/img/gemfile.png)

4. Видаліть усе в цьому файлі.

5. Введіть наступні дані та збережіть файл.

```
source "https://rubygems.org"

gem "jekyll"
```

![img](/docs/img/notepad-edit-gemfile.png)

---

## Скомпілюйте сайт

Щоб скомпілювати свій сайт Jekyll локально:

1. Змініть каталог в командному рядку: `cd documentation-theme-jekyll-gh-pages`.


2. Уведіть `jekyll serve`.


3. Щоб отримати доступ до сайту локально, скопіюйте адресу з командного рядка: [http://127.0.0.1:4000](http://127.0.0.1:4000/)

![img](/docs/img/jekyll-serve.png)

4. Вставте адресу у свій браузер, і ви побачите сайт.

![img](/docs/img/site-built-locally.png)

Ви можете отримати доступ до всього вмісту сайту локально з папки проекту.

:::note
Щоб зупинити локальний сервер, на якому запущено ваш сайт, натисніть `Ctrl+C` в командному рядку.
:::

---

## CI/CD, GitHub and IDE

> Before you publish your site online, you need to create the CI/CD pipeline. While this term sounds mysterious, there is nothing complicated about it.
>
> You need to have an editor on your computer where you will change the code and content of your site. This editor should be able to send the changes that you've made to your GitHub repository. It's like a Dropbox folder that syncs your local folder to the cloud.
>
> For this example, I will use Visual Studio Code editor/IDE.

### VSCode editor

Install VSCode from its [official site](https://code.visualstudio.com/download).

![img](/docs/img/download-vscode.png)

Useful links for setting up VSCode for viewing and editing Markdown files:

* [Using Markdown with Visual Studio Code](https://sciwiki.fredhutch.org/compdemos/vscode_markdown_howto)
* [How-To Guide: Markdown in Visual Studio Code](https://medium.com/@michael.isprihanto/how-to-guide-markdown-in-visual-studio-code-e8a68cc01f64)
* [Markdown and Visual Studio Code](https://code.visualstudio.com/docs/languages/markdown)
* [Lana Novikova's materials about VScode](https://gitlab.com/svetlnovikova/webinar/-/blob/master/post-webinar-materials.md) (In Russian)

---

### Git client

> You will also need Git client to connect VSCode to your GitHub repository. It's the same as using Word (in this case VSCode) to write/edit your document and Dropbox desktop client (in this case Git client) to sync your changes to the cloud server.

1. Install Git client from its [official site](https://git-scm.com/).

![img](/docs/img/download-git-client.png)

2. Install everything by default. You may close the Git client window.

---

### View project folder in the editor

1. Run the VSCode.

2. Select **File** > **Open Folder**.

![img](/docs/img/open-project-folder-vscode.png)

3. Open the project folder.

![img](/docs/img/open-project-folder.png)

Now you will see the folder contents in the VSCode editor. If you open the folder with content and click the **.md** file, you will see the file markup.

![img](/docs/img/markdown-markup.png)

Now you can edit the files. But you need to upload this folder to your GitHub repository to sync the changes.

---

### Upload project folder to GitHub

1. Go to the Source Control section of VSCode and click the **Publish to GitHub** button.

![img](/docs/img/publish-to-github.png)

2. Select **Publish to GitHub public repository**.

![img](/docs/img/publish-to-public-repository.png)

3. Select **Open in GitHub** to open your newly created project repository in GitHub.

![img](/docs/img/open-in-github.png)

You will see your project folder structure. Now your local folder is synced to the GitHub cloud server. Every change that you make locally will be synced to the GitHub server.

![img](/docs/img/project-your-repository.png)

---

## Publish your site

> Now when you have built the Documentation Site locally, you wonder how to publish it online for everyone to see. Although Tom tells how to publish his site on GitHub Pages, I don't recommend this. There are better and easier ways for publishing the sites built with Static Site Generators. For this example I will use Netlify.

1. Sign up to [Netlify](https://www.netlify.com/).

![img](/docs/img/netlify-signup.png)

Or log in, if you already have an account.

2. Press the **New site from Git** button.

![img](/docs/img/new-site-from-git.png)

3. Select **GitHub** as your Git provider.

![img](/docs/img/connect-to-github.png)

4. Authorize Netlify's access to your GitHub repository.

You will see the list of your repositories.

5. Pick the repository that you've created in the previous step.

![img](/docs/img/pick-repository.png)

6. Select **Deploy site**.

![img](/docs/img/deploy-settings.png)

You will see Netlify deploying your site with some funny name.

![img](/docs/img/deploy-progress.png)

{{< alert title="Note" >}}The building of your site for the first time takes several minutes. Be patient. When the build finishes, you will see the **Published** status.{{< /alert >}}

![img](/docs/img/site-deployed.png)

7. Change the site name to something more relevant.

![img](/docs/img/change-site-name.png)

8. Click the new site name to visit its page. My test site:

[https://sample-jekyll.netlify.app/](https://sample-jekyll.netlify.app/)

![img](/docs/img/sample-jekyll-site.png)

---

## Useful links

I used some help from these sites:

- [https://www.netlify.com/blog/2020/04/02/a-step-by-step-guide-jekyll-4.0-on-netlify/](https://www.netlify.com/blog/2020/04/02/a-step-by-step-guide-jekyll-4.0-on-netlify/)
- [https://www.netlify.com/blog/2015/10/28/a-step-by-step-guide-jekyll-3.0-on-netlify/](https://www.netlify.com/blog/2015/10/28/a-step-by-step-guide-jekyll-3.0-on-netlify/)
- [https://idratherbewriting.com/documentation-theme-jekyll/index.html](https://idratherbewriting.com/documentation-theme-jekyll/index.html)
- [https://github.com/tomjoht/documentation-theme-jekyll](https://github.com/tomjoht/documentation-theme-jekyll)
- [https://docs.github.com/en/free-pro-team@latest/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line](https://docs.github.com/en/free-pro-team@latest/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)