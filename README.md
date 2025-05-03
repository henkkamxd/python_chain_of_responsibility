# Tämä kuvaa tukipyyntöjä joita tulee asiakaspalveluun ja ne ovat jaettu eri tasoiksi. Niistä valitaan itselleen sopiva taso ja ne menee käsittelyyn. Jos ne eivät sovi omaan käsittelyyn ne menevät seuraavalle käsittelijälle.

from abc import ABC, abstractmethod

# Abstrakti käsittelijä
class Kasittelija(ABC):
    def __init__(self, seuraaja=None):
        self.seuraaja = seuraaja

    @abstractmethod  # decorator
    def kasittele(self, pyynto):
        pass

# Konkreettiset käsittelijät
class PerusTukiKasittelija(Kasittelija):
    def kasittele(self, pyynto):
        if pyynto == "perus":
            print("PerusTukiKasittelija käsitteli pyynnön.")
        elif self.seuraaja:
            self.seuraaja.kasittele(pyynto)
# käsittelee vain perus pyynnöt jos ei perus niin antaa seuraavalle käsittelijälle.

class TekninenTukiKasittelija(Kasittelija):
    def kasittele(self, pyynto):
        if pyynto == "tekninen":
            print("TekninenTukiKasittelija käsitteli pyynnön.")
        elif self.seuraaja:
            self.seuraaja.kasittele(pyynto)
# käsittelee vain tekniset pyynnöt jos ei ole niitä niin antaa seuraavalla käsittelijälle.

class KriittinenTukiKasittelija(Kasittelija):
    def kasittele(self, pyynto):
        if pyynto == "kriittinen":
            print("KriittinenTukiKasittelija käsitteli pyynnön.")
        elif self.seuraaja:
            self.seuraaja.kasittele(pyynto)
        else:
            print("Pyyntöä ei voitu käsitellä.")
# käsittelee kriittiset pyynnöt ja ilmoittaa jos ei pystytä käsittelemään annettua pyyntöä.

# Käsittelijäketjun rakentaminen
kriittinen = KriittinenTukiKasittelija()
tekninen = TekninenTukiKasittelija(seuraaja=kriittinen)
perus = PerusTukiKasittelija(seuraaja=tekninen)

# Esimerkkejä pyynnöistä testetaan lävitse eri pyynnöt
pyynnot = ["perus", "tekninen", "kriittinen", "tuntematon"]
for pyynto in pyynnot:
    print(f"\nKäsitellään pyyntö: {pyynto}")
    perus.kasittele(pyynto)

Käsitellään pyyntö: perus
PerusTukiKasittelija käsitteli pyynnön.

Käsitellään pyyntö: tekninen
TekninenTukiKasittelija käsitteli pyynnön.

Käsitellään pyyntö: kriittinen
KriittinenTukiKasittelija käsitteli pyynnön.

Käsitellään pyyntö: tuntematon
Pyyntöä ei voitu käsitellä.

Pitäisi tulla tälläiset toimiessa.
