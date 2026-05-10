from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.button import Button
from kivy.uix.radiobutton import RadioButton
from kivy.uix.gridlayout import GridLayout
from kivy.core.window import Window

Window.size = (350, 550)

class TestApp(App):
    def build(self):
        self.questions = [
            {
                "text": "1. Выберите столицу России:",
                "options": ["Москва", "Санкт-Петербург", "Екатеринбург"],
                "correct": 0,
            },
            {
                "text": "2. Во время войны 1941-1945 в какой город переместилось правительство СССР?",
                "options": ["Самара", "Куйбышев", "Казань"],
                "correct": 1,
            },
            {
                "text": "3. Двигатель для ракеты 'Союз', на которой полетел Юрий Гагарин, кто разработал?",
                "options": ["Сергей Королёв", "Никита Хрущёв", "Вернер Фон Браун"],
                "correct": 0,
            },
            {
                "text": "4. Как во время войны 1941-1945 США оказывали помощь СССР?",
                "options": [
                    "По морю, через Океан до города Мурманск",
                    "Через Сибирь по железной дороге",
                    "Помощь не оказывалась"
                ],
                "correct": 0,
            },
            {
                "text": "5. Кто рассчитал траектории полётов в космос?",
                "options": ["Ломоносов", "Курчатов", "Циолковский"],
                "correct": 2,
            },
        ]
        self.current_q = 0
        self.score = 0
        self.rb_group = []  # Храним объекты радиокнопок
        return self.create_question_widget()

    def create_question_widget(self):
        layout = BoxLayout(orientation='vertical', padding=10, spacing=10)
        q_text = self.questions[self.current_q]["text"]
        layout.add_widget(Label(text=q_text, size_hint=(1, 0.2)))

        options = self.questions[self.current_q]["options"]
        rb_layout = GridLayout(cols=1, spacing=5, size_hint=(1, 0.6))

        self.rb_group = []
        for idx, opt in enumerate(options):
            rb = RadioButton(group='q_group', active=False)
            lbl = Label(text=opt, size_hint_x=0.8)
            row = BoxLayout(orientation='horizontal', spacing=5)
            row.add_widget(rb)
            row.add_widget(lbl)
            rb_layout.add_widget(row)
            self.rb_group.append(rb)

        layout.add_widget(rb_layout)

        btn_next = Button(text="Далее", size_hint=(1, 0.1))
        btn_next.bind(on_press=self.check_answer)
        layout.add_widget(btn_next)

        return layout

    def check_answer(self, instance):
        selected = -1
        for idx, rb in enumerate(self.rb_group):
            if rb.active:
                selected = idx
                break
        if selected == -1:
            return

        if selected == self.questions[self.current_q]["correct"]:
            self.score += 1

        self.current_q += 1
        if self.current_q < len(self.questions):
            self.root.clear_widgets()
            new_root = self.create_question_widget()
            self.root.add_widget(new_root)
        else:
            self.show_result()

    def show_result(self):
        self.root.clear_widgets()
        res_layout = BoxLayout(orientation='vertical', padding=30, spacing=20)
        res_layout.add_widget(Label(text=f"Тест завершён!\n\nВаш результат: {self.score} из {len(self.questions)}"))
        btn_restart = Button(text="Начать заново")
        btn_restart.bind(on_press=self.restart)
        res_layout.add_widget(btn_restart)
        self.root.add_widget(res_layout)

    def restart(self, instance):
        self.current_q = 0
        self.score = 0
        self.root.clear_widgets()
        new_root = self.create_question_widget()
        self.root.add_widget(new_root)

if __name__ == "__main__":
    TestApp().run()
