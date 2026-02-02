Bawsala 1.1.4 - Structure complète et fichiers

---

main.py
```python
from kivy.lang import Builder
from kivy.uix.screenmanager import ScreenManager, Screen
from kivymd.app import MDApp

from screens.signup import SignUpScreen
from screens.login import LoginScreen
from screens.home import HomeScreen
from screens.rendezvous import generate_orienteurs_cards

class MyApp(MDApp):
    def build(self):
        self.theme_cls.primary_palette = 'Blue'
        sm = ScreenManager()
        sm.add_widget(SignUpScreen(name='signup'))
        sm.add_widget(LoginScreen(name='login'))
        home_screen = HomeScreen(name='home')
        sm.add_widget(home_screen)
        generate_orienteurs_cards(home_screen.ids.rendezvous_box)
        return sm

if __name__ == '__main__':
    MyApp().run()
```

---
screens/signup.py
```python
from kivy.uix.screenmanager import Screen

class SignUpScreen(Screen):
    pass
```

---
screens/login.py
```python
from kivy.uix.screenmanager import Screen

class LoginScreen(Screen):
    pass
```

---
screens/home.py
```python
from kivy.uix.screenmanager import Screen

class HomeScreen(Screen):
    pass
```

---
screens/rendezvous.py
```python
from kivymd.uix.card import MDCard
from kivymd.uix.label import MDLabel
from kivymd.uix.button import MDRaisedButton
from kivymd.uix.boxlayout import MDBoxLayout
from kivymd.uix.fitimage import FitImage
from kivy.metrics import dp
from kivy.utils import get_color_from_hex

orienteurs = [
    {"nom": "John Doe", "specialite": "Orientation scolaire", "disponibilite": "Lundi - Vendredi", "image": "kv/images/orienteur.png"},
    {"nom": "Jane Smith", "specialite": "Orientation professionnelle", "disponibilite": "Mardi - Samedi", "image": "kv/images/orienteur.png"},
    {"nom": "Ali Ben", "specialite": "Orientation académique", "disponibilite": "Mercredi - Vendredi", "image": "kv/images/orienteur.png"},
]

def generate_orienteurs_cards(parent_box):
    parent_box.clear_widgets()

    for o in orienteurs:
        card = MDCard(
            orientation="vertical",
            size_hint=(1, None),
            height=dp(170),
            radius=[20],
            elevation=8,
            padding=dp(12),
            md_bg_color=get_color_from_hex("#FFFFFF")
        )

        layout = MDBoxLayout(orientation="horizontal", spacing=dp(12), size_hint_y=None, height=dp(150))

        img_size = dp(90)
        img = FitImage(
            source=o["image"],
            size_hint=(None, None),
            size=(img_size, img_size),
            radius=[img_size/2]
        )
        layout.add_widget(img)

        info_layout = MDBoxLayout(orientation="vertical", spacing=dp(6))
        info_layout.add_widget(MDLabel(
            text=o["nom"], font_style="H6", bold=True, size_hint_y=None, height=dp(28),
            theme_text_color="Primary"
        ))
        info_layout.add_widget(MDLabel(
            text=f"Spécialité : {o['specialite']}", font_style="Body1", size_hint_y=None, height=dp(20),
            theme_text_color="Secondary"
        ))
        info_layout.add_widget(MDLabel(
            text=f"Disponible : {o['disponibilite']}", font_style="Caption", size_hint_y=None, height=dp(18),
            theme_text_color="Hint"
        ))

        btn = MDRaisedButton(
            text="Voir plus",
            size_hint=(None, None),
            size=(dp(100), dp(36)),
            md_bg_color=get_color_from_hex("#1976D2"),
            text_color=get_color_from_hex("#FFFFFF"),
            pos_hint={"right": 1}
        )
        btn.bind(on_release=lambda x, name=o["nom"]: print(f"Détails pour {name}"))
        info_layout.add_widget(btn)

        layout.add_widget(info_layout)
        card.add_widget(layout)
        parent_box.add_widget(card)

    parent_box.height = sum([child.height + dp(15) for child in parent_box.children])
```

---
kv/home.kv
```kv
<HomeScreen>:
    MDBoxLayout:
        orientation: "vertical"
        MDToolbar:
            title: "Bawsala"
            elevation: 10

        MDBottomNavigation:
            panel_color: app.theme_cls.primary_color

            MDBottomNavigationItem:
                name: 'rendezvous'
                text: 'Rendez-vous'
                icon: 'calendar'
                RendezVousScreen:
                    id: rendezvous_screen

            MDBottomNavigationItem:
                name: 'chat'
                text: 'Chat'
                icon: 'chat'
                MDLabel:
                    text: 'Chat Page'
                    halign: 'center'

            MDBottomNavigationItem:
                name: 'profile'
                text: 'Profile'
                icon: 'account'
                MDLabel:
                    text: 'Profile Page'
                    halign: 'center'
```

---
kv/rendezvous.kv
```kv
<RendezVousScreen>:
    ScrollView:
        do_scroll_x: False
        MDBoxLayout:
            id: rendezvous_box
            orientation: "vertical"
            padding: dp(12)
            spacing: dp(15)
            size_hint_y: None
            height: self.minimum_height
```

---
kv/images/orienteur.png
(ici se trouve votre image d'orienteur, à placer dans le dossier kv/images/)

---
Fin du fichier bawsala1_1_4.txt

