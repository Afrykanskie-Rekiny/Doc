# Członkowie projektu:
- Hubert Rybicki (s27501)
- Igor Wasilewski (s28908)
- Jakub Sobczyk (s27518)

# Wstęp:

Generowanie planów lekcji w szkołach jest złożonym problemem, który można rozwiązać za pomocą różnych algorytmów. Problem ten jest blisko związany z zagadnieniem tzw. „problemem przydziału” i często klasyfikowany jako problem NP-trudny. Oznacza to, że nie ma znanych algorytmów, które rozwiązują go efektywnie w przypadku dużych danych w rozsądnym czasie. Jednak istnieje wiele algorytmów heurystycznych i przybliżonych, które mogą być stosowane w praktyce.

# 1. Małe szkoły z umiarkowaną liczbą ograniczeń:
- Algorytm zachłanny lub backtracking mogą być wystarczające, zwłaszcza gdy ograniczenia są proste.
- Zachłanne algorytmy mogą szybko generować rozwiązania, które nie są idealne, ale wystarczające w mniejszych problemach. Przy niewielkiej liczbie nauczycieli i klas ich zastosowanie może być szybkie i skuteczne.
- Backtracking zapewnia dokładne rozwiązanie, ale może być zbyt czasochłonny w przypadku większej liczby danych.
  
# 2. Średniej wielkości szkoły z umiarkowaną liczbą ograniczeń:
- Algorytmy symulowanego wyżarzania oraz algorytmy genetyczne mogą być bardziej efektywne, ponieważ dobrze radzą sobie z przestrzenią rozwiązań, gdzie występuje wiele ograniczeń.
- Symulowane wyżarzanie może przeszukać szeroką przestrzeń możliwych planów, stopniowo je optymalizując. Jest to dobre rozwiązanie, jeśli w szkole obowiązuje wiele złożonych ograniczeń.
- Algorytmy genetyczne działają skutecznie przy umiarkowanych danych. Generują zestawy rozwiązań, krzyżują je i optymalizują, co daje dobre wyniki dla bardziej złożonych planów.

# 3. Duże szkoły z wieloma złożonymi ograniczeniami:
- Programowanie całkowitoliczbowe (ILP) jest najlepszym wyborem, jeśli potrzebne jest dokładne i optymalne rozwiązanie przy bardzo złożonych ograniczeniach. ILP pozwala na uwzględnienie dużej liczby reguł, takich jak preferencje nauczycieli, liczba sal lekcyjnych, minimalizacja okienek itd. Jednak rozwiązanie takich problemów może być czasochłonne dla bardzo dużych danych.
- Algorytmy hybrydowe (np. połączenie algorytmu zachłannego z optymalizacją przy użyciu symulowanego wyżarzania lub genetyki) mogą być bardzo efektywne. Używają prostych algorytmów do szybkiego wygenerowania początkowego rozwiązania, które jest potem optymalizowane przez bardziej zaawansowane metody.
- Tabu Search jest też popularnym algorytmem dla dużych instancji problemu, gdyż może w efektywny sposób unikać lokalnych minimów, poszukując lepszych rozwiązań.

# 4. Specyficzne preferencje i mocno ograniczone zasoby:
- Jeśli zasoby są mocno ograniczone (np. ograniczona liczba sal, nauczyciele z wieloma preferencjami), najlepszym rozwiązaniem mogą być algorytmy programowania całkowitoliczbowego lub algorytmy genetyczne. Te metody pozwalają efektywnie zająć się złożonymi ograniczeniami, które trudno uwzględnić w prostszych algorytmach.
- Algorytmy z kolorowaniem grafów mogą okazać się bardzo efektywne, jeśli kluczowym problemem jest brak konfliktów czasowych (np. nauczyciel może prowadzić tylko jedną lekcję naraz, sala może być używana tylko w jednym czasie).

## Cele Pracy Inżynierskiej:
- implementacja wszystkich wyżej wymienionych algorytmów
- dokumentacja zaimplementowanych algorytmów
- analiza rozwiązań pod kątem czasu wykonania oraz zużytych zasobów i wyciągnięcie wniosków
- (opcjonalnie) interfejs graficzny, wykonanie front endu i back endu w celu zarządzania danymi
