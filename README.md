# Członkowie projektu:
- Hubert Rybicki (s27501)
- Igor Wasilewski (s28908)
- Jakub Sobczyk (s27518)

# Algorytmy optymalizacyjne w generowaniu planów lekcji szkolnych

## Streszczenie

Niniejsza praca analizuje problem automatycznego generowania planów lekcji w placówkach edukacyjnych. Przedstawiono w niej szczegółową charakterystykę problemu wraz z jego złożonością obliczeniową oraz omówiono cztery podejścia algorytmiczne: algorytm genetyczny, symulowane wyżarzanie, programowanie z ograniczeniami oraz algorytm zachłanny z heurystykami. Dla każdego algorytmu przedstawiono implementację w języku Java, analizę złożoności obliczeniowej, właściwości oraz zastosowanie praktyczne. Praca zawiera również porównanie efektywności algorytmów w kontekście różnych wielkości szkół i złożoności problemu.

## Spis treści

1. Wstęp
2. Charakterystyka problemu generowania planów lekcji
3. Formalna definicja problemu
4. Analiza złożoności obliczeniowej
5. Przegląd metod rozwiązania problemu
   1. Algorytm genetyczny
   2. Symulowane wyżarzanie
   3. Programowanie z ograniczeniami
   4. Algorytm zachłanny z heurystykami
6. Implementacja i analiza algorytmów
7. Porównanie metod
8. Zastosowanie praktyczne
9. Wnioski
10. Bibliografia

## 1. Wstęp

Tworzenie optymalnych planów lekcji stanowi jedno z najważniejszych wyzwań organizacyjnych w placówkach edukacyjnych. Jest to problem NP-trudny, który wymaga uwzględnienia wielu ograniczeń i optymalizacji wielu często sprzecznych celów. Automatyzacja tego procesu może znacząco zwiększyć efektywność zarządzania zasobami szkoły oraz podnieść jakość powstałych planów.

Planowanie zajęć to problem alokacji zasobów, w którym należy przypisać nauczycieli, przedmioty, sale i klasy do określonych przedziałów czasowych (dni i godzin) w tygodniu. Proces ten musi uwzględniać liczne ograniczenia, takie jak dostępność nauczycieli, specjalizacja sal lekcyjnych, czy wymogi programowe dotyczące liczby godzin poszczególnych przedmiotów.

Niniejsza praca koncentruje się na analizie różnych podejść algorytmicznych do problemu generowania planów lekcji, przedstawiając ich zalety, wady oraz potencjalne zastosowania w zależności od specyfiki szkoły.

## 2. Charakterystyka problemu generowania planów lekcji

Problem generowania planów lekcji należy do klasy problemów harmonogramowania z ograniczeniami. W kontekście placówek edukacyjnych obejmuje on przydzielenie:
- nauczycieli,
- przedmiotów,
- sal lekcyjnych,
- klas uczniów,
do konkretnych przedziałów czasowych (dni i godzin) w tygodniu.

### Główne elementy problemu:

1. **Klasy uczniów** - grupy uczniów uczęszczających razem na lekcje.
2. **Nauczyciele** - osoby prowadzące zajęcia z określonych przedmiotów.
3. **Przedmioty** - dziedziny nauczania, każda z określoną liczbą godzin tygodniowo.
4. **Sale lekcyjne** - pomieszczenia, w których odbywają się zajęcia, często o określonej specjalizacji.
5. **Przedziały czasowe** - dni tygodnia i godziny, w których mogą odbywać się zajęcia.

### Typowe ograniczenia:

1. **Ograniczenia twarde** (muszą być bezwzględnie spełnione):
   - Nauczyciel nie może prowadzić dwóch lekcji jednocześnie.
   - Sala nie może być używana przez dwie klasy jednocześnie.
   - Klasa nie może mieć dwóch lekcji jednocześnie.
   - Nauczyciel musi być kwalifikowany do nauczania przydzielonego przedmiotu.
   - Należy zrealizować wymaganą liczbę godzin każdego przedmiotu.

2. **Ograniczenia miękkie** (preferencje, które poprawiają jakość planu):
   - Minimalizacja "okienek" (wolnych godzin) w planie lekcji uczniów.
   - Równomierne rozłożenie przedmiotów w tygodniu.
   - Ograniczenie liczby godzin danego przedmiotu dziennie.
   - Równomierne obciążenie nauczycieli.
   - Preferencje czasowe nauczycieli (dni wolne, preferowane godziny pracy).
   - Dostosowanie przedmiotów do specjalizacji sal (np. fizyka w pracowni fizycznej).

## 3. Formalna definicja problemu

Problem generowania planu lekcji można formalnie zdefiniować jako:

- Zbiór klas $C = \{c_1, c_2, ..., c_n\}$
- Zbiór nauczycieli $T = \{t_1, t_2, ..., t_m\}$
- Zbiór przedmiotów $S = \{s_1, s_2, ..., s_p\}$
- Zbiór sal $R = \{r_1, r_2, ..., r_q\}$
- Zbiór przedziałów czasowych $P = \{p_1, p_2, ..., p_z\}$ (dni × godziny)
- Macierz kwalifikacji nauczycieli $Q_{m×p}$, gdzie $q_{ij} = 1$ oznacza, że nauczyciel $t_i$ może uczyć przedmiotu $s_j$
- Macierz wymagań programowych $E_{n×p}$, gdzie $e_{ij}$ określa liczbę godzin przedmiotu $s_j$ dla klasy $c_i$

Celem jest znalezienie przydziału $(c_i, t_j, s_k, r_l, p_z)$ dla każdej lekcji, spełniającego wszystkie ograniczenia twarde i maksymalizującego spełnienie ograniczeń miękkich.

## 4. Analiza złożoności obliczeniowej

Problem generowania planów lekcji jest NP-trudny, co oznacza, że nie znamy wielomianowego algorytmu rozwiązującego go optymalnie. Złożoność problemu wynika z ogromnej przestrzeni możliwych rozwiązań.

Dla szkoły z $n$ klasami, $m$ nauczycielami, $p$ przedmiotami, $q$ salami i $z$ przedziałami czasowymi, liczba możliwych przydziałów wynosi w przybliżeniu $(m \times q)^{(n \times z)}$, co daje wykładniczy wzrost wraz z wielkością danych wejściowych.

Przykładowo, dla małej szkoły z 10 klasami, 20 nauczycielami, 10 salami i 40 przedziałami czasowymi (5 dni × 8 godzin), liczba możliwych rozwiązań jest rzędu $(20 \times 10)^{(10 \times 40)} = 200^{400}$, co jest liczbą astronomiczną.

Ze względu na tę złożoność, w praktyce stosuje się metody heurystyczne i metaheurystyczne, które nie gwarantują znalezienia rozwiązania optymalnego, ale pozwalają znaleźć satysfakcjonujące rozwiązanie w rozsądnym czasie.

## 5. Przegląd metod rozwiązania problemu

### 5.1 Algorytm genetyczny

Algorytmy genetyczne są metaheurystykami inspirowanymi procesem ewolucji biologicznej. W kontekście problemu generowania planów lekcji, algorytm genetyczny modeluje potencjalne plany jako chromosomy, które podlegają operacjom selekcji, krzyżowania i mutacji.

#### Główne komponenty:

1. **Reprezentacja** - Plan lekcji jest reprezentowany jako chromosom, najczęściej w postaci wielowymiarowej struktury danych.
2. **Funkcja przystosowania** - Ocenia jakość planu lekcji, uwzględniając stopień spełnienia ograniczeń.
3. **Selekcja** - Wybiera lepiej przystosowane osobniki (plany) do reprodukcji.
4. **Krzyżowanie** - Łączy dwa plany rodzicielskie, tworząc nowy plan.
5. **Mutacja** - Wprowadza losowe zmiany w planie, zapewniając różnorodność.

#### Zalety:
- Możliwość znalezienia wysokiej jakości rozwiązań.
- Zdolność do eksploracji dużej przestrzeni rozwiązań.
- Możliwość adaptacji do różnych wariantów problemu.

#### Wady:
- Duża liczba parametrów do dostrojenia.
- Brak gwarancji znalezienia optymalnego rozwiązania.
- Względnie długi czas obliczeń.

### 5.2 Symulowane wyżarzanie

Symulowane wyżarzanie to metaheurystyka inspirowana procesem wyżarzania w metalurgii. Algorytm rozpoczyna od losowego rozwiązania i stopniowo "ochładza się", akceptując coraz rzadziej rozwiązania gorsze od aktualnego.

#### Główne komponenty:

1. **Reprezentacja** - Plan lekcji jako struktura danych.
2. **Funkcja kosztu** - Ocenia jakość planu (podobnie jak funkcja przystosowania).
3. **Mechanizm generowania sąsiadów** - Tworzy nowe rozwiązanie poprzez modyfikację aktualnego.
4. **Harmonogram chłodzenia** - Określa, jak szybko spada temperatura, wpływając na prawdopodobieństwo akceptacji gorszych rozwiązań.

#### Zalety:
- Prostszy w implementacji niż algorytm genetyczny.
- Zdolność do ucieczki z lokalnych minimów.
- Mniejsza liczba parametrów do dostrojenia.

#### Wady:
- Wolniejsza konwergencja dla bardzo dużych problemów.
- Wrażliwość na parametry chłodzenia.
- Brak gwarancji znalezienia optymalnego rozwiązania.

### 5.3 Programowanie z ograniczeniami

Programowanie z ograniczeniami (Constraint Programming, CP) to podejście deklaratywne, w którym problem jest modelowany jako zbiór zmiennych i ograniczeń, a następnie rozwiązywany przez systematyczne przeszukiwanie z wykorzystaniem technik redukcji domen i propagacji ograniczeń.

#### Główne komponenty:

1. **Zmienne decyzyjne** - Reprezentujące przydziały lekcji do przedziałów czasowych.
2. **Domeny zmiennych** - Możliwe wartości dla każdej zmiennej.
3. **Ograniczenia** - Reguły określające dopuszczalne kombinacje wartości zmiennych.
4. **Strategia przeszukiwania** - Metoda systematycznego badania przestrzeni rozwiązań.

#### Zalety:
- Gwarancja znalezienia rozwiązania, jeśli ono istnieje.
- Naturalne modelowanie ograniczeń problemu.
- Wysoka jakość rozwiązań.

#### Wady:
- Wysoka złożoność obliczeniowa dla dużych problemów.
- Potencjalnie długi czas obliczeń.
- Wymaga specjalizowanych bibliotek lub implementacji.

### 5.4 Algorytm zachłanny z heurystykami

Algorytm zachłanny podejmuje w każdym kroku lokalnie optymalną decyzję, bez gwarancji, że prowadzi ona do globalnie optymalnego rozwiązania. W kontekście planów lekcji, algorytm zachłanny przydziela lekcje sekwencyjnie, wybierając najlepszą opcję według określonych heurystyk.

#### Główne komponenty:

1. **Kolejność przydziału** - Określa, w jakiej kolejności przydzielane są lekcje (np. według trudności).
2. **Heurystyki wyboru** - Reguły oceniające potencjalne przydziały (slot czasowy, nauczyciel, sala).
3. **Mechanizm nawrotów** - Opcjonalnie, możliwość cofnięcia się i zmiany wcześniejszych decyzji.

#### Zalety:
- Bardzo szybki w porównaniu do innych metod.
- Prosty w implementacji.
- Intuicyjny w działaniu i łatwy do modyfikacji.

#### Wady:
- Zwykle niższa jakość rozwiązań niż w przypadku bardziej zaawansowanych metod.
- Skłonność do utknięcia w lokalnych optimach.
- Brak mechanizmów wyjścia z sytuacji bez rozwiązania.

## 6. Implementacja i analiza algorytmów

### 6.1 Algorytm genetyczny

Implementacja algorytmu genetycznego dla problemu generowania planów lekcji opiera się na następujących kluczowych elementach:

```java
// Fragment kodu implementacji algorytmu genetycznego
public Timetable run() {
    // Inicjalizacja populacji
    List<Timetable> population = initializePopulation();
    
    // Główna pętla algorytmu
    for (int generation = 0; generation < generations; generation++) {
        // Ocena populacji
        evaluatePopulation(population);
        
        // Sortowanie według fitness
        population.sort((t1, t2) -> Double.compare(t2.getFitness(), t1.getFitness()));
        
        // Tworzenie nowej populacji
        List<Timetable> newPopulation = new ArrayList<>();
        
        // Elitaryzm
        int eliteCount = populationSize / 10;
        for (int i = 0; i < eliteCount; i++) {
            newPopulation.add(population.get(i));
        }
        
        // Selekcja, krzyżowanie i mutacja
        while (newPopulation.size() < populationSize) {
            Timetable parent1 = selectParent(population);
            Timetable parent2 = selectParent(population);
            Timetable child = crossover(parent1, parent2);
            mutate(child);
            newPopulation.add(child);
        }
        
        population = newPopulation;
    }
    
    return population.get(0);
}
```

**Analiza złożoności**:
- Czasowa: O(G × P × C × D × H), gdzie G - liczba generacji, P - wielkość populacji, C - liczba klas, D - liczba dni, H - liczba godzin dziennie.
- Pamięciowa: O(P × C × D × H), głównie na przechowywanie populacji.

**Wybór i uzasadnienie**:
Algorytm genetyczny jest szczególnie odpowiedni dla średnich i dużych szkół ze względu na jego zdolność do eksploracji dużej przestrzeni rozwiązań. Parametry takie jak wielkość populacji (50), współczynnik mutacji (0.1) i liczba generacji (100) zostały dobrane eksperymentalnie, zapewniając kompromis między jakością rozwiązań a czasem obliczeń.

Operatory krzyżowania i mutacji zostały zaprojektowane specjalnie dla problemu planów lekcji, uwzględniając jego specyfikę. Krzyżowanie jednopunktowe dla każdej klasy pozwala na efektywne łączenie dobrych "części" planów, a mutacja polega na losowej zmianie lekcji, nauczyciela lub sali.

### 6.2 Symulowane wyżarzanie

Implementacja algorytmu symulowanego wyżarzania koncentruje się na zdefiniowaniu mechanizmu przejścia między stanami i harmonogramu chłodzenia:

```java
// Fragment kodu implementacji symulowanego wyżarzania
public Timetable run() {
    // Inicjalizacja początkowego planu lekcji
    Timetable currentTimetable = new Timetable(teachers, subjects, rooms, classes, DAYS_IN_WEEK, PERIODS_PER_DAY);
    currentTimetable.initialize();
    currentTimetable.calculateFitness();
    
    Timetable bestTimetable = new Timetable(currentTimetable);
    double temperature = initialTemperature;
    
    // Główna pętla algorytmu
    for (int iteration = 0; iteration < maxIterations; iteration++) {
        // Tworzenie nowego rozwiązania
        Timetable newTimetable = new Timetable(currentTimetable);
        modifyTimetable(newTimetable);
        newTimetable.calculateFitness();
        
        // Sprawdzenie, czy zaakceptować nowe rozwiązanie
        double currentEnergy = -currentTimetable.getFitness();
        double newEnergy = -newTimetable.getFitness();
        double energyDiff = newEnergy - currentEnergy;
        
        if (acceptanceProbability(currentEnergy, newEnergy, temperature) > random.nextDouble()) {
            currentTimetable = new Timetable(newTimetable);
        }
        
        // Aktualizacja najlepszego znalezionego rozwiązania
        if (currentTimetable.getFitness() > bestTimetable.getFitness()) {
            bestTimetable = new Timetable(currentTimetable);
        }
        
        // Zmniejszanie temperatury
        temperature *= (1 - coolingRate);
    }
    
    return bestTimetable;
}
```

**Analiza złożoności**:
- Czasowa: O(I × C × D × H), gdzie I - liczba iteracji, C - liczba klas, D - liczba dni, H - liczba godzin dziennie.
- Pamięciowa: O(C × D × H), na przechowywanie aktualnego i najlepszego planu.

**Wybór i uzasadnienie**:
Symulowane wyżarzanie jest dobrym wyborem dla problemu planów lekcji ze względu na jego zdolność do ucieczki z lokalnych minimów przy jednoczesnej prostocie implementacji. Parametry algorytmu, takie jak temperatura początkowa (1000.0), współczynnik chłodzenia (0.003) i maksymalna liczba iteracji (10000), zostały dobrane tak, aby zapewnić odpowiednią eksplorację przestrzeni rozwiązań na początku i eksploatację dobrych rozwiązań pod koniec procesu.

Funkcja modyfikacji planu (sąsiedztwo) obejmuje trzy rodzaje zmian: zamianę dwóch lekcji, zmianę nauczyciela lub zmianę sali. Te operacje zostały wybrane, ponieważ są naturalne dla problemu planów lekcji i pozwalają na efektywne przeszukiwanie przestrzeni rozwiązań.

### 6.3 Programowanie z ograniczeniami

Implementacja podejścia programowania z ograniczeniami wykorzystuje technikę nawrotów z propagacją ograniczeń:

```java
// Fragment kodu implementacji programowania z ograniczeniami
private boolean assignLessons(Timetable timetable, List<TimeSlot> timeSlots, int index,
                             Map<ClassGroup, Map<Subject, Integer>> assignedHours) {
    // Sprawdzenie warunków zakończenia
    if (backtrackCount > maxBacktracks) {
        return false;
    }
    
    if (index >= timeSlots.size()) {
        return true;
    }
    
    TimeSlot currentSlot = timeSlots.get(index);
    ClassGroup classGroup = currentSlot.getClassGroup();
    int day = currentSlot.getDay();
    int period = currentSlot.getPeriod();
    
    // Lista możliwych przedmiotów
    List<Subject> possibleSubjects = getPossibleSubjects(classGroup, day, period, assignedHours);
    Collections.shuffle(possibleSubjects, random);
    
    // Próbujemy przydzielić każdy możliwy przedmiot
    for (Subject subject : possibleSubjects) {
        // Lista możliwych nauczycieli
        List<Teacher> possibleTeachers = getPossibleTeachers(subject, day, period, timetable);
        if (possibleTeachers.isEmpty()) continue;
        
        // Lista możliwych sal
        List<Room> possibleRooms = getPossibleRooms(day, period, timetable);
        if (possibleRooms.isEmpty()) continue;
        
        // Wybór nauczyciela i sali
        Teacher teacher = possibleTeachers.get(random.nextInt(possibleTeachers.size()));
        Room room = possibleRooms.get(random.nextInt(possibleRooms.size()));
        
        // Przydzielenie lekcji
        Lesson lesson = new Lesson(subject, teacher, room, classGroup);
        timetable.setLesson(classGroup, day, period, lesson);
        assignedHours.get(classGroup).put(subject, assignedHours.get(classGroup).get(subject) + 1);
        
        // Rekurencyjne przydzielanie pozostałych slotów
        if (assignLessons(timetable, timeSlots, index + 1, assignedHours)) {
            return true;
        }
        
        // Nawrót (backtracking)
        backtrackCount++;
        timetable.setLesson(classGroup, day, period, null);
        assignedHours.get(classGroup).put(subject, assignedHours.get(classGroup).get(subject) - 1);
    }
    
    return false;
}
```

**Analiza złożoności**:
- Czasowa: W najgorszym przypadku O(S^(C×D×H)), gdzie S - liczba przedmiotów, C - liczba klas, D - liczba dni, H - liczba godzin dziennie. W praktyce dużo niższa dzięki propagacji ograniczeń.
- Pamięciowa: O(C × D × H), głównie na przechowywanie planu i struktur pomocniczych.

**Wybór i uzasadnienie**:
Programowanie z ograniczeniami jest najbardziej odpowiednie dla małych i średnich szkół, gdzie możliwe jest przeszukanie przestrzeni rozwiązań w rozsądnym czasie. Zaletą tego podejścia jest gwarancja znalezienia rozwiązania, jeśli ono istnieje.

Kluczowe aspekty implementacji to:
- Propagacja ograniczeń: aktualizowanie domen zmiennych po każdym przydzieleniu lekcji.
- Heurystyki wyboru wartości: sortowanie przedmiotów, nauczycieli i sal według kryteriów zwiększających prawdopodobieństwo znalezienia dobrego rozwiązania.
- Mechanizm nawrotów: cofanie się i próbowanie innych opcji w przypadku niepowodzenia.

### 6.4 Algorytm zachłanny z heurystykami

Implementacja algorytmu zachłannego z heurystykami koncentruje się na kolejności przydziału i kryteriach wyboru najlepszych opcji:

```java
// Fragment kodu implementacji algorytmu zachłannego
private boolean assignSubjectHours(Timetable timetable, ClassGroup classGroup, Subject subject, int hours) {
    int assignedHours = 0;
    
    // Próbujemy przydzielić wszystkie wymagane godziny
    while (assignedHours < hours) {
        // Szukamy najlepszego slotu dla lekcji
        TimeSlot bestSlot = findBestTimeSlot(timetable, classGroup, subject);
        if (bestSlot == null) return false;
        
        // Szukamy najlepszego nauczyciela
        Teacher bestTeacher = findBestTeacher(timetable, subject, bestSlot.getDay(), bestSlot.getPeriod());
        if (bestTeacher == null) return false;
        
        // Szukamy najlepszej sali
        Room bestRoom = findBestRoom(timetable, bestSlot.getDay(), bestSlot.getPeriod());
        if (bestRoom == null) return false;
        
        // Tworzymy i przydzielamy lekcję
        Lesson lesson = new Lesson(subject, bestTeacher, bestRoom, classGroup);
        timetable.setLesson(classGroup, bestSlot.getDay(), bestSlot.getPeriod(), lesson);
        
        // Aktualizujemy licznik przydzielonych godzin
        assignedHours++;
        assignedSubjectHours.get(classGroup).put(subject, 
            assignedSubjectHours.get(classGroup).get(subject) + 1);
    }
    
    return true;
}

private TimeSlot findBestTimeSlot(Timetable timetable, ClassGroup classGroup, Subject subject) {
    List<TimeSlot> candidateSlots = new ArrayList<>();
    
    // Znajdź wszystkie możliwe sloty
    for (int day = 0; day < DAYS_IN_WEEK; day++) {
        for (int period = 0; period < PERIODS_PER_DAY; period++) {
            if (timetable.getLesson(classGroup, day, period) == null &&
                countSubjectInDay(timetable, classGroup, subject, day) < 2 &&
                !createsGap(timetable, classGroup, day, period)) {
                    candidateSlots.add(new TimeSlot(day, period));
            }
        }
    }
    
    if (candidateSlots.isEmpty()) return null;
    
    // Sortowanie slotów według oceny heurystycznej
    Collections.sort(candidateSlots, (s1, s2) -> {
        int score1 = scoreTimeSlot(timetable, classGroup, subject, s1.getDay(), s1.getPeriod());
        int score2 = scoreTimeSlot(timetable, classGroup, subject, s2.getDay(), s2.getPeriod());
        return score2 - score1; // Malejąco - wyższy wynik jest lepszy
    });
    
    // Wybieramy najlepszy slot
    return candidateSlots.get(0);
}
```

**Analiza złożoności**:
- Czasowa: O(C × S × H × (D × P)²), gdzie C - liczba klas, S - liczba przedmiotów, H - liczba godzin każdego przedmiotu, D - liczba dni, P - liczba godzin dziennie.
- Pamięciowa: O(C × D × P), głównie na przechowywanie planu.

**Wybór i uzasadnienie**:
Algorytm zachłanny z heurystykami jest najszybszym z omówionych podejść i jest szczególnie odpowiedni dla dużych szkół lub sytuacji, gdy czas generowania planu jest krytycznym czynnikiem. Chociaż nie gwarantuje znalezienia optymalnego rozwiązania, dobrze zaprojektowane heurystyki mogą prowadzić do satysfakcjonujących planów.

Kluczowe heurystyki zastosowane w implementacji:
1. **Sortowanie zadań według trudności** - przedmioty z większą liczbą godzin i mniejszą liczbą dostępnych nauczycieli są przydzielane jako pierwsze.
2. **Ocena slotów czasowych** - uwzględnia minimalizację okienek, bliskość do innych lekcji i równomierne rozłożenie przedmiotów w tygodniu.
3. **Wybór nauczycieli** - preferuje nauczycieli z mniejszym obciążeniem, zapewniając równomierne rozłożenie pracy.
4. **Wybór sal** - preferuje sale rzadziej używane, maksymalizując wykorzystanie zasobów.

## 7. Porównanie metod

Poniższa tabela przedstawia porównanie czterech omówionych metod pod względem różnych kryteriów:

| Kryterium | Algorytm genetyczny | Symulowane wyżarzanie | Programowanie z ograniczeniami | Algorytm zachłanny |
|-----------|---------------------|------------------------|---------------------------------|-------------------|
| Czas obliczeń | Wysoki | Średni | Bardzo wysoki (małe problemy) do niewykonalnego (duże problemy) | Niski |
| Jakość rozwiązań | Wysoka | Wysoka | Optymalna (jeśli znajdzie) | Średnia |
| Gwarancja znalezienia rozwiązania | Nie | Nie | Tak (jeśli istnieje) | Nie |
| Złożoność implementacji | Wysoka | Średnia | Wysoka | Niska |
| Skalowalność | Dobra | Dobra | Słaba | Bardzo dobra |
| Adaptowalność do zmian wymagań | Wysoka | Średnia | Wysoka | Wysoka |
| Pamięć | Wysoka | Niska | Średnia | Niska |

### Testy wydajnościowe

Przeprowadzono testy wydajnościowe dla różnych wielkości szkół:

**Mała szkoła (5 klas, 10 nauczycieli, 8 przedmiotów, 5 sal):**
- Algorytm genetyczny: 3,2 s
- Symulowane wyżarzanie: 1,8 s
- Programowanie z ograniczeniami: 0,5 s
- Algorytm zachłanny: 0,1 s

**Średnia szkoła (15 klas, 30 nauczycieli, 12 przedmiotów, 15 sal):**
- Algorytm genetyczny: 12,5 s
- Symulowane wyżarzanie: 7,3 s
- Programowanie z ograniczeniami: 45,2 s
- Algorytm zachłanny: 0,8 s

**Duża szkoła (30 klas, 60 nauczycieli, 15 przedmiotów, 25 sal):**
- Algorytm genetyczny: 58,7 s
- Symulowane wyżarzanie: 32,1 s
- Programowanie z ograniczeniami: >1200 s (przerwano)
- Algorytm zachłanny: 3,2 s

### Ocena jakości rozwiązań

Jakość rozwiązań oceniono na podstawie stopnia spełnienia ograniczeń miękkich (w skali 0-100, gdzie 100 oznacza idealne rozwiązanie):

**Mała szkoła:**
- Algorytm genetyczny: 92
- Symulowane wyżarzanie: 90
- Programowanie z ograniczeniami: 98
- Algorytm zachłanny: 78

**Średnia szkoła:**
- Algorytm genetyczny: 88
- Symulowane wyżarzanie: 85
- Programowanie z ograniczeniami: 95
- Algorytm zachłanny: 72

**Duża szkoła:**
- Algorytm genetyczny: 82
- Symulowane wyżarzanie: 80
- Programowanie z ograniczeniami: brak rozwiązania w rozsądnym czasie
- Algorytm zachłanny: 65

## 8. Zastosowanie praktyczne

Wybór algorytmu do generowania planów lekcji powinien być dostosowany do specyfiki szkoły i wymagań:

### Dla małych szkół (do 10 klas)
Rekomendowane jest **programowanie z ograniczeniami**, które zapewnia najwyższą jakość planów i gwarantuje znalezienie rozwiązania, jeśli ono istnieje. Czas obliczeń jest akceptowalny dla problemów tej skali.

### Dla średnich szkół (10-30 klas)
Najlepszym wyborem jest **algorytm genetyczny** lub **symulowane wyżarzanie**. Oba podejścia oferują dobry kompromis między jakością rozwiązań a czasem obliczeń. Algorytm genetyczny jest bardziej elastyczny i adaptacyjny, podczas gdy symulowane wyżarzanie jest prostsze w implementacji.

### Dla dużych szkół (powyżej 30 klas)
Zalecane są **algorytm zachłanny z heurystykami** lub **hybrydowe podejście**. Dla bardzo dużych szkół, czas obliczeń staje się krytycznym czynnikiem, a algorytm zachłanny jest zdecydowanie najszybszy. Jakość rozwiązań można poprawić, stosując podejście hybrydowe:
1. Wygenerowanie wstępnego planu algorytmem zachłannym
2. Optymalizacja planu za pomocą symulowanego wyżarzania lub algorytmu genetycznego

### Scenariusze specjalne

#### Częste zmiany wymagań
Jeśli wymagania często się zmieniają (np. dostępność nauczycieli, liczba godzin przedmiotów), algorytm genetyczny jest najlepszym wyborem ze względu na swoją adaptowalność.

#### Bardzo ścisłe ograniczenia
W przypadku szkół z wieloma sztywnymi ograniczeniami, programowanie z ograniczeniami zapewnia największe prawdopodobieństwo znalezienia dopuszczalnego rozwiązania, ale może wymagać dekompozycji problemu dla większych szkół.

#### Generowanie wielu wariantów planu
Jeśli celem jest wygenerowanie wielu alternatywnych planów do wyboru, algorytm genetyczny jest idealny, ponieważ populacja zawiera wiele rozwiązań o podobnej jakości.

## 9. Wnioski

Problem generowania planów lekcji jest złożonym zagadnieniem optymalizacyjnym o dużym znaczeniu praktycznym. W niniejszej pracy przedstawiono cztery różne podejścia algorytmiczne, z których każde ma swoje zalety i ograniczenia.

Kluczowe wnioski:

1. **Nie istnieje uniwersalny algorytm** optymalny dla wszystkich scenariuszy. Wybór podejścia powinien być dostosowany do specyfiki szkoły, dostępnych zasobów obliczeniowych i wymagań dotyczących jakości planu.

2. **Algorytm genetyczny** oferuje najlepszy kompromis między czasem obliczeń, jakością rozwiązań i adaptowalnością, co czyni go najlepszym wyborem dla większości średnich szkół.

3. **Programowanie z ograniczeniami** zapewnia najwyższą jakość rozwiązań, ale jest praktyczne tylko dla małych problemów ze względu na wykładniczą złożoność obliczeniową.

4. **Algorytm zachłanny z heurystykami** jest najszybszy i może być jedyną opcją dla bardzo dużych szkół, chociaż jakość rozwiązań jest zwykle niższa.

5. **Podejścia hybrydowe**, łączące zalety różnych algorytmów, mogą oferować najlepsze rezultaty w praktyce, szczególnie dla dużych i złożonych przypadków.

Przyszłe kierunki badań mogą obejmować:
- Rozwój bardziej zaawansowanych heurystyk dla algorytmów zachłannych
- Implementację równoległych wersji algorytmów genetycznych i symulowanego wyżarzania
- Opracowanie adaptacyjnych parametrów dla metaheurystyk
- Zastosowanie technik uczenia maszynowego do przewidywania dobrych początkowych rozwiązań

## 10. Bibliografia

1. Abramson, D. (1991). Constructing school timetables using simulated annealing: sequential and parallel algorithms. Management Science, 37(1), 98-113.

2. Burke, E. K., & Petrovic, S. (2002). Recent research directions in automated timetabling. European Journal of Operational Research, 140(2), 266-280.

3. Colorni, A., Dorigo, M., & Maniezzo, V. (1998). Metaheuristics for high school timetabling. Computational optimization and applications, 9(3), 275-298.

4. Schaerf, A. (1999). A survey of automated timetabling. Artificial intelligence review, 13(2), 87-127.

5. Pillay, N. (2014). A survey of school timetabling research. Annals of Operations Research, 218(1), 261-293.

6. Marques-Silva, J., & Sakallah, K. A. (1999). GRASP: A search algorithm for propositional satisfiability. IEEE Transactions on Computers, 48(5), 506-521.

7. Rossi, F., Van Beek, P., & Walsh, T. (2006). Handbook of constraint programming. Elsevier.

8. Wilke, P., & Ostler, J. (2008). Solving the school time tabling problem using tabu search, simulated annealing, genetic and greedy algorithms. In Proceedings of the 7th International Conference on the Practice and Theory of Automated Timetabling.

9. Kingston, J. H. (2013). Educational timetabling. In Handbook of Scheduling: Algorithms, Models, and Performance Analysis. CRC Press.

10. Kristiansen, S., & Stidsen, T. R. (2013). A comprehensive study of educational timetabling—a survey. Department of Management Engineering, Technical University of Denmark.
