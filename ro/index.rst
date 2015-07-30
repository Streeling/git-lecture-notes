================================================================================
Git și Github
================================================================================

Acest tutorial a fost elaborat pentru conferința EuroScipy'13 și susținut în 
cadrul acestei conferințe. Tutorialul este destinat cercetătorilor care, după 
cum știm (sau nu știm), în funcție de domeniul de crecetare pot fi sau pot să nu 
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
modificărilor (versiunilor) operate asupra fișierelor proiectului astfel încât să 
puteți reveni la anumite versiuni mai târziu. De asemenea Git face ca lucrul 
colaborativ asupra unuia și aceluiași proiect să devină mai eficient. În plus, 
poate fi utilizat ca un data-centru, în cadrul căruia să fie posibilă integrarea continuă 
și automatizarea secvenței de operații care produce programul final (build [eng]).

În acest tutorial vom studia elementele de bază ale programului Git și ale 
sitului Github. Dar înainte de toate, vom răspunde la întrebarea: ce 
este Git? Git este un sistem distribuit de control al versiunilor (DVCS - 
Distributed Version Control Software [eng]). Iar  
Github este un sit web ce oferă gratuit servicii de stocare a codului sursă, gestiunea 
acestuia realizânduse doar prin Git. Există desigur și alte sisteme 
de control al versiunii, cum ar fi: mercurial și bazaar, cu siturile web asociate, 
bitbucket (care la fel suportă și Git) și, corespunzător, launchpad.  

De ce am ales (în acest tutorial) anume git, și nu mercurial, care nu doar că 
este scris în python, dar și a fost ales de CPython (și de multe alte proiecte 
bazate pe python) pentru a păstra codul? A fost o decizie simplă, grație faptului că  
în lumea științifică, git (și github) sunt mult mai mult utilizate decît mercurial
(și bitbucket). Într-adevăr, o căutare cu ajutorul servicului scholar.google.com va da 
14,100 rezultate pentru cuvîntul cheie „github”, în timp ce pentru - „bitbucket” 
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

Înainte de a trece la studierea propriu-zisă a posibilităților oferite de git, trebuie să 
știți că git este un program foarte complex și sofisticat. În acest tutorial, cum a fost deja 
menționat, vor fi prezentate doar elementele de bază ale lui Git, respectiv 
multe lucruri vor rămâne încă neclare. Dar după o perioadă de utilizare vă veți 
acomoda și veți căuta să utilizați comenzi mult mai complexe.

În linii generale, o sesiune de lucru cu Git este compusă preponderent din urmatoarele operații:

  - modificarea fișierelor în dosarul de lucru.
  - marcarea fișierelor la care ați lucrat. Această operație va pregati o captură a dosarului
  - efectuarea unui commit a fișierelor marcate. Această operație va stocha captura în depozitul Git.
  
Initializarea depozitului de fișiere și setarea programului git
--------------------------------------------------------------------------------

Există mai multe metode de a inițializa un depozit git. Prima metodă este se 
utilizează atunci când doriți să inițializați depozitul git pentru un proiect 
care deja se află pe calculatorul dvs. Pe când a doua metodă - pentru a descărca 
un depozit git existent (obțiunând în rezultat o copie locală a acestuia).

Pentru a inițializa un proiect noi, în dosarul proiectului, inițializați arhiva 
git cu ajutorul comenzii::

Dacă sunteți în situația când vi se potrivește prima metodă, atunci treceți în 
dosarul proiectului și rulați comanda::

  git init

Ca urmare, în dosarul proiectului, se va crea un dosarul ascuns .git, în care se 
va păstra istoricul tuturor modificărilor operate asupra acestui dosar și a 
subdosarelor.

A doua metodă presupune utilizarea comenzii::

  git clone https://github.com/git-lectures/git-lecture-notes.git

Această comandă va crea o copie locală a depozitului ce conține  fișierele acestui 
tutorial.

.. remarcă:: Fiecare depozit trebuie să fie în propriul său dosar. Nu este permisă  
   crearea sau duplicarea de depozite git înauntrul unui alt depozit git deja existent, 
   adică în dosarul sau subdosarule în care deja ați inițializat un depozit git. 

Înainte de a purcede mai departe haideți să setăm git-ul. Va trebuie să faceți 
acest lucru pentru fiecare calculator în parte::

  git config --global
  git config --global user.name "Numele dvs"
  git config --global user.email dvs@deomeniuldvs.com``
  git config --global core.editor vim
  git config color.ui auto

Opțiunea ``--global`` se utilizează atunci când doriți ca setările să fie la nivel 
de utilizator. Setările vor fi stocate într-un dosar ascuns în dosarul personal. 
De asemenea puteți seta fiecare depozit git în parte, scoțînd această opțiune, sau 
puteți seta pentru întreg sistemul punând în loc de ``--global`` opțiunea ``--system``. 
De obicei, se setează depozitele la nivel de utilizator și la nivel de sistem.

Puteți verifica care-s setările cu dvs. cu::

  git config --list

Dacă ați setat la diferite nivele veți vedea mai multe înregistrări care se repetă.
Setarile pentru utilizator au prioritate în raport cu setarile la nivel de sistem și 
setarile locale au prioritate mai mare în raport cu cele de utilizator.

Crearea capturilor: operația „commit” (sau sau salvarea modificărilor)
--------------------------------------------------------------------------------

Unul dintre principalele scopuri a controlului versiunii este de a salva capturi 
ale dosarului dvs. Noi numim aceste capturi commit-uri. Fiecărei capturi îi este  
asociată careva meta informație: data creărei capturii, cine a făcut-o, care 
fișiere au fost modificate, însăși modificările făcute fișierelor etc. Git vă va 
oferi posibilitatea de a urmări modificările făcute fișierelor, întoarcerea 
întregului proiect la o anumită captură (versiune) din trecut, să vedeți 
schimbările făcute dea lungul timpului, să vedeți cine a modificat fișierul.

Acum că știm de ce verm să creăm o captură, să vedem cum putem face acest lucru.
La acest acest moment nimic nu este urmărit în proiect.
În primul rând să creăm niște dosare și fișiere în dosarul dvs.

.. code-block:: bash

   touch README

Acum aveți un fișier nou în dosar. Așa cum a fost menționat anterior, git încă nu 
ține cont de acest fișier. Mai întîi trebuie să-i comunicăm că acest fișier există::

  git add README

Acum puteți să salvați modificările::

  git commit -m "A fost adăugat un fișier README"

La acest moment, aveți un fișier urmărit, și un commit inițial.
Fiecare fișier în dosarul de lucru poate fi în una din două stări: urmărit sau nu.
Orice fișier care nu a fost adăugat la un punct anumit nu este urmărit.
Fișierele urmărite la rîndul lor pot fi în două stări: nemodificate, modificate sau capturate.

.. figure:: images/git_file_status_lifecycle.png

Puteți verifica starea fiecărui fișier din dosar cu ajutorul comenzii ``git status``.
Această comandă va afișa toate fișierele ne urmărite și modificate, și capturate::

  touch AUTHORS
  git status

Remarcăm că fișierul README nu este enumerat în timp ce fișierul AUTHORS este.
Acum să adăugăm fișierul AUTHORS. Acest fișier va fi de acum urmărit și capturat::

  git add AUTHORS
  git status

``git add`` de asemenea este folosit pentru a captura un fișier. De fapt, executînd 
``git add`` asupra unui fișier ne urmărit nu numai îl va urmări dar și-l va captura. 
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
Aplicînd ``git add`` pe acest fișier nu mută modificare în zona de staging.
Pentru a șterge un fișier care a început să fie umărit de git utilizați comanda 
``git rm``. Analog, ``git mv`` poate fi utilizată pentru a muta un fișier.

Dar a greși este omenește, ați putea dori să anulați niște stages. Două scenarii 
pot apărea: (1) ați staged un fișier pe care nu vreți să-l comiteți (2) ați făcut 
niște modificări la un fișier pe care vreți să le anulați.

Mai întîi, presupunem că ați făcut stage și vreți să-l scoateți din stage::

  touch TODO
  git add TODO

Pentru unstage::

  git reset HEAD TODO

Sintaxa este ``git reset HEAD <numefișier>``. Vom explica ce este HEAD mai tîrziu

În al doilea caz am modificat un fișier și vrem sa anulăm aceste modificări.
Pentrua  face acest lucru utilizați ``git checkout <filename>``. Dacă executați 
``git status`` remarcăm că git vă amintește ce comenzi pot fi utilizate pentru fiecare 
operație.

Exerciții
~~~~~~~~~~

  - Creați un dosar cu numele GitTutorial
  - Înauntrul acestui dosar, inițializați un depozit git (``git init``)
  - Creați un fișier cu numele AUTHORS, un fișier TODO și README.
  - Adăugați fișierul AUTHORS la zona stage. (``git add``)
  - Verificați statutul depozitului (``git status``)
  - Adăugați celelalte doua fișiere la zona stage (``git add``)
  - Salvați modificările dvs. (``git commit``).
  - Acum redenumiți fișierul AUTHORS în CONTRIBUTORS (``git mv``)
  - Adăugați numele dvs. înăuntrul fișierului CONTRIBUTORS.
  - Anulați modificările aplicate acestui fișier.


