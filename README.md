import datetime

users = [
    {'username': 'kuzminator', 'password': '3388', 'role': 'admin', 'created_at': '2024-01-01'},
    {'username': 'dub', 'password': '123', 'role': 'user', 'history': [], 'created_at': '2024-01-02'}
]

services = [
    {'id': 1, 'name': 'Стандартный номер', 'price': 3000, 'rating': 4.5, 'available': True},
    {'id': 2, 'name': 'Люкс', 'price': 7000, 'rating': 4.8, 'available': True}
]

def authenticate():
    """Функция авторизации пользователя."""
    print("Добро пожаловать в систему управления гостиницей!")
    username = input("Логин: ").strip()
    password = input("Пароль: ").strip()

    for user in users:
        if user['username'] == username and user['password'] == password:
            print(f"Добро пожаловать, {username} ({user['role']})!\n")
            return user
    print("Неверный логин или пароль. Попробуйте снова.\n")
    return None

def view_services():
    """Просмотр доступных услуг."""
    print("Доступные услуги:\n")
    for service in services:
        print(f"ID: {service['id']} | Название: {service['name']} | Цена: {service['price']} руб. | Рейтинг: {service['rating']} | {'Доступно' if service['available'] else 'Недоступно'}")
    print()

def sort_services():
    """Сортировка услуг по критериям."""
    print("Критерии сортировки:")
    print("1. По цене")
    print("2. По рейтингу")
    choice = input("Выберите критерий: ").strip()
    if choice == '1':
        sorted_services = sorted(services, key=lambda x: x['price'])
    elif choice == '2':
        sorted_services = sorted(services, key=lambda x: x['rating'], reverse=True)
    else:
        print("Некорректный выбор.")
        return
    print("Отсортированные услуги:")
    for service in sorted_services:
        print(f"ID: {service['id']} | Название: {service['name']} | Цена: {service['price']} руб. | Рейтинг: {service['rating']}\n")

def user_menu(user):
    """Меню пользователя."""
    while True:
        print("Меню пользователя:")
        print("1. Просмотреть доступные услуги")
        print("2. Сортировать услуги")
        print("3. Посмотреть историю бронирований")
        print("4. Выйти")
        choice = input("Выберите действие: ").strip()

        if choice == '1':
            view_services()
        elif choice == '2':
            sort_services()
        elif choice == '3':
            print("Ваша история бронирований:")
            print(user.get('history', []))
        elif choice == '4':
            break
        else:
            print("Некорректный выбор. Попробуйте снова.\n")

def admin_menu():
    """Меню администратора."""
    while True:
        print("Меню администратора:")
        print("1. Добавить услугу")
        print("2. Удалить услугу")
        print("3. Редактировать услугу")
        print("4. Управление пользователями")
        print("5. Выйти")
        choice = input("Выберите действие: ").strip()

        if choice == '1':
            add_service()
        elif choice == '2':
            delete_service()
        elif choice == '3':
            edit_service()
        elif choice == '4':
            manage_users()
        elif choice == '5':
            break
        else:
            print("Некорректный выбор. Попробуйте снова.\n")

def add_service():
    """Добавление новой услуги."""
    name = input("Введите название услуги: ").strip()
    price = float(input("Введите цену услуги: ").strip())
    rating = float(input("Введите рейтинг услуги: ").strip())
    available = input("Услуга доступна? (yes/no): ").strip().lower() == 'yes'

    new_service = {
        'id': len(services) + 1,
        'name': name,
        'price': price,
        'rating': rating,
        'available': available
    }
    services.append(new_service)
    print("Услуга успешно добавлена!\n")

def delete_service():
    """Удаление услуги."""
    service_id = int(input("Введите ID услуги для удаления: ").strip())
    global services
    services = [service for service in services if service['id'] != service_id]
    print("Услуга успешно удалена!\n")

def edit_service():
    """Редактирование услуги."""
    service_id = int(input("Введите ID услуги для редактирования: ").strip())
    for service in services:
        if service['id'] == service_id:
            service['name'] = input(f"Введите новое название услуги (текущее: {service['name']}): ").strip() or service['name']
            service['price'] = float(input(f"Введите новую цену услуги (текущая: {service['price']}): ").strip() or service['price'])
            service['rating'] = float(input(f"Введите новый рейтинг услуги (текущий: {service['rating']}): ").strip() or service['rating'])
            service['available'] = input(f"Услуга доступна? (yes/no, текущее: {'да' if service['available'] else 'нет'}): ").strip().lower() == 'yes'
            print("Услуга успешно обновлена!\n")
            return
    print("Услуга с указанным ID не найдена.\n")

def manage_users():
    """Управление пользователями."""
    print("Функционал управления пользователями пока не реализован.\n")

def main():
    while True:
        user = authenticate()
        if user:
            if user['role'] == 'user':
                user_menu(user)
            elif user['role'] == 'admin':
                admin_menu()

if __name__ == "__main__":
    main()
