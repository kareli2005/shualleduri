# klasebi
 
 
    class Magazia:
        def __init__(self, name, address, work_hours, income=0):
            self.name = name
            self.address = address
            self.work_hours = work_hours
            self.income = income


    class Book(Magazia):
        def __init__(self, name, address, work_hours, income, book_name, author, grade=0, quantity=20, price=8):
            super().__init__(name, address, work_hours, income)
            self.boock_name = book_name
            self.author = author
            self.grade = grade
            self.quantity = quantity
            self.price = price


    class CD(Magazia):
        def __init__(self, magazia, cd_type, cd_name, author, rating=0, quantity=30, price=5):
            super().__init__(magazia.name, magazia.address, magazia.work_hours, magazia.income)
            if cd_type == "audio_wigni":
                self.cd_type = cd_type
            elif cd_type == "musikaluri_albomi":
                self.cd_type = cd_type
            else:
                print("CD type is incorrect")
            self.cd_name = cd_name
            self.author = author
            self.rating = rating
            self.quantity = quantity
            self.price = price

        def cd_rating(self, rate_point: int):
            if rate_point < 1 or rate_point > 5:
                self.rating = self.rating
                print("something went wrong")
            else:
                print(f"you rated {self.cd_name} by {rate_point}")
                self.rating = round((rate_point + self.rating) / 2, 2)
                return f"Total rating is {self.rating}"

        def cd_buy(self, cd_cuantity: int, customer_object):
            if cd_cuantity <= self.quantity and customer_object.budget >= cd_cuantity * self.price:
                fasi = cd_cuantity * self.price
                self.quantity -= cd_cuantity
                self.income += fasi
                libre.income += fasi
                customer_object.budget -= cd_cuantity * self.price
                return str(f"you bought {cd_cuantity} {self.cd_name} for {self.price * cd_cuantity}")
            else:
                return str("something went wrong")


    class Customer:
        def __init__(self, customer_first: str, customer_last: str, customer_id: int, budget: float):
            self.customer_first = customer_first
            self.customer_last = customer_last
            self.customer_id = customer_id
            self.budget = budget


    libre = Magazia("libre", "Gldani", "08-22")
    spar = Magazia("Spar", "Delisi", "07-23")
    cd1 = CD(libre, "audio_wigni", "tafi_otaras_simgerebi", "tafi_otara")
    customer1 = Customer("Tsotne", "Kareli", 59901130177, 300)
    customer2 = Customer("Giorgi", "Merebashvili", 59901130178, 400)
    print(customer1.customer_first, customer1.customer_last)
    print(customer2.customer_first, customer2.customer_last)

    cd1.cd_buy(15, customer1)
    cd1.cd_rating(4)
    print(customer1.budget)
    print(cd1.rating)
    print(libre.income)



# bazebi
    import sqlite3
    import matplotlib.pyplot as plt

    conn = sqlite3.connect("Titanic.sqlite")
    cursor = conn.cursor()

    # 1) ramdeni gadarcha
    cursor.execute("SELECT COUNT(*) FROM Passengers WHERE Survived = 1")
    print(cursor.fetchone()[0], end="\n")

    # 1) ramdeni ver gadarcha
    cursor.execute("SELECT COUNT(*) FROM Passengers WHERE Survived = 0")
    print(cursor.fetchone()[0], end="\n")

    # 2) 10 wlian rangeshi, romel asakobriv diapazonshi ramdeni mgzavri iyo
    i = 0
    a = 10
    while i < 100:
        cursor.execute(f"SELECT COUNT(*) FROM Passengers WHERE Age BETWEEN {i} and {a}")
        print(cursor.fetchone()[0])
        i += 10
        a += 10
    print(" ")

    # 3) 10 wlian rangeshi, romel asakobriv diapazonshi ramdeni mgzavri gadarcha
    i = 0
    a = 10
    while i < 100:
        cursor.execute(f"SELECT COUNT(*) FROM Passengers WHERE Age BETWEEN {i} and {a} AND Survived=1")
        print(cursor.fetchone()[0])
        i += 10
        a += 10
    print(" ")

    # 3) 10 wlian rangeshi, romel asakobriv diapazonshi ramdeni mgzavri ver gadarcha
    i = 0
    a = 10
    while i < 100:
        cursor.execute(f"SELECT COUNT(*) FROM Passengers WHERE Age BETWEEN {i} and {a} AND Survived=0 ")
        print(cursor.fetchone()[0])
        i += 10
        a += 10
    print(" ")

    # 4) biletis klasis mixedvit, biletebis sashualo
    # pirveli klasi
    cursor.execute("SELECT SUM(Fare) / COUNT(*) FROM Passengers WHERE Pclass=1")
    print(f"biletebis sahualo pirvel klasshi iyo {cursor.fetchone()[0]}")

    # meore klasi
    cursor.execute("SELECT SUM(Fare) / COUNT(*) FROM Passengers WHERE Pclass=2")
    print(f"biletebis sahualo meore klasshi iyo {cursor.fetchone()[0]}")

    # mesame klasi
    cursor.execute("SELECT SUM(Fare) / COUNT(*) FROM Passengers WHERE Pclass=3")
    print(f"biletebis sahualo mesame klasshi iyo {cursor.fetchone()[0]}")

    # 5) sad ramdeni mgzavri avida gemze
    # Cherbourg
    cursor.execute("SELECT COUNT(*) FROM Passengers WHERE Embarked='C'")
    print(f"{cursor.fetchone()[0]} mgzavri avida Cherbourgshi")

    # Queenstown
    cursor.execute("SELECT COUNT(*) FROM Passengers WHERE Embarked='Q'")
    print(f"{cursor.fetchone()[0]} mgzavri avida Queenstownshi")

    # Southampton
    cursor.execute("SELECT COUNT(*) FROM Passengers WHERE Embarked='S'")
    print(f"{cursor.fetchone()[0]} mgzavri avida Southamptonshi")


    def average(fare, pclass, c):
        c.execute(f"SELECT SUM({fare}) / COUNT(*) FROM Passengers WHERE Pclass={pclass}")
        return c.fetchone()[0]


    fig, ax = plt.subplots()

    classes = ['1st class', '2nd class', '3rd class']
    counts = [average("Fare", 1, cursor), average("Fare", 2, cursor), average("Fare", 3, cursor)]
    bar_labels = ['red', 'blue', 'green']
    bar_colors = ['tab:red', 'tab:blue', 'tab:red']

    ax.bar(classes, counts, label=bar_labels, color=bar_colors)

    ax.set_ylabel('Price')
    ax.set_title('Average price of tickets by classes')
    ax.legend(title='Fruit color')

    plt.show()

    conn.close()
