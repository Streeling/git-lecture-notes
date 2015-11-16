================================================================================
Git și Github
================================================================================

Acest tutorial a fost elaborat pentru conferința EuroScipy'13 și susținut în 
cadrul acestei conferințe. Tutorialul este destinat cercetătorilor care, după 
cum știm (sau nu știm), în funcție de domeniul de cercetare pot fi sau pot să nu 
fie la curent cu ceea ce înseamnă un sistem de control al versiunilor.

Alte tutoriale de la aceeași conferință (bazate pe python) pot fi accesate 
`aici <http://scipy-lectures.github.io>`_.

De ce ar folosi cineva un sistem de control al versiunilor?
--------------------------------------------------------------------------------

Ca cercetător (probabil) optați pentru cercetări:

  - reproductibile
  - rapide și eficiente

.. figure:: ../images/Journal-of-Irrproducibe-Research.jpg

   The journal of Irreproducible research.

Și-n mod ideal dvs. ați dori să evitați o astfel de situație:

.. figure:: ../images/version_control.gif

  "Piled Higher and Deeper" de Jorge Cham: www.phdcomics.com

Sistemele de control al versiunilor permit reținerea (înregistrarea) istoricului 
modificărilor (versiunilor) operate asupra fișierelor proiectului astfel încât 
să puteți reveni la anumite versiuni mai târziu. De asemenea un astfle de sistem 
face ca lucrul colaborativ asupra unuia și aceluiași proiect să devină mai 
eficient. În plus, poate fi utilizat ca un data-centru, în cadrul căruia să fie 
posibilă integrarea continuă și automatizarea secvenței de operații care produce 
programul final [din eng.: build].

În acest tutorial vom studia elementele de bază ale programului Git și ale 
sitului Github. Dar înainte de toate, vom răspunde la întrebarea: ce 
este Git? Git este un sistem distribuit de control al versiunilor (din eng.: 
DVCS - Distributed Version Control Software). Iar Github este un sit web ce 
oferă gratuit servicii de stocare a codului sursă, gestiunea acestuia 
realizându-se doar prin Git. Există desigur și alte sisteme de control al 
versiunii, cum ar fi: mercurial și bazaar, cu siturile web asociate, bitbucket 
(care la fel suportă și Git) și, corespunzător, launchpad.  

De ce am ales (în acest tutorial) anume git, și nu mercurial, care nu doar că 
este scris în python, dar și a fost ales de CPython (și de multe alte proiecte 
bazate pe python) pentru a păstra codul? A fost o decizie simplă, grație faptului că  
în lumea științifică, git și github sunt mult mai mult utilizate decît mercurial
(și bitbucket). Într-adevăr, o căutare cu ajutorul servicului scholar.google.com va da 
14,100 rezultate pentru cuvăntul cheie „github”, în timp ce pentru - „bitbucket” 
rezultatul căutării va fi în număr de 2,260. Dacă aceste date nu sunt 
suficiente pentru a vă convinge, umează un tabel de pachete pe python împărțite 
în două liste: cele stocate pe github și cele stocate pe bitbucket.

+-----------------------------------+----------------------------------------+
| Github                            | Bitbucket                              |
+===================================+========================================+
| - Numpy                           | - PIL                                  |
| - Scipy                           |                                        |
| - IPython                         |                                        |
| - matplotlib                      |                                        |
| - Sympy                           |                                        |
| - Scikit-learn                    |                                        |
| - Scikit-image                    |                                        |
| - Numba                           |                                        |
| - Mayavi                          |                                        |
| - Traits                          |                                        |
| - Enable                          |                                        |
| - Enamel                          |                                        |
| - Pandas                          |                                        |
| - biopython                       |                                        |
| - Cython                          |                                        |
| - statsmodel                      |                                        |
| ....                              |                                        |
+-----------------------------------+----------------------------------------+

Înainte de a trece la studierea propriu-zisă a posibilităților oferite de git, 
trebuie să știți că git este un program foarte complex și sofisticat. În acest 
tutorial, cum a fost deja menționat, vor fi prezentate doar elementele de bază 
ale lui Git, respectiv multe lucruri vor rămâne încă neclare. Dar după o 
perioadă de utilizare vă veți acomoda și veți căuta să utilizați comenzi mult 
mai complexe.

În linii generale, o sesiune de lucru cu Git este compusă preponderent din 
următoarele operații:

  - modificarea fișierelor în dosarul de lucru;
  - marcarea fișierelor la care ați lucrat. Această operație va pregăti o\ 
captură a dosarului;
  - efectuarea unui commit a fișierelor marcate. Această operație va stoca 
captura în depozitul Git.

  
Inițializarea depozitului de fișiere și setarea programului git
--------------------------------------------------------------------------------

Există mai multe metode de a inițializa un depozit git. Prima metodă se 
utilizează atunci când doriți să inițializați depozitul git pentru un proiect 
care deja se află pe calculatorul dvs. Pe când a doua metodă - pentru a descărca 
un depozit git existent (obțiunând în rezultat o copie locală a acestuia).

Dacă sunteți în situația când vi se potrivește prima metodă, atunci treceți în 
dosarul proiectului și rulați comanda::

  git init

Ca urmare, în dosarul proiectului, se va crea un dosar ascuns .git, în care pe 
viitor va fi păstrat istoricul tuturor modificărilor operate asupra acestui dosar 
și a subdosarelor.

A doua metodă presupune utilizarea comenzii::

  git clone https://github.com/git-lectures/git-lecture-notes.git

Această comandă va crea o copie locală a depozitului ce conține  fișierele 
acestui tutorial.

.. note:: Fiecare depozit trebuie să fie în propriul său dosar. Nu este permisă  
   crearea sau duplicarea de depozite git înăuntrul unui alt depozit git deja 
existent, adică în dosarul sau subdosarule în care deja ați inițializat un 
depozit git. 

Înainte de a purcede mai departe haideți să setăm git-ul. Va trebuie să faceți 
acest lucru pentru fiecare calculator în parte::

  git config --global
  git config --global user.name "Numele dvs"
  git config --global user.email dvs@deomeniuldvs.com``
  git config --global core.editor vim
  git config color.ui auto

Opțiunea ``--global`` se utilizează atunci când doriți ca setările să fie la 
nivel de utilizator. Setările vor fi stocate într-un dosar ascuns în dosarul 
personal. De asemenea puteți seta fiecare depozit git în parte, scoțând această 
opțiune, sau puteți seta pentru întreg sistemul punând în loc de ``--global`` 
opțiunea ``--system``. De obicei, se setează depozitele la nivel de utilizator 
și la nivel de sistem.

Puteți verifica care-s setările dvs. cu::

  git config --list

Dacă ați setat la diferite nivele veți vedea mai multe înregistrări care se 
repetă. Setările pentru utilizator au prioritate în raport cu setările la nivel 
de sistem și setările locale au prioritate mai mare în raport cu cele de 
utilizator.

Crearea capturilor: operația „commit” (sau salvarea modificărilor)
--------------------------------------------------------------------------------

Unul dintre principalele scopuri al controlului versiunii este de a salva capturi 
ale dosarului dvs. Noi numim aceste capturi commit-uri. Fiecărei capturi îi este  
asociată o anumită meta informație: data creării capturii, cine a făcut-o, care 
fișiere au fost modificate, însăși modificările făcute fișierelor etc. Git vă va 
oferi posibilitatea de a urmări modificările făcute fișierelor, întoarcerea 
întregului proiect la o anumită captură (versiune) din trecut, să vedeți 
schimbările făcute de-a lungul timpului, să vedeți cine a modificat fișierul.

Acum că știm de ce, vrem să creăm o captură, să vedem cum putem face acest lucru.
La acest acest moment nimic nu este urmărit în proiect.
În primul rând să creăm niște dosare și fișiere în dosarul dvs.

.. code-block:: bash

   touch README

Acum aveți un fișier nou în dosar. Așa cum a fost menționat anterior, git încă nu 
ține cont de acest fișier. Mai întâi trebuie să-i comunicăm că acest fișier există::

  git add README

Acum puteți să salvați modificările::

  git commit -m "A fost adăugat un fișier README"

La acest moment, aveți un fișier urmărit, și un commit inițial.
Fiecare fișier în dosarul de lucru poate fi în una din două stări: urmărit sau nu.
Orice fișier care nu a fost adăugat la un punct anumit nu este urmărit.
Fișierele urmărite la rîndul lor pot fi în două stări: nemodificate, modificate sau capturate.

.. figure:: ../images/git_file_status_lifecycle.png

Puteți verifica starea fiecărui fișier din dosar cu ajutorul comenzii ``git status``.
Această comandă va afișa toate fișierele ne urmărite și modificate, și capturate::

  touch AUTHORS
  git status

Remarcăm că fișierul README nu este afișat în timp ce fișierul AUTHORS este.
Acum să adăugăm fișierul AUTHORS. Acest fișier va fi de acum urmărit și capturat::

  git add AUTHORS
  git status

``git add`` de asemenea este folosit pentru a captura un fișier. De fapt, executînd 
``git add`` asupra unui fișier ne urmărit nu numai că îl va urmări dar și-l va captura. 
``git commit`` va face o captură a fișierelor urmărite::

  git commit -m "A fost adăugat fișierul AUTHORS"

Și iată al doilea commit! Uneori veți dori să vedeți modificările care le-ați 
făcut unui fișier modificat::

  git diff

Pentru a vedea modificările în fișierul marcat, folosiți::

  git diff --cached

Și vizualizarea istoriei tuturor commit-urilor::

  git log

Acum să încercăm să rm AUTHORS::

  rm AUTHORS CONTRIBUTORS
  git status

Puteți observa că fișierul ``AUTHORS`` este cu roșu, și marcat ca fiind șters. 
Aplicând ``git add`` pe acest fișier nu mută modificare în zona de staging.
Pentru a șterge un fișier care a început să fie urmărit de git utilizați comanda 
``git rm``. Analog, ``git mv`` poate fi utilizată pentru a muta un fișier.

Dar a greși este omenește, ați putea dori să anulați niște stages. Două scenarii 
pot apărea: (1) ați staged un fișier pe care nu vreți să-l comiteți (2) ați făcut 
niște modificări la un fișier pe care vreți să le anulați.

Mai întâi, presupunem că ați făcut stage și vreți să-l scoateți din stage::

  touch TODO
  git add TODO

Pentru unstage::

  git reset HEAD TODO

Sintaxa este ``git reset HEAD <numefișier>``. Vom explica ce este HEAD mai tîrziu

În al doilea caz am modificat un fișier și vrem sa anulăm aceste modificări.
Pentru a  face acest lucru utilizați ``git checkout <filename>``. Dacă executați 
``git status`` remarcăm că git vă amintește ce comenzi pot fi utilizate pentru fiecare 
operație.

Exerciții
~~~~~~~~~~

  - Creați un dosar cu numele GitTutorial
  - Înăuntrul acestui dosar, inițializați un depozit git (``git init``)
  - Creați un fișier cu numele AUTHORS, un fișier TODO și README.
  - Adăugați fișierul AUTHORS la zona stage. (``git add``)
  - Verificați statutul depozitului (``git status``)
  - Adăugați celelalte doua fișiere la zona stage (``git add``)
  - Salvați modificările dvs. (``git commit``).
  - Acum redenumiți fișierul AUTHORS în CONTRIBUTORS (``git mv``)
  - Adăugați numele dvs. înăuntrul fișierului CONTRIBUTORS.
  - Anulați modificările aplicate acestui fișier.

Lucrul cu un depozit pe github
--------------------------------------------------------------------------------

Până acum, am lucrat local pe calculatorul personal. Ca un cercetător și un 
utilizator de calculator, puteți dori să vă împărtășiți munca (sau mai mult, 
să contribuiți la un proiect cu surse deschise!). În acest caz github este 
foarte util. Github este o platformă web pentru găzduirea proiectelor git. Nu 
numai că oferă gratuit posibilitatea de a stoca depozite git ale proiectelor 
cu surse deschise (stocarea proiectele private se face contra plată sau pot 
fi gratuite la cerere pentru proiectele pentru studenți și femei), dar oferă 
instrumente extraordinare pentru revizuirea codului, administrarea 
proiectelor, crearea pachetelor și publicarea documentației. Majoritatea 
proiectelor bazate pe python pentru munca științifică sunt găzduite pe github. 

Și acum să trecem la github și să creăm un cont. După ce am făcut acest lucru, 
putem ușor crea un proiect, făcând clic pe butonul verde de pe prima pagină.

.. image:: ../images/github_1.png

Github vă redirecționează către o pagină unde puteți specifica denumirea 
depozitului și alte detalii. Implicit depozitele găzduite pe github sunt 
publice. În cazul când doriți un depozit privat, fie achitați 7$ în fiecare 
lună. Dacă sunteți femeie sau cadru academic puteți cere depozite private 
gratuite [#]_ [#]_.

.. [#]  `Free github reposytories for women <http://adainitiative.org/2013/04/github-donates-private-repositories-to-women-learning-open-source-software/>`_

.. [#] `Github for academics <https://github.com/edu>`_

Github va afișa o pagină ce va conține o adresă și informație referitor la 
pașii ce trebuie făcuți pentru a seta depozitul local. De notat că dacă 
decideți să creați un fișier README, fișier de licență sau .gitignore pe 
github, acestea automat for fi salvate.

.. image:: ../images/github_2.png

Pentru a utiliza acest depozit nou creat vom asocia acestuia o adresă.
Pentru a adăuga un depozit pe git cu nume prescurtat executați 
``git remote add <shortname> <url>``::

  git remote add origin <github_url>

Adresa <github_url> poate fi referită ca origin. Pentru a verifica ce depozite::

  git remote

sau::

  git remote -v

Acum că am adăugat această scurtătura putem încărca noile modificări pe server::

  git push origin master

Și acum verificați depozitul dvs. de pe github !

La fel puteți aduce modificările de pe depozitul github:: 

  git fetch origin

Acestă comandă va aduce toate modificările de pe toate ramurile din proiectul 
de la distanță (noi vom vorbi despre ramuri puțin mai târziu). Până când aceste 
modificări nu le vom integra acele modificări cu lucrul dvs.

Uneori, poate apărea necesitatea de a redenumi sau șterg o remote. Pentru a face 
acest lucru, rulați ``git remote rename <old_remote_name> <new_remote_name>`` și 
``git remote rm <remote_name>``.

Crearea ramurilor și fuzionarea acestora
--------------------------------------------------------------------------------

Pentru a înțelege mai bine ce este o ramură să revenim la ceea ce este commit. 
Un commit este o ''fotografie' a depozitului într-un moment de timp. Orice comiit 
conține meta informație: un hash ce identifică commit-ul, numele autorului, data etc. 
De asemenea conține și o referință către commit-ul părinte. Astfel, în procesul de 
comitere se creează un fel de listă înlănțuită de commit-uri. 

.. figure:: ../images/git_0-300dpi.png
   :scale: 30%

O ramură nu este altceva decât un pointer către un commit:

.. figure:: ../images/git_1-300dpi.png
   :scale: 30%

De fapt, chiar de la început până acum dvs. ați utilizat o ramură numită `master`.
Crearea unei ramuri noi presupune de fapt crearea unui pointer nou care indică spre un commit:

.. figure:: ../images/git_2-300dpi.png
   :scale: 30%


Să creăm acum o ramură cu numele ``testing``. Puteți fie utiliza comenzile::

  git branch testing

Acum, de unde stie github în care ramură dvs. lucrați? Simplu, există aun pointer denumit HEAD 
care indică către ramura curentă:

.. figure:: ../images/git_3-300dpi.png
   :scale: 30%

Puteți vedea această ramură rulând::

  git branch

Ramura cu verde marcată cu un asterisc este ramura curentă în care lucrați. 
Pentru a comuta ramurile rulați::

  git checkout testing

Puteți crea și comuta la o ramură printr-o singură linie::

  git checkout -b testing


Dacă adăugați commit-uri atât în ramura ``master`` cât și în ``testing`` codul poate deja să fie diferit:

.. figure:: ../images/git_10-300dpi.png
   :scale: 30%

Să adăugăm un commit nou::

  vim AUTHORS
  git add AUTHORS
  git status
  git commit -m "Added a new author"

Astfel ați creat un nou commit în ramura ``testing``. Vă puteți întoarce la 
la ramura ``master`` rulând::

  git checkout master

Trecând la o ramură numită are ca efect sinscronizarea fișierelor urmărite cu 
ultimul commit din ramura la care vă comutați. De notat că dacă aveți modificări 
necomise sau modificări în index, git nu va permite comutarea ramurilor. 

Pentru a incorpora modificările ramurii ``testing`` în ``master``, trebuie să 
integrați ``testing`` în master. Pentru a realiza acest lucru, asigurați-vă că sunteți 
în ramura master (folosind ``git branch``), și rulați comanda::

  git merge testing

Puteți folosi git log pentru a verifica dacă modificările au fost integrate în master.
După ce o ramură a fost integrată acesta poate fi ștearsă::

  git branch -d testing

Dacă vă amintiți cum în compartimentul precedent am discutat cum să descărcăm 
modificările dintr-un depozit la distanță folosind ````git fetch``? Am mai spus atunci 
că descărcarea n-a încorporat modificările în dosarul dvs. de lucru. Atunci ce face 
această comandă... ``git fetch`` descarcă modificările din depozitul la distanță să-i zicem 
``origin`` actualizând ramurile corespunzătoare locale ``origin/branch_name``.
Dacă doriți să vă actualizați ramura dvs. ``master`` cu modificările puse în 
``origin/master``, trebuie să integrați ``origin/master`` în ``master``::

  git merge origin/master

Puteți să descărcați schimbările în ramurile de la distanță, dar niciodată (niciodată 
niciodată!!!) să nu lucrați în ele.


Exerciții
~~~~~~~~~~

  - Creați un cont github.
  - Creați un proiect git. **Nu includeți un fișier readme, un fișier .gitignore, sau 
    orice altceva în proiect prin interfața github**. Întrucât aceste opțiuni vor crea 
    un commit care va încurca în exercițiile ulterioare.
  - Adăugați o referință către proicte la distanță numită origin (``git remote
    add``). D notat că github vă poate ghida în această privință.
  - Încărcați modificările la distanță (``git push``) și verificați dacă modificările au 
    apărut pe Github.
  - Acum creați o ramură numită ``fix``. Modificați fișierul README (adăugați un titlu), 
    adăugați fișierul în index și comiteț-l.
  - Încărcați acestă ramură în proiectul de pe github:: ``git push origin fix``. Această 
    ramura trebuie să apară în interfața github.
  - Acum integrați înapoi schimbările în ramura principală.
