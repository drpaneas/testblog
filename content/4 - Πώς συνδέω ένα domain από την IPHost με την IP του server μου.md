Title: Πώς συνδέω ένα domain (IPHost) με τον server μου
Date: 2015-01-021 20:23
Tags: iphost, domain, dns
Category: how-to
Slug: pos-sindeo-ena-domain-apo-tin-IPHost-me-tin-ip-tou-server-mou
Author: Πανος Γεωργιαδης

Καταρχήν να ξεκινήσω λέγοντας ότι **δεν** έχω σκοπό να κάνω διαφήμιση την *IPHost*.
Ο λόγος που την χρησιμοποιώ, είναι γιατί ήμουν αρκετά τυχερός ώστε να κερδίσω ένα *domain* με κατάληξη **.eu** σ'έναν διαγωνισμό
του [Insomnia](http://insomnia.gr). Ε, αφού το κέρδισα, λογικό (και επόμενο είναι) να το χρησιμοποιήσω. 
Η αλήθεια είναι ότι σπάνια παίρνω μέρος σε διαγωνισμούς. Συνήθως τα Χριστούγεννα θα δείτε ότι
γίνεται πανικός σε διάφορα γνωστά *forums*, όπως είναι το [Thelab.gr](http://thelab.gr), το [Insomnia.gr](http://insomnia.gr), [HWBox](http://hwbox.org), [deltahacker](http://deltahacker.gr) κ.α. τότε πραγματικά αξίζει να πάρεις μέρος γιατί έχουν μερικά πολύ
όμορφα δώρα. Θυμάμαι χαρακτηριστικά, έναν πολύ διασκεδαστικό διαγωνισμό που είχα πάρει μέρος: ήταν στο *παλιό - καλό* **OutofSpecs**, όπου ο νικητής θα ήταν αυτός που θα κατάφερνε να πετύχει το χαμηλότερο score στο [3DMark 2001 SE](http://www.futuremark.com/benchmarks/legacy).

> Οι καλύτερες εποχές!

Τέλος πάντων, ας μην μακρυγορώ και ας μπω στο θέμα: Όπως θα ξέρετε ήδη[^1], έχω αγοράσει έναν *dedicated server* αξίας **5 Ευρώ**.
 Ω ναι, μιλάμε για real hardware και όχι για κάποιο VPS με *shared resources*. Βλέπετε, μετά από αυτό που τράβηξα με την *Dreamhost*
και το [UbuntuXtreme.com](http://ubuntuxtreme.com), όπου κάθε *"τρεις-και-λίγο"* το VPS έκανε reboot και με χρεώνε λόγω έλειψης
πόρων κάνοντας ταυτόχρονα και το website μή προσπελάσιμο από τους επισκέπτες, υποσχέθηκα στον εαυτό μου ότι δεν θα δοκιμάσω ποτέ
ξανά VPS. Ποτέ ξανά VPS. Τα VPS είναι @&*^$&^@#*^ !!!

DNS
===
Προφανώς και όταν θέλουμε ένα domain να κάνει **resolve σε μία IP**, την δουλειά την αναλαμβάνει ο DNS. Στην συγκεκριμένη
περίπτωση, θα πρέπει να πάμε στην εταιρία που έχουμε κάνει register το Domain μας, και εκεί, κάπου, στον πίνακα ελέγχου,
θα πρέπει να μας παρέχει την δυνατότητα να τροποιποιήσουμε τα *DNS Records/Zone*. Πήγα λοιπόν στο *control panel* της IPHost,
έκανα login, πήγα στα "Domains μου" και ιδού:

![Pinakas IPHost](/images/iphost1.png)

Στην συνέχεια πατάμε **Δημιουργία/Διαχείριση Ζώνης DNS**

![DNS Zone IPHost](/images/iphostdnszone.png)

... στην συνέχεια βάζω την IPv4 του *Dedicated Server* μου, η οποία είναι `37.187.4.87`, και περιμένω να ρυθμιστούν όλα μόνα τους (seriously, πολύ εύκολο :P )

![DNS Zone Completed](/images/iphostzoneok.png)

Επαλήθευση, ότι λειτουργεί
==========================
Επειδή παίρνει κάποια ώρα το όλο *update* των *nameservers*, μην περιμένετε να δείτε άμεση ανταπόκριση από τον browser. Στην δική μου περίπτωση δεν περιμένω έτσι κι αλλιώς, αφού δεν τρέχω κάποιον webserver την TCP 80. Σε πρώτη φάση λοιπόν, ας ανοίξουμε το τερματικό και ας παίξουμε με τα τόσα *παιχνίδια* που υπάρχουν στο Linux.

```bash
nslookup pingouinos.eu
Server:		192.168.178.1
Address:	192.168.178.1#53

** server can't find pingouinos.eu: NXDOMAIN
```

Όπως φαίνεται ακόμα δεν έχουν ανανεωθεί οι DNS. *Ιδέα*: Να χρησιμοποιήσω τους DNS της Google[^2], οι οποίοι
προφανώς είναι πολύ καλύτεροι από αυτούς που χρησιμοποιεί ο γερμανικός ISP μου. Για όσους δεν γνωρίζουν, οι DNS της Google είναι
`8.8.8.8` και `8.8.4.4`. Οπότε δίνουμε ξανά την ίδια εντολή, μόνο που αυτή την φορά το request θα γίνει από την Google:

```bash
 nslookup pingouinos.eu 8.8.8.8
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	pingouinos.eu
Address: 37.187.4.87
```

Ώπα! Η Google είναι ενημερωμένη, βλέπει τις αλλαγές! Αυτό σημαίνει ότι όλα τα κάναμε σωστά και λίγες ώρες,
θα ενημερωθεί και ο δικός μας πάροχος και *όλα ok*. Μπορείτε επίσης να διευρήνεσετε ακόμα περισσότερο τα πράγματα
χρησιμοποιώντας την εντολή `dig`, η οποία ρωτάει για πληροφορίες όπως `A`, `NS`, `MX`. Για παράδειγμα:

```bash
dig pingouinos.eu NS 8.8.8.8

; <<>> DiG 9.8.3-P1 <<>> pingouinos.eu NS 8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46292
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 0

;; QUESTION SECTION:
;pingouinos.eu.			IN	NS

;; AUTHORITY SECTION:
eu.			516	IN	SOA	ns04.iphost.gr. admin.iphost.gr. 2009022301 14400 7200 604800 14400

;; Query time: 2 msec
;; SERVER: 192.168.178.1#53(192.168.178.1)
;; WHEN: Wed Jan 21 21:01:41 2015
;; MSG SIZE  rcvd: 87

;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 19434
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 0

;; QUESTION SECTION:
;8.8.8.8.			IN	A

;; AUTHORITY SECTION:
.			783	IN	SOA	a.root-servers.net. nstld.verisign-grs.com. 2015012101 1800 900 604800 86400

;; Query time: 1 msec
;; SERVER: 192.168.178.1#53(192.168.178.1)
;; WHEN: Wed Jan 21 21:01:41 2015
;; MSG SIZE  rcvd: 100
```
Κάπου εδώ θα σταματήσω, και θα επανέλθω αύριο να ανανεώσω το άρθρο, ώστε να είμαστε **100% σίγουροι** ότι λειτούργησε η *"σύνδεση"*
και δεν σας γράφω σαχλαμάρες.

--------------------------------------------
# 24 ώρες μετά ...

Έχει περάσει σχεδόν μία μέρα, και ήρθε η στιγμή να επαληθεύσουμε αν το *pingouinos.eu* κάνει *resolve*
στην IP του server μου:

```bash
drpaneas@macbook:~/Desktop$ nslookup pingouinos.eu
Server:		192.168.178.1
Address:	192.168.178.1#53

Non-authoritative answer:
Name:	pingouinos.eu
Address: 37.187.4.87
```

Τέλεια! Οπότε τωρα δεν χρειάζεται να θυμάμαι την IP :D

[^1]: Διαβάστε το άρθρο [New dedicated server in da house με 5 Eυρώ](http://linuxed.gr/editorial/new-dedicated-server-in-da-house-me-5-euro/)
[^2]: Περισσότερες πληροφορίες για τους Public Google DNS θα βρείτε [εδώ](https://developers.google.com/speed/public-dns/)
