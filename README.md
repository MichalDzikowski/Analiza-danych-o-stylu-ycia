# Analiza-danych-o-stylu-ycia

## 1. Cel Projektu

Celem tego projektu była dogłębna analiza bogatego zbioru danych dotyczącego stylu życia, diety i treningów 20,000 osób. Projekt składał się z trzech głównych zadań:
1. **Testowanie Hipotez Statystycznych** w celu weryfikacji popularnych "mitów" fitnessowych.
2.  Zbudowanie **modelu regresyjnego** do szacowania procentowej zawartości tkanki tłuszczowej.
3.  Zbudowanie **modelu klasyfikacyjnego** do rekomendowania optymalnego typu diety.

Najważniejszą częścią projektu okazała się identyfikacja i rozwiązanie problemu **wycieku danych (Data Leakage)**, co doprowadziło do stworzenia realistycznych i wiarygodnych modeli.

---

## 2. Kluczowe Odkrycie: Pułapka Wycieku Danych

W obu zadaniach, początkowe modele osiągały **perfekcyjne, 100% wyniki**. Taki rezultat w analizie danych jest sygnałem alarmowym, wskazującym na to, że model "oszukuje", korzystając z informacji, których nie powinien znać.

* **W zadaniu regresji:** Model miał dostęp do cech (`BMI`, `lean_mass_kg`), które są matematycznie powiązane z przewidywaną wartością (`Fat_Percentage`). To tak, jakby próbować przewidzieć wynik równania, znając jego składniki.
* **W zadaniu klasyfikacji:** Model miał dostęp do cech (`Carbs`, `Proteins`, `Fats`), które bezpośrednio **definiują** przewidywany typ diety (np. dieta "Keto" z definicji ma mało węglowodanów i dużo tłuszczu).

Kluczowym krokiem w projekcie było zidentyfikowanie tych "zdradzieckich" cech i zbudowanie nowych, "uczciwych" modeli, które opierają się na realnych, opisowych danych o użytkowniku.

---

## 3. Proces Analityczny i Wyniki

### Zadanie 1: Predykcja Tkanki Tłuszczowej (Regresja)
Po usunięciu cech powodujących wyciek danych, zbudowano nowy model oparty na stylu życia i podstawowych danych biometrycznych.

* **Model:** XGBoost Regressor
* **Wynik (R-kwadrat):** 0.70
* **Wynik (Średni Błąd Bezwzględny):** 1.965 p.p.

**Wniosek:** Model jest w stanie z dobrą dokładnością (błąd ok. 1.965 punktu procentowego) oszacować zawartość tkanki tłuszczowej, wyjaśniając 70% zmienności w danych.

### Zadanie 2: Rekomendacja Diety (Klasyfikacja)
Po usunięciu cech-definicji, zbudowano nowy model oparty na danych o użytkowniku i jego treningach.

* **Model:** Random Forest Classifier
* **Wynik (Accuracy):** 16%

**Wniosek:** Wynik jest na poziomie losowego zgadywania (dla 6 klas `1/6 ≈ 16.7%`). Oznacza to, że na podstawie dostępnych danych o osobie i jej treningach **nie da się wiarygodnie przewidzieć**, jakiego typu dietę stosuje. Wybór diety zależy od innych, niemierzonych w tym zbiorze czynników.

---

## 4. Weryfikacja Mitu Fitnessowego (Test t-Studenta)

Przeprowadzono również analizę statystyczną w celu weryfikacji hipotezy, czy trening siłowy i kardio mają różny wpływ na tętno spoczynkowe.

* **Hipoteza Zerowa (H₀):** Średnie tętno spoczynkowe w grupie 'Cardio' jest takie samo jak w grupie 'Strength'.
* **Metoda:** Niezależny test t-Studenta.
* **Wynik (p-value):** 0.3047

**Wniosek:** Ponieważ wartość p (50.5%) jest znacznie wyższa niż standardowy próg 5%, **nie ma podstaw do odrzucenia hipotezy zerowej**. Obserwowana, minimalna różnica w tętnie między grupami jest **statystycznie nieistotna** i można ją przypisać przypadkowi.
## 4. Główny Wniosek Biznesowy

Analiza wykazała, że:
* ✅ **Można z dużą dokładnością oszacować** procentową zawartość tkanki tłuszczowej na podstawie danych o stylu życia i podstawowych pomiarów, co jest użyteczną funkcją dla aplikacji fitness.
* ❌ **Nie można wiarygodnie rekomendować** konkretnego typu diety na podstawie tych samych danych. Próba budowy takiego systemu zakończyłaby się porażką, ponieważ brakuje nam kluczowych informacji o celach i preferencjach użytkownika.

Projekt ten jest doskonałym przykładem, jak ważna jest krytyczna ocena wyników i zrozumienie danych, aby uniknąć budowania modeli, które działają idealnie tylko "na papierze".

---

## 5. Użyte Technologie

* Python
* Pandas & NumPy
* Matplotlib & Seaborn
* Scikit-learn
* XGBoost
* Jupyter Notebook
