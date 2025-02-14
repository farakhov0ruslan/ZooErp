

# ERP-система для зоопарка

## Обзор проекта
Данное консольное приложение представляет собой ERP-систему для Московского зоопарка, которая обеспечивает автоматизацию учёта животных и инвентарных предметов, проверку состояния здоровья животных с помощью ветеринарной клиники, а также вывод разнообразных отчётов (количество животных, общее потребление еды, список инвентарных объектов и т.д.). Приложение организовано с использованием принципов SOLID и DI-контейнера для упрощения тестирования и масштабирования.

## Структура проекта

```
ZooERP/
├── Program.cs              // Главный файл приложения, где настраивается DI и вызывается меню.
├── Interfaces/             // Интерфейсы IAlive, IInventory, IVeterinaryClinic.
├── Models/                 
│   ├── Animals/            // Доменные классы животных.
│   │   ├── Animal.cs       // Базовый класс Animal (номер генерируется автоматически).
│   │   ├── Herbos/         
│   │   │   ├── Herbo.cs    // Абстрактный класс для травоядных.
│   │   │   └── Monkey.cs   // Пример конкретного животного.
│   │   └── Predators/      
│   │       ├── Predator.cs // Абстрактный класс для плотоядных.
│   │       ├── Tiger.cs    
│   │       ├── Wolf.cs     
│   │       └── Rabbit.cs   // Пример животного (травоядное, наследник Herbo).
│   └── InventoryItems/     // Доменные классы для инвентарных предметов.
│       ├── Thing.cs        // Базовый класс Thing (номер генерируется автоматически).
│       ├── Table.cs        
│       └── Computer.cs     
└── Processing/             
    ├── IOController.cs     // Утилиты для цветного вывода и ввода в консоль.
    ├── MenuProcessing.cs   // Класс для вывода главного меню и корректного ввода номера действия.
    ├── ZooMenu.cs          // Класс для операций над зоопарком (добавление, вывод и т.д.).
    └── VeterinaryClinic.cs // Класс ветеринарной клиники с улучшенным, цветным выводом.
```

## Применение принципов SOLID

### Single Responsibility Principle (Принцип единственной ответственности)
- **Animal и Thing:** Каждый класс отвечает за хранение своих данных (например, количество потребляемой еды, уникальный номер) и не занимается дополнительной логикой.
- **Zoo:** Отвечает за управление списками животных и инвентарных предметов, а также за бизнес-логику приема животных (с проверкой ветеринаром).
- **VeterinaryClinic:** Ответственна только за проверку состояния здоровья животных и вывод результата проверки.
- **IOController:** Отвечает исключительно за работу с консолью (цветной вывод, ввод и разделители).
- **MenuProcessing и ZooMenu:** Разделяют логику работы с меню и непосредственное выполнение операций над зоопарком.

### Open/Closed Principle (Принцип открытости/закрытости)
- Классы Animal, Thing и их наследники реализованы таким образом, что для добавления новых типов животных или инвентарных предметов не требуется изменять уже существующий код.
- Интерфейсы (IAlive, IInventory, IVeterinaryClinic) позволяют расширять функциональность системы без изменения существующих компонентов.

### Liskov Substitution Principle (Принцип подстановки Лисков)
- Все классы, наследующие Animal, могут быть использованы в зоопарке без необходимости проверки их конкретного типа.
- Инвентарные предметы, реализующие IInventory, используются в общем списке инвентаризации, что позволяет заменять их любыми объектами, соответствующими данному интерфейсу.

### Interface Segregation Principle (Принцип разделения интерфейса)
- Интерфейсы разделены на небольшие и специализированные: IAlive отвечает за учёт потребляемой еды, IInventory – за хранение инвентарного номера, а IVeterinaryClinic – за проверку состояния здоровья.
- Это позволяет классам реализовывать только необходимый функционал без избыточных зависимостей.

### Dependency Inversion Principle (Принцип инверсии зависимостей)
- Класс Zoo зависит от абстракции IVeterinaryClinic, а не от конкретной реализации. Это позволяет легко менять или расширять функциональность ветеринарной клиники.
- DI-контейнер (Microsoft.Extensions.DependencyInjection) используется для внедрения зависимостей, что облегчает тестирование и масштабирование приложения.

## Автоматическая инкрементация номеров
- **Для животных:** В базовом классе Animal реализован статический счетчик, который при создании каждого нового животного автоматически присваивает уникальный инвентарный номер.
- **Для инвентарных предметов:** Аналогичная логика реализована в базовом классе Thing, что позволяет не запрашивать у пользователя номер, а автоматически генерировать его.
  
## Установка зависимостей
  Для корректной работы проекта необходимо установить пакет Microsoft.Extensions.DependencyInjection.
  Для этого выполните следующую команду в корневой папке проекта:

```
dotnet add package Microsoft.Extensions.DependencyInjection
```
Этот пакет используется для внедрения зависимостей (Dependency Injection), что упрощает тестирование и масштабирование приложения.
## Инструкция по запуску
1. Установите .NET SDK (например, .NET 6 или .NET 7).
2. Откройте проект в Visual Studio или другом IDE.
3. Соберите проект и запустите его через командную строку с помощью команды:
   ```
   dotnet run
   ```
4. Следуйте инструкциям в консоли для работы с ERP-системой зоопарка.

## Заключение
В проекте применены принципы SOLID, что позволяет создавать модульный, расширяемый и легко поддерживаемый код. Использование DI-контейнера упрощает тестирование и замену зависимостей, а автоматическая инкрементация номеров для животных и инвентарных предметов делает работу с приложением удобной для пользователя.

---

Этот readme-файл даёт полное представление о структуре проекта и обосновании архитектурных решений, принятых при разработке ERP-системы для зоопарка.
