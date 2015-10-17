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
acestuia realizându-se doar prin Git. Există desigur și alte sisteme 
de control al versiunii, cum ar fi: mercurial și bazaar, cu siturile web asociate, 
bitbucket (care la fel suportă și Git) și, corespunzător, launchpad.  

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

Înainte de a trece la studierea propriu-zisă a posibilităților oferite de git, trebuie să 
știți că git este un program foarte complex și sofisticat. În acest tutorial, cum a fost deja 
menționat, vor fi prezentate doar elementele de bază ale lui Git, respectiv 
multe lucruri vor rămâne încă neclare. Dar după o perioadă de utilizare vă veți 
acomoda și veți căuta să utilizați comenzi mult mai complexe.

În linii generale, o sesiune de lucru cu Git este compusă preponderent din urmatoarele operații:

  - modificarea fișierelor în dosarul de lucru.
  - marcarea fișierelor la care ați lucrat. Această operație va pregati o captură a dosarului
  - efectuarea unui commit a fișierelor marcate. Această operație va stoca captura în depozitul Git.
  
Inițializarea depozitului de fișiere și setarea programului git
--------------------------------------------------------------------------------

Există mai multe metode de a inițializa un depozit git. Prima metodă se 
utilizează atunci când doriți să inițializați depozitul git pentru un proiect 
care deja se află pe calculatorul dvs. Pe când a doua metodă - pentru a descărca 
un depozit git existent (obțiunând în rezultat o copie locală a acestuia).

Dacă sunteți în situația când vi se potrivește prima metodă, atunci treceți în 
dosarul proiectului și rulați comanda::

  git init

Ca urmare, în dosarul proiectului, se va crea un dosar ascuns .git, în care se 
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
