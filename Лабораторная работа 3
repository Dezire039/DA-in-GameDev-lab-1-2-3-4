# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev] 3 ЛАБОРАТОРНАЯ РАБОТА. РАЗРАБОТКА СИСТЕМЫ МАШИННОГО ОБУЧЕНИЯ
Отчет по лабораторной работе #3 выполнил(а):
- Буторина Анна Георгиевна
- РИ211002
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)
Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨


## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.
Ход работы:


- Создайте новый пустой 3D проект на Unity.
<img width="764" alt="2022-10-20 (1)" src="https://user-images.githubusercontent.com/114075427/196886358-a8bed892-da2d-48b4-8640-fa1363ba14e9.png">



- В созданный проект добавьте ML Agent, выбрав Window - PackageManager - Add Package from disk. Последовательно добавьте .json – файлы:
o ml-agents-release_19 / com,unity.ml-agents / package.json
o ml-agents-release_19 / com,unity.ml-agents.extensions / package.json

- Далее запускаем Anaconda Prompt для возможности запуска команд через консоль. Далее пишем серию команд для создания и активации нового ML-агента, а также для скачивания необходимых библиотек:
o mlagents 0.28.0;
o torch 1.7.1;

- Создайте на сцене плоскость, куб и сферу. Создайте простой C# скрипт-файл и подключите его к сфере:
<img width="956" alt="2022-10-20 (2)" src="https://user-images.githubusercontent.com/114075427/196889277-4e0a2840-83e2-4443-acdc-55ab0493d044.png">



- В скрипт-файл RollerAgent.cs добавьте код, опубликованный в материалах лабораторных работ

```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```

- Объекту «сфера» добавить компоненты Decision Requester, Behavior Parameters

![Снимок экрана (12 1)](https://user-images.githubusercontent.com/81166835/194755184-5ba5b4b8-bf67-4992-9df3-5b38e3f2826d.png)

- В корень проекта добавьте файл конфигурации нейронной сети

```py
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

- Запустите работу ml-агента

![Снимок экрана (15 1)](https://user-images.githubusercontent.com/81166835/194754553-ec0a1776-2c71-41da-9820-d87992b622eb.png)

- Вернитесь в проект Unity, запустите сцену, проверьте работу ML-Agent’a
![1](https://user-images.githubusercontent.com/114075427/196890497-dccafee6-4128-4896-8472-26f3e82749b4.png)

![2](https://user-images.githubusercontent.com/114075427/196890513-4971b9fd-5da2-465e-9d2b-df4d01471df8.png)

- Сделайте 3, 9, 27 копий модели «Плоскость-Сфера-Куб», запустите симуляцию сцены и наблюдайте за результатом обучения модели
![3](https://user-images.githubusercontent.com/114075427/196890780-6f1b21b7-612a-4b74-b7f9-c8c6cf95c307.png)


- После завершения обучения проверьте работу модели.

![4](https://user-images.githubusercontent.com/114075427/196891066-37cd81ca-18b1-423e-a422-2ebaf7eb8b13.png)



- Вывод: Модель успешно прошла обучение




## Задание 2
### Подробно описать каждую строку файла конфигурации нейронной сети, доступного в папке с файлами проекта по ссылке. Самостоятельно найти информацию о компонентах Decision Requester, Behavior Parameters, добавленных на сфере.
Ход работы:

```py
behaviors: #создание списка "Модель поведения" дял разных агентов
  RollerBall: #создание списка конкретного объекта
    trainer_type: ppo #тип используемого тренажера, ppo - это алгоритм обучения с подкреплением от OpenAi
    hyperparameters:
      batch_size: 10 #количество опытов на каждой итерации градиентного спуска, оно должно быть в несколько раз меньше, чем buffer_size
      buffer_size: 100 #количество опыта, которое необходимо собрать перед обновлением модели политики
      learning_rate: 3.0e-4 #начальная скорость обучения
      beta: 5.0e-4 #сила регуляризации энтропии, которая делает политику "более случайной". 
      epsilon: 0.2 #влияет на то, насколько быстро политика может развиваться во время обучения.
      lambd: 0.99 #параметр регуляризации (лямбда), используемый при вычислении обобщенной оценки преимуществ (GAE).
      num_epoch: 3 #количество проходов через буфер опыта при выполнении оптимизации градиентного спуска.Чем больше batch_size, тем больше допустимо сделать это. 
      learning_rate_schedule: linear #определяет, как скорость обучения меняется с течением времени. linear линейно уменьшает скорость обучения
    network_settings:
      normalize: false #применяется ли нормализация к входным данным векторного наблюдения
      hidden_units: 128 #Количество единиц в скрытых слоях нейронной сети
      num_layers: 2 #Количество скрытых слоев в нейронной сети
    reward_signals: #награды
      extrinsic: #внешние награды
        gamma: 0.99 #кщэффициент скидки для будущих вознаграждений, поступающих из среды
        strength: 1.0 #коэффициент, на который умножается вознаграждение, предоставляемое средой
    max_steps: 500000 #общее количество шагов 
    time_horizon: 64 #сколько шагов опыта нужно собрать для каждого агента, прежде чем добавлять его в буфер опыта
    summary_freq: 10000 #количество опыта, которое необходимо собрать перед созданием и отображением статистики обучения
```
DecisionRequester делает гибкий способ запуска процесса принятия решений агентом, без него внедрение нашего агента агента должно вручную вызывать функцию RequestDecision()
Компонент Behavior Parameters определяет, как объект принимает решения



## Задание 3
### Доработать сцену и обучить ML-Agent таким образом, чтобы шар перемещался между двумя кубами разного цвета. Кубы должны, как и в первом задании, случайно изменять координаты на плоскости.
Ход работы:




## Выводы
Игровой баланс — в играх субъективное «равновесие» между персонажами, командами, тактиками игры и другими игровыми объектами. Машинное обучение используется для настройки игрового баланса, с помощью обученных агентов с большим колличеством симуляций происходит сбор данных. Данная методика основанная на ML для тестирования игр позволяет отрегулировать баланс в игре и, как следствие, повысить интерес к прохождению данной игры.


| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
