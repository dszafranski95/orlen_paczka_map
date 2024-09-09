Demo Widgetu Ruch - Instrukcja obsługi
Opis projektu
Ten projekt przedstawia integrację widgetu Ruch do wyboru punktu odbioru paczek, z możliwością wyświetlenia mapy i wyboru konkretnego punktu. Po wybraniu punktu pojawia się modal z informacjami o wybranym punkcie, który można zamknąć przyciskiem "OK". Dodatkowo, dane wybranego punktu są zapisywane w localStorage przeglądarki.

Funkcjonalności
Automatyczna inicjalizacja widgetu:

Widget inicjuje się automatycznie po załadowaniu strony i wyświetla punkty odbioru na mapie.
Modal wyświetlający szczegóły wybranego punktu:

Po wyborze punktu odbioru przez użytkownika, pojawia się modal z informacjami o wybranym punkcie oraz metodzie dostawy.
Modal można zamknąć za pomocą przycisku "OK".
Zapis wybranego punktu do localStorage:

Wybrany punkt wraz z adresem i metodą dostawy jest zapisywany w localStorage przeglądarki, co pozwala na późniejsze wykorzystanie tych danych.
Struktura plików
index.html - główny plik HTML zawierający implementację widgetu oraz modal.
style.css - plik CSS, który definiuje wygląd strony i modala (w przypadku chęci rozszerzenia funkcji).
Instrukcja instalacji
Pobranie plików:

Widget wymaga plików JavaScript oraz CSS dostarczonych przez Ruch.
Dołącz te pliki w sekcji <head> strony HTML:
html
Copy code
<script type="text/javascript" src="https://ruch-osm.sysadvisors.pl/widget.js"></script>
<link rel="stylesheet" href="https://ruch-osm.sysadvisors.pl/widget.css"/>
Zainicjowanie widgetu:

Widget jest inicjowany automatycznie po załadowaniu strony dzięki funkcji button_init() w pliku index.html.
Początkowe ustawienia, takie jak adres startowy i filtry (np. COD, typ punktu, rodzaj dostawy), są konfigurowane w funkcji button_init().
Obsługa modala:

Modal wyświetla się po wybraniu punktu na mapie. Zawiera informacje o wybranym punkcie (adres, metoda dostawy).
Modal jest ukryty, dopóki użytkownik nie wybierze punktu.
Główne funkcje kodu
Inicjalizacja widgetu:

javascript
Copy code
function button_init() {
   wid = new RuchWidget('widget_html', {
     loadCb: on_load,
     readyCb: on_ready,
     selectCb: on_select,
     initialAddress: "58-260", 
     sandbox: 0,
     showCodFilter: 1,
     showPointTypeFilter: 1,
     showDeliveryFilter: 1
   });
   wid.init();
}
Funkcja ta tworzy nową instancję widgetu i inicjuje go na stronie.

Wyświetlanie mapy i punktów:

javascript
Copy code
function button_show() {
   wid.showWidget(
     0, 
     { 'R': 10, 'P': 11, 'U': 12, 'A': 13 }, 
     { 'R': 'ruch', 'P': 'partner', 'U': 'partner', 'A': 'orlen' }
   );
   wid.setPointType("PSP, PPP, PSF, PSD");
   wid.setDelivery(1);
}
Funkcja ta wyświetla mapę z punktami oraz ustawia filtry (typ punktów i rodzaj dostawy).

Wyświetlenie modala po wyborze punktu:

javascript
Copy code
function on_select(p) {
   if (p != null) {
      document.getElementById("modal").style.display = "block";
      document.getElementById("modal-content-info").innerHTML = 'Wybrano punkt: ' + p.a + '<br>Metoda: ' + p.m;

      // Zapisz dane do localStorage
      localStorage.setItem('selectedPoint', JSON.stringify({
         id: p.id,
         address: p.a,
         method: p.m
      }));
   }
}
Funkcja ta jest wywoływana, gdy użytkownik wybierze punkt. Wyświetla modal z informacjami o wybranym punkcie oraz zapisuje te dane do localStorage.

Zamykanie modala:

javascript
Copy code
function closeModal() {
   document.getElementById("modal").style.display = "none";
}
Zmienne konfiguracyjne
initialAddress: Adres startowy mapy (może być kodem pocztowym lub pełnym adresem).
sandbox: Wartość 0 lub 1, określająca, czy używać danych testowych (1) lub produkcyjnych (0).
showCodFilter, showPointTypeFilter, showDeliveryFilter: Określają, czy mają być wyświetlane odpowiednie filtry na stronie.
Przykładowe wykorzystanie w lokalnym projekcie
Skopiuj plik index.html do katalogu swojego projektu.
Otwórz plik index.html w przeglądarce internetowej.
Widget automatycznie zainicjuje się i wyświetli mapę.
Wybierz punkt na mapie, aby wyświetlić modal z jego szczegółami.
Dane punktu zostaną zapisane w localStorage i można je później odczytać za pomocą:
javascript
Copy code
let selectedPoint = JSON.parse(localStorage.getItem('selectedPoint'));
Uwagi końcowe
Ten projekt jest gotowym rozwiązaniem do integracji widgetu Ruch z możliwością zapisywania wybranego punktu w localStorage. Zmiany mogą być łatwo dostosowywane do potrzeb aplikacji, a dane zapisane w localStorage mogą być później wykorzystywane w innych częściach systemu.
