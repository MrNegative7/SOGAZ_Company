# Biometric ATM (Биометрический банкомат)
Во всех банках люди постоянно снимают деньги с банкоматов, используя 
при этом свою карту для ввода PIN-кода и выбором дальнейших операции. 
Сейчас все чаще и чаще существуют мошенники, которые способны 
украсть карту, получить незаметно PIN-код или даже получить допуск к 
банкомату для получения денег
Эксперты уточняют, что если злоумышленники будут использовать 
«плоскую картинку», где изображен человек в 2D, то легко смогут 
получить или воспроизвести мошенники, для получения данных о карте. 
При использовании 3D-изображения риск ниже, так как аферистам 
придется снимать человека с разных точек. Также неясно, как 
перевыпускать биометрический шаблон в случае его подделки. Если карту 
можно заменить, то с лицом клиента этого не сделаешь, отметили 
аналитики. В большинстве случаев, сейчас немало обсуждают о заказе 
новых банкоматах, в которых будут встроены биометрические АТМ, когда 
люди смогут снимать свои деньги используя при этом свои 
биометрические персональные данные
### Идея проекта
В биометрических ATM можно совершать операции без 
пластиковой карты: банкомат распознает лицо или человека по его 
внешности. Однако перед этим клиенту нужно будет предоставить 
кредитной организации свои биометрические данные, которые будут 
привязаны к счетам. Банкоматы нового поколения повысят безопасность 
операций, а также сделают процесс более удобным и быстрым, считают 
эксперты. Однако россияне пока недоверчиво относятся к биометрии, 
поэтому большой популярностью такие услуги в ближайшее время 
пользоваться не будут.
Обнаружение лица на фотографии относится к нахождению координат 
лица на изображении, тогда как локализация относится к разграничению 
границ лица, часто с помощью ограничивающей рамки вокруг лица.
### Работа алгоритма Biometric ATM
Работа алгоритма банковской системы:
- К данному алгоритму посылаются номер кредитной карты пользователя 
для записи в определенную БД. После ввода нужно вписать имя владельца 
карты.
- Затем алгоритм попросит Вас показать свое лицо на камеру в центр 
экрана. Лицо можно двигать медленно, чтобы модель нейронной сети 
смогла определить углы лица и построить ограничительную рамку для 
распознавания лица по аутентификации.
- Чтобы воспроизвести запись в базу данных, нужно включить веб-камеру. 
Модель нейронной сети воспроизведет запись около 3 секунд, чтобы 
определить лицо человека. При запуске веб-камеры записываются 
некоторые тензоры матриц из пакета Pytorch.
- запись происходит в папку, когда пользователь 
указывал номер карты. В нем хранится видеозапись пользователя для 
дальней двухфакторной аутентификации по карте. Видеозапись длится 
около 3 секунд. Саму видеозапись пришлось зашифровать “сторонним” 
форматом, чтобы злоумышленники не смогли получить доступ к данным и 
не “отравить” их
- Далее, если запустить второй код детекции, то модель нейронной сети 
попросит пользователю ввести данные карты. После этого модель 
включит веб-камеру и наложит ограничительную рамку распознавания 
лица. На первый план выходит имя владельца карты, а на второй якобы 
условная вероятность появления лица владельца карты. Например: "Процент совпадения - 90%, Авторизация прошла успешно".
### Комментарий к файлам .py
В этом репозитории предоставляются два рабочих файла формата .py.
- Первый файл (`atm_input_images.py`) представляет собой программный код с предобученной моделью MTCNN распознавания лиц для ввода биометрических данных в базу. При запуке кода пользователь
вводит номер своей карты и имя владельца карты. Система попросит держать свое лицо в центре экрана и медленно поворачивать его, чтобы запечатлеть разные углы обзора. Затем через 3 секунды появится окно с передачей видеоизображения с веб-камеры компьютера и алгоритм будет высчитывать тензоры матриц с PyTorch. Аутентификация будет происходить по следующим параметрам: глаза, нос, рот, шея и уши.
- Второй файл (`atm_detection.py`) представляет собой программный код проверки аутентфикации лица по номеру карты. Пользователь, при запуске кода, вводит номер своей карты. Если номер карты существует в базе, то через 3 секунды система создаст окно с передачей видеоизображения с веб-камеры компьютера и построенной ограничительной рамки на лице, иначе система скажет, что такого польователя не существует. После детекции лица алгоритм выдаст процент совпадения и результат аутентифкации
### Вывод по данному проекту
Архитектура MTCNN в моих программных кодах достаточно сложна для реализации. К счастью, 
существуют реализации архитектуры с открытым исходным кодом, 
которые можно обучать на новых наборах данных, а также 
предварительно обученные модели, которые можно использовать 
непосредственно для обнаружения лиц. С одной стороны, если противники знают, как работает целевая модель, 
они могут использовать ее для нахождения слабого места модели и 
соответственно инициирования атаки. С другой стороны, если 
разработчики модели знают, как модель работает, они могли определить 
уязвимость и работать над исправлением заранее. Интерпретация 
относится к понятной человеку информации, объясняющей, что имеет 
модель. В данном случае в этом алгоритме я смог защитить видеоданные 
владельца карты, чтобы злоумышленник не смог никоем образом открыть 
и испортить данные. И его можно выпускать в прод для клиентов кредитной организации
# P.S. Подробнее о работе алгоритма см. отчет `Bank systems.pdf`
