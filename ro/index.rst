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
