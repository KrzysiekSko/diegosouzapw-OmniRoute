# Fork OmniRoute jako AI Control Plane

[English](../../../FORK.md)

## Cel

To repozytorium jest utrzymywanym forkiem projektu [diegosouzapw/OmniRoute](https://github.com/diegosouzapw/OmniRoute). Zachowuje rolę OmniRoute jako bramy AI, jednocześnie rozwijając i weryfikując dodatkowy scenariusz zastosowania: kontrolowaną warstwę **AI Control Plane** pomiędzy autonomicznymi agentami AI a zmieniającym się ekosystemem modeli i providerów.

Celem jest zapewnienie środowiskom agentowym stabilnego, audytowalnego i niezależnego od konkretnego providera interfejsu do wykonywania zadań AI.

## Problem

Agent AI nie powinien samodzielnie integrować się z każdym providerem modeli, sposobem uwierzytelniania, systemem limitów i quota, konwencją endpointów oraz specyficznymi dla providera trybami awarii.

Docelowa abstrakcja wygląda następująco:

```text
Agenci AI / Orkiestratory
          |
          v
   OmniRoute Control Plane
          |
          +-- routing i wybór modelu
          +-- health i dostępność providerów
          +-- quota i limity
          +-- fallback i odporność
          +-- kontrola kosztów i użycia
          +-- telemetria i obserwowalność
          +-- polityki i governance
          |
          v
   Providerzy AI / Modele lokalne
```

Agent decyduje **co należy wykonać**. Control Plane określa **który dopuszczony model/provider powinien wykonać zadanie i zgodnie z jaką polityką routingu**.

## Zakres tego forka

Fork służy do rozwijania, walidowania i utrzymywania funkcji przydatnych w kontrolowanej infrastrukturze agentowej, w szczególności:

- deterministycznego routingu uwzględniającego polityki;
- odpornej obsługi wielu providerów i fallbacku;
- zarządzania uwierzytelnianiem i połączeniami providerów;
- uwzględniania quota, rate limitów i dostępności;
- ograniczeń capabilities modeli i długości context window;
- obserwowalności, audytowalności i evidence operacyjnego;
- routingu uwzględniającego koszty i efektywność tokenową;
- integracji z agentami AI i środowiskami orkiestracji;
- izolowanych wzorców runtime dla środowisk wymagających silniejszych granic operacyjnych.

Hermes Agent jest jednym ze scenariuszy integracyjnych, ale nie jest wymagany do korzystania z tego forka. Prywatne governance Hermesa, credentiale i artefakty operacyjne specyficzne dla środowiska są celowo utrzymywane poza tym publicznym repozytorium.

## Relacja z upstream

Repozytorium nie ma zastępować ani przesłaniać oryginalnego projektu OmniRoute.

Upstream pozostaje:

- <https://github.com/diegosouzapw/OmniRoute>

Fork stosuje model **track and selectively integrate**. Zmiany upstream mogą być oceniane i włączane, gdy pasują do zweryfikowanego baseline forka. Zmiany opracowane tutaj, które są ogólnie użyteczne dla OmniRoute, powinny — tam, gdzie jest to praktyczne — nadawać się do przekazania upstream.

## Model gałęzi i zmian

`main` jest stabilnym baseline integracyjnym forka.

```text
upstream OmniRoute
       |
       | ocena / selektywna integracja
       v
      main
       |
       +-- feature/*
       +-- fix/*
       +-- docs/*
       +-- integration/*
              |
              v
             PR
              |
        weryfikacja / review
              |
              v
             main
```

W normalnym procesie rozwoju należy unikać bezpośrednich mutacji stabilnego baseline. Zmiany runtime, providerów, governance i dokumentacji powinny być możliwe do przeglądu i jednoznacznego przypisania poprzez ukierunkowane Pull Requesty.

## Języki i tłumaczenia

Angielski jest językiem kanonicznym dokumentacji specyficznej dla forka w przypadku rozbieżności znaczeniowych między tłumaczeniami. Polski jest utrzymywany jako pierwsze tłumaczenie referencyjne.

**Zapraszamy do współtworzenia kolejnych tłumaczeń.**

Jeżeli chcesz pomóc udostępnić dokumentację forka w kolejnym języku, zachęcamy do contribution. Identyfikatory techniczne, nazwy API, zmienne środowiskowe, identyfikatory modeli oraz przykłady kodu powinny pozostać niezmienione, chyba że lokalizacja jest technicznie wymagana.

## Współtworzenie projektu

Szczególnie mile widziane są contribution dotyczące:

- routingu AI i wyboru modeli;
- integracji providerów;
- infrastruktury agentowej;
- odporności i fallbacku;
- obserwowalności i telemetrii;
- bezpieczeństwa i governance;
- kontroli quota i kosztów;
- dokumentacji i lokalizacji.

Zmiany powinny mieć jasno określony zakres oraz oddzielać ogólnie użyteczne usprawnienia OmniRoute od prywatnych lub specyficznych dla konkretnego środowiska materiałów operacyjnych.

## Attribution

OmniRoute jest rozwijany przez maintainerów i społeczność projektu upstream i jest udostępniany na licencji MIT. Ten fork zachowuje attribution oraz warunki licencyjne upstream.