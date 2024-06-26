#задача1
# Открываем исходный файл для чтения
with open('devices.txt', 'r') as file:
    lines = file.readlines()

# Создаем словарь для хранения данных
data = {}

# Обходим каждую строку в файле
for line in lines:
    line = line.strip().split('*')
    # Используем тип устройства и объем оперативной памяти в качестве ключа
    key = (line[2], line[5])
    # Увеличиваем счетчик для данного типа устройства и объема памяти
    data[key] = data.get(key, 0) + 1

# Открываем файл для записи
with open('count_company.txt', 'w') as file:
    for device_type in ['Ultrabook', 'Notebook', 'Netbook']:
        file.write(f'{device_type}\n')
        # Получаем только данные для данного типа устройства
        filtered_data = {k: v for k, v in data.items() if k[0] == device_type}
        for ram, count in filtered_data.items():
            file.write(f'{ram[1]} - {count}\n')
        file.write('\n')

# Выводим характеристику для ультрабука
ultrabook_data = {k: v for k, v in data.items() if k[0] == 'Ultrabook'}
for ram, count in ultrabook_data.items():
    print(f'{ram[1]} - {count}')

#задача2
# Открываем исходный файл для чтения
with open('devices.txt', 'r') as file:
    lines = file.readlines()

# Создаем список для хранения данных
data = []

# Обходим каждую строку в файле и добавляем в список
for line in lines:
    data.append(line.strip().split('*'))

# Сортируем список по названию компании в обратном алфавитном порядке
sorted_data = sorted(data, key=lambda x: x[0], reverse=True)

# Выводим первые пять компаний в формате: <Company> - <Product> - <Price>
for i in range(5):
    print(f'{sorted_data[i][0]} - {sorted_data[i][1]} - {sorted_data[i][8]}')
    
#задача3
# Функция для чтения данных из файла и преобразования их в структуру данных
def read_data_from_file(file_name):
    data = []
    with open(file_name, 'r') as file:
        for line in file:
            data.append(tuple(line.strip().split('*')))
    return data


# Функция для двоичного поиска самого дорогого устройства по названию компании
def binary_search_most_expensive_device(data, company):
    low = 0
    high = len(data) - 1
    max_price = -1
    result = None

    while low <= high:
        mid = (low + high) // 2
        if data[mid][0] == company:
            if int(data[mid][8]) > max_price:
                max_price = int(data[mid][8])
                result = data[mid]

        if data[mid][0] < company:
            low = mid + 1
        else:
            high = mid - 1

    return result


# Чтение данных из файла
file_name = 'devices.txt'
data = read_data_from_file(file_name)

# Запрос названия компании и поиск самого дорогого устройства
while True:
    company = input("Введите название компании (для завершения введите '='): ")
    if company == '=':
        break

    result = binary_search_most_expensive_device(data, company)

    if result is not None:
        print(f"По вашему запросу: {result[0]} найдены следующие варианты:")
        print(
            f"{result[0]} {result[1]} - тип устройства: {result[2]}; Разрешение экрана - {result[3]}; Цена - {result[8]}")
    else:
        print("У нас нет данного устройства")

#задача4
# Функция для чтения данных из файла и преобразования их в структуру данных
def read_data_from_file(file_name):
    data = []
    with open(file_name, 'r') as file:
        for line in file:
            data.append(tuple(line.strip().split('*')))
    return data


# Функция для подсчета суммарной прибыли от продажи продукции каждой компании
def calculate_total_profit(data):
    companies_profit = {}

    for entry in data:
        if entry[0] in companies_profit:
            companies_profit[entry[0]] += int(entry[8])  # Увеличиваем суммарную прибыль компании
        else:
            companies_profit[entry[0]] = int(entry[8])  # Инициализируем суммарную прибыль компании

    return companies_profit


# Чтение данных из файла
file_name = 'devices.txt'
data = read_data_from_file(file_name)

# Подсчет суммарной прибыли от продажи продукции каждой компании
companies_profit = calculate_total_profit(data)

# Вывод информации о суммарной прибыли каждой компании
for company, profit in companies_profit.items():
    print(f"Если продать все ноутбуки {company} можно заработать {profit}.")

#задача5
# Импорт необходимых библиотек
from collections import defaultdict

# Функция для чтения данных из файла и формирования хэш-таблицы
def create_hash_table(file_name):
    hash_table = defaultdict(int)
    with open(file_name, 'r') as file:
        for line in file:
            company, product, price = line.strip().split('*')[0], line.strip().split('*')[1], int(line.strip().split('*')[8])
            key = company + " " + product
            hash_table[key] = price
    return hash_table

# Чтение данных из файла и формирование хэш-таблицы
file_name = 'devices.txt'
hash_table = create_hash_table(file_name)

# Вывод первых 10 значений сформированной таблицы
count = 0
for key, value in hash_table.items():
    if count < 10:
        company, product = key.split()
        print(f"{company} {product} {value}")
        count += 1
    else:
        break

