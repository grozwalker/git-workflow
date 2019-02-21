### Rutorika: Работа с Git

#### Работа с ветками

* Каждая задача (фича\багфикс и т.д.) в новой ветке. При выполнении задач в руторике чаще всего приходится ветвится из ветки develop: `git checkout -b new-branch`

* Чтобы получить данные о новых ветках на удаленном репозитории, выполняем `git fetch`, но это не обновит текущую локальную ветку 

* Если мы хотим обновить текущую локальную ветку из удаленного репозитория, делаем `git pull origin branch-name --rebase`. Это предотвратит лишний merge-коммит и будет держать репозитрой в чистоте

* Когда задача доделана и нам надо слить свои изменения в ветку `develop`, например, то нужно выполнить такую последовательность команд
```git
> git checkout develop                  #переходим на ветку, в которую хотим произвести слияние
> git status                            #убеждаемся, что нет никаких изменений
> git pull origin develop --rebase      #забираем последние изменения
> git merge branch-name --no-ff         #--no-ff оставит память о ветке branch-name, чтобы потом легче смотреть по истории
```


#### Коммит
Когда задача закончена или мы по каким-то причинам решает сделать коммит.
 
`> git status` - смотрим изменения в рабочем дереве (обязательно проверяем, что в коммит не попадут папки вроде .idea, node_modules и прочее)

`> git add path/to/file` или `git add -A` (добавляем сразу все файлы, но надо внимательно посмотреть, чтобы ничего лишнего не попало)

`> git commit` - в окне редактора пишем осмысленное описание того, что сделали (не bugfix, а что было сделано).
Желательно оформлять коммит таким образом:
```
Commit subject          - Заголовок коммита
                        - Пустая строка
Commit body             - Тело коммита
```
Заголовок коммита:
 - начинается с большой буквы 
 - длина до 50 символов в конце точку не ставить
 - использование повелительного наклонения глагола (сделай, слей, исправь). Никаких fixed, changed и т.д.
 
 
Тело коммита:
 - Длина строки до 72 символа
 - Раскрывает что сделано и зачем, а не как это сделано 
 
 
Если вдруг вы в своей ЛОКАЛЬНОЙ ветке выполнили `git log` и совершенно случайно обнаружили список вот таких коммитов:
```git
bcdca61 fix 1
4643a5f fix header
e0ca8b9 update branch
fg8oo56 fixed header
```

То это не повод переносить все это в общий репозиторий, для этого нам надо обьеденить коммиты (засквошить)
```git
> Закоммитить или спрятать все изменения, если есть
> git log #Чтобы понимать какое количество коммитов надо надо обьеденить. В нашем случае 4
> git rebase -i HEAD~4 
```
Откроется диалоговое окно, где будет список из четырех коммитов и напротив каждого из них будет написано pick. Коммиты идут в порядке возрастания времени создания. Самый нижний - самый свежий.
```git
pick bcdca61 fix 1
pick 4643a5f fix header
pick e0ca8b9 update branch
pick fg8oo56 fixed header
```

Напротив тех коммитов, которые надо объеденить пишем squash

```git
pick bcdca61 fix 1
squash 4643a5f fix header
squash e0ca8b9 update branch
squash fg8oo56 fixed header
```
Закрываем окно редактора и в следующем окне вводим название общего коммита.

ВНИМАНИЕ! Это перезаписывает историю гита, можно выполнять только в ветке, где вы работаете один, ни в коем случае этого нельзя делать в ветке `develop` или `master`

