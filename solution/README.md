# Lösning

Lösningsförslaget finns i källkodsfilen **Account.cs**. När du hämtar hem denna fil ska den läggas i samma projektmapp som ```Program.cs```. Filen måste därefter logiskt kopplas till projektet ```BankAccounts```. Detta gör du genom att i vyn "Solution Explorer" högerklicka på projektnamnet och i dropdown-listan välja menykommandot **Add ► Existing Item...** samt i browser-dialogen leta upp den aktuella filen.

___Account.cs___

```c#
using System;

namespace BankAccounts
{
    class Account
    {
        // Konstant (constant)
        private const double Rate = 0.035;

        // Fält (fields)
        private string _name;
        private int _accountNumber;
        private double _balance;

        // Konstruktor (constructor)
        public Account(string name, int accountNUmber, double balance)
        {
            _name = name;
            _accountNumber = accountNUmber;
            Balance = balance;
        }

        // Egenskaper (properties)
        public int AccountNumber
        {
            get { return _accountNumber; }
        }
        // Alternativ som "expression bodied method":
        // public int AccountNumber => _accountNumber;

        public double Balance
        {
            get { return _balance; }
            private set
            {
                if (value < 0)
                {
                    throw new ApplicationException(
                        "The balance can not be set to an amount less than 0.");
                }
                _balance = value;
            }
        }

        public string Name
        {
            get { return _name; }
        }
        // Alt:
        // public string Name => _name;

        // Metoder (methods)
        public double Deposit(double amount)
        {
            if (amount < 0)
            {
                throw new ApplicationException(
                    "The amount can not be less than 0.");
            }

            Balance += amount;
            return _balance;
        }

        public double Withdraw(double amount, double fee)
        {
            if (amount + fee < 0 ||
                amount + fee > _balance)
            {
                throw new ApplicationException(
                    "Manage your account wisely so you do not overdraw.");
            }

            Balance -= (amount + fee);
            return _balance;
        }

        public double AddInterest()
        {
            Balance *= (1 + Rate);
            return _balance;
        }

        public void DisplayAccount()
        {
            Console.WriteLine("{0}\t{1}\t{2:c}",
                _accountNumber, _name, _balance);
        }
    }
}
```

Figur 1. Implementationen av klassen Account.

Fälten deklareras lämpligen som privata så dess värden inte kan manipuleras. Publika metoder får istället användas för att kontrollera så att det t.ex. inte går att ta ut mer pengar än vad som finns på kontot.

Den enda konstruktorn initierar objektet med de värden som önskas. Lägg märke till att konstruktorn använder sig av egenskapen ```Balance``` för att initiera det privata fältet ```_balance```. Varför? Därför att det inte ska vara möjligt att skapa ett ```Account```-objekt med ett negativt tillgodohavande. Man vill ju inte börja med att vara skyldig banken pengar. Eller?

Då ett ```Account```-objekt väl har blivit skapat ska kontonumret och namnet inte kunna ändras, därför har egenskaperna ```AccountNumber``` och ```Name``` endast en ```get```-metod. Data blir då vad man ibland kallar _read-only_, dvs. det går bara att läsa.

Egenskapen ```Balance``` har en publik ```get```-metod som returnerar tillgodohavandet. ```set```-metoden är däremot privat och används av klassen ”internt” för att ändra värdet som det privata fältet ```_balance``` har.

Metoderna ```Deposit```, ```Withdraw``` och ```AddInterest``` påverkar alla på något sätt det privata fältet ```_balance```. Diverse kontroller görs för att säkerställa att ```_balance``` inte blir negativt.

Metoden ```DisplayAmount``` presenterar en textbeskrivning av ett ```Account```-objekts status. Lämpligare hade det varit att överskugga metoden ```ToString```, men aktuell lösning fungerar tillfredställande i denna uppgift.
