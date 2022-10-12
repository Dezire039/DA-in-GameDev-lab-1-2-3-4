# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Чашкин Никита Андреевич
- РИ210941

Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

## Цель работы
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity.

Подключил API для работы с Google Sheets:

![image](https://user-images.githubusercontent.com/87475288/194066762-45a42abe-1534-4dd1-a63a-d9d8f1b14d71.png)

Python код для записи данных из скрипта в Google Sheets:

```py
import gspread
import numpy as np

gc = gspread.service_account(filename='unitydatascience-364516-d5142e967d57.json')
sh = gc.open("UnitySheets")
price = np.random.randint(2000, 10000, 11)
for i in range(1, 12):
    tempInf = str(((price[i - 1] - price[i - 2]) / price[i - 2]) * 100)
    tempInf = tempInf.replace('.', ',')
    sh.sheet1.update(f'A{i}', str(i))
    sh.sheet1.update(f'B{i}', str(price[i - 1]))
    sh.sheet1.update(f'C{i}', str(tempInf))
    print(f'{i}\t{tempInf}')

```

Создал Unity проект и написал скрипт для получения данных из Google Sheets:

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new();
    private bool statusStart = false;
    private int i = 1;

    void Start()
    {
        StartCoroutine(GoogleSheets());
    }
    
    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp =
            UnityWebRequest.Get(
                "https://sheets.googleapis.com/v4/spreadsheets/1rwDRyBppb0PxRqmkm4YcOo6ly1daXr3dHR3ULpX8HMg/values/Sheet1?key=*API-Ключ*");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }
```

Добавил функционал для воспроизведения аудио в зависимости от полученных данных:

```C#
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false &
            i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }
    
    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
```

## Задание 2: Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1

Создал второй лист, изменил код из задания #1 для корректной записи данных в Google Sheets

```py
import numpy as np
import gspread


def model(a, b, x):
    return a * x + b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5 / num) * (np.square(prediction - y)).sum()


def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0 / num) * ((prediction - y) * x).sum()
    db = (1.0 / num) * (prediction - y).sum()
    a = a - Lr * da
    b = b - Lr * db
    return a, b


def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b


gc = gspread.service_account(filename='unitydatascience-364516-d5142e967d57.json')
sh = gc.open("UnitySheets")
x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]
y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
x, y = np.array(x), np.array(y)
n_iterations = 1
initialized = False
worksheet = sh.get_worksheet(1)


for i in range(2, 9):
    if not initialized:
        worksheet.update('A1', "Значение a")
        worksheet.update('B1', "Значение b")
        worksheet.update('C1', "Количество итераций")
        worksheet.update('D1', "Потеря (loss)")
        initialized = True

    a, b = np.random.rand(1), np.random.rand(1)
    Lr = 0.000_000_01

    a, b = iterate(a, b, x, y, n_iterations)
    prediction = model(a, b, x)
    loss = loss_function(a, b, x, y)

    a_value = a[0]
    b_value = b[0]
    worksheet.update(f'A{i}', a_value)
    worksheet.update(f'B{i}', b_value)
    worksheet.update(f'C{i}', n_iterations)
    worksheet.update(f'D{i}', loss)
    print(f"a: {a_value}, b: {b_value}, iterations: {n_iterations}, loss: {loss}")
    n_iterations *= 10

```
Результат выполнения скрипта:

![image](https://user-images.githubusercontent.com/87475288/194067976-abddbf3d-3f56-4513-8264-b8c2aad7edef.png)


## Задание 3
### - Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2

Изменил код скрипта так, чтобы он подтягивал данные со второго листа, изменил значения при которых будут воспроизводиться конкретные аудиодорожки:
```C#
    void Update()
    {
        if (statusStart || i == dataSet.Count + 1) return;
        if (dataSet["Mon_" + i] >= 1000)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i]);
        }
        if (dataSet["Mon_" + i] >= 190 && dataSet["Mon_" + i] < 1000)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i]);
        }

        if (!(dataSet["Mon_" + i] <= 190)) return;
        
        StartCoroutine(PlaySelectAudioGood());
        Debug.Log(dataSet["Mon_" + i]);
    }
    
    IEnumerator GoogleSheets()
    {
        var currentResp =
            UnityWebRequest.Get(
                "https://sheets.googleapis.com/v4/spreadsheets/1rwDRyBppb0PxRqmkm4YcOo6ly1daXr3dHR3ULpX8HMg/values/Sheet2?key=*API-Ключ*");
        yield return currentResp.SendWebRequest();
        var rawResp = currentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[4]));
        }
    }
```
Вывод в консоль:

![image](https://user-images.githubusercontent.com/87475288/194089444-8bbfba4b-1306-4678-be38-6db36ae5bebe.png)

## Выводы
Получил навык работы с Google Sheets как с интсрументом для анализа данных, научился заносить и парсить данные с помощью Google Sheets. К тому же меня 5 раз назвали тираном, но только 1 раз доверились мне и 1 раз восхитились.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
