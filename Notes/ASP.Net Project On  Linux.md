---
id: ASP.Net Project On  Linux
aliases:
  - Создание и управление проектом ASP.Net через консоль
tags:
  - c_sharp
---
# Создание и управление проектом ASP.Net через консоль

### Шаг 1: Создание пустого решения

1. Откройте терминал или командную строку.
2. Перейдите в каталог, где вы хотите создать новое решение. 
    Если нужно, создайте директорию с помощью команды mkdir 
    Название_директории и затем перейдите в неё с помощью cd Название_директории.
3. Выполните следующую команду для создания пустого решения:

```bash
dotnet new sln -n Название_Решения
```

Замените Название_Решения нужным вам названием.
Эта команда создаст файл решения .sln в вашей текущей директории.

### Шаг 2: Создание проекта и добавление его в решение

Создайте новый проект (например, веб-приложение ASP.NET Core) 
и добавьте его в ваше решение с помощью следующих команд:

1. Создайте проект командой:
```bash
dotnet new web -n Название_Проекта
```

Замените Название_Проекта на предпочитаемое название проекта.
Это создаст новый проект ASP.NET Core в текущей директории.

2. Добавьте созданный проект в решение:
```bash
dotnet sln Название_Решения.sln add ./Название_Проекта/Название_Проекта.csproj
```

Убедитесь, что вы указываете правильные пути к файлу решения 
.sln и файлу проекта .csproj.

### Шаг 3: Построение решения

Теперь, когда проект добавлен в решение, 
вы можете построить решение, чтобы убедиться, 
что все компоненты корректно связаны и работают:
```bash
dotnet build Название_Решения.sln
```
Эта команда построит решение и все связанные с ним проекты.

### Общий пример
```bash
mkdir MySolutionDirectory
cd MySolutionDirectory
dotnet new sln -n MySolution
dotnet new web -n MyWebApp
dotnet sln MySolution.sln add ./MyWebApp/MyWebApp.csproj
dotnet build MySolution.sln
```

Этот пример создает новую директорию для решения, 
внутри которой создается пустое решение, добавляется новый проект 
ASP.NET Core Web и затем проект добавляется к решению. 
В конце производится сборка решения.

Для создания пустого проекта типа Class Library с использованием 
консольной утилиты dotnet, выполните следующие шаги:

1. Откройте терминал на вашем компьютере.
2. Перейдите в директорию, где вы хотите разместить новый проект. 
   Если нужно, создайте новую директорию с помощью команды mkdir 
   ИмяДиректории и затем перейдите в неё командой cd ИмяДиректории.
3. Выполните команду для создания нового проекта Class Library:
```bash
dotnet new classlib -n ИмяПроекта
```
Замените ИмяПроекта на желаемое название вашего проекта. 
Эта команда создаст новый C# проект Class Library в текущей директории. 

В результате, у вас будет создана начальная структура проекта, 
включая файл проекта (*csproj) и начальный файл класса (Class1.cs, 
который вы можете переименовать по вашему усмотрению).

### Добавление ссылки на проект

Чтобы добавить ссылку на другой проект или на библиотеку в вашем проекте 
.NET с использованием консольной утилиты dotnet, вы можете воспользоваться 
командой dotnet add reference. Вот как это можно сделать на практике:

Если у вас есть два проекта в одном решении и вы хотите, 
чтобы один проект (например, ПроектА) ссылались на другой проект 
(например, ПроектБ), выполните следующие шаги:

1. Откройте терминал или командную строку.
2. Перейдите в директорию проекта ПроектА, который будет содержать ссылку, используя команду cd.
3. Выполните команду dotnet add reference, указав путь к файлу проекта ПроектБ.csproj, на который вы хотите добавить ссылку:
```bash
dotnet add reference ../Путь/к/ПроектБ/ПроектБ.csproj
```
Убедитесь, что путь к файлу проекта указан относительно директории проекта ПроектА.

### Добавление ссылки на NuGet пакет

Если вы хотите добавить в ваш проект ссылку на пакет из NuGet, 
выполните следующие шаги:

1. Откройте терминал или командную строку.
2. Перейдите в директорию вашего проекта, в который хотите добавить пакет.
3. Выполните команду dotnet add package, указав имя пакета:
```bash
dotnet add package ИмяПакета
```
К примеру, чтобы добавить пакет Newtonsoft.Json, используйте:
```bash
dotnet add package Newtonsoft.Json
```

Эта команда найдет последнюю версию пакета Newtonsoft.Json и добавит его в зависимости вашего проекта.

### Проверка добавленных зависимостей

После добавления ссылок на проекты или NuGet пакеты, 
вы можете проверить файл проекта .csproj, чтобы убедиться, 
что ссылки были успешно добавлены. Зависимости будут отображаться в секциях `
<ItemGroup>` файла .csproj.

### Для добавления Entity Framework Core в ваш проект .NET с помощью консольной команды

Для добавления Entity Framework Core в ваш проект .NET с помощью консольной команды dotnet, следует выполнить несколько шагов. Entity Framework Core (EF Core) — это легковесная, расширяемая и кроссплатформенная версия Entity Framework, популярного ORM (Object-Relational Mapper) от Microsoft.

Вот подробная инструкция:

1. Откройте терминал или командную строку.

2. Перейдите в директорию вашего проекта .NET, в который вы хотите добавить EF Core. Используйте команду cd Путь/к/вашему/проекту.

3. Добавьте NuGet пакет EF Core. Для этого используйте команду dotnet add package, указав имя нужного вам пакета. Entity Framework Core разбит на несколько пакетов, основным из которых является Microsoft.EntityFrameworkCore. Если вы используете определенную базу данных, вам также потребуется установить соответствующий пакет для провайдера этой базы данных.
   Вот пример, как добавить основной пакет EF Core:

```bash
   dotnet add package Microsoft.EntityFrameworkCore
```

```bash
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```
   
4. (Опционально) Установите инструменты командной строки EF Core. Эти инструменты помогут вам в выполнении миграций, обновлении вашей базы данных и выполнении других задач ORM из командной строки.

   Для установки глобально (доступно в любом проекте на вашем компьютере):

```bash
	dotnet tool install --global dotnet-ef
```


   Или если вы предпочитаете установить инструменты локально для одного проекта:

```bash
   dotnet new tool-manifest # если вы еще не создали манифест инструментов в вашем проекте
   dotnet tool install dotnet-ef
```
   

5. (Опционально) Добавьте пакеты для поддержки миграций (если планируете использовать миграции для управления схемой своей базы данных):

```bash
   dotnet add package Microsoft.EntityFrameworkCore.Design
```
И для асинхронного доступа к базе данных можно добавить:

```bash
   dotnet add package Microsoft.EntityFrameworkCore.Tools
```

После добавления пакетов и инструментов в ваш проект, 
вы готовы приступить к использованию Entity Framework Core 
для работы с вашей базой данных, включая определение контекстов данных, 
сущностей и выполнения миграций.

