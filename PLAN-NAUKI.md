# Plan nauki Springa: „Spring w akcji" (wyd. 5) + projekt taco-cloud

Stan na 7.10.2026: rozdział 1.3, projekt `taco-cloud` (Spring Boot 3.5.16, Java 21, Maven).
Numeracja rozdziałów jest z pamięci. Sprawdź ją ze spisem treści swojego wydania.

## Zasady pracy (powtarzalne dla każdego rozdziału)

1. **Czytaj z kodem pod ręką.** Każdy przykład z książki wpisuj sam, nie kopiuj.
2. **Po rozdziale: 5 pytań „dlaczego?"** Np. dlaczego `@Controller`, a nie `@RestController`; skąd Spring wie, który bean wstrzyknąć. Odpowiedz własnymi słowami, na głos albo w notatce.
3. **Jedno ćwiczenie „poza książką".** Zmień jakiś szczegół i zobacz, co się zepsuje (usuń adnotację, zmień scope, zepsuj konfigurację). Błędy uczą więcej niż przykłady, które działają.
4. **Notatka w 10 linijkach**, nie streszczenie rozdziału: pojęcia, adnotacje, pułapki.
5. **Commit po każdym podrozdziale.** Zrób `git init` w `taco-cloud`, żeby było widać swoje postępy.

## Ważne: różnice między książką a Twoją wersją (Boot 3.5)

Wydanie 5 powstało na Spring 5 / Boot 2.x. Przykłady z książki wymagają tłumaczenia:

| W książce | W Twoim projekcie (Boot 3.x) |
|---|---|
| `javax.persistence`, `javax.validation` itd. | `jakarta.persistence`, `jakarta.validation` |
| `WebSecurityConfigurerAdapter` (rozdz. o Security) | usunięty. Używasz beana `SecurityFilterChain` i `HttpSecurity` w stylu lambda |
| Starter walidacji dołączany niejawnie | dodaj `spring-boot-starter-validation` |
| Część API Spring Data / Cassandra / Reactor / RSocket | drobne zmiany nazw i wersji; sprawdzaj dokumentację danej wersji |
| Testy `@WebMvcTest` i `MockMvc` | działają tak samo |

Gdy coś się nie kompiluje, to w 90% przypadków jest to `javax` → `jakarta` albo zmiana API w Security. Przy każdej takiej różnicy dopisuj notatkę „książka vs Boot 3". To też dobry materiał do systematyzacji.

## Faza 0: dokończenie rozdziału 1 (1–2 dni)

- Co robi `@SpringBootApplication`: rozbij na `@Configuration`, `@EnableAutoConfiguration`, `@ComponentScan`.
- Struktura projektu Maven, rola `pom.xml` i parenta Boota.
- Pierwszy kontroler (`HomeController`), widok Thymeleaf, test `@WebMvcTest`.
- **Umiesz wyjaśnić:** czym jest kontener IoC, bean, kontekst aplikacji; co robi Boot, kiedy uruchamiasz `main`.
- **Ćwiczenie:** usuń `@SpringBootApplication` i zobacz, co przestaje działać. Przenieś kontroler do pakietu poza `tacos`.

## Faza 1: podstawy webowe i dane (rozdz. 2–4, ok. 2 tygodnie)

- Tematy: Spring MVC, model, formularze, walidacja, `@GetMapping` / `@PostMapping`, widoki Thymeleaf; dostęp do danych przez `JdbcTemplate`, Spring Data JPA, repozytoria; (opcjonalnie) bazy nierelacyjne.
- **Budujesz:** formularz projektowania taco, zapis zamówień, encje `Taco`, `Ingredient`, `TacoOrder`.
- **Umiesz wyjaśnić:** cykl życia żądania w MVC; różnica między `JdbcTemplate` a JPA; co robi `@Transactional`; kiedy walidacja następuje i gdzie widać błędy.
- **Pułapki:** `javax` → `jakarta`; leniwe ładowanie w JPA; H2 console.
- **Powtórka:** napisz własnymi słowami, czym różni się `@Component`, `@Service`, `@Repository`, `@Controller`.

## Faza 2: bezpieczeństwo i konfiguracja (rozdz. 5–6, ok. 1–1,5 tygodnia)

- Tematy: Spring Security (uwierzytelnianie, autoryzacja, użytkownicy, `PasswordEncoder`, CSRF); `@ConfigurationProperties`, profile, `application.properties` / `yml`.
- **Uwaga:** tu najwięcej różnic względem książki (patrz tabela wyżej). Planuj dłuższy czas.
- **Umiesz wyjaśnić:** łańcuch filtrów Security; różnica 401 vs 403; po co hashowanie haseł; jak działają profile.
- **Ćwiczenie:** zablokuj endpoint dla roli, potem napisz test z `@WithMockUser`.

## Faza 3: REST, integracja, komunikaty (rozdz. 7–10, ok. 2 tygodnie)

- Tematy: `@RestController`, `RestClient` / `RestTemplate` (konsumowanie API), kolejki (JMS / RabbitMQ / Kafka), Spring Integration.
- **Budujesz:** REST API do taco i zamówień, klient tego API.
- **Umiesz wyjaśnić:** różnica `@Controller` vs `@RestController`; kody HTTP; serializacja JSON; synchroniczne vs asynchroniczne komunikaty.
- **Uwaga:** `RestTemplate` jest w trybie utrzymaniowym. Przy okazji poznaj `RestClient` (nowość w Spring 6.1).

## Faza 4: programowanie reaktywne (rozdz. 11–14, ok. 2 tygodnie)

- Tematy: Reactor (`Mono`, `Flux`), WebFlux, reaktywna persystencja, RSocket (jeśli jest w Twoim wydaniu).
- To najtrudniejszy kawałek. Poświęć czas na zrozumienie modelu, a nie na samą składnię.
- **Umiesz wyjaśnić:** czym jest backpressure; czym różni się `map` od `flatMap`; kiedy reaktywność się opłaca, a kiedy nie.

## Faza 5: środowisko produkcyjne (ostatnie rozdziały, ok. 1–2 tygodnie)

- Tematy: Actuator, monitoring, wdrożenie, Spring Cloud / usługi rozproszone (zgodnie ze spisem treści).
- **Ćwiczenie:** odpal aplikację jako jar (`./mvnw package`), sprawdź `/actuator/health`.

## Faza 6: systematyzacja (po książce)

- Jedna strona „mapa Springa": kontener → web → dane → security → integracja → reaktywność → produkcja.
- Własna ściąga z adnotacjami (co robi, kiedy użyć).
- Przerób projekt na Spring Boot 4 jako osobne ćwiczenie (zmiany modularyzacji, Jackson 3, Security 7).
- Przepytanie: 20 pytań z całego materiału (Claude może Ci je zadać).

## Szacunek czasu

Przy 5–6 godzinach tygodniowo całość to ok. 3–4 miesiące. Fazy 1–2 dają największy zysk w praktyce, fazę 4 można odłożyć, jeśli w pracy nie używasz WebFlux.
