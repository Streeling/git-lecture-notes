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

Și într-o situație ideală, dvs. ați dori să evitați acest lucru:

.. figure:: ../images/version_control.gif

  "Piled Higher and Deeper" de Jorge Cham: www.phdcomics.com

Sistemele de control al versiunilor permit reținerea (păstarrea) istoriei 
tuturor versiunilor programului dvs. facilitând în rezultat accesul imediat la 
oricare din ele. ???

În acest tutorial vom studia elementele de bază a programului Git și a 
serviciului Github. Dar înainte de toate, vom răspunde la întrebarea: ce 
este Git? Git este un program pentru controlul versiunii care permite un mod 
de lucru distribuit (DVCS - Distributed Version Control Software). La rândul său 
Github este o platformă web de stocare a proiectelor care pentru controlul 
versiunii utilizează Git. Alte programe pentru controlul versiunii sunt: 
nercurial și bazaar, cu platformele web, bitbucket (care la fel suportă și git) 
și launchpad, respectiv.  

De ce am ales (în acest tutorial) anume git, și nu mercurial, care nu doar că 
este scris în python, dar și a fost ales de CPython (și de multe alte proiecte 
pe python) pentru a păstra codul? Acestă decizie a fost destul de simplă: în 
lumea științifică, git (și github) sînt mult mai mult utilizate decît mercurial
(și bitbucket). ARGUMENT STATISTIC O căutare cu ajutorul servicului cholar.google.com va da 14,100 
rezultate pentru cuvîntul cheie „github”, în timp ce pentru - „bitbucket” 
rezultatul căutării va fi în număr de 2,260. Dacă aceste date nu sunt 
suficiente pentru a vă convinge, umează un tabel de pachete pe python împărțite 
în două liste: cele găzduite pe github și cele găzduite pe bitbucket.

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

Înainte de a trece la studierea git-ului, trebuie să știți că git este un 
program foarte complex și sofisticat. În acest tutorial, cum a fost deja 
menționat, vor fi prezentate doar elementele de bază ale lui Git, respectiv 
multe lucruri vor rămâne încă neclare. Dar după o perioadă de utilizare vă veți 
acomoda și veți căuta să utilizați comenzi mult mai complexe.

Modul de lucru va fi urmatorul:

  - modificarea fișierelor în dosarul de lucru.
  - marcarea fișierelor la care ați lucrat. Această operație va pregati o captură a dosarului
  - efectuarea unui commit a fișierelor marcate. Această operație va stocha captura în depozitul Git.
