# PFSwChO-zadanie2
#### Adrian Duwer 90902
##### Laboratorium Programowanie FullStack w chmurze obliczeniowej - sprawozdanie z zadania 2

### Zadanie 1:
W celu realizacji zadania opisanego powyżej wykorzystany zostanie kod opracowany w ramach pierwszego ze sprawozdań, który dostępny jest pod adresem:
https://github.com/aduwer/PFSwChO-zadanie1. Kluczowe pliki, które zostały tam zamieszczone to index.php oraz Dockerfile. W celu zbudowania, uruchomienia i potwierdzenia poprawności działania łańcucha Github Actions, który zbuduje obrazy kontenera z przygotowaną aplikacją utworzony został katalog .github/workflows. Katalog ten jest standardowym miejscem, w którym przechowuje się pliki konfiguracyjne dla workflow GitHub Actions. Umieszczenie pliku build.yml w tym katalogu umożliwia automatyczne wykonywanie działań w odpowiednim momencie, na przykład po pushu do repozytorium. Jest to rekomendowane miejsce dla plików konfiguracyjnych workflow GitHub Actions, ponieważ jest to standardowa praktyka i ułatwia zrozumienie struktury repozytorium dla innych osób współpracujących nad projektem. Istnieje możliwość umieszczenia pliku build.yml w innym miejscu, jednak należy pamiętać o zmianie ścieżki w deklaracji uses dla każdego kroku w workflow. <br>

#### Kod źródłowy wykorzystany w pliku build.yml umożliwia budowanie obrazów na podstawie pliku Dockerfile. Poniżej przedstawię skrócony opis jego zawartości: <br> 
a) Krok "Checkout Repository" - pobiera zawartość repozytorium, na którym został uruchomiony skrypt. <br> <br>
b) Krok "Set up QEMU" - używa akcji docker/setup-qemu-action@v1 do konfiguracji QEMU na środowisku uruchomieniowym, co jest konieczne do budowania obrazów dla architektury arm64 na platformie x86. <br> <br>
c) Krok "Set up Docker Buildx" - używa akcji docker/setup-buildx-action@v1 do konfiguracji narzędzia Docker Buildx. Buildx jest narzędziem wieloplatformowym, które umożliwia budowanie obrazów Docker dla różnych architektur. <br> <br>
d) Krok "Log in to Docker Hub" - używa akcji docker/login-action@v1 do zalogowania się do Docker Hub. Wymaga podania nazwy użytkownika i hasła do konta Docker Hub. W naszym przypadku hasłem jest wygenerowany przez na platformie Docker Hub specjalny token. Nazwa użytkownika oraz token pobierane są z tzw. „Repository secret”, które umieszczone zostały w ustawieniach repozytorium na platformie GitHub o nazwach DOCKERHUB_USERNAME oraz DOCKERHUB_TOKEN. <br> <br>
e) Krok "Build and push Docker images for amd64 and arm64" - używa akcji docker/build-push-action@v2 do budowania i wysyłania obrazów Docker. Ten krok ma następujące ustawienia: <br>
"context: . " wskazuje, że kontekst budowy (czyli katalog, w którym znajduje się plik Dockerfile oraz inne potrzebne pliki) jest aktualnym katalogiem. <br>
"platforms: linux/amd64,linux/arm64" definiuje, dla jakich platform mają być zbudowane obrazy kontenera. W tym przypadku są to amd64 i arm64. <br>
"push: true" określa, że po zbudowaniu obrazów należy je wypchnąć na zewnętrzny rejestr kontenerów (w tym przypadku Docker Hub). <br>
"tags" - lista tagów, które zostaną przypisane do zbudowanych obrazów. W tym przypadku są to tagi zależne od nazwy użytkownika w Docker Hub oraz oznaczenia wersji dla obrazów dla architektur amd64 i arm64. <br> 

Skrypt ten umożliwia automatyczne budowanie i wysyłanie obrazów Docker do Docker Hub za każdym razem, gdy nastąpi push do gałęzi "main" w repozytorium. Jeśli akcja "Build Docker Images" zakończyła się sukcesem i nie ma żadnych widocznych błędów w logach, można założyć, że Dockerfile został poprawnie zbudowany i obrazy kontenera zostały utworzone zgodnie z oczekiwaniami. <br> 
#### Aby sprawdzić, czy obrazy zostały utworzone dla prawidłowych platform i czy aplikacja działa poprawnie, uruchomiona została lokalnie na odpowiednich platformach. W celu weryfikacji poprawności pobrane zostały obrazy kontenera z Docker Hub na lokalne środowisko. <br> 

a) platforma amd64
* docker pull ajdhjaf/pfswcho-zadanie2:latest-amd64 - pobranie obrazu z Docker Hub
* docker run –p 8080:80 ajdhjaf/pfswcho-zadanie2:latest-amd64 - uruchomienie aplikacji dla platformy amd64
<br> 

b) platforma arm64
* docker pull ajdhjaf/pfswcho-zadanie2:latest-arm64 - pobranie obrazu z Docker Hub
* docker run –p 8080:80 ajdhjaf/pfswcho-zadanie2:latest-arm64 - uruchomienie aplikacji dla platformy arm64

#### Zrzuty ekranu dla poszczególnych części znajdują się w repozytorium. <br>
#### Repozytorium zawiera również plik zbiorczy (Adrian Duwer 90902.pdf), który zawiera opis poszczególnych części wraz z załączonymi zrzutami ekranu.




