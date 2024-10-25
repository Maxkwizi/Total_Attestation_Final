from datetime import datetime 

# Создание базового класс для всех животных
class Animals:
    def __init__(self, name, birth_date, animal_type):
        self.name = name  # Имя животного
        self.birth_date = birth_date # Дата рождения животного
        self.animal_type = animal_type
    def speak(self):
        return "Это животное издаёт такой звук:."  # Абстрактный метод 


# Создание подкласса для домашних животных
class Home_animals(Animals):
    def __init__(self, name, birth_date, animal_type):
        super().__init__(name, birth_date, animal_type)  # Инициализируем подкласс с родительским классом
        self.commands = []  # Список команд, которые знает животное

    def learn_command(self, command): 
        self.commands.append(command)  # Добавить команду в список

    def show_commands(self): # Показать команду
        return self.commands  


# Создание подкласса для вьючных животных
class Pack_animals(Animals):
    def __init__(self, name, birth_date, animal_type):
        super().__init__(name, birth_date, animal_type)  # Инициализируем подкласс с родительским классом
        self.commands = []  # Список команд, которые знает животное

    def learn_command(self, command): 
        self.commands.append(command)  # Добавить команду в список

    def show_commands(self):
        return self.commands  
    


# Конкретные классы домашних животных
class Собаки(Home_animals):
    def speak(self):
        return "Гав!"

class Кошки(Home_animals):
    def speak(self):
        return "Мяу!"

class Хомяки(Home_animals):
    def speak(self):
        return "Хрум!"

class Лошади(Pack_animals):
    def speak(self):
        return "Игого!"
    
class Ослы(Pack_animals):
    def speak(self):
        return "Иаа!"
    
class Верблюды(Pack_animals):
    def speak(self):
        return "Иэээ!" 


# Класс для счетчика количества животных с исключениями

class Counter:
    def __init__(self):
        self.count = 0  # Начальное значение счетчика
        self.used = False  # Флаг для отслеживания использования

    def add(self):
        if not self.used:  # Проверка на использование
            self.count += 1  # Увеличить счетчик
            self.used = True  # Устанавливаем флаг использования 

    def get_count(self):
        return self.count  # Возвращаем значение счетчика

    def __enter__(self):
        return self  # Возвращаем объект для использования в блоке with

    def __exit__(self, exc_type, exc_val, exc_tb):
        if not self.used:
            raise Exception("Счетчик животных не использовался.")  # Исключение, если счётчик не использован
        self.used = False  # Сбросить флаг счетчика



# Функция для основного меню
def main_menu():
    animals = []  # Список для хранения животных
    print()
    print("Добро пожаловать в зоопарк Лапы-хвост Рога-копыта!")

    while True:
        print("\nВыберите действие:")
        print("1. Завести новое животное")
        print("2. Показать список команд животного")
        print("3. Обучить животное новой команде")
        print("4. Перераспределить животное по типу")
        print("5. Показать всех животных")
        print("6. Выход")

        choice = input("Введите номер действия: ")

        # Завести новое животное
        if choice == '1':
            name = input("Введите имя животного: ")
            animal_type = input("Введите тип животного (собака, кошка, хомяк, лошадь, осёл, верблюд): ")
            birth_date = input("Введите дату рождения (в формате ГГГГ-ММ-ДД): ")

            # Проверка на заполненность полей
            if name and  animal_type and birth_date :
                counter = Counter()
                with counter:  # Работаем с счетчиком в блоке with
                    if animal_type == "собака":
                        animal = Собаки(name, animal_type, birth_date)
                    elif animal_type == "кошка":
                            animal = Кошки(name, animal_type, birth_date)
                    elif animal_type == "хомяк":
                            animal = Хомяки(name, animal_type, birth_date)
                    elif animal_type == "лошадь":
                            animal = Лошади(name, animal_type, birth_date)
                    elif animal_type == "осёл":
                            animal = Ослы(name, animal_type, birth_date)
                    elif animal_type == "верблюд":
                            animal = Верблюды(name, animal_type, birth_date)
                    else:
                        print("Неизвестный тип животного.")
                        continue 
                            
                    animals.append(animal)
                    counter.add()  # Увеличиваем счетчик
                    print(f"Вы завели новое животное: {animal.name} (Тип: {animal_type})")
                    print(f"Общее количество животных: {counter.get_count()}")
            else:
                print("Ошибка: Имя и тип животного не могут быть пустыми.")


        # Показать команды животного
        elif choice == '2':
            animal_name = input("Введите имя животного для показа команд: ")
            found = False
            for animal in animals:
                if animal.name == animal_name:
                    found = True
                    print("Команды, которые знает животное:", animal.show_commands())
                    break
            if not found:
                print("Животное не найдено.")

        # Обучить животное новой команде
        elif choice == '3':
            animal_name = input("Введите имя животного для обучения новой команде: ")
            command = input("Введите новую команду: ")
            for animal in animals:
                if animal.name == animal_name:
                    animal.learn_command(command)
                    print(f"{animal_name} научился новой команде: {command}")
                    break
            else:
                print("Животное не найдено.")

        # Переместить животное в другой тип
        elif choice == '4':
            animal_name = input("Введите имя животного для перераспределения: ")
            new_type = input("Введите новый тип животного (собака, кошка, хомяк, лошадь, осёл, верблюд): ")
            found = False
            for animal in animals:
                if animal.name == animal_name:
                    found = True
                    # Создаем новое животное и удаляем старое
                    if new_type == "собака":
                        new_animal = Собаки(animal.name)
                    elif new_type == "кошка":
                        new_animal = Кошки(animal.name)
                    elif new_type == "хомяк":
                        new_animal = Хомяки(animal.name)
                    elif new_type == "лошадь":
                        new_animal = Лошади(animal.name)
                    elif new_type == "осёл":
                        new_animal = Ослы(animal.name)
                    elif new_type == "верблюд":
                        new_animal = Верблюды(animal.name)
                    else:
                        print("Неизвестный новый тип животного.")
                        return
                    animals.remove(animal)  # Удаляем старое животное
                    animals.append(new_animal)  # Добавляем новое
                    print(f"Животное {animal_name} перераспрЙеделено в тип: {new_type}")
                    break
            if not found:
                print("Животное не найдено.")

        # Показать всех животных
        elif choice == '5':
            print("Список животных:")
            for animal in animals:
                commands = animal.show_commands()
                print(f"Имя: {animal.name}, Тип: {type(animal).__name__}, Команды: {commands}")
                print(f"Общее количество животных: {counter.get_count()}")

        # Выход их реестра
        elif choice == '6':
            print("Выход из реестра.")
            break

        else:
            print("Неизвестный выбор. Выберите действие из списка.")


if __name__ == "__main__":
    main_menu()