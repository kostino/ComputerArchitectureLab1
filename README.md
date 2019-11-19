# Εργασία Πρώτου Εργαστηρίου Αρχιτεκτονικής Υπολογιστών ΤΗΜΜΥ 2019

**Βλάχος Άγγελος** ΑΕΜ : 9087  
**Τριαρίδης Κωνσταντίνος** ΑΕΜ : 9159

### Περιεχόμενα
1. Καταγραφή βασικών χαρακτηριστικών συστήματος στο starter_se
2. Επαλήθευση ερ.1 από αρχεία config.ini, config.json
3. Διαφορετικά μοντέλα in-order cpu στον gem-5 και benchmarks σε δικό μας πρόγραμμα

  * Εκτέλεση προγράμματος σε TimingSimpleCPU , MinorCPU και σύγκριση αποτελεσμάτων
  * Ερμηνεία των αποτελεσμάτων , βάσει των διαφορών των μοντέλων
  * Αλλαγή παραμέτρων CPU, memory στο ίδιο μοντέλο και σύγκριση αποτελεσμάτων


### Καταγραφή βασικών χαρακτηριστικών συστήματος στο starter_se
Στην αρχή του αρχείου _starter_se.py_ δηλώνονται τα διαφορετικά μοντέλα cpu που μπορούμε να χρησιμοποιήσουμε. Εμείς επιλέξαμε (με την επιλογή --cpu="minor ") να έχουμε **τύπο cpu** MinorCPU. Εφόσον δεν χρησιμοποιήσαμε άλλες επιλογές τα άλλα βασικά χαρακτηριστικά έχουν τις default τιμές τους , δηλαδή **συχνοτητα λειτουργίας** : 4GHz , **αριθμό πυρήνων** : 1 , **είδος κύριας μνήμης** : DDR3 με συχνότητα λειτουργίας 1600MHz, 2 κανάλια και **μεγεθος κύριας μνήμης** 2GB. Υπάρχουν και άλλα βασικά χαρακτηριστικά που δεν μπορούμε να αλλάξουμε με επιλογές κατα την εκτέλεση του προγράμματος όπως το **μέγεθος γραμμής cache** : 64 bytes . Οι caches εξαρτώνται από τον τύπο cpu που δίνουμε ως παράμετρο οπότε αφου χρησιμοποιούμε minorCPU έχουμε L1 και L2 cache σύμφωνα με την δήλωση των τύπων CPU στην αρχή του αρχείου.

### Επαλήθευση ερ.1 από αρχεία config.ini, config.json
Οι πληροφορίες στα αρχεία επιβεβαιώνουν τις παρατηρήσεις του προηγούμενου ερωτήματος
