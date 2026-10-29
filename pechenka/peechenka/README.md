# ИИ от сбора до внедрения: кликер-бот на YOLOv8

## Структура проекта

```
cookie_ai/
├── app.py              # Основной бот: детекция + клики
├── train.py            # Обучение YOLOv8
├── screener.py         # Сбор датасета (скриншоты)
├── autolabel.py        # Авторазметка по эталонам
├── split_dataset.py    # Разбивка train/val (80/20)
├── data.yaml           # Конфиг датасета для YOLO
├── class.txt           # Список классов
├── requirements.txt    # Зависимости
├── yolov8n.pt          # Базовая модель YOLOv8n (pre-trained)
```

## Быстрый старт

```bash
# 1. Установка зависимостей
pip install -r requirements.txt

# 2. Сбор данных: играйте, чтобы попасть реклама, бонусы, улучшения
python screener.py                                

# 3. Создание эталонов (2-4 на класс)
python crop_template.py screenshorts/<файл>.png ad_close
python crop_template.py screenshorts/<файл>.png upgrade
python crop_template.py screenshorts/<файл>.png bonus
python crop_template.py screenshorts/<файл>.png game

# 4. Автоматическая разметка
python autolabel.py --preview preview
#    --thr 0.8  
#    --merge       
#    --model best.pt   

# 5. Проверка и правка в X-AnyLabeling (папка screenshorts)

# 6. Разбивка датасета и обучение
python split_dataset.py --clean
python train.py

# 7. Запуск бота (при необходимости поправьте путь к best.pt в app.py)
python app.py
```

## Классы объектов

| Класс | Описание |
|-------|----------|
| `game` | Игровое поле (печенька) |
| `ad_close` | Кнопка закрытия рекламы (крестик) |
| `upgrade` | Доступные улучшения (не серые) |
| `bonus` | Бонусы (золотые печеньки и др.) |


## Итеративное улучшение

После первого обучения:
1. Новые кадры размечаются моделью: `--model best.pt --merge`
2. Проверяются в preview/
3. Дообучаются: `python train.py`

## Разрешения и требования 
- Linux(надеюсь) / Windows
- Для macOS: разрешения Screen Recording + Accessibility для терминала

## Проект сделан с помощью ИИ (deepseek V4.1 flash)мяк
