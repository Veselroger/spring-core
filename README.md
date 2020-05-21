# <a id="home"></a> Spring Core Project

**Table of Content:**
- [Step1: Создание Maven проекта](#step1)
- [Step2: Создание git репозитория](#step2)
- [Step3: Подключение Spring Framework](#step3)


## [↑](#home) <a id="step1"></a> Step1: Создание Maven проекта

Нам понадобится создать Java проект. Нам понадобится:
- Java Developer Kit ([JDK](https://jdk.java.net/))
- Система сборки проектов: [Maven](https://maven.apache.org/)

Откроем командную строку (например, в Windows можно нажать ``Win + R`` и выполнить ``cmd``).\
Создадим новый каталог (make directory) при помощи команды ``mkdir``. Например:
> mkdir c:\github\spring-core

Перейдём в новый каталог, т.е. изменим рабочий каталог (change directory) командой ``cd``. Например:
> cd c:\github\spring-core

Теперь нам нужно создать сам Java проект. Поможет нам в этом система сборки проектов Maven.\
**Maven** - это система сборки проектов. Работу в Maven выполняют плагины (**plugins**).\
У каждого плагина есть одна или несколько целей (**goal**). То есть goal - это некоторая задача.\
У Maven есть плагин под названием "**archetype**", который позволяет работать с так называемыми **архетипами**.\
"**[Архетип](https://maven.apache.org/archetype/index.html)**" - это своего рода шаблон Maven проекта, по которому может быть создан новый Maven проект.

Для генерации проекта по архетипу необходимо выполнить goal под названием "**[generate](http://maven.apache.org/archetype/maven-archetype-plugin/plugin-info.html)**".
Таким образом выполним команду: 
> mvn archetype:generate

Далее Maven спросит нас, какой архетип мы хотим, предложив "**Choose a number or apply filter**".\
Maven имеет встроенный набор готовых архетипов. И для 

Для данного вопроса есть вариант по умолчанию, т.к. Maven имеет встроенный набор готовых архетипов. По умолчанию будет предложен архетип "**maven-archetype-quickstart**". Нам нам подойдёт, поэтому ничего не указываем и нажимаем Enter, чтобы выбрать вариант по умолчанию. Версия архетипа так же по умолчанию.

Далее Maven предложит выбрать **[Maven Coordinates](https://maven.apache.org/pom.html#Maven_Coordinates)**.\
Например, можно указать что-то похожее на ``groupId=com.github.veselroger`` и ``artifactId=springcore``.\
Для остальных вопросов оставляем варианты по умолчанию, т.е. нажимаем Enter.

После завершения создания проекта мы увидим сообщение ``BUILD SUCCESS``.\
Maven создаёт структуру каталогов ("**[Standard Directory Layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)"**), а так же Java классы, чтобы Java приложение могло быть выполнено.

У Maven есть так называемые "**[Build lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)**". Существует три встроенных lifecycle: ``clean``, ``default`` и ``site``.\
Каждый lifecycle состоит из фаз ("**[Phase](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)"**). Фазы выстроены в цепочку. Выполнение фазы означает выполнение и всех фаз, которые были до неё. С фазами связываются ``goal`` Maven плагинов, которые выполняются друг за другом.

То, как будут выполняться плагины и какие именно описывается в документе ``pom.xml``.
**[POM](https://maven.apache.org/pom.html)** - Project Object Model, т.е. модель проекта.\
Откроем этот файл в корне каталога проекта, который создал Maven.
Во-первых, убедимся в правильности версии Java:
```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
``` 

Далее опишем Maven плагин в блоке ``<build>``. Очистим всё прежнее содержимое и заменим его новым:
```xml
<build>
    <plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-jar-plugin</artifactId>
			<version>3.2.0</version>
            <configuration>
				<archive>
					<manifest>
						<mainClass>com.github.veselroger.App</mainClass>
					</manifest>
				</archive>
			</configuration>
		</plugin>
	</plugins>
</build>
```

Выполним первую сборку чтобы проверить, что всё хорошо. Выполним Maven команду:
> mvn package

Данная команда выполнит в том числе фазы из цепочки: ``validate``, ``compile``, ``test``. В логе сборки мы увидим сообщения от выполненных плагинов. В том числе мы увидим строку вида:
```
--- maven-jar-plugin:3.0.2:jar (default-jar) @ springcore ---
[INFO] Building jar: c:\github\spring-core\springcore\target\springcore-1.0-SNAPSHOT.jar
```

Теперь мы можем запустить данный jar командой вида:
> java -jar c:\github\spring-core\springcore\target\springcore-1.0-SNAPSHOT.jar

``target`` - это каталог, куда по умолчанию складываются артефакты сборки. Чтобы его очистить необходимо запустить lifecycle с именем clean. Чтобы быть уверенным, что в конечном jar не совместились разные сборки часто выполняется сразу несколько lifecycle. Например:
> mvn clean package

Подробнее про Maven см в "**[Maven Documentation](https://maven.apache.org/guides/index.html)**".

Теперь можно открыть проект в любимой IDE. Например: "**[Eclipse IDE](https://www.eclipse.org/downloads/)**", "**[VSCode](https://code.visualstudio.com/docs/languages/java)**" или "**[IntelliJ Idea](https://www.jetbrains.com/idea/download)**".\
В рамках данного обзора будет использована IntelliJ Idea.


## [↑](#home) <a id="step2"></a> Step2: Создание git репозитория
Наша цель - отслеживать изменения в нашем проекте, а так же сохранять наши локальные изменения так, чтобы они были доступны не только на нашей локальной машине. И в этом нам поможет система контроля версий или VCS.\
Воспользуемся наиболее используемой системой контроля версий - **[GIT](https://git-scm.com)**.

Чтобы отслеживать изменения в проекте при помощи git нужно инициализировать репозиторий в каталоге проекта.\
Чтобы инициализировать (Initialize) git репозиторий в каталоге проекта выполним команду **[git init](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init)**.\
IntelliJ Idea позволяет сразу из IDE открыть командую строку: ``View → Tool Windows → Terminal``.\
Команду **git init** можно так же выполнить через главное меню IntelliJ Idea: ``VCS → Enable Version Control Integration``.

Теперь у нас есть локальный репозиторий. Далее нам потребуется удалённый (**remote**) репозиторий.\
Создать его можно, например, на "**[github](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository)**" или "**[bitbucket](https://bitbucket.org/product/guides/basics/four-starting-steps)**".

После создания необходимо установить связь между локальным репозиторием и удалённым репозиторием.\
Для этого git использует команду **[git remote](https://www.atlassian.com/git/tutorials/syncing)**.\
Каждый remote репозиторий регистрируется под определённым именем. Принято называть основной удалённый репозиторий именем "**origin**". Выполним добавление remote репозитория, например:
> git remote add origin https://github.com/Veselroger/spring-core.git

После названия origin идёт путь к репозиторию. Например, такой репозиторий может быть размещён на github.com.
Чтобы увидеть список зарегистрированных репозиториев выполним команду:
> git remote -v

параметр "v" означает "verbose", т.е. "подробно". Подробнее можно прочитать на Github'е: "**[Adding a remote](https://help.github.com/en/github/using-git/adding-a-remote)**".\
IntelliJ Idea тоже позволяет посмотреть зарегистрированные репозитории: ``VCS → Git → Remotes``.

Теперь у нас есть ссылка на удалённый репозиторий по имени "origin". Теперь можем забрать все изменения из удалённого репозитория при помощи команды **[git pull](https://www.atlassian.com/git/tutorials/syncing/git-pull)**, например:
> git pull origin master

Каждый раз писать origin master неудобно. Поэтому можно создать связь между локальной веткой и удалённой веткой.\
Такая связь называется **upstream**. Тогда git будет знать откуда получать данные, если не указано иное.\
Например, установим связь с origin/master:
> git branch -u origin/master

Для будущих коммитов необходимо выполнить настройку git. 
Подробнее можно прочитать тут: "**[Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)**".\
Если каталог является git репозиторием, то в нём есть скрытый каталог ``.git`` и файл ``.git\config``. Это локальные настройки git. Локальные настройки git никуда не идут, на то они и локальные.\
Поэтому откроем файл ``.git\config`` и добавим в этот файл новый блок:
```
[user]
	name = Ваше имя
	email = Ваша почта
```
Теперь любые изменения, которые мы сделаем внутри данного репозитория, будут выполнены под этим пользователем.

Так как наш проект отслеживается при помощи git, то мы можем посмотреть текущие изменения в репозитории.\
Чтобы посмотреть, что изменилось в нашем git репозитории воспользуемся командой **[git status](https://www..atlassian.com/git/tutorials/inspecting-a-repository)**. Например:
> git status -u

Мы увидим все файлы, которые были изменены. Среди изменений будут не только нужные для сохранения файлы (созданные по архетипу), но и файлы IDE и артефакты сборки, которые не должны сохраняться в git (каталог ``target``).\
Этот же статус можно увидеть в IntelliJ Idea: ``View → Tool Windows → Commit`` или ``VCS → Commit`` .\
Кроме того удобно в "**Commit Tool Window**" нажать на иконку с квадратиками ("**Group By**") и выбрать "**Directory**".

Чтобы исключить файлы из отслеживания git'ом необходимо создать файл "**.gitignore**".\
В терминале это можно сделать открыв файл на редактирование. Например:
> notepad .gitignore

В IntelliJ Idea можно выбрать каталог\файл, который нужно исключить (например, target) и выбрать ``VCS → Git → Add to .gitignore`` или через контекстное меню исключаемого файла\каталога. При отстутсвии файл будет создан.

Содержимое файла может быть следующим:
```
/target/
/.idea/
/springcore.iml
```

Чтобы изменения сохранить необходимо их сначала добавить в staging area при помощи команды **[git add](https://www.atlassian.com/git/tutorials/saving-changes)**, после выполнить **[git commit](https://www.atlassian.com/git/tutorials/saving-changes/git-commit)**.

IntelliJ Idea позволяет через ``VCS → Commit`` открыть "**Commit Window**", выбрать файлы и нажать "**Commit**".\
Если мы волним команду **[git log](https://www.atlassian.com/git/tutorials/git-log)**, то мы увидим сделанные нами коммиты. Тоже можно сделать в IntelliJ Idea на вкладке git (``View → Tool Windows → Git``) в разделе "log".

Осталось отправить изменения на удалённый репозиторий, чтобы изменения стали видны всем. 
Т.к. у нас настроен upstream ветки, то нам достаточно просто выполнить **[git push](https://www.atlassian.com/ru/git/tutorials/syncing/git-push)**.\
Git push можно выполнить в IntelliJ Idea через ``VCS → Git → Push``.



## [↑](#home) <a id="step3"></a> Step3: Подключение Spring Framework