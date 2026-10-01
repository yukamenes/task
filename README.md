# Урок: Створення системи управління кораблем та пострілів

## Що ми сьогодні зробимо

Минулого разу ми створили **каркас гри** на KivyMD:

- створили кілька екранів;
- налаштували `MDScreenManager`;
- навчили програму перемикатися між екранами;
- налаштували тему KivyMD.

Сьогодні ми перетворимо цей каркас на просту гру:

1. створимо головне меню;
2. додамо картинку космічного корабля;
3. створимо ігровий екран;
4. додамо кнопки керування кораблем;
5. навчимо корабель рухатися вліво та вправо;
6. створимо систему пострілів;
7. навчимося створювати кулі під час натискання кнопки;
8. зробимо так, щоб кулі летіли вгору;
9. розділимо програму на логічні частини.

> **Важливо:** сьогодні ми працюємо тільки з двома файлами: `main.py` та `shooter.kv`.
>
> Файл `shooter.kv` створимо самостійно під час цього уроку.

---

# 1. Як має виглядати проєкт наприкінці

Спочатку у вас є приблизно така структура:

```text
назва_проєкту/
└── main.py
```

Після виконання всіх кроків вона має стати такою:

```text
назва_проєкту/
├── main.py
├── shooter.kv
└── assets/
    └── images/
        └── rocket.png
```

### Що тут знаходиться?

- `main.py` — Python-код гри.
- `shooter.kv` — опис зовнішнього вигляду інтерфейсу.
- `assets/images/rocket.png` — картинка нашого космічного корабля.

---

# 2. Створюємо папки для картинки

У VS Code відкрийте папку вашого проєкту.

У ній створіть:

```text
assets
```

Усередині `assets` створіть:

```text
images
```

У результаті:

```text
назва_проєкту/
├── main.py
└── assets/
    └── images/
```

Тепер покладіть у папку `images` файл із кораблем.

Назвемо його:

```text
rocket.png
```

У результаті повинно бути:

```text
assets/images/rocket.png
```

## Якщо у вас вже є картинка з іншою назвою

Наприклад:

```text
spaceship.png
```

можна або перейменувати її на:

```text
rocket.png
```

або всюди в коді замінити:

```text
rocket.png
```

на назву вашого файлу.

---

# 3. Як правильно взяти шлях до картинки у VS Code

Це важливий момент.

Не потрібно вручну писати довгі шляхи на кшталт:

```text
C:\Users\Student\Desktop\MyGame\assets\images\rocket.png
```

Такі шляхи залежать від конкретного комп'ютера.

Ми використовуємо **відносний шлях**.

## Зробіть так:

1. У VS Code відкрийте панель **Explorer**.
2. Знайдіть файл `rocket.png`.
3. Натисніть на ньому **правою кнопкою миші**.
4. Оберіть **Copy Relative Path**.
5. Вставте отриманий шлях у код.

У нашому випадку він має виглядати приблизно так:

```text
assets/images/rocket.png
```

Саме цей шлях ми використовуватимемо в `.kv`-файлі.

> **Запам'ятайте:** якщо програма не знаходить картинку, перше, що потрібно перевірити, — правильність шляху до неї.

---

# 4. Створюємо файл `shooter.kv`

У папці, де знаходиться `main.py`, створіть новий файл:

```text
shooter.kv
```

У результаті:

```text
назва_проєкту/
├── main.py
├── shooter.kv
└── assets/
    └── images/
        └── rocket.png
```

## Чому файл називається саме `shooter.kv`?

Наш клас програми називається:

```python
class ShooterApp(MDApp):
```

Kivy автоматично намагається знайти файл розмітки, назва якого пов'язана з назвою класу:

```text
ShooterApp → shooter.kv
```

Тому **не називайте цей файл `main.kv`**, якщо ми не змінюємо логіку підключення KV.

---

# 5. Спочатку перевіримо картинку

У файл `shooter.kv` вставте:

```kv
#:set color_cosmos 0.2, 0, 0.1, 1

<MainScreen>:
    MDBoxLayout:
        orientation: 'vertical'
        padding: dp(50)
        spacing: dp(30)

        MDIconButton:
            icon: "cog"
            pos_hint: {"right": 1, "top": 1}
            icon_size: dp(50)

        Image:
            source: 'assets/images/rocket.png'
            size_hint_y: 0.7
            size_hint_x: 1
            allow_stretch: True

        MDRectangleFlatButton:
            text: "PLAY"
            font_size: sp(60)
            pos_hint: {'center_x': 0.5}
            on_press:
                root.manager.current = 'game'
```

Запустіть програму.

Якщо все зроблено правильно, програма повинна запуститися без помилки, а на головному екрані має з'явитися картинка корабля та кнопка `PLAY`.

---

# 6. Що ми щойно написали?

## `<MainScreen>:`

```kv
<MainScreen>:
```

Це правило KV-файлу для нашого Python-класу:

```python
class MainScreen(MDScreen):
```

Тобто ми описуємо, **як має виглядати `MainScreen`**.

---

## `MDBoxLayout`

```kv
MDBoxLayout:
    orientation: 'vertical'
```

`MDBoxLayout` — це контейнер, який розташовує елементи один за одним.

Значення:

```text
vertical
```

означає вертикальне розташування.

---

## `padding`

```kv
padding: dp(50)
```

Відступ від країв контейнера.

`dp()` — одиниця вимірювання Kivy, яка допомагає робити розміри більш передбачуваними на різних екранах.

---

## `spacing`

```kv
spacing: dp(30)
```

Відстань між елементами.

---

## `Image`

```kv
Image:
    source: 'assets/images/rocket.png'
```

`Image` показує картинку.

`source` — шлях до файлу картинки.

---

## Кнопка PLAY

```kv
MDRectangleFlatButton:
    text: "PLAY"
```

Це кнопка.

А ця частина:

```kv
on_press:
    root.manager.current = 'game'
```

означає:

> коли користувач натискає кнопку, змінити поточний екран на `game`.

Саме такий екран ми зараз створимо.

---

# 7. Повертаємося до `main.py`

Зараз у вас є код приблизно такого типу:

```python
from kivymd.app import MDApp
from kivymd.uix.screen import MDScreen
from kivymd.uix.screenmanager import MDScreenManager


class MenuScreen(MDScreen):
    pass


class GameScreen(MDScreen):
    pass


class SettingsScreen(MDScreen):
    pass


class ShooterApp(MDApp):
    def build(self):
        self.theme_cls.theme_style = "Dark"
        self.theme_cls.primary_palette = "Purple"

        self.sm = MDScreenManager()

        self.sm.add_widget(MenuScreen(name="menu"))
        self.sm.add_widget(GameScreen(name="game"))
        self.sm.add_widget(SettingsScreen(name="settings"))

        return self.sm


app = ShooterApp()
app.run()
```

Тепер ми змінимо цей код.

---

# 8. Змінюємо назву меню

Замість:

```python
class MenuScreen(MDScreen):
    pass
```

напишіть:

```python
class MainScreen(MDScreen):
    pass
```

### Навіщо?

Тепер головний екран називатиметься `MainScreen`.

У KV-файлі ми вже написали:

```kv
<MainScreen>:
```

Назви в Python та KV повинні відповідати одна одній.

---

# 9. Додаємо необхідні імпорти

На початку `main.py` нам знадобляться додаткові можливості.

Замініть верхню частину файлу на:

```python
from kivymd.app import MDApp
from kivymd.uix.widget import MDWidget
from kivymd.uix.screenmanager import MDScreenManager
from kivymd.uix.screen import MDScreen

from kivy.clock import Clock
from kivy.metrics import dp
from kivy import platform
from kivy.core.window import Window
```

## Для чого кожен імпорт?

### `MDApp`

```python
from kivymd.app import MDApp
```

Основний клас для створення KivyMD-програми.

---

### `MDWidget`

```python
from kivymd.uix.widget import MDWidget
```

Базовий клас для нашої кулі.

Куля буде окремим об'єктом на екрані.

---

### `MDScreenManager`

```python
from kivymd.uix.screenmanager import MDScreenManager
```

Керує екранами програми.

---

### `MDScreen`

```python
from kivymd.uix.screen import MDScreen
```

Дозволяє створювати окремі екрани.

---

### `Clock`

```python
from kivy.clock import Clock
```

`Clock` дозволяє виконувати функцію через певні проміжки часу.

Це буде потрібно для руху корабля та куль.

---

### `dp`

```python
from kivy.metrics import dp
```

Використовуємо `dp()` для розмірів і швидкостей.

---

### `platform`

```python
from kivy import platform
```

Дозволяє перевірити, на якій платформі запущена програма.

Наприклад:

```text
android
windows
linux
macos
```

---

### `Window`

```python
from kivy.core.window import Window
```

Дозволяє налаштовувати розмір вікна програми на комп'ютері.

---

# 10. Створюємо константи швидкості

Після імпортів напишіть:

```python
FPS = 60

BULLET_SPEED = dp(10)
SHIP_SPEED = dp(5)
```

## Що таке `FPS`?

```python
FPS = 60
```

FPS — frames per second, тобто кількість оновлень за секунду.

Ми будемо оновлювати стан гри приблизно 60 разів на секунду.

---

## Швидкість кулі

```python
BULLET_SPEED = dp(10)
```

Кожного оновлення куля рухатиметься вгору на певну кількість пікселів.

---

## Швидкість корабля

```python
SHIP_SPEED = dp(5)
```

Це швидкість переміщення корабля вліво або вправо.

> Згодом ви зможете експериментувати з цими значеннями.

---

# 11. Створюємо `MainScreen`

Після констант напишіть:

```python
class MainScreen(MDScreen):
    pass
```

Це головний екран гри.

Його зовнішній вигляд ми описали у `shooter.kv`.

---

# 12. Створюємо клас кулі

Тепер найважливіша нова частина.

Напишіть:

```python
class Shot(MDWidget):
    pass
```

## Що таке `Shot`?

`Shot` — це один постріл.

Коли користувач натискає кнопку пострілу, програма буде створювати **новий об'єкт класу `Shot`**.

Наприклад:

```text
Shot 1
Shot 2
Shot 3
Shot 4
```

Кожна куля буде окремим об'єктом.

---

# 13. Описуємо зовнішній вигляд кулі в `shooter.kv`

Відкрийте `shooter.kv`.

Після опису `MainScreen` додайте:

```kv
<Shot>:
    size_hint: None, None
    size: dp(20), dp(20)
    md_bg_color: app.theme_cls.primary_color
```

Отже, в KV-файлі тепер є:

```kv
<MainScreen>:
    ...

<Shot>:
    size_hint: None, None
    size: dp(20), dp(20)
    md_bg_color: app.theme_cls.primary_color
```

## Що це означає?

```kv
size_hint: None, None
```

Кажемо Kivy:

> не визначай розмір автоматично.

Далі ми самі задаємо:

```kv
size: dp(20), dp(20)
```

Тобто куля буде квадратом приблизно `20 × 20`.

---

# 14. Створюємо ігровий екран

У `main.py` після `Shot` напишіть:

```python
class GameScreen(MDScreen):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

        Clock.schedule_interval(self.update, 1 / FPS)

        self.eventkeys = {}
        self.cartridge = []
```

Тут відбувається кілька важливих речей.

---

# 15. Що таке `__init__`?

```python
def __init__(self, *args, **kwargs):
```

`__init__` — це метод, який виконується під час створення об'єкта.

Коли створюється:

```python
GameScreen()
```

Python викликає:

```python
__init__()
```

Ми використовуємо його, щоб підготувати ігровий екран до роботи.

---

# 16. Навіщо `super()`?

```python
super().__init__(*args, **kwargs)
```

Ми передаємо керування батьківському класу `MDScreen`.

Тобто ми не ламаємо стандартну логіку `MDScreen`, а доповнюємо її власною.

---

# 17. Запускаємо оновлення гри

```python
Clock.schedule_interval(self.update, 1 / FPS)
```

Це одна з найважливіших команд уроку.

Вона говорить:

> викликай метод `update` приблизно 60 разів на секунду.

Тому:

```python
FPS = 60
```

а:

```python
1 / FPS
```

дає приблизно:

```text
0.0167 секунди
```

Тобто оновлення відбувається приблизно кожні 16,7 мс.

---

# 18. Система натиснутих кнопок

Створюємо словник:

```python
self.eventkeys = {}
```

У ньому будемо зберігати стан кнопок.

Наприклад:

```python
{
    'left': True,
    'right': False,
    'shot': False
}
```

Це означає:

- `left` натиснута;
- `right` не натиснута;
- `shot` не натиснута.

---

# 19. Магазин для куль

Створюємо список:

```python
self.cartridge = []
```

`cartridge` — це умовний магазин/список усіх куль, які зараз існують у грі.

Наприклад:

```python
[
    bullet1,
    bullet2,
    bullet3
]
```

Коли ми створюємо нову кулю, додаємо її до цього списку.

---

# 20. Створюємо метод `update`

Усередині `GameScreen` після `__init__` додайте:

```python
    def update(self, dt):
        for key in self.eventkeys:
            if self.eventkeys[key] == True:
                if key == 'left':
                    self.moveLeft()

                if key == 'right':
                    self.moveRight()

                if key == 'shot':
                    self.shot()
                    self.eventkeys[key] = False

        # Керування кулями
        for bullet in self.cartridge:
            bullet.pos[1] += BULLET_SPEED
```

> **Увага на відступи!**
>
> `def update` знаходиться всередині `GameScreen`, тому перед ним має бути **4 пробіли**.
>
> Код усередині `update` має ще **4 додаткові пробіли**.

---

# 21. Як працює `update()`?

Спочатку:

```python
for key in self.eventkeys:
```

Програма перебирає всі кнопки, які є у словнику.

Потім:

```python
if self.eventkeys[key] == True:
```

перевіряє:

> ця кнопка зараз натиснута?

Якщо так — виконується відповідна дія.

---

# 22. Рух вліво

```python
if key == 'left':
    self.moveLeft()
```

Якщо натиснута кнопка `left`, викликається:

```python
self.moveLeft()
```

---

# 23. Рух вправо

```python
if key == 'right':
    self.moveRight()
```

Якщо натиснута кнопка `right`, викликається:

```python
self.moveRight()
```

---

# 24. Постріл

```python
if key == 'shot':
    self.shot()
    self.eventkeys[key] = False
```

Якщо натиснута кнопка `shot`, викликаємо:

```python
self.shot()
```

Після цього:

```python
self.eventkeys[key] = False
```

скидаємо стан кнопки.

Це потрібно для того, щоб один натиск створював один постріл.

---

# 25. Рух куль

Нижче ми перебираємо всі кулі:

```python
for bullet in self.cartridge:
    bullet.pos[1] += BULLET_SPEED
```

У Kivy позиція має координати:

```text
x — горизонтальна координата
y — вертикальна координата
```

Тому:

```python
bullet.pos[1]
```

— це координата `y`.

Якщо збільшувати `y`, об'єкт рухається вгору.

Отже:

```python
bullet.pos[1] += BULLET_SPEED
```

означає:

> кожне оновлення гри піднімай кулю вгору.

---

# 26. Створюємо керування кнопками

Після `update()` додайте:

```python
    def pressKey(self, key):
        self.eventkeys[key] = True

    def releaseKey(self, key):
        self.eventkeys[key] = False
```

## `pressKey`

```python
def pressKey(self, key):
    self.eventkeys[key] = True
```

Коли кнопку натиснули — записуємо:

```text
True
```

---

## `releaseKey`

```python
def releaseKey(self, key):
    self.eventkeys[key] = False
```

Коли кнопку відпустили — записуємо:

```text
False
```

---

# 27. Створюємо рух корабля

Додайте:

```python
    def moveLeft(self):
        self.ids.ship.pos[0] -= SHIP_SPEED

    def moveRight(self):
        self.ids.ship.pos[0] += SHIP_SPEED
```

Тут використовується:

```python
self.ids.ship
```

`ids` дозволяє Python-коду звернутися до елемента, якому ми дали `id` у KV-файлі.

Ми трохи нижче створимо:

```kv
id: ship
```

тому в Python можемо написати:

```python
self.ids.ship
```

---

# 28. Як працює рух?

Для руху вліво:

```python
self.ids.ship.pos[0] -= SHIP_SPEED
```

Зменшуємо `x`.

Для руху вправо:

```python
self.ids.ship.pos[0] += SHIP_SPEED
```

Збільшуємо `x`.

Отже:

```text
x зменшується → ліворуч
x збільшується → праворуч
```

---

# 29. Створюємо постріл

Останній метод `GameScreen`:

```python
    def shot(self):
        shot = Shot(
            pos=(self.ids.ship.center_x, self.ids.ship.top)
        )

        self.cartridge.append(shot)
        self.ids.front.add_widget(shot)
```

## Що відбувається?

Спочатку:

```python
shot = Shot(...)
```

створюємо нову кулю.

Її початкова позиція:

```python
pos=(self.ids.ship.center_x, self.ids.ship.top)
```

Тобто куля з'являється біля верхньої частини корабля.

---

# 30. Додаємо кулю до магазину

```python
self.cartridge.append(shot)
```

`append()` додає елемент у список.

Було:

```python
self.cartridge = []
```

Після першого пострілу:

```python
self.cartridge = [shot1]
```

Після другого:

```python
self.cartridge = [shot1, shot2]
```

і так далі.

---

# 31. Додаємо кулю на екран

```python
self.ids.front.add_widget(shot)
```

У KV-файлі ми створимо контейнер:

```kv
id: front
```

Саме до нього будемо додавати кулі.

Важливо розрізняти:

```python
self.cartridge.append(shot)
```

і:

```python
self.ids.front.add_widget(shot)
```

Перша команда додає кулю до **списку Python**.

Друга — додає кулю **на екран**.

---

# 32. Тепер повністю замінюємо `main.py`

Коли ви зрозуміли всі частини, замініть увесь вміст `main.py` на цей код:

```python
from kivymd.app import MDApp
from kivymd.uix.widget import MDWidget
from kivymd.uix.screenmanager import MDScreenManager
from kivymd.uix.screen import MDScreen

from kivy.clock import Clock
from kivy.metrics import dp
from kivy import platform
from kivy.core.window import Window


FPS = 60
BULLET_SPEED = dp(10)
SHIP_SPEED = dp(5)


class MainScreen(MDScreen):
    pass


class Shot(MDWidget):
    pass


class GameScreen(MDScreen):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

        Clock.schedule_interval(self.update, 1 / FPS)

        self.eventkeys = {}
        self.cartridge = []

    def update(self, dt):
        for key in self.eventkeys:
            if self.eventkeys[key] == True:
                if key == 'left':
                    self.moveLeft()

                if key == 'right':
                    self.moveRight()

                if key == 'shot':
                    self.shot()
                    self.eventkeys[key] = False

        # Керування кулями
        for bullet in self.cartridge:
            bullet.pos[1] += BULLET_SPEED

    def pressKey(self, key):
        self.eventkeys[key] = True

    def releaseKey(self, key):
        self.eventkeys[key] = False

    def moveLeft(self):
        self.ids.ship.pos[0] -= SHIP_SPEED

    def moveRight(self):
        self.ids.ship.pos[0] += SHIP_SPEED

    def shot(self):
        shot = Shot(
            pos=(self.ids.ship.center_x, self.ids.ship.top)
        )

        self.cartridge.append(shot)
        self.ids.front.add_widget(shot)


class ShooterApp(MDApp):
    def build(self):
        self.theme_cls.theme_style = "Dark"
        self.theme_cls.primary_palette = "Orange"

        self.sm = MDScreenManager()

        self.sm.add_widget(MainScreen(name="main"))
        self.sm.add_widget(GameScreen(name="game"))

        return self.sm


if platform != 'android':
    Window.size = (450, 900)
    Window.top = 100
    Window.left = 600


app = ShooterApp()
app.run()
```

---

# 33. Тепер створюємо ігровий екран у `shooter.kv`

У `shooter.kv` після блоку `<MainScreen>` додайте:

```kv
<GameScreen>:
    MDFloatLayout:
        md_bg_color: color_cosmos

        MDFloatLayout:
            id: game

        MDFloatLayout:
            id: back

        MDFloatLayout:
            id: front

        Ship:
            id: ship
            center: root.center
```

## Що тут створюється?

Ми створюємо кілька шарів.

### `game`

```kv
id: game
```

Основний шар гри.

### `back`

```kv
id: back
```

Шар заднього плану.

### `front`

```kv
id: front
```

Передній шар.

Саме сюди ми будемо додавати кулі:

```python
self.ids.front.add_widget(shot)
```

---

# 34. Створюємо клас `Ship` у KV

Перед `<GameScreen>` додайте:

```kv
<Ship@Image>:
    source: 'assets/images/rocket.png'
    size_hint: None, None
    size: dp(100), dp(100)
```

Тут ми створюємо спеціальний елемент `Ship`.

Він базується на:

```text
Image
```

Тому може показувати картинку.

---

# 35. Чому `Ship@Image`?

Запис:

```kv
<Ship@Image>:
```

означає:

> створити новий KV-клас `Ship`, який базується на `Image`.

Після цього ми можемо написати:

```kv
Ship:
    id: ship
```

---

# 36. Розташування корабля

У `GameScreen` ми написали:

```kv
Ship:
    id: ship
    center: root.center
```

`root` тут — це поточний `GameScreen`.

Тому:

```kv
root.center
```

означає центр ігрового екрана.

Отже, корабель спочатку з'являється в центрі.

---

# 37. Додаємо кнопки керування

Тепер усередині `<GameScreen>` після `Ship` додайте:

```kv
        MDFloatLayout:
            id: interface

            MDBoxLayout:
                spacing: dp(20)
                padding: dp(20)

                MDFloatingActionButton:
                    icon: "arrow-left-bold"
                    on_press: root.pressKey('left')
                    on_release: root.releaseKey('left')

                MDFloatingActionButton:
                    icon: "arrow-right-bold"
                    md_bg_color: app.theme_cls.primary_color
                    on_press: root.pressKey('right')
                    on_release: root.releaseKey('right')

                Widget:

                MDFloatingActionButton:
                    icon: "arrow-up-bold"
                    md_bg_color: app.theme_cls.primary_color
                    on_press: root.pressKey('shot')
                    on_release: root.releaseKey('shot')
```

---

# 38. Як працює кнопка вліво?

```kv
MDFloatingActionButton:
    icon: "arrow-left-bold"
    on_press: root.pressKey('left')
    on_release: root.releaseKey('left')
```

Коли кнопку натиснули:

```kv
on_press: root.pressKey('left')
```

викликається:

```python
pressKey('left')
```

У результаті:

```python
self.eventkeys['left'] = True
```

Потім `update()` бачить:

```python
key == 'left'
```

і викликає:

```python
self.moveLeft()
```

Корабель рухається.

Коли кнопку відпускаємо:

```kv
on_release: root.releaseKey('left')
```

стан кнопки змінюється на:

```python
False
```

---

# 39. Як працює кнопка пострілу?

Кнопка:

```kv
MDFloatingActionButton:
    icon: "arrow-up-bold"
    md_bg_color: app.theme_cls.primary_color
    on_press: root.pressKey('shot')
    on_release: root.releaseKey('shot')
```

При натисканні:

```python
self.eventkeys['shot'] = True
```

Під час наступного оновлення:

```python
if key == 'shot':
    self.shot()
```

створюється нова куля.

---

# 40. Чому ми використовуємо `root`?

У KV:

```kv
root
```

— це кореневий об'єкт поточного правила.

У нашому випадку всередині:

```kv
<GameScreen>:
```

`root` — це `GameScreen`.

Тому:

```kv
root.pressKey('left')
```

означає:

> викликати метод `pressKey()` у нашого `GameScreen`.

---

# 41. Повний файл `shooter.kv`

Тепер замініть увесь вміст `shooter.kv` на:

```kv
#:set color_cosmos 0.2, 0, 0.1, 1


<MainScreen>:
    MDBoxLayout:
        orientation: 'vertical'
        padding: dp(50)
        spacing: dp(30)

        MDIconButton:
            icon: "cog"
            pos_hint: {"right": 1, "top": 1}
            icon_size: dp(50)

        Image:
            source: 'assets/images/rocket.png'
            size_hint_y: 0.7
            size_hint_x: 1
            allow_stretch: True

        MDRectangleFlatButton:
            text: "PLAY"
            font_size: sp(60)
            pos_hint: {'center_x': 0.5}
            on_press:
                root.manager.current = 'game'


<Shot>:
    size_hint: None, None
    size: dp(20), dp(20)
    md_bg_color: app.theme_cls.primary_color


<Ship@Image>:
    source: 'assets/images/rocket.png'
    size_hint: None, None
    size: dp(100), dp(100)


<GameScreen>:
    MDFloatLayout:
        md_bg_color: color_cosmos

        MDFloatLayout:
            id: game

        MDFloatLayout:
            id: back

        MDFloatLayout:
            id: front

        Ship:
            id: ship
            center: root.center

        MDFloatLayout:
            id: interface

            MDBoxLayout:
                spacing: dp(20)
                padding: dp(20)

                MDFloatingActionButton:
                    icon: "arrow-left-bold"
                    on_press: root.pressKey('left')
                    on_release: root.releaseKey('left')

                MDFloatingActionButton:
                    icon: "arrow-right-bold"
                    md_bg_color: app.theme_cls.primary_color
                    on_press: root.pressKey('right')
                    on_release: root.releaseKey('right')

                Widget:

                MDFloatingActionButton:
                    icon: "arrow-up-bold"
                    md_bg_color: app.theme_cls.primary_color
                    on_press: root.pressKey('shot')
                    on_release: root.releaseKey('shot')
```

---

# 42. Важливо: `dp()` та `sp()` у KV

У нашому KV-файлі є:

```kv
dp(50)
```

та:

```kv
sp(60)
```

Kivy/KivyMD дозволяє використовувати ці функції без окремого імпорту в KV-файлі.

### `dp`

Використовуємо переважно для:

- розмірів;
- відступів;
- відстаней;
- координат.

Наприклад:

```kv
padding: dp(50)
```

### `sp`

Використовуємо переважно для:

- розміру тексту.

Наприклад:

```kv
font_size: sp(60)
```

---

# 43. Що має відбутися після запуску?

Запустіть:

```text
main.py
```

Має з'явитися головне меню.

На ньому:

- картинка корабля;
- кнопка `PLAY`.

Натисніть:

```text
PLAY
```

Має відкритися ігровий екран.

На ньому:

- корабель;
- кнопка вліво;
- кнопка вправо;
- кнопка пострілу.

---

# 44. Перевіряємо рух

Натисніть кнопку:

```text
←
```

Корабель повинен рухатися вліво.

Натисніть:

```text
→
```

Корабель повинен рухатися вправо.

### Якщо корабель не рухається

Перевірте:

1. чи є в `GameScreen` метод:

```python
def pressKey(self, key):
    self.eventkeys[key] = True
```

2. чи є:

```python
def releaseKey(self, key):
    self.eventkeys[key] = False
```

3. чи є:

```python
def moveLeft(self):
    self.ids.ship.pos[0] -= SHIP_SPEED
```

4. чи є:

```python
def moveRight(self):
    self.ids.ship.pos[0] += SHIP_SPEED
```

5. чи правильно написаний `id` корабля:

```kv
id: ship
```

---

# 45. Перевіряємо постріл

Натисніть:

```text
↑
```

Має з'явитися маленький квадрат — наша куля.

Вона повинна рухатися вгору.

Натисніть кнопку кілька разів.

Має з'являтися кілька куль.

---

# 46. Що відбувається під час одного пострілу?

Повний ланцюжок такий:

```text
Натискання кнопки
        ↓
pressKey('shot')
        ↓
eventkeys['shot'] = True
        ↓
update()
        ↓
shot()
        ↓
створюється Shot
        ↓
Shot додається в cartridge
        ↓
Shot додається у front
        ↓
update()
        ↓
bullet.pos[1] += BULLET_SPEED
        ↓
куля рухається вгору
```

Це дуже важлива схема сьогоднішнього уроку.

---

# 47. Чому потрібен `Clock`?

Без:

```python
Clock.schedule_interval(self.update, 1 / FPS)
```

метод:

```python
update()
```

не виконувався б автоматично.

А отже, гра не знала б:

- коли перевірити натиснуті кнопки;
- коли перемістити корабель;
- коли перемістити кулі;
- коли виконати інші ігрові дії.

`Clock` фактично створює **ігровий цикл**.

---

# 48. Схема нашої гри

Можна уявити програму так:

```text
                ShooterApp
                    │
                    ▼
             MDScreenManager
              /            \
             /              \
            ▼                ▼
      MainScreen         GameScreen
                              │
                    ┌─────────┼─────────┐
                    │         │         │
                    ▼         ▼         ▼
                 корабель   кнопки    кулі
                              │         │
                              ▼         ▼
                           pressKey   Shot
                                        │
                                        ▼
                                  рух через update()
```

---

# 49. Типові помилки

## Помилка 1. `FileNotFoundError` або картинка не відображається

Перевірте:

```kv
source: 'assets/images/rocket.png'
```

і структуру:

```text
проєкт/
├── main.py
├── shooter.kv
└── assets/
    └── images/
        └── rocket.png
```

Особливо перевірте назву:

```text
rocket.png
```

та:

```text
Rocket.png
```

— це можуть бути різні файли.

---

## Помилка 2. `NameError: name 'dp' is not defined`

Перевірте `main.py`:

```python
from kivy.metrics import dp
```

---

## Помилка 3. `No rule for ...`

Перевірте, чи збігаються назви Python-класів і KV-правил.

Наприклад:

```python
class MainScreen(MDScreen):
```

повинен мати:

```kv
<MainScreen>:
```

---

## Помилка 4. `self.ids.ship` не знайдено

Перевірте, чи є у `shooter.kv`:

```kv
Ship:
    id: ship
```

---

## Помилка 5. `self.ids.front` не знайдено

Перевірте:

```kv
MDFloatLayout:
    id: front
```

---

## Помилка 6. Кулі не рухаються

Перевірте:

```python
Clock.schedule_interval(self.update, 1 / FPS)
```

і:

```python
for bullet in self.cartridge:
    bullet.pos[1] += BULLET_SPEED
```

---

## Помилка 7. Кнопка PLAY не працює

Перевірте:

```kv
on_press:
    root.manager.current = 'game'
```

А також у `main.py`:

```python
self.sm.add_widget(GameScreen(name="game"))
```

---

# 50. Контрольний список

Перед завершенням перевірте все по черзі.

### Файли

- [ ] Є `main.py`.
- [ ] Є `shooter.kv`.
- [ ] Є папка `assets`.
- [ ] Усередині є папка `images`.
- [ ] Усередині `images` є `rocket.png`.

### `main.py`

- [ ] Імпортований `MDWidget`.
- [ ] Імпортований `Clock`.
- [ ] Є `FPS`.
- [ ] Є `BULLET_SPEED`.
- [ ] Є `SHIP_SPEED`.
- [ ] Є `MainScreen`.
- [ ] Є `Shot`.
- [ ] Є `GameScreen`.
- [ ] Є `update()`.
- [ ] Є `pressKey()`.
- [ ] Є `releaseKey()`.
- [ ] Є `moveLeft()`.
- [ ] Є `moveRight()`.
- [ ] Є `shot()`.
- [ ] Є `cartridge`.
- [ ] Є `eventkeys`.

### `shooter.kv`

- [ ] Є `<MainScreen>:`.
- [ ] Є `<Shot>:`.
- [ ] Є `<Ship@Image>:`.
- [ ] Є `<GameScreen>:`.
- [ ] Є `id: ship`.
- [ ] Є `id: front`.
- [ ] Є кнопка вліво.
- [ ] Є кнопка вправо.
- [ ] Є кнопка пострілу.
- [ ] Шлях до картинки правильний.

---

# 51. Самостійне завдання

Якщо основна частина вже працює, виконайте додаткові завдання.

## Завдання 1. Змінити швидкість корабля

Знайдіть:

```python
SHIP_SPEED = dp(5)
```

Спробуйте:

```python
SHIP_SPEED = dp(8)
```

або:

```python
SHIP_SPEED = dp(3)
```

Запустіть програму та порівняйте рух.

---

## Завдання 2. Змінити швидкість куль

Знайдіть:

```python
BULLET_SPEED = dp(10)
```

Спробуйте інше значення.

Наприклад:

```python
BULLET_SPEED = dp(15)
```

Що змінилося?

---

## Завдання 3. Змінити розмір кулі

У `shooter.kv` знайдіть:

```kv
size: dp(20), dp(20)
```

Спробуйте:

```kv
size: dp(10), dp(10)
```

або:

```kv
size: dp(30), dp(30)
```

---

## Завдання 4. Змінити колір кулі

Зараз:

```kv
md_bg_color: app.theme_cls.primary_color
```

Спробуйте встановити інший колір.

Наприклад:

```kv
md_bg_color: 1, 0, 0, 1
```

Пам'ятайте, що колір записується як:

```text
R, G, B, A
```

де:

- `R` — червоний;
- `G` — зелений;
- `B` — синій;
- `A` — прозорість.

---

## Завдання 5. Змінити картинку

Знайдіть іншу PNG-картинку космічного корабля.

Покладіть її в:

```text
assets/images/
```

та змініть:

```kv
source: 'assets/images/rocket.png'
```

на шлях до нової картинки.

### Обов'язково використовуйте `Copy Relative Path` у VS Code.

---

# 52. Мініпитання для перевірки себе

Спробуйте відповісти без підглядання в код.

### 1. Для чого потрібен `Clock`?

<details>
<summary>Відповідь</summary>

Для регулярного виклику `update()` та організації ігрового циклу.

</details>

### 2. Де зберігаються кулі?

<details>
<summary>Відповідь</summary>

У списку:

```python
self.cartridge
```

</details>

### 3. Як куля рухається вгору?

<details>
<summary>Відповідь</summary>

Збільшується її координата `y`:

```python
bullet.pos[1] += BULLET_SPEED
```

</details>

### 4. Для чого потрібен `id: ship`?

<details>
<summary>Відповідь</summary>

Щоб звернутися до корабля з Python через:

```python
self.ids.ship
```

</details>

### 5. Для чого потрібен `id: front`?

<details>
<summary>Відповідь</summary>

Щоб додавати створені кулі на екран:

```python
self.ids.front.add_widget(shot)
```

</details>

### 6. Що робить `append()`?

<details>
<summary>Відповідь</summary>

Додає елемент у список.

Наприклад:

```python
self.cartridge.append(shot)
```

додає нову кулю до списку куль.

</details>

---

# 53. Головне, що потрібно зрозуміти

Сьогодні ми не просто написали багато коду.

Ми створили основу **ігрової логіки**.

У нас є:

```text
ІНТЕРФЕЙС
   │
   ├── кнопка вліво
   ├── кнопка вправо
   └── кнопка пострілу
          │
          ▼
     GameScreen
          │
          ▼
       update()
          │
     ┌────┴────┐
     ▼         ▼
  корабель   кулі
     │         │
     ▼         ▼
   рух       рух
```

Тобто кнопки змінюють стан гри, а `update()` регулярно перевіряє цей стан і виконує необхідні дії.

Це один із базових принципів створення ігор.

---

# 54. Фінальний результат

Після уроку у вас має бути:

```text
назва_проєкту/
│
├── main.py
│
├── shooter.kv
│
└── assets/
    │
    └── images/
        │
        └── rocket.png
```

А програма повинна дозволяти:

```text
┌─────────────────────────────┐
│                             │
│          КОРАБЕЛЬ           │
│                             │
│            PLAY             │
│                             │
└─────────────────────────────┘
                │
                ▼
┌─────────────────────────────┐
│                             │
│                             │
│           🚀                │
│                             │
│                             │
│                             │
│                             │
│   ←        →          ↑     │
└─────────────────────────────┘
```

**Мета уроку виконана, якщо корабель рухається вліво/вправо, а після натискання кнопки пострілу кулі з'являються біля корабля та летять вгору.**
