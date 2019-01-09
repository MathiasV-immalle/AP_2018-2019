# Klassen
## Oefening 1
### Klasse Program
```csharp
class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Geef voornaam: ");
            string voornaam = Console.ReadLine();
            Console.Write("Geef achternaam: ");
            string achternaam = Console.ReadLine();
            Console.Write("Geef geboortedatum: ");
            DateTime geboorteDatum = DateTime.Parse(Console.ReadLine());
            Persoon persoon = new Persoon(voornaam, achternaam, geboorteDatum);
            Console.WriteLine(persoon);
        }
    }
```
### Klasse Persoon
```csharp
class Persoon
    {
        private DateTime geboorteDatum;
        private DateTime minimumGeboorteDatum = new DateTime(1990,01,01);

        public string Voornaam { get; set; }
        public string Achternaam { get; set; }
        public DateTime GeboorteDatum
        {
            get { return geboorteDatum; }
            set {
                if (value >= minimumGeboorteDatum && value <= DateTime.Today)
                    geboorteDatum = value;
                else
                    geboorteDatum = DateTime.Today;
            }
        }
        public Persoon(string voornaam, string achternaam, DateTime geboorteDatum)
        {
            this.Voornaam = voornaam;
            this.Achternaam = achternaam;
            this.GeboorteDatum = geboorteDatum;
        }

        private int BerekenLeeftijd()
        {
            int jaren = DateTime.Today.Year - geboorteDatum.Year;
            DateTime verjaardag = new DateTime(DateTime.Today.Year, geboorteDatum.Month, geboorteDatum.Day);
            if (DateTime.Today <= verjaardag)
                jaren -= 1;
            return jaren;
        }
        public override string ToString()
        {
            string uitvoer = "Voornaam: " + Voornaam + "\nAchternaam: " + Achternaam + "\nLeeftijd: " + BerekenLeeftijd();
            return uitvoer;
        }

    }
```
## Oefening 2
### Klasse Program
```csharp
class Program
    {
        static Random random = new Random();
        static void Main(string[] args)
        {
            Kaart kaart = new Kaart();
            Kaart[] kaarten = new Kaart[5];
            kaarten = kaart.BepaalRandomKaarten();
            for (int teller = 0; teller < 5; teller++)
                Console.WriteLine(kaarten[teller]);

            //Methode BepaalVolgendeKaart testen
            Kaart kaart1 = new Kaart(Waarden.Koning, Soorten.Ruiten);
            Console.WriteLine("****************************************");
            Console.WriteLine(kaart1);
            Console.WriteLine(kaart1.BepaalVolgendeKaart());
        }
    }
```
### Klasse Speelkaarten
```csharp
enum Waarden { Aas, Twee, Drie, Vier, Vijf, Zes, Zeven, Acht, Negen, Tien, Boerin, Dame, Koning};
    enum Soorten { Schoppen, Klaveren, Ruiten, Harten };
    class Kaart
    {
        static Random random = new Random();
        private Waarden waarde;
        const int AANTAL_WAARDEN = 5;

        private Waarden Waarde
        {
            get { return waarde; }
            set
            {
                if (value <= Waarden.Koning && value >= Waarden.Aas)
                    waarde = value;
                else
                    waarde = Waarden.Aas;
            }
        }

        private Soorten soort;

        private Soorten Soort
        {
            get { return soort; }
            set
            {
                if (value <= Soorten.Harten && value >= Soorten.Schoppen)
                    soort = value;
                else
                    soort = Soorten.Schoppen;
            }
        }


        public Kaart(Waarden waarde, Soorten soort)
        {
            Waarde = waarde;
            Soort = soort;
        }
        public Kaart() : this((Waarden)random.Next(0, 13), (Soorten)random.Next(0, 4))
        {
        }


        public Kaart BepaalVolgendeKaart()
        {
            Waarden waarde = (Waarden)(((int)Waarde + 1) % 13);
            Soorten soort = this.Soort;

            if (Waarde == Waarden.Aas)
            {
                soort = (Soorten)(((int)Soort + 1) % 4);
            }


            Kaart kaart = new Kaart(waarde, soort);
            return kaart;
        }

        public Kaart[] BepaalRandomKaarten()
        {
            Kaart[] kaarten = new Kaart[AANTAL_WAARDEN];
            for(int teller = 0; teller < AANTAL_WAARDEN; teller++)
            {
                kaarten[teller] = new Kaart();
            }
            return kaarten;
        }

        public override string ToString()
        {
            return "Waarde = " + Waarde + "\nSoort = " + Soort;
        }
    }
```
## Oefening 3
### Klasse Program
```csharp
class Program
    {
        public static void Main(string[] args)
        {
            const int AANTAL_KEREN_GOOIEN = 1000;
            PaarDobbelstenen paarDobbelstenen = new PaarDobbelstenen();
            Console.WriteLine(paarDobbelstenen.KerenDubbelZes(AANTAL_KEREN_GOOIEN));
        }
    }
```
### Klasse Dobbelsteen
```csharp
public class Dobbelsteen
    {
        static Random random = new Random();

        private int waarde;

        public int Waarde
        {
            get { return waarde; }
            set {
                    if (value <= 6 && value > 0)
                        waarde = value;
                    else
                        waarde = 1;
                }
        }

        public Dobbelsteen(int waarde)
        {
            Waarde = waarde;
        }

        public Dobbelsteen() : this(random.Next(1, 7))
        {

        }

        public bool IsZes()
        {
            return Waarde == 6;
        }

        public int Gooi()
        {
            Waarde = random.Next(1, 7);
            return Waarde;
        }

        public override string ToString()
        {
            return "Waarde: " + Waarde;
        }
    }
```
### Klasse PaarDobbelstenen
```csharp
public class PaarDobbelstenen
    {
        public Dobbelsteen Dobbelsteen1 { get; set; }
        public Dobbelsteen Dobbelsteen2 { get; set; }

        public PaarDobbelstenen(Dobbelsteen dobbelsteen1, Dobbelsteen dobbelsteen2)
        {
            Dobbelsteen1 = dobbelsteen1;
            Dobbelsteen2 = dobbelsteen2;
        }

        public PaarDobbelstenen() : this(new Dobbelsteen(), new Dobbelsteen())
        {

        }

        public bool DubbelZes()
        {
            return Dobbelsteen1.IsZes() && Dobbelsteen2.IsZes();
        }

        public void Gooien()
        {
            Dobbelsteen1.Gooi();
            Dobbelsteen2.Gooi();
        }

        public int KerenDubbelZes(int aantalKeer)
        {
            int aantalKerenZes = 0;
            for(int teller = 0; teller < aantalKeer; teller++)
            {
                this.Gooien();
                if (DubbelZes())
                    aantalKerenZes++;
            }
            return aantalKerenZes;

        }

        public override string ToString()
        {
            return "Dobbelsteen1: " + Dobbelsteen1.Waarde + "\nDobbelsteen2: " + Dobbelsteen2.Waarde;
        }
    }
```
## Oefening4
### Klasse Program
```csharp
class Program
{
    static Random random = new Random();
    static void Main(string[] args)
    {
        Reis reis1 = new Reis(new DateTime(2018 ,10, 1) ,new DateTime(2018, 10, 10) ,(Vervoersmiddelen)random.Next(3) ,(Verblijven)random.Next(2));
        Reis reis2 = new Reis(new DateTime(2019 , 1, 1), new DateTime(2019, 1, 25), (Verblijven)random.Next(2));
        Reis reis3 = new Reis(DateTime.Today, DateTime.Today);

        Console.WriteLine(reis1);
        Console.WriteLine(reis2);
        Console.WriteLine(reis3);
    }
}
```
### Klasse Reis
```csharp
enum Vervoersmiddelen { Vliegtuig, Bus, Trein }
enum Verblijven { Hotel, Jeugdherberg }
class Reis
{
    public const int PRIJS_VLIEGTUIG = 500;
    public const int PRIJS_TREIN = 350;
    public const int PRIJS_BUS = 200;
    public const int PRIJS_HOTEL_PER_DAG = 75;
    public const int PRIJS_JEUGDHERBERG_PER_DAG = 20;
    public const double KORTING_2WEKEN = 90D / 100;
    public const double KORTING_4WEKEN = 85D / 100;
    public const int GEEN_KORTING = 1;
    public const int TWEE_WEKEN = 14;
    public const int VIER_WEKEN = 28;

    private DateTime vertrekHeenDatum;

    public DateTime VertrekHeenDatum
    {
        get { return vertrekHeenDatum; }
        set
        {
            //if (DateTime.Compare(value, DateTime.Today) < 0)
            if (value > DateTime.Today)
                vertrekHeenDatum = DateTime.Today;
            else
                vertrekHeenDatum = value;
        }
    }

    private DateTime vertrekTerugKeerDatum;

    public DateTime VertrekTerugKeerDatum
    {
        get { return vertrekTerugKeerDatum; }
        set
        {
            if (DateTime.Compare(value, vertrekHeenDatum) > 0)
                vertrekTerugKeerDatum = value;
            else
                vertrekTerugKeerDatum = vertrekHeenDatum.AddDays(7);
        }
    }

    public Vervoersmiddelen Vervoer { get; set; }
    public Verblijven Verblijf { get; set; }

    public Reis(DateTime sVertrekHeenDatum, DateTime sVertrekTerugKeerDatum, Vervoersmiddelen sVervoer,  Verblijven sVerblijf)
    {
        this.VertrekHeenDatum = sVertrekHeenDatum;
        this.VertrekTerugKeerDatum = sVertrekTerugKeerDatum;
        this.Vervoer = sVervoer;
        this.Verblijf = sVerblijf;
    }

    public Reis(DateTime sVertrekHeenDatum, DateTime sVertrekTerugKeerDatum) : this(sVertrekHeenDatum, sVertrekTerugKeerDatum, Vervoersmiddelen.Vliegtuig, Verblijven.Hotel)
    {
    }

    public Reis(DateTime sVertrekHeenDatum, DateTime sVertrekTerugKeerDatum, Verblijven sVerblijf) : this(sVertrekHeenDatum, sVertrekTerugKeerDatum, Vervoersmiddelen.Vliegtuig, sVerblijf)
    {
    }

    public int TotaalPrijs()
    {
        return VervoersPrijs() + VerblijfsPrijs();
    }

    public int VerblijfsPrijs()
    {
        TimeSpan reisDuur = VertrekTerugKeerDatum.Subtract(VertrekHeenDatum);
        int prijsVerblijf;

        if (Verblijf == Verblijven.Hotel)
            prijsVerblijf = PRIJS_HOTEL_PER_DAG;
        else
            prijsVerblijf = PRIJS_JEUGDHERBERG_PER_DAG;

        return (prijsVerblijf * reisDuur.Days); 
    }

    public int VervoersPrijs()
    {
        int prijsVervoer;

        if (Vervoer == Vervoersmiddelen.Bus)
            prijsVervoer = PRIJS_BUS;
        else if (Vervoer == Vervoersmiddelen.Trein)
            prijsVervoer = PRIJS_TREIN;
        else
            prijsVervoer = PRIJS_VLIEGTUIG;

        return prijsVervoer;
    }

    public double TotaalPrijsMetKorting()
    {

        return VervoersPrijs() + (VerblijfsPrijs() * Korting());
    }

    public double Korting()
    {
        double korting;
        TimeSpan reisDuur = VertrekTerugKeerDatum.Subtract(VertrekHeenDatum);
        if ((reisDuur.Days - 2) > VIER_WEKEN)
            korting = KORTING_4WEKEN;
        else if ((reisDuur.Days - 2) > TWEE_WEKEN)
            korting = KORTING_2WEKEN;
        else
            korting = GEEN_KORTING;

        return korting;
    }

    public override string ToString()
    {
        return
        (
            "Vervoer: " + this.Vervoer + '\n' +
            "Verblijf: " + this.Verblijf + '\n' +
            "VertrekDatum: " + this.vertrekHeenDatum.ToShortDateString() + '\n' +
            "TerugKeerDatum: " + this.VertrekTerugKeerDatum.ToShortDateString() + '\n' +
            "Totaal prijs zonder korting: " + string.Format("{0:n}",TotaalPrijs()) + '\n' + 
            "Totaal prijs met korting: " + string.Format("{0:n}",TotaalPrijsMetKorting()) + '\n'
        );
    }

}
```
## Oefening 5
### Klasse Persoon
```csharp
enum Land { België, Nederland, Luxemburg}
    class Persoon
    {
        //property's
        public string Naam { get; set; }
        public string Voornaam { get; set; }
        public DateTime GeboorteDatum { get; set; }
        public Land Afkomst { get; set; }

        //constructors

        public Persoon (string naam, string voornaam, DateTime geboorteDatum, Land afkomst)
        {
            Naam = naam;
            Voornaam = voornaam;
            GeboorteDatum = geboorteDatum;
            Afkomst = afkomst;
        }

        //Methodes

        public override string ToString()
        {
            return Naam + "" + Voornaam + " " + GeboorteDatum.ToShortDateString() + " " + Afkomst;

        }
    }
```
### Klasse Bankrekening
```csharp
class Bankrekening
    {
        private const double INTREST_PERCENTAGE = 0.0125;
        private const int BANKKAART_DELER = 97;
        

        //Property's
        public double Saldo { get; set; }

        private long rekeningnummer;

        public long Rekeningnummer
        {
            get { return rekeningnummer; }
            set { rekeningnummer = value; }
        }

        public DateTime Registratie_Datum { get; set; }

        public Persoon Eigenaar { get; set; }
        

        //constructors

        public Bankrekening (double saldo, long rekeningnummer, Persoon eigenaar)
        {
            Saldo = saldo;
            Rekeningnummer = rekeningnummer;
            Registratie_Datum = DateTime.Today;
            Eigenaar = eigenaar;
        }

        //methodes

        public bool Geldig()
        {
            if (((Rekeningnummer / 100) % BANKKAART_DELER) == (Rekeningnummer % 100))
                return true;
            else
                return false;
        }
        
        public void Storten(double bedrag)
        {
            Saldo += bedrag;
        }

        public void Afhalen(double bedrag)
        {
            Saldo -= bedrag;
        }

        public double Bekijk_Saldo()
        {
            return Saldo + (Saldo * INTREST_PERCENTAGE);
        }

        public override string ToString()
        {
            return Saldo + " " + Rekeningnummer + " " + Registratie_Datum.ToShortDateString() + " " + Eigenaar + " " + Geldig().ToString();
        }
    }
```
