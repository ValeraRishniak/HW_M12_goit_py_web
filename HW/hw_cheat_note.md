коротка "шпаргалка" по виконанню ДЗ 12
    
    Налаштовуємо основу проєкту
1. Копіюємо проєкт з ДЗ 11 для підготовки проєкту ДЗ 12 (https://github.com/ValeraRishniak/HW_M11_goit_py_web).

    оновлюємо моделі 
1. Додаємо авторизацію та аутентифікацію в наш RESTful API застосунок
2. Створюємо модель користувача (User) у файлі src/database/models.py та робимо зв'язок між контактом та користувачем. Код з конспекту.

    Виконуємо міграції моделей

3. Запустити створення міграцій в корені проекту наступною консольною командою:
	alembic revision --autogenerate -m 'Тут пишемо коротний опис оновлень в моделях ('add user')' (початкова міграція - 'Init')
4. Якщо файл з міграцією успішно створився в папці migrations/versions, то застосуємо створену міграцію командою:
	alembic upgrade head

    Створюємо схеми валідації
1. У файлі src/schemas.py створюємо Pydantic-модель Usera, яка визначає «схему» або іншими словами дійсну форму даних та його відображення в базі даних. Код з конспекту.

    Створюємо репозиторій користувача
1. Створюємо файл src/repository/users.py в який додаємо функції щодо обробки Usera у базі даних (створення, витягання з БД, додавання токена). Код з конспекта.

    Сервіс аутентифікації та авторизації
1. Створимо файл/модуль src/services/auth.py та всередині визначимо клас служби аутентифікації Auth та визначаємо методи для підтримки операцій аутентифікації та авторизації. Код з конспекту.
2. Створюємо екземпляр класу auth_service = Auth() у файлі/модулі src/services/auth.py, який будемо використовувати у всьому коді для виконання операцій аутентифікації та авторизації. Код з конспекту.

    Маршрути аутентифікації
1. Створимо файл src/routes/auth.py для функцій аутентифікації та авторизації. Код з конспекту.
2. Підключаємо новий роутер у головному файлі застосунку main.py. Код аналогічний конспекту.

    Додаємо авторизацію
1. У файлі src/repository/contacts.py необхідно у кожній функції репозиторію  в параметрах зазначати про наявність користувача, а також під час запитів враховувати належність тегу конкретному користувачеві (filter(Contact.user_id == user.id)). Код аналогічний конспекту, відповідно до вимог ДЗ. 
2. У маршрути для роботи з контактами (src/routes/contacts.py) необхідно додати авторизацію за допомогою методу get_current_user класу Auth. Код аналогічний конспекту, відповідно до вимог ДЗ. 
3. Для кожного маршруту у файлі src/routes/contacts.py, де необхідна авторизація, потрібно додати параметр за допомогою залежності Depends(auth_service.get_current_user). Код аналогічний конспекту, відповідно до вимог ДЗ. 



Запускaємо сервер кодом із теки з проєктом (консоль у теці із файлом main.py): 
    uvicorn main:app --host localhost --port 8000 --reload

подальші дії робимо у postman за посиланням: 
    http://localhost:8000/відповідно_до_кожного_необхідного_завдання, 
    а саме:
            1. створюємо обліковий запис (http://localhost:8000/api/auth/signup)
                {
                "username": "FirstTestUser",
                "email": "FTU@i.ua",
                "password": "qwerty"
                }
            2. здійснюємо аутентифікацію користувача (http://localhost:8000/api/auth/login)
                {
                "username": "FTU@i.ua",
                "password": "qwerty"
                }
                в результаті отрмуємо access_token
            3. створюємо декілька контактів (http://localhost:8000/api/contacts/)
                {
                "first_name": "Reckless",
                "last_name": "Footballman",
                "email": "reckless@i.ua",
                "phone_number": "555-dont-call-me",
                "birthday": "2010-10-10",
                "additional_data": "I created for test"
                }
            4. отримуємо список усіх контактів (http://localhost:8000/api/contacts/all)
                відправляємо пустий запит -> отримуємо список контактів конкретного користувача
            5. отримуємо один контакт за ідентифікатором (http://localhost:8000/api/contacts/7)
                результат ->
                {
                "first_name": "John",
                "last_name": "Dow",
                "email": "JDtest@i.ua",
                "phone_number": "5551122",
                "birthday": "2000-10-10",
                "additional_data": "You create me for test",
                "id": 7
                }   
            6. оновлюємо контакт (знаходимо за ідентифікатором) (http://localhost:8000/api/contacts/8)
                результат ->
                {
                "first_name": "Reckless change",
                "last_name": "Footballman change",
                "email": "reckless@i.ua",
                "phone_number": "555-change ",
                "birthday": "2010-10-10",
                "additional_data": "I changed for test",
                "id": 8
                }   
            7. видаляємо контакт (знаходимо необхідний за ідентифікатором) (http://localhost:8000/api/contacts/remove/7)
                результат ->
                при здійснені запиту усіх контактів авторизованого користувача (http://localhost:8000/api/contacts/all) отримуємо лише контакт з ID - 8
            8. перевіряємо пошук за ім'ям, прізвищем та електронною поштою (http://localhost:8000/api/contacts/find/{query})
            9. перевіряємо пошук за кількістю днів до дня народження (http://localhost:8000/api/contacts/birthday/{days})

DONE!!!