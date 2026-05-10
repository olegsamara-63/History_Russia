## Проблема ясна! ❌

Вы скопировали не сам код, а команду `echo` с кавычками. И ошибка `NameError: name 'App' is not defined` означает, что в файле отсутствует импорт `from kivy.app import App`.

Давайте **исправим это раз и навсегда**.

---

## ✅ Простой способ: Создайте новый файл в Pydroid правильно

### Шаг 1: В Pydroid создайте **НОВЫЙ** файл

1. Откройте Pydroid
2. Нажмите на **три полоски** (меню) → **Новый файл**
3. Назовите его **quiz.py** (не main.py, не main_history.sh)

### Шаг 2: Скопируйте ЭТОТ чистый код и вставьте

```python
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.button import Button
from kivy.uix.gridlayout import GridLayout

class HistoryTestApp(App):
    def build(self):
        self.questions = [
            {"text": "1. Столица России?", "options": ["Москва", "Санкт-Петербург", "Екатеринбург"], "correct": 0},
            {"text": "2. Правительство СССР в 1941 эвакуировали в:", "options": ["Самара", "Куйбышев", "Казань"], "correct": 1},
            {"text": "3. Создатель двигателя для ракеты Союз:", "options": ["Королёв", "Хрущёв", "Фон Браун"], "correct": 0},
            {"text": "4. Помощь США в 1941-1945:", "options": ["Морские конвои", "Через Сибирь", "Не помогали"], "correct": 0},
            {"text": "5. Кто рассчитал космические траектории?", "options": ["Ломоносов", "Курчатов", "Циолковский"], "correct": 2},
        ]
        self.current_q = 0
        self.score = 0
        self.selected_answer = None
        return self.question_screen()

    def question_screen(self):
        q = self.questions[self.current_q]
        layout = BoxLayout(orientation='vertical', padding=20, spacing=15)
        layout.add_widget(Label(text=q["text"], font_size=20, size_hint=(1, 0.2)))
        
        grid = GridLayout(cols=1, spacing=10, size_hint=(1, 0.6))
        self.answer_buttons = []
        
        for i, opt in enumerate(q["options"]):
            btn = Button(text=opt, font_size=16, size_hint_y=None, height=50)
            btn.bind(on_press=lambda x, idx=i: self.select_answer(idx))
            grid.add_widget(btn)
            self.answer_buttons.append(btn)
        
        layout.add_widget(grid)
        next_btn = Button(text="Далее", font_size=18, size_hint=(1, 0.1))
        next_btn.bind(on_press=self.next_question)
        layout.add_widget(next_btn)
        return layout

    def select_answer(self, idx):
        self.selected_answer = idx
        for i, btn in enumerate(self.answer_buttons):
            btn.background_color = (0.2, 0.8, 0.2, 1) if i == idx else (0.5, 0.5, 0.5, 1)

    def next_question(self, instance):
        if self.selected_answer is None:
            return
        
        if self.selected_answer == self.questions[self.current_q]["correct"]:
            self.score += 1
        
        self.current_q += 1
        self.selected_answer = None
        
        if self.current_q < len(self.questions):
            self.root.clear_widgets()
            self.root.add_widget(self.question_screen())
        else:
            self.show_result()

    def show_result(self):
        self.root.clear_widgets()
        layout = BoxLayout(orientation='vertical', padding=30, spacing=20)
        layout.add_widget(Label(text=f"Тест завершён!\n\nПравильных ответов: {self.score} из {len(self.questions)}", font_size=20))
        restart_btn = Button(text="Пройти заново", font_size=18)
        restart_btn.bind(on_press=self.restart)
        layout.add_widget(restart_btn)
        self.root.add_widget(layout)

    def restart(self, instance):
        self.current_q = 0
        self.score = 0
        self.selected_answer = None
        self.root.clear_widgets()
        self.root.add_widget(self.question_screen())

if __name__ == "__main__":
    HistoryTestApp().run()
```

### Шаг 3: Запустите

Нажмите на **зелёную стрелку "Run"** (▶️) в правом верхнем углу Pydroid.

**Не через терминал, а через кнопку Run!**

---

## 📱 Альтернатива: Запуск через терминал (если хотите)

Если хотите через терминал, то сначала **создайте файл через редактор Pydroid** (как описано выше), а затем выполните:

```bash
cd /storage/emulated/0/my_apk_s
python3 quiz.py
```

Но убедитесь, что файл называется `quiz.py`, а не то, что вы вставляли через `echo`.

---

## ⚠️ Важно: Удалите испорченные файлы

В терминале выполните:

```bash
rm /storage/emulated/0/my_apk_s/main_history.sh
rm /storage/emulated/0/my_apk_s/main.py
```

Оставьте только `quiz.py`.

---

**Создайте новый файл через редактор Pydroid, вставьте код выше и нажмите Run. Напишите, что получилось!**
