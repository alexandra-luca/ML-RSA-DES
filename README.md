# Machine Learning for Cryptographic Algorithm Identification

## Summary

This is a simulation I did while following the paper "Machine Learning for Cryptographic Algorithm Identification". Paper citation: `Barbosa, F., Vidal, A., & Mello, F. (2016). Machine learning for cryptographic algorithm identification. Journal of Information Security and Cryptography (Enigma), 3(1), 3-8.`

In this project, I created a simple neural network that is able to classify ciphertexts encrypted with either RSA or DES, slightly better than random chance (57%).

## Descriere generala

Am ales articolul “Machine Learning for Cryptographic Algorithm Identification” de F. M. Barbosa, A. R. S. F. Vidal si F. L. de Mello, din 2016, si am implementat un model ML care identifica algoritmul cu care a fost criptat un text.

Am implementat un model ML (retea neurala) in Python folosind PyTorch, care invata sa diferentieze intre doi algoritmi de criptare – RSA si DES.

Am parcurs urmatorii pasi:
* alegerea a 2 algoritmi de criptare (RSA si DES)
* crearea unei functii care genereaza texte aleatoare criptate cu algoritmii
respectivi
* crearea unui data loader
* implementarea modelului ML
* antrenarea si evaluarea rezultatelor

La final, modelul primeste ca input un ciphertext criptat cu unul din algoritmi (fie DES, fie RSA), si scoate ca output probabilitatea pentru fiecare algoritm de criptare.

## Detalii de implementare

-	Proiectul este implementat in Python, folosind JupyterLab si Conda
-	Pentru modelul de machine learning, am folosit PyTorch
-	Algoritmii de criptare RSA si DES au fost preluati din biblioteca Crypto
-	Pentru a obtine date, am generat doua dataseturi aleatoare, ambele de cate 1000 de stringuri:
    o	Datasetul “random” are stringurile formate din litere alfanumerice aleatoare
    o	Datasetul “words” are stringurile formate din cuvinte in limba engleza separate prin spatii – am preluat o lista de cuvinte in engleza de aici: 
https://github.com/dwyl/english-words/blob/master/words_dictionary.json
-	Ambele dataseturi contin stringuri de cate 128 de caractere
-	Am criptat fiecare string atat cu RSA, cat si cu DES
-	Pentru separarea train-test am realizat un split 90% - 10% (900 de texte din fiecare algoritm de criptare pentru train, 100 de texte din fiecare algoritm de criptare pentru test)
-	Deoarece criptarea RSA dureaza mult, am salvat toate textele originale si criptate intr-un fisier local (textele criptate le-am salvat codificate in format Base64)
-	Pentru a realiza interfata dintre date si modelul de ML am creat un dataset custom folosind clasa Dataset din PyTorch, si un dataloader custom folosind clasa DataLoader din PyTorch
-	Pentru antrenare, am folosit un loss de tip CrossEntropy, si un optimizator de tip SGD cu LR=0.001 si momentum=0.9
-	Reteaua neurala foloseste doua layere Linear:
    -	Un layer 128x16
    -	Un layer 16x2
    -	Ambele layere sunt urmate de un layer ReLU
-	Pentru antrenare, am rulat reteaua neurala timp de 100 de epoci
-	Pentru generarea graficelor si tabelelor, am folosit bibliotecile Matplotlib si Pandas
-	Outputul modelului este un vector cu doua probabilitati: probabilitatea ca inputul sa fi fost criptat cu RSA, si probabilitatea ca inputul sa fi fost criptat cu DES

## Rezultate experimentale

Loss-ul scade aproape constant de-a lungul celor 100 de epoci de antrenare.

<img width="306" alt="loss" src="https://user-images.githubusercontent.com/62810952/150661390-dadb14e0-846a-4be0-b406-df4a048fe7f8.PNG">

Valoare loss la antrenare. Axa OX: epoca curenta (de la 0 la 100). Axa OY: valoarea loss-ului

Accuracy-ul este calculat ca raportul dintre numarul de texte clasificate corect si numarul de texte totale din setul de test. Un accuracy de 50% este echivalent cu un model aleator. Am obtinut un accuracy care se mentine constant la 50% pana pe la epoca 70, dupa care creste la aproximativ 57%.

<img width="305" alt="accuracy" src="https://user-images.githubusercontent.com/62810952/150661404-b4b1fc0b-138a-4908-87aa-bfaa36fda19d.PNG">

Valoare accuracy pe datasetul de test. Axa OX: epoca curenta (de la 0 la 100). Axa OY: valoare accuracy

Matricea de confuzie pentru rezultatele obtinute pe datasetul de test, la finalul antrenarii, este cea din figura de mai jos. Din cele 200 de texte criptate cu RSA, modelul recunoaste cu succes 112 (56%), si returneaza rezultatul gresit pentru 88 (44%). Dintre textele criptate cu DES (tot 200 in total), modelul intoarce rezultatul corect pentru 116 (58%), si rezultatul gresit pentru 84 (42%).

<img width="171" alt="matrix" src="https://user-images.githubusercontent.com/62810952/150661418-74b2fc7f-58dc-4355-92dd-9cef9bd29352.PNG">

Matricea de confuzie obtinuta pe setul de test
