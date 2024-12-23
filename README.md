import datetime
from tabulate import tabulate
from functools import reduce


users = [
    {
        'username': 'admin_user',
        'password': 'admin',
        'role': 'admin',
        'created_at': '2024-01-01'
    },
    {
        'username': 'john_doe',
        'password': 'user123',
        'role': 'user',
        'subscription_type': 'Premium',
        'booking_history': [],
        'created_at': '2024-09-01'
    }
]


rooms = [
    {'id': 1, 'name': 'Стандартный номер', 'price': 3000, 'rating': 4.2, 'date_added': '2024-12-01'},
    {'id': 2, 'name': 'Люкс', 'price': 7000, 'rating': 4.8, 'date_added': '2024-12-05'},
    {'id': 3, 'name': 'Семейный номер', 'price': 5000, 'rating': 4.5, 'date_added': '2024-12-10'}
]


def authenticate():
    print("Добро пожаловать в гостиницу!")
    while True:
        username = input("Логин (или 'exit' для выхода): ").strip()
        if username.lower() == 'exit':
            print("Выход из программы.")
            return None

        password = input("Пароль: ").strip()
        for user in users:
            if user['username'] == username and user['password'] == password:
                print(f"\nДобро пожаловать, {username} ({user['role']})!\n")
                return user

        print("Неверный логин или пароль. Попробуйте снова.")


def user_menu():
    while True:
        print("\nВыберите действие:")
        print("1. Просмотреть доступные номера")
        print("2. Найти номер")
        print("3. Сортировать номера по цене")
        print("4. Фильтровать номера по рейтингу")
        print("5. Вывести названия всех номеров")
        print("6. Подсчитать общую стоимость всех номеров")
        print("7. Выйти")

        choice = input("Ваш выбор: ").strip()
        if choice == '1':
            view_rooms()
        elif choice == '2':
            search_room()
        elif choice == '3':
            sort_rooms()
        elif choice == '4':
            filter_rooms_by_rating()
        elif choice == '5':
            list_room_names()
        elif choice == '6':
            calculate_total_cost()
        elif choice == '7':
            print("Выход из пользовательского меню.")
            break
        else:
            print("Некорректный выбор. Попробуйте снова.")

def view_rooms():
    print("\nДоступные номера:")
    headers = ["ID", "Название", "Цена", "Рейтинг", "Дата добавления"]
    table = [[r['id'], r['name'], r['price'], r['rating'], r['date_added']] for r in rooms]
    print(tabulate(table, headers=headers, tablefmt="grid"))

def search_room():
    keyword = input("Введите название номера для поиска: ").lower()
    results = [r for r in rooms if keyword in r['name'].lower()]
    if results:
        print("\nНайденные номера:")
        headers = ["ID", "Название", "Цена", "Рейтинг", "Дата добавления"]
        table = [[r['id'], r['name'], r['price'], r['rating'], r['date_added']] for r in results]
        print(tabulate(table, headers=headers, tablefmt="grid"))
    else:
        print("\nНомера не найдены.")

def sort_rooms():
    sorted_rooms = sorted(rooms, key=lambda r: r['price'])
    print("\nНомера отсортированы по цене:")
    headers = ["ID", "Название", "Цена", "Рейтинг", "Дата добавления"]
    table = [[r['id'], r['name'], r['price'], r['rating'], r['date_added']] for r in sorted_rooms]
    print(tabulate(table, headers=headers, tablefmt="grid"))

def filter_rooms_by_rating():
    try:
        min_rating = float(input("Введите минимальный рейтинг: "))
    except ValueError:
        print("Некорректный ввод. Рейтинг должен быть числом.")
        return

    filtered = [r for r in rooms if r['rating'] >= min_rating]
    if filtered:
        print("\nНомера с рейтингом выше или равным", min_rating)
        headers = ["ID", "Название", "Цена", "Рейтинг", "Дата добавления"]
        table = [[r['id'], r['name'], r['price'], r['rating'], r['date_added']] for r in filtered]
        print(tabulate(table, headers=headers, tablefmt="grid"))
    else:
        print("\nНет номеров с таким рейтингом.")

def list_room_names():
    names = [r['name'] for r in rooms]
    print("\nНазвания номеров:")
    for idx, name in enumerate(names, start=1):
        print(f"{idx}. {name}")

def calculate_total_cost():
    total_cost = sum(r['price'] for r in rooms)
    print(f"\nОбщая стоимость всех номеров: {total_cost} руб.")


current_user = authenticate()
if current_user:
    if current_user['role'] == 'user':
        user_menu()
    else:
        print("У администратора нет действий в данной версии.")
print("Выход из системы. До свидания!")
