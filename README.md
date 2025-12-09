import json

# ===============================
# Įkelti duomenis iš failo
# ===============================

def ikelti_duomenis():
    global vartotojai
    try:
        with open("bankas.json", "r") as f:
            vartotojai = json.load(f)
    except:
        vartotojai = []


# ===============================
# Išsaugoti duomenis į failą
# ===============================

def issaugoti_duomenis():
    with open("bankas.json", "w") as f:
        json.dump(vartotojai, f, indent=4)


# ===============================
# Sukurti naują vartotoją
# ===============================

def sukurti_vartotoja():
    vardas = input("Įveskite vartotojo vardą: ")
    
    naujas = {
        "vardas": vardas,
        "balansas": 0,
        "istorija": []
    }
    
    vartotojai.append(naujas)
    print("Vartotojas sukurtas!")


# ===============================
# Rasti vartotoją pagal vardą
# ===============================

def rasti_vartotoja(vardas):
    for v in vartotojai:
        if v["vardas"] == vardas:
            return v
    return None


# ===============================
# Patikrinti balansą
# ===============================

def balansas():
    vardas = input("Įveskite vartotojo vardą: ")
    v = rasti_vartotoja(vardas)

    if v:
        print(f"{vardas} balansas: {v['balansas']} EUR")
    else:
        print("Vartotojas nerastas.")


# ===============================
# Įnešti pinigus
# ===============================

def inesti_pinigus():
    vardas = input("Įveskite vartotojo vardą: ")
    v = rasti_vartotoja(vardas)

    if v:
        suma = float(input("Kiek pinigų norite įnešti? "))
        v["balansas"] += suma
        v["istorija"].append(f"+{suma} EUR įnešta")
        print("Operacija sėkminga!")
    else:
        print("Vartotojas nerastas.")


# ===============================
# Išimti pinigus
# ===============================

def isimti_pinigus():
    vardas = input("Įveskite vartotojo vardą: ")
    v = rasti_vartotoja(vardas)

    if v:
        suma = float(input("Kiek pinigų norite išimti? "))
        if suma <= v["balansas"]:
            v["balansas"] -= suma
            v["istorija"].append(f"-{suma} EUR išimta")
            print("Operacija sėkminga!")
        else:
            print("Nepakanka lėšų.")
    else:
        print("Vartotojas nerastas.")


# ===============================
# Rodyti operacijų istoriją
# ===============================

def rodyti_istorija():
    vardas = input("Įveskite vartotojo vardą: ")
    v = rasti_vartotoja(vardas)

    if v:
        print("\n--- Operacijų istorija ---")
        for eilute in v["istorija"]:
            print(eilute)
    else:
        print("Vartotojas nerastas.")


# ===============================
# Programa
# ===============================

ikelti_duomenis()

while True:
    print("\n=== MINI BANKO SISTEMA ===")
    print("1 - Sukurti vartotoją")
    print("2 - Patikrinti balansą")
    print("3 - Įnešti pinigus")
    print("4 - Išimti pinigus")
    print("5 - Operacijų istorija")
    print("6 - Išeiti")

    pasirinkimas = input("Pasirinkite: ")

    if pasirinkimas == "1":
        sukurti_vartotoja()
    elif pasirinkimas == "2":
        balansas()
    elif pasirinkimas == "3":
        inesti_pinigus()
    elif pasirinkimas == "4":
        isimti_pinigus()
    elif pasirinkimas == "5":
        rodyti_istorija()
    elif pasirinkimas == "6":
        issaugoti_duomenis()
        print("Duomenys išsaugoti. Viso gero!")
        break
    else:
        print("Neteisingas pasirinkimas!")
