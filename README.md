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
