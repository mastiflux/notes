
**Aufgabe 1**

Medium 
(
	medium_ID: INT
	titel: VARCHAR(35)
	fsk_freigabe: INT
	typ: VARCHAR(35)
)

Genre
(
	genre: VARCHAR(35)
	mediumID: INT -> Medium.medium_ID
)

Datenträger 
(
	datenträgerID: INT
	tagesgebühr: FLOAT
	typ: VARCHAR(35)
	anzahl: INT
	standort: VARCHAR(35)
)

Kunden 
(
	kundenID: INT
	name: VARCHAR(35)
	vorname: VARCHAR(35)
	geburtsdatum: DATE
	anrede: VARCHAR(35)
	titel: VARCHAR(35)
	alter: INT
)

Adresse 
(
	straße: VARCHAR(35)
	hausNummer: INT
	stadt: VARCHAR(35)
	plz: INT
	kundenID: INT -> Kunden.kundenID
)

Datenträger.speichert.Medium 
(
	datenträgerID: INT -> Datenträger.datenträgerID
	mediumID: INT -> Medium.medium_ID
)

Kunden.leihen.Datenträger 
(
	ausleihdatum: DATE
	rückgabedatum: DATE
	gesamtgebühr: FLOAT
	bezahlt: TINYINT
	kundenID: INT -> Kunden.kundenID
	datenträgerID: INT -> Datenträger.datenträgerID
)

---

**Aufgabe 2**

- Alter verletzt Normalformregel (ist abhängig von Geburtsdatum)    -> entfernen

*fertige Normalform:*
! ==hervorgehobene== Attribute = Schlüssel !

Medium 
(
	==medium_ID==: INT
	titel: VARCHAR(35)
	fsk_freigabe: INT
	typ: VARCHAR(35)
)

Genre
(
	genre: VARCHAR(35)
	==mediumID==: INT -> Medium.medium_ID
)

Datenträger 
(
	==datenträgerID==: INT
	tagesgebühr: FLOAT
	typ: VARCHAR(35)
	anzahl: INT
	standort: VARCHAR(35)
)

Kunden 
(
	==kundenID==: INT
	name: VARCHAR(35)
	vorname: VARCHAR(35)
	geburtsdatum: DATE
	anrede: VARCHAR(35)
	titel: VARCHAR(35)
)

Adresse 
(
	straße: VARCHAR(35)
	hausNummer: INT
	stadt: VARCHAR(35)
	plz: INT
	==kundenID==: INT -> Kunden.kundenID
)

Datenträger.speichert.Medium 
(
	==datenträgerID==: INT -> Datenträger.datenträgerID
	==mediumID==: INT -> Medium.medium_ID
)

Kunden.leihen.Datenträger 
(
	ausleihdatum: DATE
	rückgabedatum: DATE
	gesamtgebühr: FLOAT
	bezahlt: TINYINT
	==kundenID==: INT -> Kunden.kundenID
	==datenträgerID==: INT -> Datenträger.datenträgerID
)



