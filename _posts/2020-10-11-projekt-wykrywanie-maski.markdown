---
layout: post
title:  "Projekt wykrywanie maski"
date:   2020-10-11 22:56:34 +0200
categories: sztuczna inteligencja
---
Sztuczna inteligencja pozwala nauczyć komputer nowych zachowań w sposób, 
który bardziej przypomina w jaki uczą się ludzie czyli poprzez pokazanie przykładów.

W projekcie dam sztucznej inteligencji zdjęcia ludzi w maseczkach i bez, nauczy się rozpoznawać maseczki ze zdjęć.

Do katalogu "human_face" skopiowałem zdjęcia twarzy niczym nie zakrytej z internetu, a do katalogu "face_mask" twarze zakryte maską.

Do nauczenia sztucznego mózgu użyje biblioteki (coś w rodzaju rozszerzenia dającego nowe możliwości ) <https://fast.ai>, w pierwszym kroku pobiorę i zainstaluje bibliotekę i podłączę ją do mojego programu:
~~~ python
!pip install -Uqq fastbook
import fastbook
fastbook.setup_book()
from fastbook import *
from fastai.vision.widgets import *
~~~


