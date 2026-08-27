# Operátory v C++ (Cheatsheet)

Tento přehled obsahuje vzorové zápisy deklarací (v hlavičkovém souboru `.h`), definic (v implementačním souboru `.cpp`) a volání v hlavní funkci `main` pro nejčastější operátory.

---

## 1. Výstupní proud (`operator<<`)

Používá se pro snadný výpis celého objektu do konzole pomocí `std::cout`.

**Deklarace (`.h`)**
```cpp
friend std::ostream& operator<<(std::ostream& os, const CLASS& obj);
```

**Definice (`.cpp`)**
```cpp
std::ostream& operator<<(std::ostream& os, const CLASS& obj) {
    os << "Jmeno: " << obj.jmeno << ", Hodnota: " << obj.hodnota;
    return os;
}
```

**Volání v `main`**
```cpp
std::cout << u1 << std::endl;
```

---

## 2. Složené přiřazení (`operator+=`, `operator-=`)

Mění přímo aktuální objekt, ke kterému se hodnota přičítá nebo odčítá.

### Operátor `+=`

**Deklarace (`.h`)**
```cpp
CLASS& operator+=(double hodnota);
```

**Definice (`.cpp`)**
```cpp
CLASS& CLASS::operator+=(double hodnota) {
    this->hodnota += hodnota; // nebo např. vector.push_back(hodnota);
    return *this;
}
```

**Volání v `main`**
```cpp
u1 += 50.0;
```

### Operátor `-=`

**Deklarace (`.h`)**
```cpp
CLASS& operator-=(double hodnota);
```

**Definice (`.cpp`)**
```cpp
CLASS& CLASS::operator-=(double hodnota) {
    this->hodnota -= hodnota;
    return *this;
}
```

**Volání v `main`**
```cpp
u1 -= 50.0;
```

---

## 3. Aritmetické operátory (`+`, `-`, `*`, `/`, `%`)

Vytváří a vracejí **zcela nový objekt** (proto chybí `&` u návratového typu a na konci je `const`).

### Operátor `+` (součet dvou objektů)

**Deklarace (`.h`)**
```cpp
CLASS operator+(const CLASS& druhy) const;
```

**Definice (`.cpp`)**
```cpp
CLASS CLASS::operator+(const CLASS& druhy) const {
    std::string noveJmeno = this->jmeno + " a " + druhy.jmeno;
    double novaHodnota = this->hodnota + druhy.hodnota;
    
    CLASS komplet(noveJmeno, novaHodnota);
    return komplet;
}
```

**Volání v `main`**
```cpp
CLASS u3 = u1 + u2;
```

### Operátor `-` (odečtení dvou objektů)

**Deklarace (`.h`)**
```cpp
CLASS operator-(const CLASS& druhy) const;
```

**Definice (`.cpp`)**
```cpp
CLASS CLASS::operator-(const CLASS& druhy) const {
    std::string noveJmeno = this->jmeno + " bez " + druhy.jmeno;
    double novaHodnota = this->hodnota - druhy.hodnota;
    
    CLASS komplet(noveJmeno, novaHodnota);
    return komplet;
}
```

**Volání v `main`**
```cpp
CLASS u3 = u1 - u2;
```

### Operátor `*` (násobení číslem)

**Deklarace (`.h`)**
```cpp
CLASS operator*(double nasobek) const;
```

**Definice (`.cpp`)**
```cpp
CLASS CLASS::operator*(double nasobek) const {
    CLASS komplet(this->jmeno, this->hodnota * nasobek);
    return komplet;
}
```

**Volání v `main`**
```cpp
CLASS u3 = u1 * 2.5;
```

### Operátor `/` (dělení číslem)

**Deklarace (`.h`)**
```cpp
CLASS operator/(double delitel) const;
```

**Definice (`.cpp`)**
```cpp
CLASS CLASS::operator/(double delitel) const {
    CLASS komplet(this->jmeno, this->hodnota / delitel);
    return komplet;
}
```

**Volání v `main`**
```cpp
CLASS u3 = u1 / 2.0;
```

### Operátor `%` (zbytek po dělení)

**Deklarace (`.h`)**
```cpp
int operator%(int zaklad) const;
```

**Definice (`.cpp`)**
```cpp
int CLASS::operator%(int zaklad) const {
    return static_cast<int>(this->hodnota) % zaklad;
}
```

**Volání v `main`**
```cpp
int zbytek = u1 % 5;
```

---

## 4. Inkrementace a Dekrementace (`++`, `--`)

Dělí se na **Prefix** (mění hned) a **Postfix** (vrací starou hodnotu, pak mění). Postfix se pozná podle parametru `(int)`.

### Operátor `++`

**Deklarace (`.h`)**
```cpp
CLASS& operator++();       // Prefix: ++u1
CLASS operator++(int);     // Postfix: u1++
```

**Definice (`.cpp`)**
```cpp
// Prefix
CLASS& CLASS::operator++() {
    this->hodnota++;
    return *this;
}

// Postfix
CLASS CLASS::operator++(int) {
    CLASS staryStav = *this;
    this->hodnota++;
    return staryStav;
}
```

**Volání v `main`**
```cpp
++u1;  // Zvýší a hned použije
u1++;  // Použije aktuální a pak zvýší
```

### Operátor `--`

**Deklarace (`.h`)**
```cpp
CLASS& operator--();       // Prefix: --u1
CLASS operator--(int);     // Postfix: u1--
```

**Definice (`.cpp`)**
```cpp
// Prefix
CLASS& CLASS::operator--() {
    this->hodnota--;
    return *this;
}

// Postfix
CLASS CLASS::operator--(int) {
    CLASS staryStav = *this;
    this->hodnota--;
    return staryStav;
}
```

**Volání v `main`**
```cpp
--u1;
u1--;
```

---

## 5. Porovnávací operátory (`<`, `>`, `==`, `!=`, `<=`, `>=`)

Vrací vždy `bool` (pravda / nepravda) a nemění původní objekty (proto `const`).

**Deklarace (`.h`)**
```cpp
bool operator<(const CLASS& u) const;
bool operator>(const CLASS& u) const;
bool operator==(const CLASS& u) const;
bool operator!=(const CLASS& u) const;
bool operator<=(const CLASS& u) const;
bool operator>=(const CLASS& u) const;
```

**Definice (`.cpp`)**
```cpp
bool CLASS::operator<(const CLASS& u) const { return this->hodnota < u.hodnota; }
bool CLASS::operator>(const CLASS& u) const { return this->hodnota > u.hodnota; }
bool CLASS::operator==(const CLASS& u) const { return this->hodnota == u.hodnota; }
bool CLASS::operator!=(const CLASS& u) const { return !(*this == u); }
bool CLASS::operator<=(const CLASS& u) const { return this->hodnota <= u.hodnota; }
bool CLASS::operator>=(const CLASS& u) const { return this->hodnota >= u.hodnota; }
```

**Volání v `main`**
```cpp
if (u1 < u2)  { std::cout << "u1 je mensi\n"; }
if (u1 > u2)  { std::cout << "u1 je vetsi\n"; }
if (u1 == u2) { std::cout << "jsou stejne\n"; }
if (u1 != u2) { std::cout << "jsou ruzne\n"; }
if (u1 <= u2) { std::cout << "u1 je mensi nebo rovno\n"; }
if (u1 >= u2) { std::cout << "u1 je vetsi nebo rovno\n"; }
```

---

## 6. Přístupové operátory (`[]`, `()`)

### Indexovací operátor `[]` (čtení z vektoru)

**Deklarace (`.h`)**
```cpp
int operator[](size_t index) const;
```

**Definice (`.cpp`)**
```cpp
int CLASS::operator[](size_t index) const {
    return vektor.at(index);
}
```

**Volání v `main`**
```cpp
u1.pridejhodnotu(20)
int prvniPrvek = u1[0];
std::cout << "Prvni prvek: " << u1[0] << std::endl;
```

### Volací operátor `()` (funktor)

**Deklarace (`.h`)**
```cpp
void operator()(int hodnota);
```

**Definice (`.cpp`)**
```cpp
void CLASS::operator()(int hodnota) {
    this->hodnota += hodnota;
}
```

**Volání v `main`**
```cpp
u1(100); // Zavolá objekt jako funkci a předá hodnotu 100
```
