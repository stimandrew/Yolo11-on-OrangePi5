# Yolo11-on-OrangePi5
##Конвертер модели onnx->rknn
# Установка инструмента для конвертации файлов в формат .rknn
Версии:

1. Ubuntu 24
2. python 3-12.3
3. rknn-toolkit2-2.3.2

## Пункт 1. Cоздание виртуального окружения python

1. Устанавливаем venv и pip

	```bash
	sudo apt-get install python3-venv
	sudo apt install python3-pip
	```

2. Создаём папку для виртуального окружения

	```bash
	mkdir ~/virtualenv
	cd ~/virtualenv
	```

3. Создаём виртуальное окружение

	```bash
	python3 -m venv .
	```

4. Активируем виртуальное окружение
	
	```bash
	source bin/activate	
	```

5. Устанавливаем необходимые зависимоти

	Нам необходимо скачать или получить файл `requirements_cp312-2.3.2.txt` 
из репозитория [rknn-toolkit2]
(https://github.com/airockchip/rknn-toolkit2/tree/master)
по пути `./rknn-toolkit2/packages/x86_64/`. Помещаем этот файл в папку 
виртуального окружения и выполняем команду

	```bash
	wget https://github.com/airockchip/rknn-toolkit2/blob/master/rknn-toolkit2/packages/x86_64/requirements_cp312-2.3.2.txt
	pip install -r ./requirements_cp312-2.3.2.txt
	```

6. Устанавливаем модуль для расширения
	
	Из директории, которая была в прошлом пункте получаем файл 
`rknn_toolkit2-2.3.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`,
	скачиваем в папку с виртуальным окружением и устанавливаем его с помощью команды

	```bash
 	wget https://github.com/airockchip/rknn-toolkit2/blob/master/rknn-toolkit2/packages/x86_64/rknn_toolkit2-2.3.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
	pip install ./rknn_toolkit2-2.3.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
	```

## Пункт 2. Настройка конвертера

1. Скачиваем скрипт-конвертер 

	В репозитории [rknn-zoo](https://github.com/airockchip/rknn_model_zoo/tree/main) 
по пути `./examples/yolo11/python/` скачиваем в папку с виртуальным окружением 
файл `convert.py`.

	```bash
	wget https://github.com/airockchip/rknn_model_zoo/blob/main/examples/yolo11/python/convert.py
 	```

3. Скачиваем дополнительные файлы из датасета для конвертации *(пока неясно зачем,
но нужно)*

	Для этого полностью переносим папку `datasets` из прошлого репозитория 
в виртуальное окружение.
	```bash
	wget https://github.com/Femidis/Yolo11-on-OrangePi5/releases/download/v1.0/datasets.tar.xz
 	tar -xvf datasets.tar.xz
 	```
 
	Заходим в convert.py и меняем 4 строку которая 
выглядит вот так:

	```python
	DATASET_PATH = '../../../datasets/COCO/coco_subset_20.txt'
	```
	на

	```python
 	DATASET_PATH = './datasets/COCO/coco_subset_20.txt'
	```
## Пункт 3. Конвертация модели
Теперь для выполнения конвертации необходимо с активированным виртуальным 
окружением выполнить следующую команду

```bash
python3 convert.py /path/to/cnn.onnx rk3588 i8 /path/to/result/cnn.rknn
```











