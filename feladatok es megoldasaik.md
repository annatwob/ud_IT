### 1\.

Önaláírt tanúsítvány készítése

Készítsen önaláírt tanúsítványt a self.my-business.eu (CN) részére és mentse a következő állományba: /root/self.my-business.eu.crt.



**openssl req -x509 -nodes -days 365 -newkey rsa:2048**

**-keyout /root/self.my-business.eu.key**

**-out /root/self.my-business.eu.crt**

**-subj "/C=HU/ST=Budapest/L=Budapest/O=MyBusiness/OU=IT/CN=self.my-business.eu"**



### 2\.

Minden hozzáférés eltávolítása

Biztosítsa, hogy a /root/198784.key állományhoz egyetlen felhasználónak se legyen hozzáférése. Távolítson el minden jogosultsággal kapcsolatos bejegyzést a fájlról!



**setfacl -b /root/198784.key**

**chmod 000 /root/198784.key**



### 3\.

ACL: olvasási jog felhasználónak

Adjon olvasási jogot az operator felhasználónak a /root/198785.txt fájlhoz ACL bejegyzés segítségével. (Mást az állományon és attribútumain ne változtasson.)



**setfacl -m u:operator:r /root/198785.txt**

**getfacl /root/198785.txt**



### 4\.

Fiók lejárati dátum

Állítsa át a user198786 felhasználói fiók beállításait úgy, hogy az 2025-01-01 dátummal járjon le.



**usermod -e 2025-01-01 user198786**

**chage -l user198786**



### 5\.

Új felhasználó + jelszó zárolás

Készítsen egy új user198787 nevű felhasználót és zárolja a jelszavát!



**useradd user198787**

**usermod -L user198787**

**passwd -l user198787**



### 6\.

LUKS fájlrendszer tartalmának másolása

A /root/198789 fájl egy LUKS formátumú titkosított fájlrendszert tartalmaz. A jelszó: cbmvd. Másolja a fájlrendszer tartalmát a következő katalógusba: /root



**cryptsetup luksOpen /root/198789 asd**

**mount /dev/mapper/asd /root/asd**

**cp -a /root/asd/. /root**             

**umount + cryptsetup luksClose**



### 7\.

Fájl tulajdonosának módosítása

Állítsa át a /root/198790.txt fájl attribútumait úgy, hogy tulajdonosa az operator felhasználó legyen. (Mást ne változtasson.)



**chown operator /root/198790.txt**



### 8\.

Tanúsítvány kiállítása CSR alapján

Állítson ki tanúsítványt a /root/198792.csr állományban található tanúsítványaláírási kérés alapján a /root/198791.crt tanúsítványban szereplő szervezet nevében felhasználva annak /root/198791.key állományban tárolt privát kulcsát. A kiállított tanúsítványt a következő állományban tárolja: /root/198792.crt.



**openssl x509 -req -in /root/198792.csr -CA /root/198791.crt -CAkey /root/198791.key -CAkey /root/198791.key -CAcreateserial -out /root/198792.crt. -days 365**



### 9\.

Visszafejtés des-ede + PBKDF2

Fejtse vissza a /root/198793.enc állomány tartalmát a következő jelszóval: 198793, feltéve hogy az des-ede algoritmussal készült és a kulcsgenerálás pbkdf2 szerint történt. Az eredményt /root/198793.txt állományban tárolja.



**openssl enc -d -des-ede -pbkdf2 -in /root/198793.enc -out /root/198793.txt -pass pass:198793**



### 10\.

SHA-256 fájl aláírás

Írja alá a /root/198794.txt állományt az alábbi privát kulcs segítségével. Használjon SHA 256 hash függvényt. Az aláírás a következő állományba kerüljön: /root/198794.sign!



**openssl dgst -sha256 -sign /root/priv.key -out /root/198794.sign /root/198794.txt**



### 11\.

Nem root tulajdonú fájl keresése

Keresse meg azt az állományt a /root/198795/ katalógusban, melynek tulajdonosa nem a root felhasználó. Írja a fájl (abszolút) elérési útját a következő állományba: /root/198795.ans



**find /root/198795/ ! -user root > /root/198795.ans**



### 12\.

Readonly mindenkinek

Állítsa át a /root/198796.txt állomány jogosultságait úgy, hogy az csak olvasható legyen (bárki is férjen hozzá).



**chmod 444 /root/198796.txt**



### 13\.

Új partíció + ext2 + readonly mount

Készítsen egy új /dev/sdc1 partíciót a /dev/sdc lemezen. Formázza a partíciót ext2 fájlrendszerrel és csatolja csak olvashatóként a /mnt katalógusra.



**fdisk /dev/sdc**

**# n, p, 1, Enter, Enter, w**

**mkfs.ext2 /dev/sdc1**

**mount -o ro /dev/sdc1 /mnt**



### 14\.

RSA privát kulcs (512 bit, PKCS8)

Készítsen egy 512 bites RSA privát kulcsot és mentse el PKCS8 formátumban a következő állományba: /root/198797.pem.



**openssl genpkey -algorithm RSA -pkeyopt rsa\_keygen\_bits:512 -out /root/198797.pem.**



### 15\.

Adott bájtméretű fájl

Készítsen egy 198798 bájt méretű állományt /root/198798.dat néven.



**dd if=/dev/zero of=/root/198798.dat bs=1 count=198798**



### 16\.

Ki írta alá? (verify)

Ki írta alá a /root/contract-86302.txt fájlt: Anne, Bob, Carol, Daniel vagy Eve? Az aláírás SHA-256 hash algoritmussal készült, és a /root/contract-86302.sha256.signature fájlban van tárolva. A nyilvános kulcsok: /root/<Név>-86302.pub.pem. Az eredmény (név) kerüljön a /root/86302.ans fájlba.



**for n in Anne Bob Carol Daniel Eve; do \\**

&#x20; **echo -n "$n: "; \\**

&#x20; **openssl dgst -sha256 -verify /root/$n-86302.pub.pem \\**

&#x20;   **-signature /root/contract-86302.sha256.signature /root/contract-86302.txt; \\**

**done**

**# akinél "Verified OK":**

**echo "Carol" > /root/86302.ans**



### 17\.

Összes ACL törlése

Töröld a /root/160093.txt fájl összes ACL-bejegyzését.



**setfacl -b /root/160093.txt**



### 18\.

Első N bájt másolása

Másold a /root/160067.dat fájl első 70 bájtját a /root/160067.part fájlba.



**dd if=/root/160067.dat of=/root/160067.part bs=1 count=70**



### 19\.

AES-128-CBC visszafejtés (key + IV)

Fejtsd vissza a /root/86261.enc fájlt az aes-128-cbc algoritmus használatával. Használd a 86261 értéket kulcsként (key) és a 0 értéket inicializációs vektorként (IV). Az eredményt mentsd el a /root/86261.txt fájlba.



**openssl aes-128-cbc -d -in /root/86261.enc -K 86261 -iv 0 > /root/86261.txt**



### 20\.

SSH bejelentkezés engedélyezése privát kulccsal

Engedélyezze, hogy az alábbi privát kulcs tulajdonosa root felhasználóként be tudjon jelentkezni. (A privát kulcsot pl. /root/temp\_kulcs-be mentsd, majd a publikus részét tedd be a /root/.ssh/authorized\_keys-be.)



**chmod 600 /root/temp\_kulcs**

**mkdir -p /root/.ssh**

**chmod 700 /root/.ssh**

**ssh-keygen -y -f /root/temp\_kulcs >> /root/.ssh/authorized\_keys**

**chmod 600 /root/.ssh/authorized\_keys**



### 21\.

Új user + SSH kulcs root-hoz

Hozza létre a user160184 felhasználót, és generáljon számára egy új, alapértelmezett SSH RSA kulcsot (/home/user160184/.ssh/id\_rsa) jelszó (passphrase) megadása nélkül. Tegye lehetővé, hogy a user160184 bejelentkezhessen root-ként az RSA privát kulcs segítségével.



**useradd -m user160184**

**su - user160184 -c "ssh-keygen -t rsa -N '' -f /home/user160184/.ssh/id\_rsa"**

**cat /home/user160184/.ssh/id\_rsa.pub >> /root/.ssh/authorized\_keys**

**chmod 600 /root/.ssh/authorized\_keys**



### 22\.

Jelszó max élettartam (chage)

Állítsa be az operator felhasználónak, hogy a jelszó maximum 100 napig legyen érvényes, és kényszerítse ki azonnal a jelszócserét a következő bejelentkezéskor.



**chage -M 100 operator**

**chage -d 0 operator**



### 23\.

Sticky bites fájlok keresése

Keresse meg a /root/valami/ katalógusban azokat a fájlokat, melyeknek be van állítva a sticky bit. Az elérési utakat írja a /root/valasz fájlba.



**find /root/valami/ -perm /1000 > /root/valasz**

















