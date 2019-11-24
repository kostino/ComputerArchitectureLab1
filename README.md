# Εργασία Πρώτου Εργαστηρίου Αρχιτεκτονικής Υπολογιστών ΤΗΜΜΥ 2019

**Βλάχος Άγγελος** ΑΕΜ : 9087  
**Τριαρίδης Κωνσταντίνος** ΑΕΜ : 9159

### Περιεχόμενα
1. [Καταγραφή βασικών χαρακτηριστικών συστήματος στο starter_se](https://github.com/kostino/ComputerArchitectureLab1#Καταγραφή-βασικών-χαρακτηριστικών-συστήματος-στο-starter_se)
2. [Επαλήθευση ερ.1 από αρχεία config.ini, config.json](https://github.com/kostino/ComputerArchitectureLab1#επαλήθευση-ερ1-από-αρχεία-configini-configjson)
3. [Διαφορετικά μοντέλα in-order cpu στον gem-5 ](https://github.com/kostino/ComputerArchitectureLab1#διαφορετικά-μοντέλα-in-order-cpu-στον-gem-5) και [benchmarks σε δικό μας πρόγραμμα](https://github.com/kostino/ComputerArchitectureLab1#βenchmarks-σε-δικό-μας-πρόγραμμα-σε-timingsimplecpu-και-minorcpu) 
  a) Εκτέλεση προγράμματος σε TimingSimpleCPU , MinorCPU και σύγκριση αποτελεσμάτων  
  b) Ερμηνεία των αποτελεσμάτων , βάσει των διαφορών των μοντέλων  
  c) Αλλαγή παραμέτρων CPU, memory στο ίδιο μοντέλο και σύγκριση αποτελεσμάτων  
4. [Κριτική Εργασίας](https://github.com/kostino/ComputerArchitectureLab1#Κριτική-εργασίας)  


### Καταγραφή βασικών χαρακτηριστικών συστήματος στο starter_se
Στην αρχή του αρχείου _starter_se.py_ δηλώνονται τα διαφορετικά μοντέλα cpu που μπορούμε να χρησιμοποιήσουμε. Εμείς επιλέξαμε (με την επιλογή --cpu="minor ") να έχουμε **τύπο cpu** MinorCPU. Εφόσον δεν χρησιμοποιήσαμε άλλες επιλογές τα άλλα βασικά χαρακτηριστικά έχουν τις default τιμές τους , δηλαδή **συχνοτητα λειτουργίας** : 4GHz , **αριθμό πυρήνων** : 1 , **είδος κύριας μνήμης** : DDR3 με συχνότητα λειτουργίας 1600MHz, 2 κανάλια και **μεγεθος κύριας μνήμης** 2GB. Υπάρχουν και άλλα βασικά χαρακτηριστικά που δεν μπορούμε να αλλάξουμε με επιλογές κατα την εκτέλεση του προγράμματος όπως το **μέγεθος γραμμής cache** : 64 bytes . Οι caches εξαρτώνται από τον τύπο cpu που δίνουμε ως παράμετρο οπότε αφου χρησιμοποιούμε minorCPU έχουμε L1 και L2 cache σύμφωνα με την δήλωση των τύπων CPU στην αρχή του αρχείου.

### Επαλήθευση ερ.1 από αρχεία config.ini, config.json
Οι πληροφορίες στα αρχεία επιβεβαιώνουν τις παρατηρήσεις του προηγούμενου ερωτήματος αφού βλέπουμε πως όντως έχουμε τύπο CPU minorCPU,
```python
type=MinorCPU
```

με συχνότητα λειτουργίας 4GHz(το clock μετράει χρόνο σε ns),
```python
clock=250
```
ενώ η κύρια μνήμη έχει μέγεθος 2GB( 2^31 θέσεις ) και δύο κανάλια
```python
mem_ranges=0:2147483647
memories=system.mem_ctrls0 system.mem_ctrls1
```
και οι caches μέγεθος γραμμής 64 bytes
```python
cache_line_size=64
```
### Διαφορετικά μοντέλα in-order cpu στον gem-5
Στον gem5 υπάρχουν τρία μοντέλα in-order CPUs : το MinorCPU και τα δύο μοντέλα με βάση το SimpleCPU , το TimingSimpleCPU και το AtomicSimpleCPU.
#### SimpleCPU
Το SimpleCPU αποτελεί ένα πολύ απλό στην υλοποίηση  in order μοντέλο κατασκευασμένο για χρήση σε απλά τεστ , όπου δεν είναι απαραίτητη η χρήση κάποιου πιο περίπλοκου μοντέλου.
##### AtomicSimpleCPU
Το AtomicSimpleCPU αποτελεί υλοποίηση του SimpleCPU μοντέλου που χρησιμοποιεί πρόσβαση στη μνήμη τύπου atomic(_atomic memory access_). Αυτό σημαίνει ότι υπολογίζει προσεγγιστικά τον χρόνο πρόσβασης στην cache από τις χρονικές προσεγγίσεις των atomic accesses οι οποίες επιστρέφουν μια προσέγγιση του χρόνου που θα χρειαστούν για το access , ενώ η επιστροφή τιμής γίνεται στο τέλος του access function. Με αυτό τον τρόπο αποφεύγεται το queuing delay και το resource contention διότι η cpu γνωρίζει προσεγγιστικά τον χρόνο πρόσβασης στην μνήμη πριν αυτή ολοκληρωθεί.
##### TimingSimpleCPU
Το TimingSimpleCPU αποτελεί υλοποίηση του SimpleCPU μοντέλου που χρησιμοποιεί πρόσβαση στη μνήμη τύπου timing(_timing memory access_). Αυτό σημαίνει ότι σε κάθε πρόσβαση στην cache καθυστερεί και περιμένει την απάντηση από το σύστημα μνήμης (είτε NACK εάν δεν μπορούσε να ολοκληρωθεί το αίτημα είτε την τιμή στην μνήμη που ζητήθηκε) πριν συνεχίσει την εκτέλεση εντολών ,υπάρχει δηλαδή resource contention και queuing delay , αφού ο επεξεργαστής περιμένει την ολοκλήρωση της πρόσβασης στην μνήμη για να συνεχίσει.
#### MinorCPU
Το MinorCPU μοντέλο αποτελεί ένα in-order μοντέλο με συγκεκριμένο pipeline(Fetch1-Fetch2-Decode-Execute) αλλά προσαρμοζόμενα data structures και προσαρμοζόμενη συμπεριφορά εκτέλεσης. Το σταθερό pipeline του βοηθάει στην αναγνώριση και οπτικοποίηση μέσα σε αυτό της κάθε εντολής από μία προσομοίωση. Ο MinorCPU έχει επίσης υλοποιημένο έναν branch predictor που ενεργεί στο δεύτερο στάδιο του pipeline(Fetch2).
###  Βenchmarks σε δικό μας πρόγραμμα σε TimingSimpleCPU και MinorCPU
Αρχικά τρέχουμε πρόγραμμα που κατασκευάζει πίνακα με 1000 τυχαίους ακεραίους και μετράει πόσοι από αυτούς είναι άρτιοι.  
Στην αρχή χρησιμοποιούμε default παραμέτρους(freq=1GHz , mem= DDR3 1600MHz 8x8 500MB) και το τρέχουμε με μοντέλο CPU MinorCPU και TimingSimpleCPU.
```zsh
$ ./build/ARM/gem5.opt -d labres/ configs/example/se.py --cpu-type=MinorCPU --caches -c gem5TestARM
$ ./build/ARM/gem5.opt -d labres/ configs/example/se.py --cpu-type=TimingSimpleCPU --caches -c gem5TestARM

```
Τα αποτελέσματα χρόνου εκτέλεσης sim_seconds είναι τα εξής:
* _MinorCPU_ : 0.001158 s
* _TimingSimpleCPU_ : 0.002400 s

Προφανώς η διαφορά είναι ιδιαίτερα αισθητή. Αρχικά ο MinorCPU έχει υλοποιημένο branch predictor. Σε ένα πρόγραμμα όπως **η προηγούμενη έκδοση** (αλλάχθηκε διότι η μικρή του πολυπλοκότητα δεν μας επέτρεπε να δούμε ξεκάθαρα τις πολύ αισθητές διαφορές στον χρόνο εκτέλεσης από τα δύο μοντέλα) του δικού μας με μία μεγάλη for loop που αυξάνει έναν counter κάθε φορά , δηλαδή ένα branch στο οποίο φτάνει το πρόγραμμα πολλές φορές στη σειρά και είναι taken πολλές φορές απανωτά ο predictor έχει πάρα πολύ μεγάλο ποσοστό επιτυχίας(_~80%_) άρα συνολικά το πρόγραμμα πολύ γρηγορότερη εκτέλεση σε αυτό το CPU Model.Η καινούργια έκδοση του προγράμματος μας κάνει και έναν έλεγχο μέσα στη loop οπότε ο branch predictor έχει αρκετά χαμηλότερη ακρίβεια.Είναι ένα πρόγραμμα που περιλαμβάνει πολλές απανωτές **προσβάσεις στη μνήμη** όμως , οπότε η pipelined λογική του _MinorCPU_ μοντέλου του δίνει πολύ μεγάλο πλεονέκτημα σε σχέση με την λογική του _TimingSimpleCPU_ ο οποίος μπλοκάρει την εκτέλεση περιμένοντας το αποτέλεσμα της πρόσβασης στη μνήμη.  

### Αλλαγή παραμέτρων CPU , memory και ερμηνεία αποτελεσμάτων
1. **Αρχικά αλλάξαμε και στα δύο μοντέλα το ρολόι στα 2 και 5 GHz και κάναμε μετρήσεις χρόνου εκτέλεσης(χρόνος σε sec):**  
>![Bench with diff clocks](https://i.imgur.com/XjJdX62.png)  

Όπως βλέπουμε στο πινακάκι απο πάνω η επιτάχυνση του μοντέλου _TimingSimpleCPU_ είναι ελάχιστη (**0.2%**) καθώς το μοντέλο περιορίζεται από τις αργές προσβάσεις στη μνήμη οι οποίες συνεχίζουν να κάνουν bottleneck το πρόγραμμα παρά την αύξηση του ρολογιού του επεξεργαστή. Στην περίπτωση του μοντέλου _MinorCPU_ η επιτάχυνση είναι σημαντικά μεγαλύτερη (**5.1%**) αφού επιταχύνονται τα στάδια του pipeline που δεν αφορούν την πρόσβαση στη μνήμη. Η επιτάχυνση και σε αυτή την περίπτωση περιορίζεται βέβαια από τον σχεδιασμό του προγράμματος μας, καθώς σε ένα πρόγραμμα που θα είχε λιγότερες προσβάσεις μνήμης και περισσότερες αριθμητικές η λογικές πράξεις η επιτάχυνση θα ήταν μεγαλύτερη.

2. **Στη συνέχεια διατηρώντας το ρολόι στο 1GHz αλλάξαμε την μνήμη σε DDR4 στα 2400Mhz:**  
>![Bench with diff memory](https://i.imgur.com/qTzjndE.png)  

Παρατηρούμε ότι δεν αλλάζει καθόλου ο χρόνος εκτέλεσης σε κανένα από τα δύο μοντέλα ενώ ενστικτωδώς προβλέψαμε ότι θα είχαμε μεγάλη επιτάχυνση (γρηγορότερη μνήμη => γρηγορότερη πρόσβαση). Ένας πιθανός λόγος για το αποτέλεσμα αυτό είναι η κατασκευή του προγράμματος μας: Κατασκευάζουμε πίνακες μεγέθους 1000 στοιχείων οι οποίοι είναι αρκετά μικροί οπότε μεγάλα κομμάτια τους φορτώνονται σε ένα block της cache. Έτσι οι απανωτές προσβάσεις γίνονται στην cache και οι προσβάσεις στην κύρια μνήμη είναι ελάχιστες άρα η αλλαγή της ταχύτητας στην κύρια μνήμη δεν έχει αισθητό αποτέλεσμα.  
**[Όλες οι μετρήσεις](https://docs.google.com/spreadsheets/d/1jACGWOk938Q6ivjetw6T5KrBNOgfjyQoj8Q4R_XHtiI/edit?usp=sharing)**  
### Κριτική εργασίας
Η εργασία αυτή εάν και συνολικά εύκολη στο κομμάτι της γνωριμίας με το περιβάλλον του gem5,τα αρχεία με τα στατιστικά,τις παραμέτρους εκτέλεσης κτλ και εκεί δεν αντιμετωπίσαμε ιδιαίτερο πρόβλημα. Μας έβαλε , επίσης, στη διαδικασία να μπορούμε να προσεγγίσουμε ένα κακογραμμένο documentation (δηλαδή αυτό του gem5) και να αναζητάμε πληροφορίες για πολύ εξειδικευμένα προγράμματα και τεχνολογίες. Επίσης μπήκαμε στη λογική να μάθουμε την πλατφόρμα που χρησιμοποιούμε και να μην ακολουθούμε απλά τις οδηγίες, καθώς χρησιμοποιήσαμε OSX Catalina και παρά τις οδηγίες στο documentation του gem5 το build της πλατφόρμας ήταν σκέτος εφιάλτης καθώς προέκυπταν προβλήματα παντού παρόλο που σύμφωνα με τους κατασκευαστές του gem5 η πλατφόρμα είναι optimised για build σε OSX και Linux. Βέβαια αυτό με βοήθησε προσωπικά να ψάξω και να καταλάβω πως λειτουργούν οι package installers στο OSX και πως χειρίζονται διαφορετικά το ίδιο πράγμα διαφορετικοί c compilers. **Συμβουλή** για χρήστες OSX: χρησιμοποιήστε homebrew για όλα τα πακέτα(έχει και το python-pip) και μήν χρησιμοποιήσετε τον ήδη εγκατεστημένο gcc(clang 10.5) καθώς σχεδόν στο τέλος του build( 45 λεπτά αφου ξεκίνησε .........) πετάει error: hidden overloaded virtual function. Κατεβάστε gcc-7 μέσω brew και ονομάστε τον ως default gcc έτσι(στο OSX δεν υπάρχει update alternatives):
```zsh
$ brew install gcc-7
$ export PATH=/usr/local/bin:$PATH
$ cd /usr/local/bin
$ ln -s  gcc-7  gcc

```

### Πηγές
* [Gem5 Documentation](http://gem5.org/Main_Page)  
* [The Arm Research Starter Kit: System Modeling using gem5](https://github.com/arm-university/arm-gem5-rsk/blob/master/gem5_rsk.pdf)
