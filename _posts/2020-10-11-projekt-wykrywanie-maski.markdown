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

Pobieramy obrazki z dysku 
~~~ python
path = Path('masks') # miejsce_na_dysku_gdzie_mam_dwa_katalogi_z_obrazkami
fns = get_image_files(path)
~~~

Konfigurujemy biblioteka fast.ai, pobierała obrazki (ImageBlock), zwracała czy ktoś jest w masce lub nie (CategoryBlock), pobierała obrazki z plików na dysku (get_image_files), wewnetrznie sprawdzała czy dobrze pracuje (20% obrazków przeznacza do testów - valid_pct=0.2, zawsze zaczyna z tego samego punktu - seed=42), 
informacje czy ktoś w maseczce czy nie do nauczania pobiera z nazwy katalogu ( dlatego ważne jest dobre skopiowanie obrazków do odpowiednich katalogów na dysku (get_y=parent_label), dla ułatwienia zmień rozmiar obrazków na ten sam żeby je traktować tak samo (item_tfms=Resize(128)).

~~~ python
masks = DataBlock(
    blocks=(ImageBlock, CategoryBlock), 
    get_items=get_image_files, 
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(128))
~~~

Ładujemy zdjęcia do biblioteki
~~~ python
dls = masks2.dataloaders(path)
~~~

Tworzymy nauczyciela (cnn_learner), który pobiera z sieci już wcześniej nauczoną sztuczną inteligencję na wielu zdjęciach  i startujemy z uczeniem (właściwie douczaniem w czterech krokach, w każdym kroku przegląda wszystkie zdjęcia, na swoim doświadczeniu czym więcej powtórek w nauce tym lepiej przyswojona wiedza)
~~~ python
learn = cnn_learner(dls, resnet18, metrics=error_rate)
learn.fine_tune(4)
~~~
Zapisujemy naszą douczoną sztuczną inteligencje na dysk
~~~ python
learn.export()
~~~
