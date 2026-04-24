**Schauspieler** 
{
schauspielerID: INT
name: VARCHAR(45)
vorname: VARCHAR(45)
}

**Serien**
{
serienID: INT
serienTitel: VARCHAR(45)
staffelID: INT
staffelTitel: VARCHAR(45)
episodenID: INT
episodenTitel: VARCHAR(45)
}

**Schauspieler.spieltMit.Serien**
{
schauspielerID: INT -> Schauspieler.schauspielerID
serienID: INT -> Serien.serienID
staffelID: INT -> Serien.staffelID
episodenID: INT -> Serien.episodenID
}

**Genre**
{
serienID: INT -> Serien.serienID
staffelID: INT -> Serien.staffelID
epsiodenID: INT -> Serien.episodenID
genre: VARCHAR(45)
}

