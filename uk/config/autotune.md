# Автоналаштування

Автоналаштування автоматизує процес налаштування контролерів швидкості та ставлення PX4, які є найважливішими контролерами для стабільного та реактивного польоту (інші налаштування є більш "необов'язковими"). Наразі це увімкнено для багатокоптерних, фіксованих крил та гібридних ВТОЛ фіксованих крилових літаків.

Настроювання потрібно робити лише один раз і рекомендується, якщо ви використовуєте транспортний засіб, який вже був налаштований виробником (і не змінювався з того часу).

:::warning
Автоналаштування виконується під час польоту. Каркас повинен добре літати, щоб впоратися з помірними перешкодами, і повинен бути уважно контрольований:

- Перевірте, що ваш автомобіль достатньо [стабільний для автоналаштування](#pre-tuning-test).
- Будьте готові перервати процес автоналагодження. Ви можете зробити це, змінивши режими польоту або використовуючи перемикач увімкнення / вимкнення автонаведення ([якщо налаштовано](#enable-disable-autotune-switch-fixed-wing)).
- Перевірте, що автомобіль добре літає після налаштування.

:::

@[youtube](https://youtu.be/5xswOhhqrIQ)

## Попереднє налаштування тесту

Транспортний засіб повинен бути здатний літати і належним чином стабілізувати себе перед запуском автоматичного налаштування. Цей тест дозволяє забезпечити безпечний польот транспортного засобу в режимах управління положенням.

:::info Під час [Налаштування рами](../config/airframe.md) ви повинні були вибрати раму, яка найбільше відповідає вашому транспортному засобу. Можливо, це досить добре працюватиме для запуску автоналаштування.
:::

Переконайтеся, що транспортний засіб достатньо стабільний для автоналаштування:

1. Виконайте звичайний контрольний перелік безпеки перед польотом, щоб переконатися, що зона польоту чиста і має достатньо місця.
1. Зльот та підготовка до тесту
   - **Багатокоптери:** Злітайте та зависайте на висоті 1 м над землею в режимі [Режим висоти](../flight_modes_mc/altitude.md) або стабілізованому режимі.
   - **Фіксований крило:** Злітайте та летіть з крейсерською швидкістю в режимі [Режим позиції](../flight_modes_mc/position.md) або [Режим висоти](../flight_modes_mc/altitude.md).
1. Використовуйте палицю кочення пульта керування RC для виконання наступного маневру, нахиливши транспортний засіб лише на кілька градусів: _нахил ліворуч > нахил праворуч > центр_ (Весь маневр повинен зайняти близько 3 секунд). Транспортний засіб повинен стабілізуватися протягом 2 коливань.
1. Повторіть маневр, нахиляючись з більшими амплітудами при кожної спроби. Якщо транспортний засіб може стабілізуватися протягом 2 коливань під кутом близько 20 градусів, перейдіть до наступного кроку.
1. Повторіть ті ж маніпуляції, але по осі поля. Як вище, почніть з невеликих кутів і підтвердіть, що транспортний засіб може стабілізуватися самостійно протягом 2 коливань, перш ніж збільшувати нахил.

Якщо безпілотник може стабілізувати себе протягом 2 коливань, він готовий до процедури автоматичного налаштування.

Якщо ні, перейдіть до розділу [вирішення проблем](#troubleshooting), де пояснюється мінімальна ручна настройка для підготовки автомобіля до автоматичної настройки.

### Процедура автоналагодження

Послідовність автоматичного налаштування повинна бути виконана в **безпечній зоні польоту, з достатньою площею**. Це займає близько 40 секунд ([між 19 і 68 секундами](#how-long-does-autotuning-take)). Для найкращих результатів ми рекомендуємо проводити тестування в спокійні погодні умови.

Рекомендовані режими для автоналагодження - [Режим утримання](../flight_modes_fw/hold.md) (FW) та [Режим висоти](../flight_modes_mc/altitude.md) (MC), але можна використовувати будь-який інший режим польоту. Під час автоматичного налаштування RC палиці все ще можна використовувати для польоту транспортного засобу.

:::info Послідовність автоматичного налаштування можна перервати у будь-який момент, змінивши режими польоту або використовуючи [ввімкнути / вимкнути перемикач автоматичного налаштування](#enable-disable-autotune-switch-fixed-wing) (якщо налаштовано).
:::

Кроки наступні:

1. Виконайте [тест передналаштування](#pre-tuning-test).
1. Зліт за допомогою керування RC та підготовка до тесту:
   - **Багатокоптери:** Зліт за допомогою пульта дистанційного керування в [Режимі висоти](../flight_modes_mc/altitude.md). Наведіть транспортний засіб на безпечній відстані та на кілька метрів над землею (між 4 та 20 м).
   - **Планер:** Після польоту з крейсерською швидкістю активуйте [Режим утримання](../flight_modes_mc/hold.md). Це допоможе літаку летіти по колу на постійній висоті та швидкості.
1. Enable autotune.

:::tip
Якщо налаштовано [перемикач увімкнення/вимкнення автоналаштування](#enable-disable-autotune-switch-fixed-wing), ви можете просто перемкнути перемикач у положення "увімкнено".
:::

   1. У QGroundControl відкрийте меню: **Налаштування Транспортного Засобу > Налаштування PID**

      ![Tuning Setup > Autotune Enabled](../../assets/qgc/setup/autotune/autotune.png)

   1. Виберіть вкладки _Контролер швидкості_ або _Контролер нахилу_.
   1. Переконайтеся, що кнопка увімкнення **Автопідгонки** увімкнена (це відобразить кнопку **Автопідгонки** та видалить селектори ручного налаштування).
   1. Прочитайте спливаюче вікно попередження та натисніть на **OK**, щоб почати налаштування.

1. Дрон спочатку почне виконувати швидкі рухи кочення, а потім рухи тангажу та рухи курсу. Прогрес відображається на панелі прогресу, поруч з кнопкою _Автоналадка_.
1. Застосувати налаштування:

   - **Фіксований крило:** Налаштування буде негайно/автоматично застосовано й перевірено в польоті (за замовчуванням). PX4 потім проведе 4-секундний тест і поверне нове налаштування, якщо буде виявлено проблему.
   - **Багатороторники:** Вручну приземліться та роззбройтеся, щоб застосувати нові параметри налаштування. Піднімайтеся обережно і вручну перевіряйте, що транспортний засіб стійкий.

1. Якщо виникають сильні коливання, негайно приземліться й дотримуйтеся інструкцій у розділі [Усунення неполадок](#troubleshooting) нижче.

Додаткові примітки:

- **VTOL:** Гібридні ВТОЛ фіксовані крила повинні бути налаштовані двічі, слідуючи інструкціям для багтороторників у режимі MC та інструкціям для фіксованих крил у режимі FW.
- **Багатокоптер:** Інструкції вище налаштовують транспортний засіб у [Режим висоти](../flight_modes_mc/altitude.md). Замість цього ви можете здійснити зльот у режимі [Зльоту](../flight_modes_mc/takeoff.md) та налаштувати режим [Позиції](../flight_modes_mc/position.md), якщо _відомо_, що транспортний засіб стабільний в цих режимах.
- **Фіксований крило:** Автоналагодження також може бути запущено в режимі [Режим висоти](../flight_modes_fw/altitude.md) або [Режим позиції](../flight_modes_fw/position.md). Проте виконання тесту під час прямого польоту потребує більшої безпечної зони для налаштування і не дає значно кращого результату налаштування.
- Чи налаштування застосовується у повітрі чи після посадки можна [налаштувати за допомогою параметрів](#apply-parameters-when-in-air-landed).

## Вирішення проблем

#### Дрон коливається при виконанні випробувальних маневрів перед автоматичним налаштуванням

- повільні осциляції (1 осциляція на секунду або повільніше): це часто відбувається на великих платформах і означає, що петля утримання позиції рухається занадто швидко порівняно з петлею швидкості.
  - **Мультикоптер:** зменшити [MC_ROLL_P](../advanced_config/parameter_reference.md#MC_ROLL_P) та [MC_PITCH_P](../advanced_config/parameter_reference.md#MC_PITCH_P) на 1,0 одиницю.
  - **Фіксований крило:** збільшити [FW_R_TC](../advanced_config/parameter_reference.md#FW_R_TC) та [FW_P_TC](../advanced_config/parameter_reference.md#FW_P_TC) на кроки по 0.1.
- швидкі коливання (більше 1 коливання на секунду): це тому, що підсилення петлі швидкості занадто високе.
  - **Мультикоптер:** зменшити `MC_[ROLL|PITCH|YAW]RATE_K` на 0,02 одиниці
  - **Фіксований крило:** зменшити [FW_RR_P](../advanced_config/parameter_reference.md#FW_RR_P), [FW_PR_P](../advanced_config/parameter_reference.md#FW_PR_P), [FW_YR_P](../advanced_config/parameter_reference.md#FW_YR_P) на кроки по 0.01.

#### Послідовність автоматичної настройки не вдається

Якщо безпілотник не рухався достатньо під час автоматичного налаштування, алгоритм ідентифікації системи може мати проблеми з визначенням правильних коефіцієнтів. Збільште [FW_AT_SYSID_AMP](../advanced_config/parameter_reference.md#FW_AT_SYSID_AMP), [MC_AT_SYSID_AMP](../advanced_config/parameter_reference.md#MC_AT_SYSID_AMP) на одиницю і знову викличте автоматичне налаштування.

#### Дрон коливається після автоналагодження

Через вплив ефектів, які не враховані в математичній моделі, такі як затримки, насичення, швидкість наростання, гнучкість конструкції, коефіцієнт підсилення петлі може бути занадто великим. Щоб виправити це, слідувати тим самим крокам, описаним [коли дрон коливається в попередньому тесті перед налаштуванням автоматичного налаштування](#the-drone-oscillates-when-performing-the-testing-maneuvers-prior-to-the-auto-tuning).

#### Я все ще не можу зрозуміти, як це працює

Спробуйте налаштувати вручну, використовуючи відповідні посібники:

- [Посібник з налаштування PID для багатокоптерів](../config_mc/pid_tuning_guide_multicopter_basic.md) (Керівництво/Простий)
- [Посібник з налаштування PID для багатокоптерів](../config_mc/pid_tuning_guide_multicopter.md) (Advanced/Detailed)
- [Посібник з налаштування ПІД-регулятора з нерухомим крилом](../config_fw/pid_tuning_guide_fixedwing.md)

## Необов'язкова Конфігурація

### Застосовувати параметри у повітрі/приземленні

За замовчуванням транспортні засоби MC приземляються до застосування параметрів, тоді як транспортні засоби FW застосовують параметри у повітрі, а потім перевіряють, що контролери працюють належним чином. Цю поведінку можна налаштувати за допомогою параметрів [MC_AT_APPLY](../advanced_config/parameter_reference.md#MC_AT_APPLY) та [FW_AT_APPLY](../advanced_config/parameter_reference.md#FW_AT_APPLY) відповідно:

- `0`: надбавки не застосовуються. Це використовується для тестування, якщо користувач хоче перевірити результати автоналаштування алгоритму без прямого їх використання.
- `1`: застосувати здобутки після роззброєння (типово для мультироторів). Оператор може перевірити нове налаштування під час обережного зльоту.
- `2`: застосувати негайно (за замовчуванням для фіксованих відгуків). Нове налаштування застосовується, перешкоди надсилаються контролеру, а стабільність контролюється протягом наступних 4 секунд. Якщо керуюче коло нестійке, керуючі коефіцієнти негайно повертаються до свого попереднього значення. Якщо тест пройшов успішно, пілот може використовувати нове налаштування.

### Увімкнути/вимкнути перемикач автотюнінгу (фіксований крило)

Пульт дистанційного керування може бути налаштований для увімкнення / вимкнення автодоналаштування (у будь-якому режимі) за допомогою додаткового каналу RC.

Для відображення перемикача:

1. Виберіть канал RC на вашому контролері для використання перемикача увімкнення / вимкнення автоналадки.
1. Встановіть [RC_MAP_AUX1](../advanced_config/parameter_reference.md#RC_MAP_AUX1) для відповідності каналу RC вашому перемикачу (ви можете використовувати будь-який з `RC_MAP_AUX1` по `RC_MAP_AUX6`).
1. Встановіть [FW_AT_MAN_AUX](../advanced_config/parameter_reference.md#FW_AT_MAN_AUX) на вибраний канал (тобто `1: Aux 1`, якщо ви відобразили `RC_MAP_AUX1`).

Автоналаштування буде вимкнено, коли перемикач знаходиться нижче `0.5` (в діапазоні ручного керування від `[-1, 1]` та увімкнено, коли канал перемикача знаходиться вище `0.5`.

Якщо використовується перемикач RC AUX для увімкнення автоналагодження, переконайтеся, що ви [вибрали вісі налаштування](#select-tuning-axis-fixed-wing) перед польотом.

### Виберіть Ось Тюнінгу (Фіксований Крило)

Літальні апарати з фіксованим крилом (тільки) можуть вибирати, які вісі налаштовуються за допомогою параметра бітової маски [FW_AT_AXES](../advanced_config/parameter_reference.md#FW_AT_AXES):

- біт `0`: кидок (за замовчуванням)
- біт `1`: кидок (за замовчуванням)
- біт `2`: крен

## Розробники/SDKs

Автоналаштування починається за допомогою команди MAVLink [MAV_CMD_DO_AUTOTUNE_ENABLE](https://mavlink.io/en/messages/common.html#MAV_CMD_DO_AUTOTUNE_ENABLE).

На момент написання повідомлення його пересилаються на регулярні інтервали для опитування PX4 на предмет прогресу: `COMMAND_ACK` включає результат, що операція в процесі виконання, а також прогрес у відсотках. Операція завершується, коли прогрес досягає 100% або транспортний засіб приземляється і роззброюється.

:::info Це не є виконанням протоколу довгострокової команди [командного протоколу довгострокової команди](https://mavlink.io/en/services/command.html#long_running_commands) у відповідності до MAVLink. PX4 повинен транслювати прогрес, оскільки протокол не дозволяє опитування.
:::

Функція ще не підтримується MAVSDK.

## Background/Detail

PX4 використовує [PID контролери](../flight_stack/controller_diagrams.md) (швидкість, ставлення, швидкість та положення), щоб розрахувати вихідні дані, необхідні для переміщення транспортного засобу з його поточного оціненого стану, щоб відповідати бажаному заданому значенню. Контролери повинні бути добре налаштовані, щоб отримати найкращу продуктивність з автомобіля. Зокрема, погано налаштований регулятор швидкості призводить до менш стабільного польоту у всіх режимах і потребує більше часу на відновлення після перешкод.

Загалом, якщо ви використовуєте [конфігурацію рами](../config/airframe.md), яка схожа на ваш транспортний засіб, то транспортний засіб зможе літати. Однак, якщо конфігурація точно не відповідає вашому обладнанню, вам слід налаштувати регулятори швидкості та кута нахилу. Налаштування контролерів швидкості та позиції менш важливе, оскільки вони менше піддаються динаміці транспортного засобу, і типова конфігурація налаштування для схожого аеродинамічного корпусу часто є достатньою.

Автоналаштування забезпечує автоматичний механізм для налаштування регуляторів швидкості та кута нахилу. Це можна використовувати для налаштування літаків з фіксованим крилом та  мультикоптерних транспортних засобів, а також літаків VTOL, коли літають як мультикоптерний транспортний засіб або з фіксованим крилом (перехід між режимами повинен бути налаштований вручну). Теоретично це повинно працювати для інших типів транспортних засобів, які мають регулятор швидкості, але наразі підтримуються лише вищезазначені типи.

Автоматична настройка працює добре для конфігурацій багатокоптерів та фіксованих крил, які підтримує PX4, за умови, що рама не занадто гнучка (див. [нижче для отримання додаткової інформації](#does-autotuning-work-for-all-supported-airframes)).

Транспортний засіб повинен перебувати в режимі стабілізації висоти (такому як [Режим висоти](../flight_modes_mc/altitude.md), [Режим утримання](../flight_modes_mc/hold.md) або [Режим позиції](../flight_modes_mc/position.md)). Стек польоту застосує невелике збурення до транспортного засобу в кожній з осей, а потім спробує розрахувати нові налаштувальні параметри. Для літаків нове налаштування застосовується в повітрі за замовчуванням, після чого транспортний засіб перевіряє нові налаштування і повертає налаштування, якщо контролери нестабільні. Для мультикоптера транспортний засіб приземляється і застосовує нові параметри налаштування після відбронювання; пілот повинен обережно злетіти і протестувати налаштування.

Процес налаштування займає близько 40 секунд ([між 19 і 68 секундами](#how-long-does-autotuning-take)). Стандартна поведінка може бути налаштована за допомогою [параметрів](#optional-configuration).

### FAQ

#### Які типи кадрів підтримуються?

Автоналаштування увімкнено для мультикоптерів, фіксованих крил та гібридних VTOL фіксованих крилових літаків.

Хоча це ще не активовано для інших типів кадрів, в теорії його можна використовувати з будь-яким кадром, який використовує контролер швидкості.

#### Чи працює автоналадка для всіх підтримуваних конструкцій?

Математична модель, яку використовує автоналаштування для оцінки динаміки дрона, передбачає, що це лінійна система без зв'язку між осями (SISO) та з обмеженою складністю (2 полюси та 2 нулі). Якщо справжній безпілотник занадто далеко від цих умов, модель не зможе відтворити реальну динаміку безпілотника.

На практиці, автоналаштування, як правило, добре працює для планерів та мультикоптерів, за умови, що рама не занадто гнучка.

#### Як довго триває автоналагодження?

Налаштування займає 5-20 секунд на вісь (припиняється, якщо налаштування не вдалося встановити за 20 секунд) + пауза 2 секунди між кожною віссю + 4 секунди тестування, якщо нові коефіцієнти застосовано у повітрі.

Мультикоптер повинен налаштовувати всі три осі, і за замовчуванням не перевіряє нові виграші у повітрі. Налаштування, отже, займе від 19 с (`5 + 2 + 5 + 2 + 5`) до 64 с (`20x3 + 2x2`).

За замовчуванням літак налагоджує всі три осі, а потім перевіряє нові коефіцієнти в повітрі. Діапазон становить від 25 с (`5 + 2 + 5 + 2 + 5 + 2 + 4`) до 70 с (`20x3 + 3x2 + 4`).

Зверніть увагу, що вищезазначені налаштування є значеннями за замовчуванням. Мультикоптер може вибрати проведення тестів в повітрі, а планер може вибрати не робити цього. Додатково, літак з фіксованим крилом може вибрати налаштувати менше вісей.

За анекдотичними даними, зазвичай це займає близько 40 секунд для будь-якого засобу пересування.


<!--
#### How vigorous is the disturbance applied by tuning

This might be added later. I'd like to just point to a video.

If not, perhaps say "not very" but you should expect that the vehicle might deflect by as much as 20degrees and so should be able to cope with that deflection with default tuning.

-->

## Дивись також

- [Посібник з налаштування PID для багатокоптерів](../config_mc/pid_tuning_guide_multicopter_basic.md) (Керівництво/Простий)
- [Посібник з налаштування PID для багатокоптерів](../config_mc/pid_tuning_guide_multicopter.md) (Advanced/Detailed)
- [Посібник з налаштування ПІД-регулятора з нерухомим крилом](../config_fw/pid_tuning_guide_fixedwing.md)
